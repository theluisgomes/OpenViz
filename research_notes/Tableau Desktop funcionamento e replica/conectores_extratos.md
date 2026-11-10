# Conectores, extratos e preparação de dados no Tableau Desktop

Contexto local da réplica (OpenViz / craft.json): stack Rust + egui; `app/src/data.rs` já carrega CSV (`csv` crate), sample embutido e planilhas Excel/ODS via Calamine (`Source::File` / `Source::Excel` com escolha de sheet). Não há live DB, extract `.hyper`, unions/joins/relationships, Data Interpreter, PDF/JSON/espacial, nem formatos `.twb`/`.twbx`/`.tds`/`.tdsx`.

## Quais famílias de conectores o Tableau Desktop expõe e quais são must-have para um MVP open-source?

### Takeaway
O Connect pane do Desktop organiza conectores em: Tableau Server/Cloud (dados publicados), arquivos locais, servidores/bancos (nativos + “More”), nuvem/SaaS, e genéricos (JDBC/ODBC, Web Data Connector, plugins Exchange). Para um MVP open-source alinhado à réplica atual, o núcleo é a família de arquivos (CSV/texto, Excel) mais um store colunar local no lugar de Hyper; servidores/nuvem e genéricos podem ficar para fases posteriores.

### Cited Findings
- No Connect pane do Desktop, a ordem documentada é: Search for Data → Tableau Server; em seguida tipos de arquivo; depois tipos de servidor comuns/recentes; “More” abre a lista completa de conectores. — [Connect to Your Data](https://help.tableau.com/current/pro/desktop/en-us/basicconnectoverview.htm)
- Tableau fornece conectores nativos otimizados para arquivos e bancos listados; se o tipo não estiver listado, opções genéricas incluem Other Databases (JDBC), Other Databases (ODBC), Web Data Connector ou plugin via Tableau Connector SDK (suporte limitado). — [Connect to Your Data](https://help.tableau.com/current/pro/desktop/en-us/basicconnectoverview.htm)
- A lista de conectores suportados depende da versão/edição comprada; a referência pública de tech specs lista dezenas de fontes (Excel, text/CSV, JSON, PDF, spatial, statistical, Access, SQL Server, Postgres, MySQL, Redshift, BigQuery, Snowflake, Salesforce, etc., além de ODBC 3.0, JDBC 4.0, WDC e Tableau Exchange). — [Connect to Your Data](https://help.tableau.com/current/pro/desktop/en-us/basicconnectoverview.htm); [All Technical Specifications](https://www.tableau.com/products/techspecs)
- Conectores de arquivo documentados individualmente: Text (`.txt`, `.csv`, `.tab`, `.tsv`); Microsoft Excel (`.xls`, `.xlsx`); JSON File (schema inferido dos primeiros 10 000 rows, flatten, opção “Scan Entire Document”); PDF File (scan de páginas para achar tabelas; ignora títulos/notas); Spatial File (Shapefile, MapInfo, KML, GeoJSON, TopoJSON, Esri File Geodatabase; geometrias point/line/polygon; desde 10.2). — [Text File](https://help.tableau.com/current/pro/desktop/en-us/examples_text.htm); [Excel](https://help.tableau.com/current/pro/desktop/en-us/examples_excel.htm); [JSON File](https://help.tableau.com/current/pro/desktop/en-us/examples_json.htm); [PDF File](https://help.tableau.com/current/pro/desktop/en-us/examples_pdf.htm); [Spatial File](https://help.tableau.com/current/pro/desktop/en-us/examples_spatial_files.htm)
- Tech specs agrupam ainda: Statistical Files (SAS `.sas7bdat`, SPSS `.sav`, R `.rdata`/`.rda`); Google Sheets; Microsoft Access (Windows); Tableau Data Extract; spatial (Shapefile, KML, GeoJSON, MapInfo). Public Edition tem subset focada em arquivos + Sheets + OData + WDC. — [All Technical Specifications](https://www.tableau.com/products/techspecs)
- Partner-built connectors do Tableau Exchange aparecem em Additional Connections; após install/restart ficam em “To a Server”; mesmos recursos de data source/visualize; publish para Cloud com partner connectors “not currently supported”. — [Use Partner-Built Connectors](https://help.tableau.com/current/pro/desktop/en-us/gallery_connectors.htm)
- ODBC: usa API ODBC/SQL quando não há named connector; Tableau não garante que todo driver funcione (alguns só para extracts); preferir named connector quando existir; Desktop ≥2023.3 só 64-bit ODBC. — [Tableau and ODBC](https://help.tableau.com/current/pro/desktop/en-us/odbc_tableau.htm)
- Literatura de modelagem classifica fontes Desktop em flat files, database servers, data from the web, e published data sources (Tableau data servers). — [Data Modeling with Tableau (O’Reilly, Cap. 7)](https://www.oreilly.com/library/view/data-modeling-with/9781803248028/B18338_07.xhtml)
- Réplica local (`craft.json` + `data.rs`): Rust + egui; já CSV + Excel/ODS (Calamine) + sample; craft note confirma Excel/ODS e picker de sheet na Data Source screen. — arquivos locais do projeto `/home/l/Documentos/tableaudesktop-2026-3-0/craft.json` e `app/src/data.rs`

### Inferences
- Famílias práticas para inventário de MVP: (1) Files, (2) Relational/warehouse servers, (3) Cloud warehouses & SaaS apps, (4) Cubes/OLAP, (5) Published Tableau sources, (6) Generic extensibility (JDBC/ODBC/WDC/Exchange).
- Must-have MVP open-source (prioridade pela frequência de uso em Desktop “personas” de arquivo e pelo estado da réplica): Text/CSV, Excel (.xlsx/.xls), opcionalmente JSON; depois PDF table extraction e spatial se mapas forem objetivo; live SQL (Postgres/SQLite) só se o produto quiser paridade “to a server” cedo.
- Não-must para MVP: Exchange TACO plugins, cubos SSAS/Essbase, dezenas de SaaS (Salesforce, Marketo, etc.), parity completa de named cloud warehouses.

### Gaps
- A página “list of data connectors” no site Tableau (linkada a partir do Help) não foi enumerada linha a linha nesta pesquisa além do techspecs; a lista exata do Connect pane pode variar por OS (Windows-only Access/SAP BW) e Apple Silicon (alguns ** marcados).
- Não há fonte oficial Tableau que defina “must-have MVP” — a priorização acima é inferência para réplica open-source, não especificação do produto.

## Como Live vs Extract se comportam, e o que está documentado publicamente sobre o Hyper engine?

### Takeaway
Live envia queries dinâmicas ao sistema de origem (agregados voltam ao Tableau); Extract materializa um snapshot local em `.hyper` (filtros, agregação, Top N/sample, refresh full ou incremental) consultado pelo Data Engine Hyper. Hyper é documentado como engine SQL relacional in-memory/columnar (compile queries, paralelo em CPU, spool para disco sob pressão de memória); a Hyper API pública cria/lê/atualiza `.hyper` via `hyperd` + SQL, sem documentar o formato binário proprietário para implementações de terceiros.

### Cited Findings
- Arquitetura híbrida: live connection envia dynamic queries à base e deixa detalhe no source; in-memory extract usa Data Engine powered by Hyper para reduzir carga e acelerar re-queries; extracts podem ser subsets (N registros, %, filtros) e suportam incremental refresh. — [Data Strategy (Blueprint)](https://help.tableau.com/current/blueprint/en-us/bp_data_strategy.htm)
- Extract = subset salvo separado do dataset original, para performance e features não disponíveis na origem; refresh full (substitui tudo) ou incremental (só linhas novas desde o refresh anterior); formato de criação atual é `.hyper`; `.tde` deprecated (Cloud/Public/Server 2023.1+; 2024.2 última que abre `.tde`). — [Extract Your Data](https://help.tableau.com/current/pro/desktop/en-us/extracting_data.htm)
- Benefícios documentados de extracts: datasets grandes (até bilhões de rows), melhor performance de views, funcionalidade extra vs origem, offline access no Desktop. — [Extract Your Data](https://help.tableau.com/current/pro/desktop/en-us/extracting_data.htm)
- Dialog Extract Data: Data Storage (Logical Tables vs Physical Tables); Filters; Aggregation (aggregate visible dimensions, roll up dates); Number of Rows (All / Sample / Top N); Incremental refresh (coluna + date range; replace vs append last rows). Campos hidden são excluídos do extract. — [Extract Your Data](https://help.tableau.com/current/pro/desktop/en-us/extracting_data.htm)
- Logical tables (default): uma tabela de extract por logical table; joins aplicados na criação do extract; habilita extract filters, aggregation, Top N, RAWSQL; não permite append se >1 logical table. Physical tables: joins em query time; condições restritas (só equality joins, mesmos tipos, sem RAWSQL/incremental/filters/Top N/sample/append); útil para tamanho e row-level security. Estrutura de relationships fica no `.tds`, não no `.hyper` — conectar só ao `.hyper` perde relacionamentos. — [Extract Your Data](https://help.tableau.com/current/pro/desktop/en-us/extracting_data.htm)
- Incremental: maioria das fontes; requer All Rows e sem Aggregation; 2024.1+ permite key não única com advanced settings (remove rows com highest value prévio, depois query ≥ previous high); 2024.2+ subrange refresh re-extrai janela temporal para capturar updates. — [Extract Your Data](https://help.tableau.com/current/pro/desktop/en-us/extracting_data.htm)
- Hyper = in-memory Data Engine para create/refresh/query de extracts e também cross-database joins (federação); powers Desktop, Server, Cloud, Public. Benefícios desde 10.5: create mais rápido (sem sort pós-ingest), extracts maiores, queries mais rápidas. Design: memory-optimized, CPU-parallel, compiling query engine, min/max por coluna, mini-indices, block-level dictionaries. Spool para disco se memória insuficiente / >~80% RAM. — [Tableau Server Data Engine](https://help.tableau.com/current/server/en-us/data_engine2_intro.htm)
- Shadow extracts: criados para workbooks baseados em non-legacy Excel/text ou statistical files para carregar mais rápido. — [Tableau Server Data Engine](https://help.tableau.com/current/server/en-us/data_engine2_intro.htm)
- Hyper API (pública): automação sobre arquivos `.hyper` — create, insert, delete, update, read; languages: Python, Java, C++, .NET; bundles `hyperd` + client; utilities HyperProcess, Connection, Inserter, catalog, SQL escaping; use cases incluem ETL custom e extracts para fontes não suportadas nativamente. Docs descrevem Hyper como “full-fledged, relational SQL database system”. — [Hyper API (developer tools)](https://www.tableau.com/developer/tools/hyper-api); [Client-side API | Hyper](https://tableau.github.io/hyper-db/docs/hyper-api/); [Tableau Hyper API learning](https://www.tableau.com/developer/learning/tableau-hyper-api)

### Inferences
- Paridade conceitual Live vs Extract em um MVP: toggle “consultar origem” vs “materializar cópia colunar local”; incremental = append/replace por coluna monotônica (timestamp/id), com caveat de updates/deletes (mitigado no Tableau 2024.1+).
- Hyper API permite interoperar com o ecossistema Tableau, mas não é substituto open-source embutível: é runtime proprietário (`hyperd`) sob licença Tableau. Para réplica OSS, o papel de Hyper deve ser preenchido por DuckDB/Polars/DataFusion+Parquet, não por reimplementação do formato `.hyper`.

### Gaps
- Documentação pública não detalha o layout binário on-disk de `.hyper` (propositalmente fora de escopo desta pesquisa — não fazer reverse-engineering).
- Comportamento exact de “live” em arquivos (Excel/CSV) vs databases: Help de Server menciona shadow extracts para Excel/text/statistical; a fronteira live-vs-shadow para files não foi detalhada em página Desktop dedicada nesta rodada.
- Não foi encontrada especificação pública de throughput/limites de rows para Hyper além de claims qualitativos (“billions of rows”).

## O que a Data Source page permite (relationships, joins, unions, pivot, split, custom SQL, interpret metadata)?

### Takeaway
A Data Source page é a superfície de modelagem pré-análise: canvas lógico (relationships/“noodles”) sobre canvas físico (joins/unions; pivots só aí), grid de preview (~1000 rows) para tipos/renomear/ocultar/aliases/cálculos, Custom SQL como tabela lógica, Split/Custom Split em strings, e Data Interpreter para limpar Excel/CSV/PDF/Google Sheets (títulos, footers, sub-tables) sem alterar o arquivo original.

### Cited Findings
- Data model tem duas camadas: logical layer (Relationships canvas — default; relationships sem join type fixo; Tableau escolhe joins conforme contexto da sheet); physical layer (Join/Union canvas — double-click logical table; joins e unions; pivots só nesta camada). Cada logical table contém ≥1 physical table. — [The Tableau Data Model](https://help.tableau.com/current/pro/desktop/en-us/datasource_datamodel.htm)
- Relationships são o método default/recomendado; preservam LOD das tabelas; joins explícitos (inner/left/right/full outer) quando se quer controle/filtragem/duplicação deliberada; published data sources não podem ser joined (usar relationships); join keys devem ter mesmo tipo. — [Join Your Data](https://help.tableau.com/current/pro/desktop/en-us/joining_tables.htm)
- Union: append de rows; tabelas da mesma connection; idealmente mesma estrutura (nº campos, nomes, tipos); New Union no left pane quando suportado; campos gerados Sheet/Table Name usáveis como join keys. — [Union Your Data](https://help.tableau.com/current/pro/desktop/en-us/union.htm)
- Pivot: colunas → “Pivot field names” / “Pivot field values”; disponível ao selecionar ≥2 colunas em Excel, text, Google Sheets, PDF; outras fontes no Desktop podem pivot via custom SQL (`UNION ALL`). — [Pivot Data from Columns to Rows](https://help.tableau.com/current/pro/desktop/en-us/pivot.htm)
- Trailhead lista prep na Data Source page: rename data source; Data Interpreter; text file options; relationships/joins/unions; edit columns; change data types; multi-connection / cross-database join; grid mostra ~primeiras 1000 rows; grid no Relationships canvas é por tabela selecionada; no Joins canvas mostra dados merged; pivot só no Joins canvas; grid não para cubes. — [Connect to Data in Tableau Desktop (Trailhead)](https://trailhead.salesforce.com/content/learn/modules/the-tableau-workflow/connect-to-data-in-tableau-desktop)
- Custom SQL: New Custom SQL no Data Source page; deve ser um único SELECT*; resultado vira tabela na logical layer; Tableau envelopa a query em SELECT externo (impacto em performance); usos: union across tables, recast para cross-db joins, reduzir/reestruturar dados. — [Connect to a Custom SQL Query](https://help.tableau.com/current/pro/desktop/en-us/customsql.htm)
- Split / Custom Split: no grid da Data Source page (ou Transform no Data pane); delimiter/padrão; auto até ~10 campos; Custom Split controla first/last N ou all occurrences; baseado em função SPLIT; nem todas as fontes suportam; web authoring sem menu Split. — [Split a Field into Multiple Fields](https://help.tableau.com/current/pro/desktop/en-us/split.htm)
- Data Interpreter: Excel, Text/CSV, PDF, Google Sheets; detecta títulos, notes, footers, empty cells, sub-tables; checkbox “Use Data Interpreter”; Review results abre key em Excel; não altera underlying data; limpa toda a connection; indisponível se dados já estão “limpos” ou se há muitas rows/columns (limites documentados na página). Pós-Interpreter: pivot, split, filters. — [Clean Data with Data Interpreter](https://help.tableau.com/current/pro/desktop/en-us/data_interpreter.htm)

### Inferences
- Superfície mínima de prep para MVP da réplica: canvas 1 tabela → grid com rename/tipo/role; depois unions de arquivos; joins simples; pivot em crosstabs Excel/CSV; Interpreter como “nice-to-have” heurístico. Relationships com join contextual por sheet é o diferencial avançado (mais custoso).
- “Interpret metadata” no sentido Tableau = type inference + Data Interpreter + schema levels (JSON) + found tables (PDF/Interpreter), não um único botão genérico.

### Gaps
- Limites exatos de rows/columns que desabilitam Data Interpreter não foram capturados na síntese do fetch (página menciona condições; valores numéricos precisos não constam no excerpt usado).
- Detalhe de “Edit Data Source filters” vs extract filters vs context filters ficou fora do foco desta nota.
- Réplica atual: Data Source screen existe para escolher sheet Excel, mas não implementa relationships/joins/unions/pivot/Interpreter/custom SQL (observação de código, não Help Tableau).

## O que são .twb / .twbx / .tds / .tdsx / .hyper e como se relacionam?

### Takeaway
`.twb` = workbook (XML/lógica de sheets/dashboards/stories) sem embutir dados locais; `.twbx` = zip do `.twb` + cópias de dados locais/imagens/extracts para compartilhar offline. `.tds` = atalho de conexão + metadata (cálculos, defaults) sem dados; `.tdsx` = zip `.tds` + arquivos locais/extracts. `.hyper` = extract colunar local; relacionamentos/estrutura lógica ficam no `.tds`/workbook, não no `.hyper` sozinho.

### Cited Findings
- `.twb`: workbook com uma ou mais worksheets e zero ou mais dashboards/stories. — [Tableau File Types and Folders](https://help.tableau.com/current/pro/desktop/en-us/environ_filesandfolders.htm)
- `.twbx`: packaged workbook = single zip com workbook + supporting local file data + background images; melhor para quem não tem acesso aos dados originais; abrível em Desktop ou Reader; conteúdo armazenado em plain text (dados legíveis). Inclui: background images, custom geocoding/shapes, local cubes, Access, Excel, extracts `.hyper`, text (`.csv`, `.txt`, etc.). — [Tableau File Types and Folders](https://help.tableau.com/current/pro/desktop/en-us/environ_filesandfolders.htm); [Packaged Workbooks](https://help.tableau.com/current/pro/desktop/en-us/save_savework_packagedworkbooks.htm)
- `.hyper`: extract = cópia local de subset ou dataset completo para share, offline, performance. — [Tableau File Types and Folders](https://help.tableau.com/current/pro/desktop/en-us/environ_filesandfolders.htm)
- `.tds`: connection shortcut — info para conectar + modificações (default properties, calculated fields, groups); sem dados reais. — [Tableau File Types and Folders](https://help.tableau.com/current/pro/desktop/en-us/environ_filesandfolders.htm)
- `.tdsx`: zip de `.tds` + local file data (`.hyper`, text, Excel, Access, local cubes). — [Tableau File Types and Folders](https://help.tableau.com/current/pro/desktop/en-us/environ_filesandfolders.htm)
- Relação extract ↔ metadata: arrangement/connections entre tabelas ficam no `.tds`, não no `.hyper`; abrir só `.hyper` perde relationships e dificulta refresh (Tableau trata como original source). — [Extract Your Data](https://help.tableau.com/current/pro/desktop/en-us/extracting_data.htm)
- Extract criado a partir de `.twbx` é armazenado dentro do package; Save dialog pode não aparecer; unpackage para acessar o `.hyper`. — [Extract Your Data](https://help.tableau.com/current/pro/desktop/en-us/extracting_data.htm)
- Legacy `.tde` substituído por `.hyper`; deprecated em produtos cloud/server desde 2023. — [Extract Your Data](https://help.tableau.com/current/pro/desktop/en-us/extracting_data.htm)

### Inferences
- Diagrama mental: Workbook (`.twb`) referencia Data Source (`.tds` ou embutido); Data Source referencia connection live e/ou Extract (`.hyper`); Packaging zipa workbook+assets (`.twbx`) ou datasource+assets (`.tdsx`).
- Para réplica OSS: formato próprio de projeto (já com serde em `data.rs`) é mais realista que parse fiel de `.twb` XML proprietário; `.twbx`/`.tdsx` como “zip de manifesto + Parquet/Arrow” é analogia útil.

### Gaps
- Schema XML interno de `.twb`/`.tds` não é especificação aberta documentada pelo Help citado; blogs de terceiros descrevem comportamento mas não são schema oficial.
- `.tbm` (bookmark), `.tfl` (Prep), etc. existem mas ficam fora do escopo das key questions.

## Quais bibliotecas open-source (Rust ou language-agnostic) podem substituir file connectors e um extract store colunar?

### Takeaway
Para file connectors em Rust: `csv` + Calamine (já na réplica) cobrem CSV/Excel/ODS; JSON via `serde_json`/`polars`; PDF table extraction e spatial exigem crates adicionais (menos “drop-in”). Para extract store: DuckDB (SQL analítico embarcado, CSV/Parquet/JSON/Excel) ou Polars/DataFusion sobre Apache Arrow + Parquet são os substitutos públicos mais próximos do papel de Hyper — sem compatibilidade de arquivo `.hyper`.

### Cited Findings
- Polars: query engine de DataFrames em Rust; SIMD/multithreaded; lazy/eager; streaming larger-than-RAM; Arrow columnar; I/O plugins; MIT. — [pola-rs/polars](https://github.com/pola-rs/polars/)
- DuckDB: analytical DB in-process; lê/escreve CSV, Parquet, JSON, Excel; SQL rico; clientes incluindo Rust (`duckdb-rs` com Arrow/Parquet/JSON/CSV e integração Polars). — [duckdb/duckdb](https://github.com/duckdb/duckdb); [duckdb/duckdb-rs](https://github.com/duckdb/duckdb-rs/)
- Apache DataFusion: query engine Rust com Arrow in-memory; SQL + DataFrame; CSV/Parquet/JSON/Avro; planner + execução vetorizada; Apache-2.0. — [datafusion docs.rs](https://docs.rs/datafusion/latest/datafusion/); [crates.io/datafusion](https://crates.io/crates/datafusion)
- fastexcel: reader Excel rápido Rust/Python baseado em Calamine + Arrow (PyCapsule / Polars). — [ToucanToco/fastexcel](https://github.com/ToucanToco/fastexcel/)
- Calamine: usado pela réplica (`data.rs`) para Excel/ODS via `open_workbook_auto`. — `app/src/data.rs` do projeto local
- Hyper API (não OSS como engine): referência do que um extract store precisa expor (create table, insert, update, delete, SQL query) — útil como checklist de API, não como dependência MIT/Apache embutível. — [Hyper API](https://www.tableau.com/developer/tools/hyper-api)
- Vortex (LF AI): formato colunar Arrow-compatible open-source (alternativa a Parquet com claims de scan/random access); integrações Arrow/DataFusion/DuckDB/Polars. — [vortex-data/vortex](https://github.com/vortex-data/vortex/)

### Inferences
- Mapeamento sugerido para MVP Rust+egui:
  | Papel Tableau | Opção OSS |
  |---|---|
  | Text/CSV connector | `csv` / Polars / DuckDB `read_csv` |
  | Excel connector | Calamine (atual) ou fastexcel→Arrow |
  | JSON connector | serde_json flatten ou Polars/DuckDB |
  | Extract `.hyper` | DuckDB file ou Parquet dataset + DataFusion/Polars |
  | Custom SQL | DuckDB ou DataFusion SQL |
  | Incremental refresh | INSERT/DELETE por chave em DuckDB/Parquet append+compact |
- Evitar prometer leitura/escrita nativa de `.hyper` de usuários Tableau sem Hyper API proprietária; export “compatível Tableau” só via Hyper API se o produto aceitar dependência não-OSS.

### Gaps
- Não foi feita benchmark comparativa DuckDB vs Polars vs DataFusion para workloads “estilo Tableau” (viz queries com GROUP BY + filtros).
- Crates Rust específicas para PDF table extraction e para Shapefile/GeoJSON (ex.: `geo`, `shapefile`, `geojson`) não foram avaliadas em profundidade nesta rodada.
- Licenciamento dual/comercial de alguns componentes DuckDB extensions e drivers ODBC de terceiros precisa de review legal antes de ship — não coberto aqui.
)
