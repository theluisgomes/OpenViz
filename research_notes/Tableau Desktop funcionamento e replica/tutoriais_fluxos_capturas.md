# Tableau Desktop: tutoriais, fluxos e inventário visual de capturas

## What is the step-by-step flow of a typical Tableau Desktop session (connect → prep → worksheet → Show Me / Marks → filters → dashboard → format/export)?

### Takeaway
A sessão canónica de Tableau Desktop segue Start page → conectar dados → (opcional) Data Source page com relationships/joins e grelha → worksheet com Data pane + shelves + Marks + Show Me → filtros/cores → dashboard com actions → story/export/share; o tutorial oficial “Get Started” materializa isto em 8 passos com Sample – Superstore.

### Cited Findings
- O tutorial oficial “Get Started with Tableau Desktop” orquestra exactamente: (1) Connect, (2) drag-and-drop first look, (3) focus with filters/color, (4) maps, (5) drill-down / Top filter, (6) dashboard, (7) story, (8) share — orçamento 1–3 horas, baseado numa história de retalho com Sample – Superstore. — [Tutorial: Get Started with Tableau Desktop](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-home.htm)
- Na Start page o utilizador escolhe connector (ficheiro, servidor, Saved Data Sources); clicar **Sample - Superstore** abre worksheet, coloca campos no Data pane com tipos/roles atribuídos automaticamente; o separador **Data Source** (canto inferior esquerdo) mostra até ~1 000 linhas e permite sort/hide/split/rename sem alterar o ficheiro original. — [Step 1: Connect to a sample data source](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-connect.htm)
- Workspace canónico (worksheet): (A) workbook name, (B) cards/shelves, (C) toolbar, (D) view/canvas, (E) ícone Start page, (F) Side Bar = Data + Analytics, (G) Data Source tab, (H) status bar, (I) sheet tabs (worksheet/dashboard/story). — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm)
- Combinar tabelas: camada lógica = **relationships** (“noodles”) na Data Source canvas (default); camada física = joins/unions (double-click na logical table). Relationships recomendadas como primeira abordagem; joins só quando necessário. — [Relate Your Data](https://help.tableau.com/current/pro/desktop/en-us/relate_tables.htm); [The Tableau Data Model](https://help.tableau.com/current/pro/desktop/en-us/datasource_datamodel.htm); [Join Your Data](https://help.tableau.com/current/pro/desktop/en-us/joining_tables.htm)
- Construir vista: arrastar campos para Columns/Rows ou “active areas”; pills azuis = discrete/dimensions, verdes = continuous/measures; Marks card controla Color/Size/Label/Detail/Tooltip/Shape; Show Me recomenda tipos de gráfico. — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm); [Build Common Chart Types](https://help.tableau.com/current/pro/desktop/en-us/dataview_examples.htm); [Start Building by Dragging Fields](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_dragging.htm)
- Barras / linhas / scatter: exercícios oficiais em “Build Common Chart Types in Data Views” usam todos Sample – Superstore. — [Build Common Chart Types](https://help.tableau.com/current/pro/desktop/en-us/dataview_examples.htm)
- Dual axis: arrastar medida para o lado direito da vista até linha tracejada preta, **ou** right-click na medida em Rows/Columns → **Dual Axis**; até 4 eixos empilhados (2 Columns + 2 Rows); Synchronize Axis no eixo secundário. — [Add Axes for Multiple Measures](https://help.tableau.com/current/pro/desktop/en-us/multiple_measures.htm)
- Filtros: right-click campo no Data pane → **Show Filter**; cards de filtro à direita da vista, arrastáveis; opções Edit Filter, Apply to worksheets, Only relevant values, etc. — [Filter Data from Your Views](https://help.tableau.com/current/pro/desktop/en-us/filtering.htm); [Get Started Step 3 (Focus)](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-focus.htm)
- Dashboard actions: Dashboard → Actions → Add Action (Filter, Highlight, Go to Sheet, Change Parameter, Change Set Values); triggers Hover / Select / Menu. — [Actions and Dashboards](https://help.tableau.com/current/pro/desktop/en-us/actions_dashboards.htm); [Parameter Actions](https://help.tableau.com/current/pro/desktop/en-us/actions_parameters.htm)
- Export: Worksheet → Export → Image (BMP/JPEG/PNG/SVG); Worksheet → Export → Crosstab to Excel; File → Print to PDF; também Copy Image / Copy Crosstab. — [Export Views as Image](https://help.tableau.com/current/pro/desktop/en-us/save_export_image.htm); [Export Data](https://help.tableau.com/current/pro/desktop/en-us/save_export_data.htm); [Print / Print to PDF](https://help.tableau.com/current/pro/desktop/en-us/printing.htm)
- Toolbar Desktop inclui Undo/Redo/Save, New Data Source, Pause/Run Updates, New Worksheet (+ dashboard/story), Duplicate, Clear, Swap, Sort Asc/Desc, Highlight, Group, Show Mark Labels, Fix Axes, Fit, Show/Hide Cards, Presentation Mode, Share, **Show Me**. — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm)

### Inferences
- O fluxo “connect → Data Source prep → build sheets (bars/lines/scatter) → dual axis → filters → dashboard actions → export” é o núcleo beginner→intermediate que um clone deve cobrir para “parecer Tableau em uso”.
- Sample Superstore + Get Started 8 passos + série “Learn Tableau Desktop” (vídeos) + “Build Common Chart Types” formam o trilho comportamental de referência, não apenas UI estática.

### Gaps
- Não há capturas locais de Tableau Desktop autêntico neste repositório (`reference/` vazio; clean room proíbe ler o DMG) — o fluxo “autêntico” abaixo é documentado via help.tableau.com, não via PNG local. — [capture/README.md](/home/l/Documentos/tableaudesktop-2026-3-0/capture/README.md); [app/GAP.md](/home/l/Documentos/tableaudesktop-2026-3-0/app/GAP.md)
- URL antigo `datasource_relationships_overview.htm` devolve 404; documentação actual está em `relate_tables.htm` / `datasource_datamodel.htm`.

---

## Which official Tableau tutorials / free training videos / Sample Superstore exercises are the gold-standard references?

### Takeaway
Os gold standards públicos são: (1) tutorial escrito Get Started (8 steps, Superstore), (2) Free Training → “Learn Tableau Desktop” (8 vídeos ~26 min), (3) “Build Common Chart Types” com Superstore, (4) webinar “Introduction to Tableau Desktop: Part 1 – Superstore”.

### Cited Findings
- **Get Started Tutorial (escrito)** — 8 passos, Superstore, 1–3h: connect → first look → focus → maps → drill-down → dashboard → story → share. — [Get Started home](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-home.htm)
- Superstore também disponível em Tableau Public Sample Data se não estiver embutido. — [Step 1 Connect](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-connect.htm)
- **Free Training Videos** em tableau.com/learn/training: secção **Learn Tableau Desktop** — 8 vídeos, ~26 min: Getting Started; Connecting to Data; Navigating the Workspace; Dimensions/Measures vs Continuous/Discrete; Building Dashboards; Combining Data; Mapping; Calculations. — [Free Training Videos](https://www.tableau.com/learn/training)
- Série Cloud/Server no mesmo hub cobre Relate Tables, Data Pane, Shelves/Cards, Show Me, Filters, Building a Dashboard (containers, actions) — útil como checklist de comportamento mesmo para Desktop. — [Free Training Videos](https://www.tableau.com/learn/training)
- **Build Common Chart Types in Data Views** — todos os exercícios usam Sample – Superstore. — [dataview_examples](https://help.tableau.com/current/pro/desktop/en-us/dataview_examples.htm)
- Webinar Vault: **Introduction to Tableau Desktop: Part 1 – Superstore** (~56 min) — launch, connect, first questions. — [Intro webinar](https://www.tableau.com/learn/webinars/intro-tableau-desktop-part1-superstore)
- Learning path “Getting Started with Tableau Desktop” / Fundamentals / Intermediate em learning paths. — [Data Steward Learning Path](https://www.tableau.com/learn/learning-paths/data-steward)
- Workspace tour video ligado oficialmente: “Navigating the Workspace in Tableau Desktop” (~3 min) a partir da página Workspace. — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm)
- Desktop Free Edition aponta para os mesmos getting-started resources. — [Desktop Free Edition](https://www.tableau.com/products/desktop-free/download?os=windows)

### Inferences
- Para a equipa Rust, a ordem de referência comportamental deveria ser: Get Started steps 1–6 (até dashboard) → Common Chart Types (bar/line/scatter/heatmap/pie) → dual axis + dashboard filter actions → export image/PDF.
- O dataset interno da réplica (“Sample – Shop Orders”, 2 400 rows) espelha o papel pedagógico do Superstore, não o schema Superstore (Category/Sub-Category/Segment/Customer/…).

### Gaps
- Não se listaram aqui os títulos exactos de cada exercício individual dentro de `dataview_examples.htm` (página índice; subpáginas não foram todas fetched nesta ronda).
- Conteúdo exacto frame-a-frame dos vídeos “Learn Tableau Desktop” não foi transcrito (só metadados/tópicos do hub de treino).

---

## What exactly appears in each local screenshot (annotated UI regions)?

### Takeaway
**Todas as PNGs locais inventariadas são da réplica OpenViz (Rust), não do Tableau Desktop autêntico**; cobrem Data Source, worksheets (bars/lines/scatter), dashboards Overview/Board, e rounds de check — inventário detalhado por ficheiro abaixo.

### Cited Findings

#### Nota prévia de proveniência
- Clean room: não ler o DMG Tableau; construir a partir de descrição pública e imagens de referência do projecto. — [/home/l/Documentos/tableaudesktop-2026-3-0/capture/README.md](/home/l/Documentos/tableaudesktop-2026-3-0/capture/README.md)
- GAP do app: `reference/` vazio; “Não há captura do programa original.” Layout actual = dados à esquerda, prateleiras em cima, Show Me à direita, abas em baixo. — [/home/l/Documentos/tableaudesktop-2026-3-0/app/GAP.md](/home/l/Documentos/tableaudesktop-2026-3-0/app/GAP.md)

---

#### 1. `/home/l/Documentos/tableaudesktop-2026-3-0/app/shots/Overview.png`
**Tipo:** Dashboard “Overview” (grelha 2×2) — livro amostra Shop Orders.

| Região | Conteúdo visível |
|--------|------------------|
| **Menu** | File, Edit, Data, Worksheet, Dashboard, Help |
| **Toolbar** | Logo OpenViz (barras coloridas); Undo, Redo, Save; **+ Data**, **+ Sheet**, **+ Dashboard**; Swap; Sort Asc; Sort Desc; Clear; **Show Me** (direita) |
| **Left pane (Dashboard)** | Título “Dashboard”; **Sheets Shown** checklist: Sales by Region, Monthly Sales, Profit vs Sales, Profit Heat Map (todos checked); **Layout** Columns=`2`, “Show sheet titles” checked; **Order** lista com setas up/down |
| **Canvas** | 4 tiles com título + botão **Open** (canto superior direito de cada tile) |
| **TL chart** | “Sales by Region” — barras horizontais empilhadas; Y=Region (West…South); X=SUM(Sales) ~0–200K; cor=Product Line (Home/Office/Outdoor/Tech) |
| **TR chart** | “Monthly Sales” — multi-line; Y=SUM(Sales) ~0–20K; X=MONTH(Order Date) Jan 2023–Dec 2025; cor=Channel (Online/Partner/Store) |
| **BL chart** | “Profit vs Sales” — scatter; Y=SUM(Profit) −1K–2K; X=SUM(Sales) 0–15K; cor=Product Line |
| **BR chart** | “Profit Heat Map” — crosstab Region × Product Line; células numéricas; legenda divergente SUM(Profit) −5 249…5 249 (vermelho→azul) |
| **Sheet tabs** | Data Source · Sales by Region · Monthly Sales · Profit vs Sales · Profit Heat Map · **Overview** (activo) · + sheet/dashboard |
| **Status** | `Sample – Shop Orders · 2,400 rows · 10 Fields` |

---

#### 2. `/home/l/Documentos/tableaudesktop-2026-3-0/app/shots/Data_Source.png`
**Tipo:** Página Data Source — amostra built-in.

| Região | Conteúdo |
|--------|----------|
| **Menu/Toolbar** | Igual ao Overview (incl. Show Me mesmo nesta página) |
| **Left – Connections** | “Sample – Shop Orders” (Built-in sample); botões “Open a CSV file…”, “Use sample data”, “= Create calculated field…” |
| **Left – Fields** | Dimensions (azul): Order ID, Order Date, Region, Product Line, Channel; Measures (verde): Sales, Profit, Quantity, Discount, Days to Ship; cada um com label Dimension/Measure |
| **Main grid** | Header `Sample – Shop Orders` + `2,400 rows • 10 fields`; botão **Go to worksheet ▶**; grelha ~31 linhas visíveis; headers com ícones Abc / calendário / `#`; dados ORD-10000…, datas 2023, regiões, product lines, sales/profit (lucros negativos com sinal −) |
| **Tabs** | **Data Source** (activo) · Sales by Region · Monthly Sales · Profit vs Sales · Profit Heat Map · Overview |
| **Status** | Mesmo metadata de rows/fields |
| **Ausente vs Tableau doc** | Sem canvas de relationships/noodles no topo; preview ocupa a área principal imediatamente |

---

#### 3. `/home/l/Documentos/tableaudesktop-2026-3-0/app/shots/board.png`
**Tipo:** Dashboard alternativo “Board” — 3 colunas (area / pie / text table).

| Região | Conteúdo |
|--------|----------|
| **Left Dashboard pane** | Sheets Shown: Area chart, Pie chart, Text table (checked); Layout Columns=`3`; Show sheet titles; Order com setas |
| **Canvas “Board”** | 3 colunas verticais, cada uma com **Open** |
| **Col 1** | Area chart empilhada; X=`QUARTER(Order Date)` 2023 Q1–2025 Q4; Y=`SUM(Sales)`; legenda Channel Online/Partner/Store |
| **Col 2** | Pie Product Line: Tech 48%, Home 22%, Outdoor 20%, Office 10% + legenda |
| **Col 3** | Text table: rows Region×Channel; columns Home/Office/Outdoor/Tech |
| **Tabs** | Data Source · Area chart · Pie chart · Text table · **Board** |
| **Status** | `Sample — Shop Orders • 2,400 rows • 11 fields`; direita: `Opened workbook target/test-files/test.ovw` |

---

#### 4. `/home/l/Documentos/tableaudesktop-2026-3-0/app/shots/Monthly_Sales.png`
**Tipo:** Worksheet line chart com Show Me aberto.

| Região | Conteúdo |
|--------|----------|
| **Toolbar extra** | Inclui **T Labels** (além de Swap/Sort/Clear) |
| **Data pane** | Sample - Shop Orders; Search; Dimensions Abc/calendar; Measures `#`; **+ Calculation** |
| **Filters shelf** | Vazio — “Drop fields here to filter” |
| **Marks card** | Dropdown **Automatic**; Color / Size / Label / Detail; pill azul **Channel** em Color; Mark size slider; “Show mark labels” checkbox |
| **Columns** | `MONTH(Order Date)` (azul) |
| **Rows** | `SUM(Sales)` (verde) |
| **Canvas** | Título “Monthly Sales”; multi-line; Y 0–18K; X datas mensais 2023-01…; **Legend Channel** (Online blue / Partner orange / Store green) |
| **Show Me (direita)** | Grelha de ícones de chart (~15+); **line** destacado; texto “In this view: 2 dimension(s), 1 measure(s)” + blurb blue/green |
| **Tabs** | Data Source · Sales by Region · **Monthly Sales** · Profit vs Sales · Profit Heat Map · Overview |

---

#### 5. `/home/l/Documentos/tableaudesktop-2026-3-0/app/shots/Profit_vs_Sales.png`
**Tipo:** Worksheet scatter.

| Região | Conteúdo |
|--------|----------|
| **Columns** | `SUM(Sales)` (verde) |
| **Rows** | `SUM(Profit)` (verde) |
| **Marks** | Dropdown **Circle**; Color=`Product Line`; Detail=`Region` + `QUARTER(Order Date)`; mark size slider |
| **Canvas** | Scatter Sales×Profit; zero-line em Profit; pontos coloridos; **Legend Product Line** (Home/Office/Outdoor/Tech) |
| **Show Me** | Scatter icon destacado; help text “How it works” |
| **Tabs** | … · **Profit vs Sales** (activo) · … |

---

#### 6. `/home/l/Documentos/tableaudesktop-2026-3-0/app/shots/excel.png`
**Tipo:** Data Source após abrir Excel.

| Região | Conteúdo |
|--------|----------|
| **Connections** | `shops – Shops`; path `target/test-files/shops.xlsx > Shops`; dropdown “Sheet in this file” = Shops; Open CSV/Excel; Use sample; Create calculated field |
| **Fields** | Dimensions: Store, Opened; Measures: Revenue, Staff |
| **Grid** | `shops – Shops 3 rows · 4 fields`; 3 linhas (North/South/North; dates 2024-01…03; Revenue 1250.5/980/1410.25; Staff 4/3/5); ícones Abc/calendar/`#` |
| **Tabs** | **Data Source** · Sheet 1 · + sheet/dashboard |
| **Status** | Esquerda: rows/fields; direita azul: `Opened 3 rows from target/test-files/shops.xlsx` |

---

#### 7. `/home/l/Documentos/tableaudesktop-2026-3-0/app/shots/now.png`
**Tipo:** Worksheet “Sales by Region” stacked bar (referência “agora”).

| Região | Conteúdo |
|--------|----------|
| **Columns** | `SUM(Sales)` |
| **Rows** | `Region` |
| **Marks** | Automatic; **Product Line** on Color; size slider; show mark labels |
| **Canvas** | Barras horizontais empilhadas West→South; X 0–160K; legend Product Line |
| **Show Me** | ~24 thumbnails; stacked bar destacado; “2 dimension(s), 1 measure(s)” |
| **Filters** | Vazio |
| **Tabs** | Data Source · **Sales by Region** · Monthly Sales · Profit vs Sales · Profit Heat Map · Overview |

---

#### 8. `/home/l/Documentos/tableaudesktop-2026-3-0/checks/round-2/screenshot.png`
**Tipo:** Check visual — Sales by Region (estado quase idêntico a `now.png`).

| Região | Conteúdo |
|--------|----------|
| Igual a now.png | Mesmo layout worksheet stacked bar; Data pane tip de drag-drop no fundo do painel; Show Me com stacked bar outlined; status `2,400 rows • 10 fields` |

---

#### 9. `/home/l/Documentos/tableaudesktop-2026-3-0/checks/round-1/screenshot.png`
**Tipo:** Check visual — Sales by Region (existe).

| Região | Conteúdo |
|--------|----------|
| Igual família now/round-2 | Sales by Region stacked bar; Show Me ~15 ícones (contagem aparente menor que round-2/now); pills SUM(Sales)/Region; Product Line on Color |

---

#### 10. `/home/l/Documentos/tableaudesktop-2026-3-0/checks/round-0/screenshot.png`
**Tipo:** Check visual round-0 — Sales by Region (existe; único PNG em round-0/).

| Região | Conteúdo |
|--------|----------|
| Igual família | Sales by Region stacked bar completa; Show Me aberto; mesmas shelves/Marks/legend |

---

### Inventário cruzado — o que as capturas cobrem do fluxo canónico

| Fase do fluxo Tableau | Evidência local OpenViz |
|----------------------|-------------------------|
| Connect sample | Data_Source.png |
| Connect Excel | excel.png |
| Relate tables / multi-table model | **Não aparece** |
| Worksheet bar | now.png, round-0/1/2 |
| Worksheet line | Monthly_Sales.png |
| Worksheet scatter | Profit_vs_Sales.png |
| Show Me / Marks | Monthly_Sales, Profit_vs_Sales, now, rounds |
| Filters shelf (com filtro activo) | Shelf vazio em todas as folhas vistas |
| Dual axis | **Não aparece** |
| Dashboard tiled | Overview.png, board.png |
| Dashboard actions (filter/highlight) | **Não aparece** |
| Story | Tab/icon “story” nas abas; sem screenshot de Story workspace |
| Format pane / export PDF | **Não aparece** (GAP: export CSV + PNG janela) |

### Inferences
- O livro amostra OpenViz (`starter_workbook`) é o artefacto visual principal: 4 sheets + Overview espelham o arco pedagógico Superstore (região, tempo, scatter, heatmap → dashboard).
- Rounds 0→2 sugerem regressão visual estável na mesma folha “Sales by Region”, não novos ecrãs.

### Gaps
- Nenhuma captura local mostra: Start page Tableau-like, Analytics pane, Pages shelf, Tooltip shelf completo, Format pane, filter cards interactivos, dual axis, map, story authoring, ou export dialogs.
- Contagem exacta de ícones Show Me varia nas descrições (15 vs 24) — possível diferença de rendering/zoom; não há metadados de resolução no inventário.

---

## How does the current Rust replica (OpenViz) differ from authentic Tableau (gap analysis for UI fidelity)?

### Takeaway
As capturas locais mostram uma **aproximação layout-level** (Data | Marks/Filters | Columns/Rows | canvas | Show Me | tabs) mas diferem do Tableau documentado em branding, toolbar, Data Source (sem noodles), Dashboard (checkboxes vs Objects/Tiled/Floating), Analytics/Pages/Story/Format, e várias ações de toolbar/export/actions.

### Cited Findings
- Workspace Tableau documentado: Side Bar = Data **e** Analytics; Data Source page com model canvas; toolbar com Pause Updates, Duplicate, Highlight, Group, Fix Axes, Fit, Presentation Mode, Share; status bar com mark/row/column counts e SUM agregados. — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm)
- Data Source Tableau: logical layer relationships (“noodles”) + physical joins; multi-table. — [Relate Your Data](https://help.tableau.com/current/pro/desktop/en-us/relate_tables.htm); [Data Model](https://help.tableau.com/current/pro/desktop/en-us/datasource_datamodel.htm)
- OpenViz GAP (código/docs locais): uma tabela; sem join/union/pivot/DB; dashboard = grelha 1–4 colunas com checkboxes (não drag Objects); sem Format pane rico; Color/Size/Label não abrem editores (só slider mark_size); hover tooltip broken por overlay rect; export CSV + PNG janela (não PDF/crosstab Excel menu canónico); `.ovw` JSON não `.twb`. — [/home/l/Documentos/tableaudesktop-2026-3-0/app/GAP.md](/home/l/Documentos/tableaudesktop-2026-3-0/app/GAP.md); [/home/l/Documentos/tableaudesktop-2026-3-0/app/WORKBOOK.md](/home/l/Documentos/tableaudesktop-2026-3-0/app/WORKBOOK.md)
- Evidência visual OpenViz (todas as PNGs em `app/shots/` e `checks/`): branding **OpenViz**; botões texto **+ Data / + Sheet / + Dashboard**; Dashboard pane = Sheets Shown + Columns N + Order arrows; botão **Open** em cada tile (não standard Tableau “go to sheet” hover chrome); Show Me presente; pills blue/green presentes.

### Tabela gap (réplica observada vs Tableau oficial documentado)

| Área | OpenViz (shots locais) | Tableau Desktop (docs oficiais) |
|------|------------------------|----------------------------------|
| Brand / Start | Logo OpenViz; sem Start page Tableau icon tour | Tableau icon → Start page; Connect pane rico |
| Toolbar | Undo/Redo/Save, +Data/+Sheet/+Dashboard, Swap, Sort×2, Clear, T Labels, Show Me | + Pause/Run update, Duplicate, Totals (menu), Highlight, Group, Fix Axes, Fit, Cards, Presentation, Share… |
| Data Source | Preview grid + field list; CSV/Excel/sample | Canvas relationships/joins + preview; multi-table |
| Data pane | Dimensions/Measures + Calculation | + hierarquias, sets, parameters, Measure Names/Values, Analytics pane |
| Shelves | Columns, Rows, Filters, Marks (Color/Size/Label/Detail) | + Pages, Tooltip, Shape; Marks dropdown completo |
| Show Me | Presente, recomenda por contagem dim/measure | Presente; outline laranja no best match |
| Filters UX | Drop zone; sem filter **cards** visíveis nas shots | Show Filter → cards interactivos à direita |
| Dual axis | Não visto | Dual Axis + Synchronize Axis |
| Dashboard | Checkbox + N columns grid; “Open” buttons | Size, Tiled/Floating, Objects (H/V/Text/Image/Blank/Web/Extension/Navigation), Layout tab, Actions |
| Stories | Ícone + nas tabs | Story workspace completo (Get Started step 7) |
| Export | (código) CSV sheet + PNG window | Image/PDF/Crosstab/PowerPoint/Copy |
| Status bar | source · rows · fields | marks · rows · columns · aggregate sum; warnings |
| File format | `.ovw` JSON | `.twb` / `.twbx` (marca — não copiar) |

### Inferences
- Fidelidade actual é **workflow-skeleton** (connect → shelves → Show Me chart types → multi-sheet dashboard grid), não pixel/UI-chrome parity.
- Maiores gaps de “usar Tableau” para a réplica: relationships, filter cards, dual axis, dashboard objects/actions, format, tooltips fiáveis, Analytics pane.
- Comparar PNG OpenViz com PNG Tableau autêntico **não é possível neste repo** — gap analysis assenta em docs públicos + shots da réplica.

### Gaps
- Sem screenshot autêntico Tableau Desktop no workspace para side-by-side pixel. — [app/GAP.md](/home/l/Documentos/tableaudesktop-2026-3-0/app/GAP.md)
- Diferenças tipográficas exactas (Tableau Regular vs sans do egui) não medidas quantitativamente.

---

## What keyboard shortcuts and right-click contexts are most used in tutorials?

### Takeaway
Nos tutoriais oficiais, o padrão dominante é **drag-and-drop + right-click menus** (Show Filter, Dual Axis, Synchronize Axis, Show Me via Ctrl+1); atalhos de shelves (Alt+Shift+R/C/…) e Undo/Swap são os mais relevantes para authoring intermédio.

### Cited Findings

#### Atalhos oficiais mais relevantes ao fluxo beginner→intermediate
Fonte: [Keyboard Shortcuts](https://help.tableau.com/current/pro/desktop/en-us/shortcut.htm)

| Acção | Windows | Mac | Uso em tutoriais |
|-------|---------|-----|------------------|
| Show Me | Ctrl+1 / Ctrl+Shift+1 | ⌘1 | Abrir recomendador de charts |
| Undo / Redo | Ctrl+Z / Ctrl+Y | ⌘Z / ⌘⇧Z | Iteração de vistas |
| Swap rows/columns | Ctrl+W | ⌃⌘W | Barras horiz↔vert |
| Save | Ctrl+S | ⌘S | Persistência workbook |
| New worksheet | Ctrl+M | ⌘T | Multi-sheet → dashboard |
| Connect to data | Ctrl+D | ⌘D | Start of session |
| Find in Data pane | Ctrl+F | ⌘F | Localizar campos Superstore |
| Add field to sheet | Enter / double-click | Return / double-click | Alternativa a drag |
| Field → Columns | Alt+Shift+C (também Y) | ⌥⇧C | Authoring rápido |
| Field → Rows | Alt+Shift+R (também X) | ⌥⇧R | Idem |
| Field → Filters | Alt+Shift+F | ⌥⇧F | Filtros |
| Field → Color | Alt+Shift+O | ⌥⇧O | Encoding |
| Field → Label | Alt+Shift+T | ⌥⇧T | Labels |
| Field → Detail | Alt+Shift+L | ⌥⇧L | Granularidade (scatter) |
| Field → Size | Alt+Shift+I | ⌥⇧I | Size encoding |
| Drop Field menu | Right-click+drag | Option+drag | Escolher aggregation/role ao soltar |
| Copy field to another shelf | Ctrl+drag | ⌘drag | Duplicar pill |
| Clear worksheet | Alt+Shift+Backspace | ⌥⇧Delete | Reset vista |
| Dashboard grid | G | G | Layout dashboard |
| Dashboard/Layout tabs | T | T | Dashboard authoring |
| Presentation mode | F7 / Ctrl+H | ⌥Return | Apresentar findings |
| Select tools | A rect / D lasso / S radial | same | Interacção marks |
| Print | Ctrl+P | ⌘P | Caminho para PDF (Mac Print dialog) |

#### Right-click / context menus citados nos tutoriais e help ligados
- Data pane field → **Show Filter** (Get Started Step 3; Filtering help). — [Focus results](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-focus.htm); [Filtering](https://help.tableau.com/current/pro/desktop/en-us/filtering.htm)
- Pill on shelf → **Remove**; shelf → **Clear Shelf**. — [Dragging fields](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_dragging.htm)
- Measure on Rows/Columns → **Dual Axis**. — [Multiple measures](https://help.tableau.com/current/pro/desktop/en-us/multiple_measures.htm)
- Secondary axis → **Synchronize Axis**; **Move marks to front/back**. — [Multiple measures](https://help.tableau.com/current/pro/desktop/en-us/multiple_measures.htm); [Dragging fields](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_dragging.htm)
- Filter card caret → Edit Filter, Remove Filter, Apply to worksheets, Format Filters, Only relevant values, Include/Exclude, Hide Card. — [Filtering](https://help.tableau.com/current/pro/desktop/en-us/filtering.htm)
- Field → Change Data Type (ex.: para permitir Synchronize Axis). — [Multiple measures](https://help.tableau.com/current/pro/desktop/en-us/multiple_measures.htm)
- Mark / Dashboard → Actions Menu trigger; Dashboard → Actions dialog. — [Actions and Dashboards](https://help.tableau.com/current/pro/desktop/en-us/actions_dashboards.htm)
- Measure Values → Default Properties → Number Format (padrão avançado Measure Names/Values). — documentação community/help associada a Measure Names workflows (uso comum pós-tutorial; ver também help Measure Names)

### Inferences
- Para paridade de “sensação tutorial”, a réplica deveria priorizar: right-click Show Filter + Dual Axis + Clear Shelf, Ctrl+Z, Show Me toggle, double-click field to add, Ctrl+W swap — antes de atalhos raros (Pages F4, cell size Ctrl+B).
- OpenViz shots não mostram menus de contexto; GAP não lista shortcuts — área em aberto para fidelidade de input.

### Gaps
- Frequência exacta de cada shortcut nos vídeos “Learn Tableau Desktop” não foi medida (sem transcript).
- Lista completa de itens do menu de contexto do Data pane (Create Calculated Field, Hierarchy, Aliases, Default Properties, Convert to Dimension/Measure, etc.) não foi fetched página a página nesta ronda; apenas os contextos explicitamente referidos nos tutoriais Get Started / Dual Axis / Filtering.

---

## Apêndice: mapa rápido fonte → artefacto

| Necessidade da equipa | Fonte primária |
|----------------------|----------------|
| Fluxo sessão E2E | [Get Started Tutorial](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-home.htm) |
| Anatomia UI | [Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm) |
| Charts Superstore | [Common Chart Types](https://help.tableau.com/current/pro/desktop/en-us/dataview_examples.htm) |
| Relationships | [Relate Tables](https://help.tableau.com/current/pro/desktop/en-us/relate_tables.htm) |
| Dual axis | [Multiple Measures](https://help.tableau.com/current/pro/desktop/en-us/multiple_measures.htm) |
| Dashboard actions | [Actions and Dashboards](https://help.tableau.com/current/pro/desktop/en-us/actions_dashboards.htm) |
| Vídeos free | [tableau.com/learn/training](https://www.tableau.com/learn/training) |
| Inventário visual local | PNGs sob `/home/l/Documentos/tableaudesktop-2026-3-0/app/shots/` e `checks/round-{0,1,2}/` |
| Gap produto réplica | [/home/l/Documentos/tableaudesktop-2026-3-0/app/GAP.md](/home/l/Documentos/tableaudesktop-2026-3-0/app/GAP.md) |
