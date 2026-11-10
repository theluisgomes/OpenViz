# Modelo conceitual VizQL: shelves, pills e semântica analítica

## How does VizQL map visual encodings (Rows, Columns, Color, Size, Label, Detail, Tooltip, Path, Shape, Angle) to queries and marks?

### Takeaway
VizQL (herdeiro do formalismo Polaris) trata a configuração de shelves/Marks como uma especificação visual declarativa que compila simultaneamente em consultas (SQL/MDX: SELECT, GROUP BY, WHERE, ORDER BY, agregações) e em comandos de desenho (tipo de mark + encodings retinais). Dimensions nas shelves definem o nível de detalhe (grain) e o GROUP BY; measures são agregadas a esse grain; encodings no Marks card mapeiam campos a propriedades visuais sem necessariamente alterar o eixo da tabela.

### Cited Findings
- O formalismo Polaris é uma linguagem de consulta visual declarativa que especifica apresentações gráficas em tabela e **compila tanto as queries quanto os drawing commands** necessários à visualização. — [ACM CACM: Polaris (Stolte, Tang, Hanrahan)](https://dl.acm.org/doi/fullHtml/10.1145/1400214.1400234)
- A interface principal é drag-and-drop de campos para **shelves**; cada configuração de campos nas shelves é uma **visual specification** que determina: mapeamento de data sources para layers; número/ordem de rows/columns/layers; seleção e partição de records em panes; grouping/aggregation; tipo de gráfico em cada pane; mapeamento de campos para propriedades retinais dos marks. — [ACM CACM: Polaris](https://dl.acm.org/doi/fullHtml/10.1145/1400214.1400234)
- A **table algebra** (operandos = campos ordinais/quantitativos; operadores cross ×, nest /, concatenation +) formaliza a estrutura de eixos: campos ordinais particionam com headers; campos quantitativos geram eixos. Expressões em x/y/z definem colunas, linhas e layers. — [ACM CACM: Polaris](https://dl.acm.org/doi/fullHtml/10.1145/1400214.1400234)
- Em Polaris/Tableau, **todos os campos nas shelves entram no SELECT**; measures são agregadas; dimensions entram no **GROUP BY**, com dimensions adicionais no shelf de Level-of-Detail; o filter shelf corresponde a WHERE; sort/agregação via menus nos pills. — [ACM CACM: Polaris](https://dl.acm.org/doi/fullHtml/10.1145/1400214.1400234)
- VizQL é a linguagem proprietária por trás do rendering do Tableau: drag-and-drop gera uma query visual que é traduzida/compilada em SQL (ou MDX). Gera WHERE (filtros), ORDER BY (sort), GROUP BY (nível de detalhe/agregação); estende SQL/MDX com sintaxe para estrutura de rows/columns, mark type e atributos visuais. Shelves + Marks card + filters **coletivamente** geram a expressão SQL enviada à fonte. — [What is VizQL? | Tableau](https://www.tableau.com/learn/how-to/what-is-vizql)
- VizQL é proprietário e **não pode ser visto ou customizado** pelo usuário; o SQL compilado pode ser inspecionado via Performance Recorder / logs. — [What is VizQL? | Tableau](https://www.tableau.com/learn/how-to/what-is-vizql)
- A pipeline de dados do Tableau compila/otimiza a query visual em uma ou mais statements SQL, executadas no Hyper ou live; um Logical Query / Hyper Query Plan é um DAG de nós (extract, project, select/filter, aggregate, join). — [To See and Understand Queries | Tableau](https://www.tableau.com/blog/see-and-understand-queries)
- Whitepaper Tableau: VizQL statements definem o mapeamento de records do banco para marks; alguns campos controlam geometria (posição, size, orientation); outros controlam atributos visuais (color, pattern). — [Visual Analysis for Everyone (PDF)](http://viewer.media.bitpipe.com/1178304416_32/1204134507_351/VisualAnalysisforEveryone.pdf)
- **Rows / Columns**: dimensions discretas criam headers de linha/coluna; campos contínuos criam eixos. Dimensions em Rows/Columns aumentam o número de marks (nível de detalhe). Em queries Tableau, dimensions na view são expressas em SQL como **Group By**. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- Adicionar uma dimension a Rows, Columns **ou** a propriedades do Marks card (Color, Size, etc.) afeta o **level of detail**; Detail no Marks card separa marks por membros da dimension **sem** mudar a estrutura de headers da tabela. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm); [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- **Marks card**: define mark type e encodings Color, Size, Shape, Text/Label, Detail, Tooltip. Múltiplos campos em Label, Detail, Tooltip e Color; Size e Shape aceitam apenas um campo por vez. — [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm)
- **Color**: discrete → paleta categórica por membro; continuous → legenda quantitativa com range contínuo. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm); [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- **Size**: discrete → separa marks (como Detail) e atribui tamanho por membro; continuous → tamanho proporcional ao valor. — [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- **Detail**: dimension no Detail aumenta granularidade dos marks sem alterar estrutura de rows/columns. — [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- **Path**: disponível só com mark types Line ou Polygon; controla estilo de linha (linear/step/jump) ou ordem de conexão dos marks (dimension ou measure). — [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm); [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- **Angle**: aparece no Marks card quando mark type = Pie; determina a medida angular das fatias (ex.: Sales em Angle → 360° = total; Color particiona fatias). Pie **nunca** é escolhido automaticamente. — [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- **Shape**: mark type Shape e/ou encoding Shape no Marks card; dimension → shape único por membro; measure → bins e shape por bucket. — [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- **Automatic mark type** (regra dos campos internos de Rows/Columns): Text se dimensions em ambos; Shape se measures em ambos; Bar se dimension + measure; Line se date + measure. — [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- **Show Me** sugere viz types a partir dos campos selecionados e cria a view automaticamente conforme requisitos mínimos de cada tipo. — [Use Show Me to Start a View](https://help.tableau.com/current/pro/desktop/en-us/buildauto_showme.htm)
- Polaris: propriedades retinais baseadas em Bertin (shape, size, orientation, color, texture); taxonomia de gráficos por eixos: ordinal-ordinal, ordinal-quantitative, quantitative-quantitative. — [ACM CACM: Polaris](https://dl.acm.org/doi/fullHtml/10.1145/1400214.1400234)
- Tableau blog oficial: Polaris introduziu especificação interativa via drag-and-drop, VizQL descrevendo visualização + data query, e retrieval live do banco. — [From Prototype to Product](https://www.tableau.com/blog/prototype-product-software-engineering-tableaus-early-days)
- Paper original Stanford Polaris: visual specs geram um conjunto preciso de relational queries. — [Stanford Polaris page](https://graphics.stanford.edu/papers/polaris/)

### Inferences
- Para uma réplica Rust, o “motor VizQL” MVP é essencialmente: (1) AST da visual specification a partir das shelves; (2) compilação para plano de agregação (GROUP BY = union de dimensions em Rows/Columns/Marks LOD encodings); (3) renderização de marks com encodings retinais; (4) heurística Automatic mark a partir dos innermost fields.
- Tooltip é encoding de apresentação (hover), tipicamente sem alterar GROUP BY além do LOD já definido pelos outros shelves — o Help enfatiza Detail/Color/Size/Label como afeto ao LOD; Tooltip é listado como propriedade multi-campo no Marks card, mas a documentação oficial de “how dimensions affect LOD” cita Rows/Columns e encodings do Marks card de forma genérica; tratar Tooltip como LOD-affecting quando o campo for dimension é a interpretação conservadora alinhada a “qualquer shelf que carregue a dimension no nível de detalhe da virtual table” (também usado em table calcs).

### Gaps
- Sintaxe interna exata / AST proprietária do VizQL não é publicada (só comportamento e papers Polaris). — [What is VizQL?](https://www.tableau.com/learn/how-to/what-is-vizql)
- Semântica precisa de Tooltip vs Detail no GROUP BY SQL não está detalhada com a mesma clareza que Detail nas páginas oficiais de marks/LOD.
- Orientação (orientation) existia em Polaris como encoding retinal; no Desktop atual o Marks card público enfatiza Color/Size/Label/Detail/Tooltip/Shape/Path/Angle — mapeamento 1:1 Polaris→UI atual não está documentado campo a campo nas Help pages consultadas.
- Algoritmo completo do Show Me (além de “requisitos mínimos por viz type”) não está publicado em detalhe normativo.

---

## What is the precise meaning of discrete vs continuous and dimension vs measure in Tableau, including default aggregations?

### Takeaway
Há **dois eixos ortogonais**: (1) **role** dimension vs measure (como o campo entra na query: agrupar vs agregar) e (2) **scale** discrete vs continuous (como o campo é desenhado: headers vs eixos; paleta categórica vs range). Azul = discrete, verde = continuous. Measures são agregadas por default (tipicamente SUM); dimensions tipicamente não. Defaults de role/scale usam heurísticas de tipo/cardinalidade e são convertíveis.

### Cited Findings
- Cada campo recebe data type e um role: tipicamente discrete dimension ou continuous measure (menos comum: continuous dimension ou discrete measure). — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- **Dimensions** agrupam/segmentam dados (qualitativos: nomes, datas, geo); **afetam o level of detail**. **Measures** são agregadas (quantitativos); ao arrastar para a view, Tableau aplica agregação. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- **Continuous** (verde): eixo em Rows/Columns. **Discrete** (azul): headers em Rows/Columns. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm); [Visual Cues and Icons](https://help.tableau.com/current/pro/desktop/en-us/tips_visualcues.htm)
- Cue de measure no pill: agregação prefixada, ex. `AVG([Age])`. Exceções: datas com granularidade (`WEEK(...)`); dimensions agregadas com CNT/ATTR; table calcs (ícone delta); view desagregada; cubes (agregação na fonte). — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- Strings/Boolean **não podem** ser continuous; datas são a continuous dimension mais comum; numeric dimensions e todas as measures podem ser discrete ou continuous. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- Converter measure para Discrete **não** remove agregação: continua sendo measure agregada, só muda headers vs axis. Converter dimension para Measure aplica agregação e tipicamente produz eixo contínuo. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm); [The Best Visualization for You](https://www.tableau.com/learn/get-started/desktop-viz-design) (converte discrete ≠ convert to dimension)
- Em queries, dimensions na view → **Group By**. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- Measures agregadas automaticamente; agregação default pode ser vista/alterada; na estrutura de dados para análise, default de measures é **SUM**. — [Data Aggregation in Tableau](https://help.tableau.com/current/pro/desktop/en-us/calculations_aggregation.htm); [Structure Data for Analysis](https://help.tableau.com/current/pro/desktop/en-us/data_structure_for_analysis.htm)
- Default aggregation configurável via Default Properties > Aggregation no Data pane; tipo de agregação “varia dependendo do tipo de view” — Help aconselha sempre verificar. — [Edit Default Settings for Fields](https://help.tableau.com/current/pro/desktop/en-us/datafields_fieldproperties.htm); [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- Agregações predefinidas incluem: SUM, AVG, Median, Count, Count Distinct, Min, Max, Percentile, Std. Dev / Pop., Variance / Pop., Attribute (ATTR), Dimension, Disaggregate. — [Data Aggregation in Tableau](https://help.tableau.com/current/pro/desktop/en-us/calculations_aggregation.htm)
- Polaris/CACM: escala (ordinal vs quantitative) e role (dimension vs measure) são **ortogonais**; heurísticas de tipo e cardinalidade definem defaults mas ambos são alteráveis. Measures usam aggregation function; dimensions formam groups. Quantitative→continuous (axes); ordinal→discrete (headers). — [ACM CACM: Polaris](https://dl.acm.org/doi/fullHtml/10.1145/1400214.1400234)
- **Calculated fields**: criados quando a fonte não tem o campo necessário; salvos no data source. Resultado de cálculo agregado é sempre measure (incl. ATTR/MIN de dimension). Não misturar agregado e desagregado na mesma expressão. — [Work with Data Fields](https://help.tableau.com/current/pro/desktop/en-us/datafields_understanddatawindow.htm); [Aggregate Functions](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_aggregate_create.htm)
- **Parameters**: variáveis workbook-wide (number/date/string) que substituem constantes em calculations, filters ou reference lines; precisam ser referenciados e tipicamente mostrados via parameter control. — [Create Parameters](https://help.tableau.com/current/pro/desktop/en-us/parameters_create.htm); [Work with Data Fields](https://help.tableau.com/current/pro/desktop/en-us/datafields_understanddatawindow.htm)
- **Sets**: campos customizados que definem subset; dynamic (condição/top, membership muda com dados; single dimension) vs fixed (membership estático; multi-dimension possível); na viz tipicamente In/Out ou Show Members; combináveis (união/interseção/diferença). — [Create Sets](https://help.tableau.com/current/pro/desktop/en-us/sortgroup_sets_create.htm)
- **Groups**: combinam membros relacionados de uma dimension em categorias de nível mais alto (correção de dados / what-if); opção Include Other; campo de grupo no Data pane. — [Group Your Data](https://help.tableau.com/current/pro/desktop/en-us/sortgroup_groups_creating.htm)
- **Bins**: convertem continuous measure (ou numeric dimension) em dimension discreta de intervalos iguais; header = lower limit inclusivo; fórmula sugerida de nº de bins: `3 + log2(n) * log(n)`; binned fields **não** podem ser usados em calculations (workaround via FLOOR). — [Create Bins from a Continuous Measure](https://help.tableau.com/current/pro/desktop/en-us/calculations_bins.htm)
- **Hierarchies**: datas auto-hierarquizadas; hierarchies custom (ex. Region→State→County) permitem drill +/- no pill. — [Create Hierarchies](https://help.tableau.com/current/pro/desktop/en-us/qs_hierarchies.htm)
- Ícones: azul=discrete, verde=continuous, `=` calculation, Venn=set, +/- hierarchy, delta=table calc, cilindro laranja=secondary blend source. — [Visual Cues and Icons](https://help.tableau.com/current/pro/desktop/en-us/tips_visualcues.htm)

### Inferences
- O modelo mental correto para o engine é uma matriz 2×2 (Dim/Meas × Discrete/Continuous), não “blue=dimension / green=measure”.
- Default SUM é o comportamento documentado mais estável para measures numéricas novas; a frase “varies by view type” sugere heurísticas adicionais (ex. Count em histogramas via Show Me) que devem ser tratadas como regras de UI/Show Me, não como mudança da agregação default do campo no Data pane.

### Gaps
- Tabela normativa completa de “qual agregação default por data type × situação de view” não está publicada de forma exaustiva; Help diz que o tipo “varies depending on the type of view” sem enumerar todos os casos. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- Heurísticas exatas de classificação automática Dim/Meas na conexão (além de “data type and domain cardinality” no paper) não estão na Help atual com pseudocódigo.

---

## How do Relationships (logical layer) differ from Joins (physical layer) and from Data Blending?

### Takeaway
O data model Tableau tem **duas camadas**: logical (Relationships / “noodles”, tabelas normalizadas, joins contextuais na análise) e physical (Joins/Unions com tipo de join fixo, merge em uma tabela lógica). **Blending** é outro mecanismo: sheet-level, queries independentes por data source, agregação antes da combinação visual, comportamento tipo left join (primary/secondary) — não cria um data source blended publicável.

### Cited Findings
- Data model: **logical layer** (default) combina tabelas com **relationships (noodles)**; **physical layer** combina com **joins/unions** (double-click na logical table). — [The Tableau Data Model](https://help.tableau.com/current/online/en-us/datasource_datamodel.htm)
- Logical tables permanecem distintas (normalizadas), LOD nativo preservado; physical tables são merged numa flat table que define a logical table. Relationships não usam join type up-front; Tableau escolhe joins automaticamente conforme campos/contexto da viz. — [The Tableau Data Model](https://help.tableau.com/current/online/en-us/datasource_datamodel.htm); [How Relationships Differ from Joins](https://help.tableau.com/current/online/en-us/datasource_relationships_learnmorepage.htm)
- Relationships: sem join type up-front; defer joins até análise; many-to-many / full outer supportados; não duplicam aggregates (com Performance Options Many-to-Many); reduzem necessidade de LOD FIXED para deduplicar. Joins: estáticos, Venn icons, tipo/cláusula obrigatórios, merge fixo, podem dropar unmatched ou duplicar aggregates. — [How Relationships Differ from Joins](https://help.tableau.com/current/online/en-us/datasource_relationships_learnmorepage.htm)
- Relationships são o método default/recomendado; joins ainda existem no physical layer quando se quer controle/filtragem/duplicação deliberada. Relationships “eventualmente usam joins behind the scenes”. — [Join Your Data](https://help.tableau.com/current/pro/desktop/en-us/joining_tables.htm)
- **Blending**: traz info de secondary data source para a view com a primary; **nunca combina dados diretamente**; query independente por fonte → agrega ao nível apropriado → apresenta juntos; sheet-by-sheet; não cria blended data source publicável. — [Blend Your Data](https://help.tableau.com/current/pro/desktop/en-us/multiple_connections.htm)
- Primeiro campo usado define **primary** (check azul); demais fontes na sheet = **secondary** (check/barra laranja); linking fields; comportamento comparável a **left join** (só valores secondary com match na primary). — [Blend Your Data](https://help.tableau.com/current/pro/desktop/en-us/multiple_connections.htm)
- Comparativo oficial (Plan the Data Source): Relationships = potencial de joins contextuais sem merge fixo; Joins = merge fixo com tipo; Unions = append rows; Blends = fontes separadas, agregadas a nível comum, mimic left join. Blending útil quando linkagem muda por sheet ou fontes que não permitem relationships/joins (ex. cubes). — [Plan the Data Source](https://help.tableau.com/current/pro/desktop/en-us/datasource_plan.htm)
- Whitepaper blending: primary estabelece contexto; blending começa ao trazer campos de outra fonte; arquitetura de integração criada em runtime adaptando-se às queries. — [Data Blending whitepaper](https://www.tableau.com/learn/whitepapers/dynamic-data-blending)

### Inferences
- Para réplica: **joins físicos** (single logical table) cobrem o MVP de multi-tabela estilo pré-2020; **relationships** exigem query planner que, por viz, escolha quais tabelas tocar e quais joins/agregações emitir (mais próximo do valor moderno do Tableau); **blending** é pipeline paralelo (2+ result sets + join pós-agregação no client/viz layer).
- Noodles ≠ joins com UI diferente: a diferença semântica central é **quando** e **com que cardinalidade/LOD** o join acontece.

### Gaps
- Algoritmo exato de seleção de join type em relationships (inner vs left vs etc. por combinação de campos/filtros) não está publicado como especificação formal — só comportamento descrito (“automatically selects join types”).
- Detalhes do “multi-fact relationships” / field-level relatedness (2024.2+) foram apenas mencionados de passagem nas páginas de Data pane; não aprofundados nesta pesquisa.

---

## What are LOD FIXED/INCLUDE/EXCLUDE, table calc addressing/partitioning, and when each is used?

### Takeaway
**LOD expressions** recalculam agregações no data source em granularidade explícita (FIXED = dimensões declaradas, independente da view; INCLUDE = view + dims extras; EXCLUDE = view menos dims) e interagem com a hierarquia de filtros. **Table calculations** transformam valores **já na virtual table da viz** (local), particionando/endereçando dimensions do LOD da view — úteis para running total, % of total, rank, difference; não enxergam dados filtrados fora da viz.

### Cited Findings
- Sintaxe LOD: `{[FIXED | INCLUDE | EXCLUDE] <dimension declaration> : <aggregate expression>}` dentro de `{}`. — [Level of Detail Expressions](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_lod.htm)
- **FIXED**: calcula só com dims especificadas, **sem referência** ao LOD da view; ignora filtros da view **exceto** context, data source e extract filters. Ex.: `{ FIXED [Region] : SUM([Sales]) }`. — [Level of Detail Expressions](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_lod.htm); [FIXED LOD](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_lod_fixed.htm)
- **INCLUDE**: dims especificadas **além** das dims da view; útil para grain mais fino no DB e reaggregação na view. — [Level of Detail Expressions](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_lod.htm)
- **EXCLUDE**: remove dims da view do cálculo; útil para % of total / diferença vs média geral; não usável em expressões row-level sem dims. — [Level of Detail Expressions](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_lod.htm); [EXCLUDE LOD](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_lod_exclude.htm)
- Também existe **Table-Scoped LOD** (escopo da tabela). — [Level of Detail Expressions](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_lod.htm)
- Filtros: FIXED antes de dimension filters (após context); INCLUDE/EXCLUDE após dimension filters. Exemplo: ratio com FIXED no denominador mantém total global mesmo filtrando states. — [Filters and LOD Expressions](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_lod_filters.htm); [Understanding LOD whitepaper](https://www.tableau.com/learn/whitepapers/understanding-lod-expressions)
- FIXED dá o mesmo resultado independente da viz; INCLUDE/EXCLUDE mudam com a estrutura da view. — [EXCLUDE LOD](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_lod_exclude.htm)
- **Table calculation**: transformação aplicada aos valores na visualização; computed on **local data in Tableau**; baseado no que está na viz; **não considera** measures/dimensions filtrados fora da viz. Casos: rankings, running totals, percent of total. — [Transform Values with Table Calculations](https://help.tableau.com/current/pro/desktop/en-us/calculations_tablecalculations.htm)
- Virtual table da table calc = dimensions no level of detail (shelves/cards da worksheet). Toda dimension do LOD deve ser **partitioning** (escopo; cálculo reinicia por partição) ou **addressing** (direção do cálculo). — [Transform Values with Table Calculations](https://help.tableau.com/current/pro/desktop/en-us/calculations_tablecalculations.htm)
- Compute Using presets: Table (across/down/across then down/down then across), Pane (...), Cell, Specific Dimensions. Em Specific Dimensions, dims **selecionadas** = addressing; **não selecionadas** = partitioning; opção At the level para restart por posição. — [Transform Values with Table Calculations](https://help.tableau.com/current/pro/desktop/en-us/calculations_tablecalculations.htm)
- Ícone delta no pill indica table calculation. — [Visual Cues and Icons](https://help.tableau.com/current/pro/desktop/en-us/tips_visualcues.htm)

### Inferences
- Regra prática de uso: **LOD** quando o grain do cálculo deve divergir do grain da query/viz **no data source** (cohort, tamanho de cliente, % vs total não filtrado); **table calc** quando a operação é relativa à **layout/ordem da viz** (running along months, % of row, rank within pane).
- LOD ≈ “subquery/agregação aninhada + join back”; table calc ≈ “window function / pós-processamento do result set da viz”.

### Gaps
- Ordem de operações completa do Tableau (extract → DS filter → context → FIXED → dim filter → INCLUDE/EXCLUDE → measure filter → table calc, etc.) é amplamente citada na comunidade; a hierarquia FIXED vs INCLUDE/EXCLUDE vs filtros está nas fontes oficiais acima, mas uma página única “Order of Operations” canônica não foi fetchada nesta rodada (whitepaper LOD resume FIXED vs INCLUDE/EXCLUDE na filtering hierarchy).
- Semântica formal de Table-Scoped LOD (`{SUM([Sales])}`) pouco detalhada além da menção na página LOD.
- Interação exata LOD × Relationships (quando FIXED deixa de ser necessário) descrita qualitativamente, sem spec de rewrite de query.

---

## Which of these concepts are mandatory for a credible MVP replica vs phase-2?

### Takeaway
Um MVP crível deve reproduzir o núcleo Polaris/VizQL: pills Dim/Meas × Discrete/Continuous, shelves Rows/Columns + Marks (Color/Size/Label/Detail/Tooltip), agregação+GROUP BY, mark types Automatic básicos (bar/line/text/shape), filtros simples, calculated fields row-level/aggregate básicos, e joins físicos single-table. LOD, table calcs, Relationships, blending, sets/groups/bins/hierarchies/parameters avançados e encodings Path/Angle/Pie/Density são fase 2 (diferenciam “Desktop-like” mas não bloqueiam o loop visual analysis fundamental).

### Cited Findings
- O paper Polaris valida que o núcleo de valor é: shelves → visual specification → queries + drawing; table algebra; taxonomia de marks; encodings retinais; dimensions/measures com aggregation. Tableau Desktop comercializou esse núcleo. — [ACM CACM: Polaris](https://dl.acm.org/doi/fullHtml/10.1145/1400214.1400234); [From Prototype to Product](https://www.tableau.com/blog/prototype-product-software-engineering-tableaus-early-days); [Stanford Polaris](https://graphics.stanford.edu/papers/polaris/)
- VizQL unifica descrição de tabela/gráfico com compilação SQL/MDX; shelves + marks + filters são a linguagem do usuário. — [What is VizQL?](https://www.tableau.com/learn/how-to/what-is-vizql); [Visual Analysis for Everyone PDF](http://viewer.media.bitpipe.com/1178304416_32/1204134507_351/VisualAnalysisforEveryone.pdf)
- Automatic mark type a partir de innermost Rows/Columns é comportamento documentado central à UX. — [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- Relationships são o default moderno e recomendado vs joins; blending é sheet-level e pós-agregação. — [How Relationships Differ from Joins](https://help.tableau.com/current/online/en-us/datasource_relationships_learnmorepage.htm); [Blend Your Data](https://help.tableau.com/current/pro/desktop/en-us/multiple_connections.htm)
- LOD e table calcs são camadas analíticas explícitas além do grain da view; cada um com modelo de scoping próprio. — [Level of Detail Expressions](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_lod.htm); [Table Calculations](https://help.tableau.com/current/pro/desktop/en-us/calculations_tablecalculations.htm)
- Sets, groups, bins, hierarchies, parameters são features de modelagem/UX documentadas no Help, construídas sobre o sistema de fields — não fazem parte do formalismo Polaris original publicado. — [Create Sets](https://help.tableau.com/current/pro/desktop/en-us/sortgroup_sets_create.htm); [Group Your Data](https://help.tableau.com/current/pro/desktop/en-us/sortgroup_groups_creating.htm); [Create Bins](https://help.tableau.com/current/pro/desktop/en-us/calculations_bins.htm); [Create Hierarchies](https://help.tableau.com/current/pro/desktop/en-us/qs_hierarchies.htm); [Create Parameters](https://help.tableau.com/current/pro/desktop/en-us/parameters_create.htm)

### Inferences

**MVP (fase 1) — obrigatório para “parecer Tableau” no ciclo hipótese→viz:**
| Conceito | Por quê |
| --- | --- |
| Dim vs Meas + Discrete vs Continuous (blue/green pills) | Ortogonalidade documentada; base de query + rendering |
| Rows/Columns shelves + table structure (headers vs axes) | Table algebra / visual spec |
| Marks: type Automatic + Bar/Line/Text/Shape/Circle; Color, Size, Label, Detail, Tooltip | Compilação query+draw; LOD da view |
| Agregação default (SUM etc.) + GROUP BY a partir das dimensions no LOD | Semântica central Polaris/Help |
| Filters shelf básico (dimension members / measure range) | WHERE na compilação VizQL |
| Calculated fields: row-level + aggregate simples | Análise sem sair do produto |
| Single table + joins físicos (left/inner/right/full) | Multi-tabela mínima sem planner lógico |
| Show Me mínimo ou atalhos equivalentes (opcional mas alta ROI de UX) | Acelera credibilidade |

**Fase 2 — diferenciadores “Desktop completo”:**
| Conceito | Por quê adiar |
| --- | --- |
| LOD FIXED/INCLUDE/EXCLUDE + ordem de filtros | Subqueries aninhadas; pós-MVP analítico |
| Table calcs (addressing/partitioning, quick calcs) | Engine de pós-processamento da virtual table |
| Relationships / noodles / logical layer | Query planner contextual; substitui muito uso de LOD |
| Data blending primary/secondary | Segundo pipeline de execução |
| Sets (In/Out, combined, set actions) | Cohorts; pode aproximar com filters/calcs no MVP |
| Groups / bins / hierarchies | Açúcar de modelagem sobre dimensions |
| Parameters + parameter actions | Interatividade; MVP pode hardcodar |
| Path, Angle/Pie, Polygon, Gantt, Density, Map | Marks especializados |
| Dual axis, Measure Names/Values, reference lines, forecasting | Camadas de produto além do núcleo VizQL |

**Fase 3 / polishidade alta (fora do escopo conceitual mínimo):** Hyper engine, Performance Recorder parity, cube/MDX, multi-fact relatedness 2024.2+, set actions dashboard — dependem de infra ou features de produto não cobertas pelo formalismo Polaris.

### Gaps
- Não há roadmap oficial Tableau dizendo “ordem de implementação para clones”; a priorização MVP vs fase-2 acima é **inferência de engenharia** ancorada em (a) o que Polaris/VizQL papers tratam como núcleo e (b) o que o Help oficial trata como extensões (LOD, table calc, relationships, blend).
- Critérios de “credibilidade” de usuários (ex.: “sem FIXED não é Tableau”) são subjetivos e não mensurados nesta pesquisa.
- Patentes específicas (números/claims) sobre VizQL/shelves não foram levantadas nesta rodada — apenas papers acadêmicos e docs/whitepapers públicos.
