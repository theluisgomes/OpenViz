# Arquitetura Rust para réplica open-source do fluxo Tableau Desktop (OpenViz / egui)

Notas de pesquisa clean-room: conceitos de UX documentados publicamente e padrões de BI open-source; sem redistribuir marca, assets ou binários proprietários. Destinado a planeamento de arquitetura e roadmap faseado — não a implementação.

## Qual é um split sensato de crates/módulos (workbook, catalog, query, viz spec, renderer, connectors, export)?

### Takeaway
Separar um workspace Cargo com crates sem GUI (`ov-workbook`, `ov-catalog`, `ov-query`, `ov-expr`, `ov-viz`, `ov-connectors`, `ov-export`) e um binário `openviz`/`ov-ui` em egui que só orquestra estado e desenha; o projeto actual já esboça essa fronteira em módulos monolíticos e no ROADMAP (`query` atrás das prateleiras).

### Cited Findings
- egui recomenda separar crates por responsabilidade (`egui` / `emath` / `epaint` / `eframe`); apps grandes devem manter o modelo puro sem dependência GUI para testes headless — [egui ARCHITECTURE.md](https://github.com/emilk/egui/blob/main/ARCHITECTURE.md); [danpokhrel tutorial Ch.5](https://danpokhrel.github.io/tutorial/ch05-architecture.html).
- Padrão prático egui: `main` fino → `app` dono de estado → `ui/` por painel; extrair primeiro lógica pura, depois `draw_*` por painel — [Codegiz egui modules](https://www.codegiz.com/blog/rust-module-system-in-egui-separate-ui-from-logic-gui-tutorial/); [Shaken Fist PLAN-app-split](https://shakenfist.com/components/ryll/plans/PLAN-app-split/).
- Apache Superset separa: definição de chart/dashboard (metadata), query context → datasource → transformação → payload de viz; conectores/engine specs à parte — [Superset architecture](https://github.com/apache/superset/blob/master/docs/admin_docs/installation/architecture.mdx); [gitdiagram apache/superset](https://gitdiagram.com/apache/superset); [superset/viz.py BaseViz lifecycle](https://github.com/apache/superset/blob/d23b0cad/superset/viz.py).
- Perspective separa Table (dados columnulares) / View (configuração de pivot/filtros/agregação) / Viewer (UI); queries no motor, UI só pede a janela visível — [Perspective data architecture](https://perspective-dev.github.io/guide/explanation/architecture.html); [large datasets guide](https://perspective-dev.github.io/guide/use_cases/large_datasets.html).
- OpenViz actual: um crate `openviz` com `main`, `app/{mod,views,widgets,dialogs}`, `model`, `data`, `expr`, `chart`, `paint`, `tests` (~6.3k LOC); contrato documentado: prateleiras = API da vista; `paint` desenha marcas; motor futuro atrás de `query` — ficheiros locais `app/src/main.rs`, `app/ROADMAP.md`, `app/GAP.md`.
- Ajuda pública Tableau descreve workspace (Data pane, shelves, Marks, Sheets/Dashboards/Stories) como superfície de produto a espelhar conceptualmente — [Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm).

### Inferences
- Split recomendado (workspace):
  - `ov-workbook` — `WorkbookFile`, `Sheet`, `Dashboard`, `Pill`, `Filter`, `SheetStyle`, versionamento `format`; serde only.
  - `ov-catalog` — papéis Dimension/Measure, Kind, calculated field metadata, relações join/union, geographic roles; não executa queries.
  - `ov-expr` — lexer/parser AST, type check (row vs agg vs table-calc vs LOD), lowering para plano.
  - `ov-query` — trait `QueryEngine` + plano (dims, measures, filters, windows, joins); backend Polars (fase 1) pluggable.
  - `ov-viz` — `Plot` / mark layout a partir de shelves + Show Me rules (sem egui).
  - `ov-render` ou manter no UI — conversão Plot → shapes egui (`paint` actual).
  - `ov-connectors` — trait `Connector` (CSV, Excel/ODS, Parquet extract, depois Postgres/SQLite); entrega Arrow/Polars Frame ao catalog.
  - `ov-export` — CSV, PNG (clip do rect do gráfico), PDF futuro.
  - `openviz` (bin) — egui shell: menus, shelves DnD, Format panel, dashboard layout, threading do `collect()` do motor.
- Mantém-se monorepo único publicado como pasta `app/` até à fase 1B; extrair crates quando `query` e `expr` ganharem testes de paridade e compile times do Polars forem um problema (Polars documenta features opt-in por causa de tempos de compilação — [docs.rs/polars](https://docs.rs/polars/latest/polars/)).
- Plugins: trait object + `libloading`/processo separado só pós-1.0; até lá, conectores como features Cargo (`connectors-postgres`).

### Gaps
- Não há benchmark público do monólito OpenViz vs workspace multi-crate neste repositório.
- Padrão de plugin runtime (vs features Cargo) para BI desktop em Rust não foi encontrado em fonte primária comparável a Tableau Extensions; tratar como decisão pós-1.0.

## Como Polars, DataFusion, DuckDB (FFI) e Arrow comparam como backend analítico desktop?

### Takeaway
Arrow é o formato em memória partilhado, não o motor; para OpenViz desktop, Polars é o encaixe mais natural (Rust-native, lazy group-by/window/join, alinhado ao ROADMAP); DuckDB via `duckdb-rs` bundled é o melhor plano B para SQL e spill-to-disk; DataFusion brilha se se quiser um toolkit extensível de planos, não um DataFrame pronto.

### Cited Findings
- Arrow define layout columnar partilhado; motores passam dados sem serialize se falarem Arrow nativamente — [4Analytics in-process engines](https://www.4analytics.com/guides/how-to-choose-an-in-process-analytics-engine/).
- Polars: DataFrame Rust sobre Arrow, API eager/lazy, expressões compostas, group_by/join/window; recomenda lazy + allocator custom (jemalloc/mimalloc até ~25% impacto) — [docs.rs/polars](https://docs.rs/polars/latest/polars/).
- DataFusion: motor SQL/DataFrame extensível em Rust/Arrow, pensado para embutir em produtos (TableProvider, UDFs, optimizer); “toolkit, not finished product” — [DataFusion introduction](https://datafusion.apache.org/user-guide/introduction.html); [4Analytics](https://www.4analytics.com/guides/how-to-choose-an-in-process-analytics-engine/).
- DuckDB: BD analítica embutida C++, SQL-first, spill-to-disk maduro, storage persistente; Rust via `duckdb` crate com feature `bundled` (compila DuckDB) e Arrow interchange — [duckdb.org Rust client](https://duckdb.org/docs/2.0/clients/rust/overview); [duckdb-rs GitHub](https://github.com/duckdb/duckdb-rs/).
- Comparativos 2025–2026: Polars = DataFrame library; DuckDB = embedded DB; DataFusion = engine toolkit; workloads >RAM favorecem DuckDB spill; Rust-native app favorece Polars ou DataFusion — [iotdigitaltwinplm comparison](https://iotdigitaltwinplm.com/polars-vs-duckdb-vs-datafusion-apache-arrow-dataframe-engine-comparison-2026/); [Iceberg Lakehouse single-node](https://iceberglakehouse.com/posts/2026-05-23-single-node-data-engineering-duckdb-datafusion-polars-lakesail/); [Ibis bench](https://ibis-project.org/posts/ibis-bench/).
- Micro-benchmark amador (evento log): DuckDB tipicamente mais rápido que Polars/DataFusion em várias agregações SQL; autor nota que H2O.ai sugere Polars ≈ DuckDB e a configuração pode não estar óptima — [zaynetro OLAP post](https://www.zaynetro.com/post/2023-playing-with-olap/).
- Perspective: motor C++/WASM columnuar + Arrow; UI pede só janela do resultado; modo virtual traduz View → SQL em DuckDB/ClickHouse/Polars — [Perspective large datasets](https://perspective-dev.github.io/guide/use_cases/large_datasets.html); [perspective-dev/perspective](https://github.com/finos/perspective).

### Inferences
- Recomendação faseada alinhada ao ROADMAP local: trait `QueryEngine` → implementação `PolarsEngine` primeiro (paridade com `model::group`); Arrow RecordBatch como fronteira de I/O entre connectors e engine.
- DuckDB como segundo backend quando: joins grandes >RAM, extratos `.duckdb` persistentes, ou SQL Lab-like; custo = build C++ bundled, binário maior, FFI.
- DataFusion como caminho se no futuro se quiser optimizer rules custom (LOD lowering como plan nodes) ou Ballista; overhead de wiring TableProviders vs “só carregar LazyFrame”.
- UI egui deve correr `collect()` em thread/worker e aplicar resultado no frame seguinte (Polars/DataFusion não são async-friendly no thread UI — [docs.rs/polars](https://docs.rs/polars/latest/polars/) recomenda `spawn_blocking` em contexto async).

### Gaps
- Não há números públicos medidos no hardware-alvo do README OpenViz para 1M linhas group-by com Polars vs DuckDB vs DataFusion neste repo.
- Maturidade exacta de “out-of-core” Polars vs spill DuckDB muda com releases; revalidar na versão pinada antes da fase 2.

## Como desenhar um subset de linguagem de expressões (campos calculados)?

### Takeaway
Modelar três camadas semânticas documentadas pela ajuda Tableau (row-level, aggregate/view-level, table calc) mais LOD FIXED como subset 1.0; AST próprio → type inference → lowering para expressões Polars/SQL, em vez de reimplementar um avaliador row-a-row para tudo o que o motor já faz.

### Cited Findings
- Tableau distingue: basic calcs (nova coluna), LOD (`{FIXED|INCLUDE|EXCLUDE … : agg}`), table calculations (sobre a “virtual table” da vista; ranking, running total, percent of total, period diffs) — [Calculations overview](https://www.tableau.com/learn/get-started/calculations); [LOD expressions](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_lod.htm); [Choosing calculation type](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_understand_which.htm); [Table calculations](https://help.tableau.com/current/pro/desktop/en-us/calculations_tablecalculations.htm).
- Table calcs exigem addressing vs partitioning sobre dimensões da vista; só fazem sentido quando os valores já estão na viz — [Table calculations](https://help.tableau.com/current/pro/desktop/en-us/calculations_tablecalculations.htm); [Choosing calculation type](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_understand_which.htm).
- Cenários “só table calc”: ranking, recursão (totais corridos), moving calc, inter-row — [Choosing calculation type](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_understand_which.htm).
- Polars expressions: `Fn(Series)->Series` compositáveis, paralelas, usáveis em select/agg/window — [docs.rs/polars](https://docs.rs/polars/latest/polars/).
- OpenViz `expr.rs` actual: AST `Num|Field|Neg|Bin|Call`; funções ABS, ROUND, MIN, MAX, SQRT, IFNULL; avaliação row callback; sem IF, datas, texto, table calc, LOD — ficheiro local `app/src/expr.rs`; `app/GAP.md`.
- ROADMAP fase 3: alargar linguagem; table calcs sobre resultado agregado; LOD FIXED primeiro; compilador escreve plano `query`; `expr` fica frente do parser — `app/ROADMAP.md`.

### Inferences
- Design AST sugerido:
  - `Expr` com variantes tipadas pós-check: `Row(Expr)`, `Agg(AggExpr)`, `Table(TableExpr)`, `Lod { kind: Fixed, dims, agg }`.
  - Parser inspira sintaxe familiar (`[Campo]`, IF/THEN/ELSE, funções UPPER/LEFT/DATEPART) sem copiar implementação proprietária.
  - Type rules: misturar row e agg sem agregação explícita = erro (espelha distinção Tableau row vs `SUM(a)/SUM(b)`).
  - Lowering: row/agg → Polars `Expr` / `group_by().agg()`; table calc → pós-processamento no `Grouped`/`DataFrame` resultado (window over partition keys derivados das shelves); FIXED → subquery/join aggregate-back (Polars join após group_by das dims FIXED).
- Parâmetros: binding no catalog (`Parameter { name, value, domain }`) substituído antes do lowering.
- Testes: golden vectors por função; paridade `model::group` vs Polars; casos LOD/table calc da documentação pública (não de workbooks `.twb` proprietários).

### Gaps
- Semântica exacta de INCLUDE/EXCLUDE e ordem de filtros vs LOD FIXED no Tableau é complexa; ROADMAP adia INCLUDE/EXCLUDE — documentação pública cobre a forma, mas edge cases de filtros de contexto merecem suite própria quando se implementar.
- Não se pesquisou um dialect open-source que clone 1:1 a sintaxe Tableau; DuckDB SQL / Polars Expr são backends, não front-ends de calc language.

## Que formato de persistência de workbook é realista?

### Takeaway
Manter JSON versionado `.ovw` (já especificado) como formato nativo; evoluir para pacote zip (JSON + Parquet extract) pós-relações; inspirar layout de dashboard em grelha Metabase/Lightdash e serialização YAML de entidades, sem tentar fidelidade de escrita `.twb` no 1.0.

### Cited Findings
- OpenViz `.ovw`: JSON UTF-8, `format: openviz-workbook-1`, source + calcs + sheets + dashboards; vista sem linhas; `style` opcional com `serde(default)` — `app/WORKBOOK.md`.
- Tableau `.twb` é XML; `.twbx` é pacote; comunidade/migração extrai XML para datasources, calcs, filters, layouts — [Lightdash migrate from BI](https://docs.lightdash.com/get-started/migrate-from-another-bi-tool) (descreve unzip `.twbx` → `.twb` XML; não é spec oficial Tableau).
- Metabase: cards/dashboards em representação YAML portátil + API JSON com grelha 24 colunas (`row/col/size_x/size_y`, `parameter_mappings`) — [Metabase representation format](https://github.com/metabase/agent-skills/blob/main/skills/metabase-representation-format/SKILL.md); [dashboard API spec](https://github.com/onsen-ai/metabase-skill/blob/main/specs/dashboard-api-spec.md).
- Lightdash: dashboards YAML com `tiles` e coordenadas `x/y/w` — [Lightdash dashboard reference](https://github.com/lightdash/lightdash/blob/main/skills/developing-in-lightdash/resources/dashboard-reference.md).
- Evidence: BI-as-code em Markdown + SQL + components (não um workbook binário) — [evidence.dev](https://evidence.dev/).
- Perspective: estado do viewer serializável / re-hidratável; dados Arrow à parte — [Perspective overview](https://perspective.finos.org/guide/explanation/view.html).
- ROADMAP OpenViz: campo `format` sobe em breaking changes; pós-1.0 zip JSON+Parquet; leitura best-effort `.twb` depois do formato estável — `app/ROADMAP.md`.

### Inferences
- Estratégia: (1) `.ovw` JSON canónico human-diffable; (2) `.ovwx` = zip(`workbook.json`, `extracts/*.parquet`, fonts opcionais) quando extratos existirem; (3) import `.twb` best-effort separado, nunca export como claim de compatibilidade total.
- Schema: documentar com JSON Schema ou testes “livro antigo abre”; ignorar campos desconhecidos (já na spec WORKBOOK).
- Dashboard model: migrar de `columns: 1..=4` + lista de sheet ids para tiles `{sheet_id, x, y, w, h}` inspirados em Metabase/Lightdash quando a fase 4 (drag) começar — evita reescrever persistência duas vezes se se adoptar grelha cedo na fase 0/4.
- YAML como alternativa: útil para git-friendly (Lightdash/Metabase serdes) mas JSON já está; opcional export YAML, não substituir `.ovw` no 1.0.

### Gaps
- Spec completa `.twb` XML não é publicada oficialmente pela Tableau como formato aberto estável; qualquer import depende de engenharia reversa comunitária — risco legal/técnico: só leitura best-effort documentada.
- Evidence/Perspective não mapeiam 1:1 para shelves+marks; inspiração limitada a serialização de view config vs dados.

## O que UIs BI open-source ensinam sobre priorização de features para UX de authoring “Desktop-like”?

### Takeaway
Priorizar o loop authoring mínimo (ligar dados → encoding visual → filtro → dashboard com acção → export) antes de mapas, stories e servidores; separar camadas query/viz como Superset/Perspective; curar tipos de gráfico (RawGraphs) em vez de proliferar marks.

### Cited Findings
- Superset: chart builder + dashboards + SQL Lab; async/cache/workers são opcionais para features avançadas — [Superset architecture](https://github.com/apache/superset/blob/master/docs/admin_docs/installation/architecture.mdx).
- Perspective: performance desktop-like no browser via motor columnuar + virtual scroll; configuração View simétrica API/UI — [Perspective](https://perspective.finos.org/guide/explanation/view.html); [large datasets](https://perspective-dev.github.io/guide/use_cases/large_datasets.html).
- RawGraphs: foco spreadsheet → encoding → SVG export; charts curados, não “qualquer chart” via PR — [rawgraphs-app](https://github.com/rawgraphs/raw/); [rawgraphs-charts](https://github.com/rawgraphs/rawgraphs-charts).
- Metabase: pipeline de produto (necessidade → síntese → design → build); features fora de scope explicitamente — [Metabase CONTRIBUTING](https://www.metabase.com/docs/latest/CONTRIBUTING); roadmap actual enfatiza dashboards, transforms, viz table — [Metabase roadmap](https://www.metabase.com/roadmap).
- Lightdash: semantic layer + YAML; migração a partir de Tableau extrai conceitos (datasource, calcs, filters, layouts) — [migrate guide](https://docs.lightdash.com/get-started/migrate-from-another-bi-tool).
- Evidence: reports as code; forte em export PDF/XLSX e reproducibilidade, fraco como DnD shelves — [evidence.dev](https://evidence.dev/).
- Pedidos de utilizador OpenViz (questions.json): Format (cores/tipografia), tooltips, dashboard vazio com drag, export rico — ficheiro local `questions.json`.
- ROADMAP fases 0→6: estilo/export/tooltip → folha entregável + motor Polars → multi-tabela → calcs → dashboard acções → mapas → 1.0 — `app/ROADMAP.md`.

### Inferences
- Ordem de valor Desktop-like: (1) shelves+Show Me estáveis, (2) format/export/tooltip (percepção de “app completa”), (3) query engine+joins, (4) calcs/table/LOD, (5) dashboard cross-filter, (6) maps/stories/live SQL.
- Não competir com Superset/Metabase no servidor; diferencial OpenViz = authoring offline egui + ficheiro workbook.
- Show Me = regras curadas (RawGraphs lesson); novos chart types só com encoding rules e snapshot visual.
- Semantic layer leve (roles, calcs, num formats no catalog) espelha Metabase measures/segments sem exigir warehouse.

### Gaps
- Nenhuma das UIs open-source listadas é um clone de shelves Tableau em desktop Rust; lições são por analogia de produto, não de código.
- “Visual similarity” a Tableau Desktop não tem métrica pública standard; Craft checks actuais só validam “arranca e desenha” (`craft.json`).

## O que o projeto já implementa e quais os gaps vs arquitectura alvo?

### Takeaway
OpenViz já cobre o esqueleto de UX (shelves, Show Me, folhas, painel simples, `.ovw`, CSV/Excel, calcs mínimos, Format panel parcial, testes unitários); faltam o crate `query`/Polars, joins, linguagem de calc completa, dashboard DnD/acções, conectores DB, pacote zip, e testes de snapshot visual sistemáticos.

### Cited Findings
- Stack: Rust + egui/eframe 0.36, MIT, binário `openviz`; deps: serde_json, csv, calamine, image/png — `app/Cargo.toml`; `craft.json` (`stack: Rust + egui`).
- Dados: `Dataset` com colunas `Vec` (text/num/date), Sample/CSV/Excel+ODS; uma tabela; sem join — `app/src/data.rs`; `app/GAP.md`.
- Modelo: `Pill`, shelves, aggs SUM/AVG/MEDIAN/CNT/CNTD/MIN/MAX, date parts, filters, `SheetStyle`, `WorkbookFile`, `group()` in-memory — `app/src/model.rs`.
- Viz: `chart.rs` (~948 LOC) Show Me + agregação; `paint.rs` (~1066 LOC) eixos/marcas/legendas/hover; Format UI em `views.rs` (paleta, cores, tamanhos, font path) — fontes locais.
- Expr: subset aritmético + poucas funções — `app/src/expr.rs`.
- Persistência e docs: `WORKBOOK.md`, `ROADMAP.md`, `GAP.md`, `WEEK.md`.
- Craft: build + screenshot 1440×900 passam; nota de que não abre ficheiros do programa original — `craft.json`.
- questions.json: utilizadores pedem Format/tooltips/dashboard vazio; algumas respostas de sessões read-only ficaram desactualizadas face ao código (Format já existe parcialmente em `views.rs`).
- Hover: `allocate_rect` ainda referido em `views.rs` (dashboard) e historicamente em `paint` — gap de tooltip parcialmente documentado em `GAP.md`/`ROADMAP.md`.
- Testes: CSV types, formulas, workbook roundtrip/style default — `app/src/tests.rs` (~288 LOC); sem egui_kittest snapshots ainda.
- egui_kittest oferece snapshot PNG com threshold YIQ e `UPDATE_SNAPSHOTS` — [egui_kittest README](https://github.com/emilk/egui/blob/main/crates/egui_kittest/README.md).

### Inferences
- Gap map (actual → alvo 1.0 ROADMAP):

| Área | Actual | Alvo |
|---|---|---|
| UI shell | Monólito `app/` + panels | `ov-ui` fino; painéis por ficheiro; worker para query |
| Catalog/data | Uma `Dataset` Vec | Relações, Parquet extract, roles geo |
| Query | `model::group` | `ov-query` + Polars |
| Expr | Row arithmetic | AST tipado + table/LOD FIXED |
| Viz spec | `chart::compute` | `ov-viz` independente de egui |
| Renderer | `paint` egui | Manter; clip PNG; PDF |
| Connectors | CSV/Excel | +Parquet; depois Postgres/SQLite |
| Workbook | `.ovw` JSON | +format bumps; `.ovwx` zip |
| Dashboard | Checkbox grid 1–4 | Drag tiles + filter actions |
| Testing | Unit + craft screenshot | Paridade query + egui_kittest snapshots + golden calcs |

- Arquitectura não exige reescrita big-bang: introduzir `query` atrás de `chart::compute` com teste de paridade (já prescrito no ROADMAP fase 1B).
- Legal: nome OpenViz; sem assets Tableau; clean-room documentado em `GAP.md` (`reference/` vazio).

### Gaps
- `GAP.md` e algumas respostas em `questions.json` estão parcialmente desactualizados relativamente ao Format panel e `SheetStyle` já presentes no código — inventário de “current” deve preferir `src/` + `WORKBOOK.md` + `ROADMAP.md`.
- Não se mediu similaridade visual quantitativa vs qualquer referência; craft só confirma smoke render.
- Estado exacto do bug do tooltip em `paint.rs` vs `views.rs` após mudanças recentes não foi validado em runtime nesta sessão de pesquisa (só leitura estática / grep).

## Roadmap faseado (síntese arquitectura + produto)

### Takeaway
O ROADMAP local 0–6 já é coerente com a pesquisa externa; o único reforço arquitectural crítico pré-fase 3 é o trait `QueryEngine` + fronteira Arrow e a disciplina AST row/agg/table/LOD.

### Cited Findings
- Fases OpenViz 0 (repo/estilo/PNG/tooltip/dashboard vazio) → 1A apresentação + 1B Polars → 2 joins → 3 calcs → 4 dashboard acções → 5 maps → 6 1.0 package — `app/ROADMAP.md`.
- Escolha de motor in-process deve seguir forma de workload e linguagem da equipa; desktop single-user encaixa na categoria embedded columnar — [4Analytics](https://www.4analytics.com/guides/how-to-choose-an-in-process-analytics-engine/).
- Snapshot visual egui disponível via egui_kittest — [egui_kittest](https://github.com/emilk/egui/blob/main/crates/egui_kittest/README.md).

### Inferences
- Fase −1 (agora, docs only): congelar diagrama de crates e trait `QueryEngine` / `Connector` / `ExprLowering` neste doc de notas.
- Fase 0: sem Polars ainda; corrigir hover; Format discoverability; empty dashboard; PNG clip; CI + egui_kittest smoke de 1–2 ecrãs.
- Fase 1B: Polars atrás de `query`; Arrow/Parquet extract path; UI sem mudança.
- Fase 2–3: joins no plano; expr compiler; table calcs no resultado agregado.
- Fase 4+: dashboard tiles + actions alimentam o mesmo plano (filtros de acção).
- Pós-1.0: DuckDB backend opcional; `.twb` read; server noutro repo.

### Gaps
- Esforço em pessoa-mês por fase não está calibrado com dados empíricos de contribuidores.
- Decisão PDF dependency ainda aberta no ROADMAP.
