# Referências visuais oficiais do Tableau Desktop (Help / Learn)

Catálogo curado de páginas oficiais `help.tableau.com` com screenshots/diagramas da UI real do Desktop, sequência Get Started (Sample Superstore), padrões UI documentados que uma réplica (OpenViz) precisa igualar, e PNGs oficiais baixados em `assets/`. Fontes limitadas a documentação pública; sem scrapes de workbooks Tableau Public como binários.

---

## Quais páginas oficiais do Help têm os melhores screenshots/diagramas de Start page, Data Source (relationships/noodles), worksheet (shelves+Marks), Show Me, dashboard e Analytics pane?

### Takeaway
As melhores figuras oficiais da UI Desktop concentram-se em Workspace, Start Page, Data Model (camadas logical/physical com noodles), Shelves/Cards, Show Me, Analytics Pane e Create a Dashboard; cada uma publica PNGs sob `…/Img/` no mesmo host Help.

### Cited Findings
- **Workspace completo (melhor overview A–I):** menus, toolbar, Data pane, cards/shelves, View, ícone Start, Side Bar (Data + Analytics), aba Data Source, status bar, sheet tabs; figura principal `environ_workspace1.png`. — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm); imagem: `https://help.tableau.com/current/pro/desktop/en-us/Img/environ_workspace1.png`
- **Start page (três painéis Connect / Open / Discover):** figuras `environment_startpage.png`, `environment_startpage_connect.png` (+ open/discover). — [Start Page](https://help.tableau.com/current/pro/desktop/en-us/environment_startpage.htm); [Navigate Between Start Page and Workspace](https://help.tableau.com/current/pro/desktop/en-us/startpage_workspace_navigate_between.htm)
- **Data Source / relationships (“noodles”):** documentação chama relationships de “flexible connecting lines” / “noodles”; canvas lógico vs físico (Venn = joins). Diagramas: `data_model_logicallayer_ex1_sm.png`, `data_model_physlayer_ex1_sm.png`, `data_model_layers_sm.png`, schemas `data_model_schema*.png`. — [Relate Your Data](https://help.tableau.com/current/pro/desktop/en-us/relate_tables.htm); [The Tableau Data Model](https://help.tableau.com/current/pro/desktop/en-us/datasource_datamodel.htm); [How Relationships Differ from Joins](https://help.tableau.com/current/pro/desktop/en-us/datasource_relationships_learnmorepage.htm)
- **Worksheet shelves + Marks:** Columns/Rows, Marks card (Color/Size/Shape/Label/Detail/Tooltip), Filters, Pages, legends; figuras `build_manual_shelves_marks1.png`, `wwd_shelf_cr*.png`, `filter_shelf*.png`, `pages_shelf1.png`, `dropFieldHere.png`. — [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm); [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- **Show Me card:** botão toolbar + painel com tipos de viz e outline laranja no recomendado; figuras `environ_showmebutton.png`, `showme1_10.0.png`, `showme2.png`, `showme3.png`, `showme_chooseforme.png`. — [Use Show Me to Start a View](https://help.tableau.com/current/pro/desktop/en-us/buildauto_showme.htm)
- **Dashboard layout/objects:** New Dashboard, Sheets list, Objects (Horizontal/Vertical, Text, Image, Web Page, Blank, Navigation, Download, Extension, Pulse Metric); figura `dashboard_create.png`; tiled vs floating em página separada. — [Create a Dashboard](https://help.tableau.com/current/pro/desktop/en-us/dashboards_create.htm); [Size and Lay Out Your Dashboard](https://help.tableau.com/current/pro/desktop/en-us/dashboards_organize_floatingandtiled.htm); [Format Dashboard Objects](https://help.tableau.com/current/pro/desktop/en-us/Formatting_Dashboard_Objects.htm)
- **Analytics pane:** aba ao lado de Data no Side Bar; drag de reference lines, box plots, trends, forecasts, totals; drop targets Table/Pane/Cell (e eixos dual); figura `environ_analytics_pane.png` + `analytics_drop_*.png`. — [Analytics Pane](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_analytics_pane.htm)
- **Dual Marks cards (eixo dual):** uma Marks card por medida + card All; figuras em `multiple_measures*.png`. — [Add Axes for Multiple Measures](https://help.tableau.com/current/pro/desktop/en-us/multiple_measures.htm)
- **Format pane (worksheet):** Format menu abre painel de controle (Font/Borders/Shading/Lines/Filters etc.); figura `wksht_5.png`. — [Format at the Worksheet Level](https://help.tableau.com/current/pro/desktop/en-us/formatting_worksheet.htm); [Format Your Work](https://help.tableau.com/current/pro/desktop/en-us/formatting.htm)
- **Cards rearranjáveis / Presentation Mode:** cursor de move, barras pretas de drop, Show/Hide Cards, Reset Cards. — [Reorganizing the Workspace](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_reorganize.htm)
- Treinamento oficial (vídeos, não screenshots estáticos): [tableau.com/learn/training](https://www.tableau.com/learn/training); webinar Superstore [Intro to Tableau Desktop Part 1](https://www.tableau.com/learn/webinars/intro-tableau-desktop-part1-superstore).

### Inferences
- Para um apêndice visual de réplica, priorizar: `environ_workspace1.png` (mapa A–I), `environment_startpage.png`, `data_model_layers_sm.png` + logical/physical, `showme1_10.0.png`, `environ_analytics_pane.png`, `dashboard_create.png` / `gs_Build2.png`, `build_manual_shelves_marks1.png`, `pages_shelf1.png`, `multiple_measures7.png`.
- Páginas “Learn more” / FAQ de relationships têm menos screenshots que Data Model; usar Data Model + Relate Your Data juntos.

### Gaps
- `relate_tables.htm` (texto rico sobre noodles) não listou muitos `Img/` no HTML nesta coleta — as figuras de noodles/camadas estão principalmente em `datasource_datamodel.htm`.
- Nomes de ficheiro de imagens mudam entre versões do Help (`showme1_10.0.png`); URLs absolutas atuais devem ser revalidadas se o Help for republicado.
- Training vault/webinars exigem registo; não foram capturados frames de vídeo.

---

## O que a sequência oficial Get Started (Sample Superstore) mostra passo a passo, com nomes de elementos de UI?

### Takeaway
O tutorial oficial de 8 passos (~1–3 h) conecta a **Sample - Superstore** na Start page, constrói views com **Columns/Rows/Marks/Filters**, mapa geográfico, Top N, dashboard interativo e story — sempre com nomes explícitos de shelves, cards, toolbar e abas.

### Cited Findings
- Hub do tutorial e índice dos 8 passos. — [Tutorial: Get Started with Tableau Desktop](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-home.htm)

#### Step 1 — Connect (`get-started-tutorial-connect.htm`)
- Após abrir Desktop: **Start page** com (1) **Tableau icon** (toggle Start ↔ workspace), (2) **Connect pane** (file/server/saved), (3) Quick Start Accelerators, (4) Quick Start Sample Workbooks, (5) **Open** (recent), (6) **Discover**.
- Em Connect → **Saved Data Sources** → clicar **Sample - Superstore**.
- Pós-conexão Tableau: abre **nova worksheet**; mostra data source; popula **Data pane** com fields; atribui data types e roles (dimension/measure).
- Aba **Data Source** (canto inferior esquerdo): primeiras 1 000 linhas; voltar via tab **Sheet 1**.
- Figuras: `gs_Connect1.png`, `gs_Connect_Superstore.png`, `gs_Connect4.png`. — [Step 1: Connect](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-connect.htm)

#### Step 2 — Drag and drop (`get-started-tutorial-drag.htm`)
- Vocabulário UI: **Undo**; **Data pane**; **Cards and Shelves** (Columns/Rows vs Marks); **Show/Hide Cards**; Minimize Data/Analytics.
- Drag **Order Date** → **Columns** shelf → pills `YEAR(Order Date)` + indicador **Abc**.
- Drag **Sales** → **Rows** → agregação SUM; mark type automático **line** para tempo.
- Mudar tipo via dropdown do **Marks card** (ex.: Area); tutorial pede voltar a **Automatic**.
- Drag **Category** à direita de YEAR(Order Date) em Columns → **bar chart**.
- Toolbar **Swap Rows and Columns**; **Show Mark Labels**; hover → **tooltip**.
- Double-click / drag **Sub-Category** → Columns → mais headers/marks (ex.: 68 marks).
- Alternativa: Sub-Category em **Color** no Marks → stacked bars + **legend card**. — [Step 2: Drag and drop](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-drag.htm)

#### Step 3 — Focus (`get-started-tutorial-focus.htm`)
- Right-click **Order Date** / **Sub-Category** → **Show Filter** → **filter cards** à direita (móveis; linha de drop).
- Drag **Profit** → **Color** no Marks → paleta divergente + **color legend**.
- **Edit Colors** via botão Color no Marks.
- Filtrar Sub-Category (Bookcases/Machines/Tables); drag **Region** → **Rows** (esquerda de SUM(Sales)).
- Renomear sheet (double-click tab) → `Sales by Product/Region`; **Duplicate** sheet → `Sales in the South`; drag Region → **Filters shelf**; uncheck **Show Header** no pill Region.
- **File > Save As** → pasta **My Tableau Repository**. — [Step 3: Focus your results](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-focus.htm)

#### Step 4 — Explore geographically (`get-started-tutorial-explore.htm`)
- Campos geográficos com ícone de **globe** no Data pane; roles Country/State/City/Postal Code já atribuídos no Superstore.
- Construir **map view**; drag **Region** → Filters → South; marks por estado. — [Step 4: Explore geographically](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-explore.htm)

#### Step 5 — Drill down / Top N (`get-started-tutorial-drilldown.htm`)
- Keep Only / Exclude em estados → **Inclusions** no Filters (ícone de set).
- Plus icon no pill **State** em Rows → drill para **City**.
- Drag **City** → Filters → tab **Top** → **By field** → **Bottom** 5 → Sum(Profit) (**Top N Filter**).
- Drill City → **Postal Code**; Exclude Jacksonville NC; remover Postal Code de Rows (filtro permanece).
- Quebrar por **Sub-Category** para produtos. — [Step 5: Drill down](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-drilldown.htm)

#### Step 6 — Dashboard (`get-started-tutorial-build.htm`)
- Botão **New dashboard**; **Dashboard pane** com lista de sheets.
- Drag `Sales in the South` e `Profit Map` (acima); clear **Show header** / **Hide Field Labels for Columns**; **Hide Title**.
- Remover filter cards duplicados; mover **Profit color legend**; filter **Year of Order Date** → **Floating** + **Single Value (Slider)**.
- Ícone **Use as Filter** no mapa; filter → **Apply to Worksheets > All Using This Data Source**.
- Renomear tab → `Regional Sales and Profit`.
- Conceitos **Tiled vs Floating**. — [Step 6: Build a dashboard](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-build.htm)

#### Step 7 — Story (`get-started-tutorial-story.htm`)
- **Story pane** → New story point / **Duplicate**; alterar filtros (ex. Machines) sem alterar ponto anterior; **captions**.
- Story points por estado/ano (NC 2022–2025) com Duplicate. — [Step 7: Build a story](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-story.htm)

#### Step 8 — Share (`get-started-tutorial-share.htm`)
- Publicar em Tableau Server / Cloud / Public (fora do escopo de UI de authoring local). — [Step 8 via home index](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-home.htm)

### Inferences
- Fidelidade comportamental mínima para a réplica: Start → Saved Superstore → Sheet1 Data pane → Order Date/Sales shelves → Category/Sub-Category → Show Filter cards → Profit on Color → Duplicate + Region filter → map + Top N → New Dashboard + Use as Filter + floating slider → Story Duplicate/captions.
- Nomes de UI a preservar literalmente: Columns, Rows, Marks, Filters shelf, Show Filter (card), Data pane, Analytics (não usado no GS mas no workspace), Show Me, Dashboard pane, Objects, Use as Filter, Floating/Tiled, Story pane.

### Gaps
- GIFs animados do tutorial (“Check your work”) não foram descarregados (só PNGs estáticos).
- Step 4/5/7/8: conteúdo sintetizado via search + fetch parcial; páginas explore/drilldown/story não foram re-fetched linha a linha nesta passada (síntese baseada em snippets oficiais do Help).
- Dataset Superstore em Tableau Public Sample Data é Excel externo; não foi baixado (só docs/imagens).

---

## Quais diferenças visuais / de comportamento vs OpenViz os docs oficiais do Tableau enfatizam (filter cards, Pages, dual Marks, relationship canvas, Format pane)?

### Takeaway
Os docs oficiais não mencionam OpenViz; enfatizam padrões de UI Desktop que uma réplica tipicamente omite ou simplifica: Filters como **cards móveis** (além do Filters shelf), **Pages shelf** + page control/history, **múltiplas Marks cards** em dual axis, canvas de **relationships/noodles** (lógico ≠ joins Venn), e **Format pane** dockado via menu Format (Side Bar contextual).

### Cited Findings
- **Filter shelf vs filter cards:** campos vão ao **Filters shelf**; **Show Filter** cria **filter cards** à direita, rearranjáveis com barra de drop; cards são “card types”. — [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm); [Step 3 Get Started](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-focus.htm); [Reorganizing the Workspace](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_reorganize.htm)
- **Pages shelf:** dimensão/medida em Pages cria sequência + **page control** (jump, slider, playback F4, Show History/trails); Sync em dashboards (Desktop only). — [Shelves and Cards Reference – Pages](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm)
- **Dual Marks cards:** dual axis → Marks card por medida + **All**; customizar mark type/color só na card ativa. — [Multiple Measures / Dual Axes](https://help.tableau.com/current/pro/desktop/en-us/multiple_measures.htm)
- **Relationship canvas:** logical layer = noodles/relationships (sem join type/Venn); physical layer = joins/unions (double-click logical table); root table = primeira arrastada. — [The Tableau Data Model](https://help.tableau.com/current/pro/desktop/en-us/datasource_datamodel.htm); [Relate Your Data](https://help.tableau.com/current/pro/desktop/en-us/relate_tables.htm)
- **Format pane docking / Side Bar:** Side Bar troca entre Data, Analytics, Story, Dashboard, Layout, **Format**; Format > Font/Borders/… abre control pane; copy/paste formatting entre worksheets (Desktop). — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm); [Format at the Worksheet Level](https://help.tableau.com/current/pro/desktop/en-us/formatting_worksheet.htm)
- **Show/Hide Cards + Presentation Mode:** Pages/Filters/Legends toggles; Presentation Mode esconde shelves/toolbar/Data pane, deixa view + legends/filters/parameters. — [Reorganizing the Workspace](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_reorganize.htm)
- **Legends / Measure Values / Parameter controls / Page control** aparecem condicionalmente. — [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm)
- **Analytics pane** como sibling tab do Data pane (não um menu único). — [Analytics Pane](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_analytics_pane.htm)
- Dashboard: **Layout pane** (padding, border, background, corner radius 2026.1+), Objects, Floating vs Tiled. — [Format Dashboard Objects](https://help.tableau.com/current/pro/desktop/en-us/Formatting_Dashboard_Objects.htm); [Size and Lay Out](https://help.tableau.com/current/pro/desktop/en-us/dashboards_organize_floatingandtiled.htm)

### Inferences
- Checklist de paridade visual “Desktop-like” derivado só dos docs: (1) pills blue/green em Columns/Rows, (2) Marks card com property buttons, (3) Filters shelf + filter cards dockáveis, (4) Pages + page control, (5) dual Marks + All, (6) Data Source logical noodles vs physical Venn, (7) Format pane no Side Bar, (8) Analytics tab, (9) Dashboard Objects + Layout pane, (10) Start page Connect/Open/Discover.
- Screenshots locais do projeto OpenViz **não** devem ser citados como “Tableau”; usar apenas Help PNGs em `assets/` ou URLs Help.

### Gaps
- Não há documento oficial Tableau vs OpenViz; diferenças são inferidas dos padrões UI que o Help documenta, não de um gap analysis pixel-a-pixel OpenViz.
- Layout pixel-exact (larguras default do Side Bar, altura das shelves) não está especificado no Help.
- Comparação lado a lado com builds OpenViz deste repositório não foi feita nesta tarefa.

---

## É possível guardar PNGs/JPGs ilustrativos oficiais localmente para o apêndice do relatório?

### Takeaway
Sim: 27 PNGs válidos foram descarregados com `curl` apenas de `help.tableau.com` para `research_notes/Tableau Desktop funcionamento e replica/assets/`; alguns nomes de URL adivinhados devolveram HTML 404 e foram removidos.

### Cited Findings
- Host permitido usado: `https://help.tableau.com/current/pro/desktop/en-us/Img/…` e `https://help.tableau.com/current/guides/get-started-tutorial/en-us/Img/…`.
- Imagens verificadas como `PNG image data` (não HTML de erro).

#### Inventário local (`assets/`)

| Ficheiro local | Origem (URL) | Motivo no apêndice |
| --- | --- | --- |
| `environ_workspace1.png` | [Workspace Img](https://help.tableau.com/current/pro/desktop/en-us/Img/environ_workspace1.png) | Workspace A–I completo |
| `environment_startpage.png` | [Start page](https://help.tableau.com/current/pro/desktop/en-us/Img/environment_startpage.png) | Start page overview |
| `environment_startpage_connect.png` | [Start Connect](https://help.tableau.com/current/pro/desktop/en-us/Img/environment_startpage_connect.png) | Painel Connect detalhado |
| `data_model_layers_sm.png` | [Data model layers](https://help.tableau.com/current/pro/desktop/en-us/Img/data_model_layers_sm.png) | Logical vs physical |
| `data_model_logicallayer_ex1_sm.png` | [Logical layer](https://help.tableau.com/current/pro/desktop/en-us/Img/data_model_logicallayer_ex1_sm.png) | Noodles / relationships canvas |
| `data_model_physlayer_ex1_sm.png` | [Physical layer](https://help.tableau.com/current/pro/desktop/en-us/Img/data_model_physlayer_ex1_sm.png) | Joins Venn canvas |
| `build_manual_shelves_marks1.png` | [Shelves Marks](https://help.tableau.com/current/pro/desktop/en-us/Img/build_manual_shelves_marks1.png) | Marks card encodings |
| `dropFieldHere.png` | [Drop field](https://help.tableau.com/current/pro/desktop/en-us/Img/dropFieldHere.png) | Drop field here grid |
| `filter_shelf2.png` | [Filters](https://help.tableau.com/current/pro/desktop/en-us/Img/filter_shelf2.png) | Filters shelf |
| `pages_shelf1.png` | [Pages](https://help.tableau.com/current/pro/desktop/en-us/Img/pages_shelf1.png) | Pages shelf / paging |
| `environ_showmebutton.png` | [Show Me button](https://help.tableau.com/current/pro/desktop/en-us/Img/environ_showmebutton.png) | Toolbar Show Me |
| `showme1_10.0.png` | [Show Me card](https://help.tableau.com/current/pro/desktop/en-us/Img/showme1_10.0.png) | Show Me pane / tipos |
| `environ_analytics_pane.png` | [Analytics](https://help.tableau.com/current/pro/desktop/en-us/Img/environ_analytics_pane.png) | Analytics pane |
| `multiple_measures5.png` | [Dual Marks](https://help.tableau.com/current/pro/desktop/en-us/Img/multiple_measures5.png) | Marks cards por medida |
| `multiple_measures7.png` | [Dual axis view](https://help.tableau.com/current/pro/desktop/en-us/Img/multiple_measures7.png) | Dual axis + Marks |
| `dashboard_create.png` | [Create Dashboard](https://help.tableau.com/current/pro/desktop/en-us/Img/dashboard_create.png) | New Dashboard UI |
| `rearrange3.png` | [Rearrange cards](https://help.tableau.com/current/pro/desktop/en-us/Img/rearrange3.png) | Cards dockáveis |
| `wksht_5.png` | [Format worksheet](https://help.tableau.com/current/pro/desktop/en-us/Img/wksht_5.png) | Format pane / fonts |
| `gs_Connect1.png` | [GS Step 1](https://help.tableau.com/current/guides/get-started-tutorial/en-us/Img/gs_Connect1.png) | Start page no tutorial |
| `gs_Connect_Superstore.png` | [Superstore saved](https://help.tableau.com/current/guides/get-started-tutorial/en-us/Img/gs_Connect_Superstore.png) | Sample - Superstore |
| `gs_Drag1.png` / `gs_Drag28.png` / `gs_Drag10.png` | [GS drag/focus](https://help.tableau.com/current/guides/get-started-tutorial/en-us/Img/gs_Drag10.png) | Worksheet shelves em ação |
| `gs_FocusFilter1.png` | [GS filters](https://help.tableau.com/current/guides/get-started-tutorial/en-us/Img/gs_FocusFilter1.png) | Filter cards |
| `gs_Build1.png` / `gs_Build2.png` | [GS dashboard](https://help.tableau.com/current/guides/get-started-tutorial/en-us/Img/gs_Build2.png) | Dashboard layout tutorial |
| `gs_Story1.png` | [GS story](https://help.tableau.com/current/guides/get-started-tutorial/en-us/Img/gs_Story1.png) | Story chrome |

### Inferences
- O apêndice do relatório pode citar caminhos relativos `assets/<nome>.png` + URL canónica Help para atribuição.
- Preferir imagens grandes do Get Started (`gs_Build2`, `gs_Drag10`, `gs_Connect1`, `environ_workspace1`) para “look & feel”; diagramas `data_model_*` para noodles.

### Gaps
- Tentativas falhadas (404/HTML stubs removidos): `environ_startpage.png` (nome errado; correto é `environment_startpage.png`), `relationships_canvas.png`, `showme.png`, `Analytics_pane.png` (correto: `environ_analytics_pane.png`).
- Não descarregados nesta passada (existem no Help): mais `showme2/3`, `analytics_drop_*`, `data_model_schema*`, GIFs do Get Started, `pages7.png`, `gs_Story*` restantes.
- Licença: imagens são documentação pública Tableau Help; uso interno de pesquisa/réplica — não redistribuir como assets de produto sem rever termos Tableau.
- Nenhum workbook `.twbx` / scrape Tableau Public foi descarregado (conforme restrição).
