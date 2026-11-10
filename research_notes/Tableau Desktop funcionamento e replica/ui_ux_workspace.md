# Tableau Desktop UI, workspace layout, and interaction patterns

## What are the named regions of the Tableau Desktop workspace and what does each do?

### Takeaway
Tableau Desktop’s worksheet workspace is a fixed composition of menus, toolbar, Side Bar (Data / Analytics), shelves and cards (Columns, Rows, Marks, Filters, Pages, legends), the View (canvas), Data Source tab, sheet tabs, and status bar—each with a documented role in building and navigating visualizations.

### Cited Findings
- The Tableau workspace consists of menus, a toolbar, the Data pane, cards and shelves, and one or more sheets; sheets can be worksheets, dashboards, or stories. — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm)
- Official labeled regions (A–I): workbook name; cards and shelves; toolbar; View (canvas where you create a viz); Start page icon; Side Bar (Data pane and Analytics pane on a worksheet); Data Source tab; status bar; sheet tabs. — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm)
- Cards and shelves receive dragged fields to add data to the view; the View is the canvas for the visualization. — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm); [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm)
- Every worksheet contains shelves and cards such as Columns, Rows, Marks, Filters, Pages, Legends, and more; placing fields builds structure, controls detail/marks, and encodes color/size/shape/text/detail. — [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm)
- **Columns / Rows:** Columns shelf creates table columns; Rows creates rows; any number of fields allowed; dimensions create headers; measures create quantitative axes; inner fields on Rows/Columns determine default mark type (e.g., measure + dimension → bar). — [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm)
- **Marks card:** Sets mark type and encodes marks with color, size, shape, text, and detail; click icon next to a field to change which property it uses; Label/Detail/Tooltip/Color can hold multiple fields; Size and Shape only one field at a time; SHIFT-drag to add a second field to Color without replacing. — [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm)
- **Filters shelf:** Specifies which data to include/exclude; can use measures, dimensions, or both; internal filters use fields already in the view; external filters use fields not on Rows/Columns/Marks; all filtered fields appear on Filters. — [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm)
- **Pages shelf:** Breaks a view into a series of pages (one per member); measure on Pages is auto-converted to discrete; adds a page control (jump, manual advance, auto-playback, optional Show History). — [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm)
- **Data pane (Side Bar):** Shows data source connections and fields; dimensions appear above a gray line and measures below for each table/folder; used as the source for drag-and-drop into the view. — [Work with Data Fields in the Data Pane](https://help.tableau.com/current/pro/desktop/en-us/datafields_understanddatawindow.htm); [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm)
- **Analytics pane:** Sibling tab to Data in the left Side Bar; drag reference lines, box plots, trend lines, forecasts, totals, and custom reference items into the view with Table/Pane/Cell scope targets. — [Analytics Pane](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_analytics_pane.htm)
- Side Bar panes change by context: Data, Analytics, Story, Dashboard, Layout, Format. — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm)
- **Status bar:** Bottom of workspace; shows menu-item descriptions and current-view info (e.g., mark count, rows/columns, aggregate totals); can show query-cancel, precision, and special-values warning icons. — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm); [Salesforce Trailhead – Tableau Desktop workspace](https://trailhead.salesforce.com/content/learn/modules/the-tableau-desktop-workspace/get-to-know-tableau-desktop)
- **Sheet tabs:** Bottom tabs for worksheets, dashboards, and stories; Data Source is a separate leftmost tab (not a sheet type). — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm); [Workbooks and Sheets](https://help.tableau.com/current/pro/desktop/en-us/environ_workbooksandsheets.htm)
- Conditional chrome: Color/Shape/Size legends, Measure Values shelf, Parameter controls, Title, Caption, Summary card, and Page control appear as needed based on what is in the view. — [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm)
- Toolbar and status bar can be shown/hidden via Window menu; Side Bar minimize restores from bottom-left; Show/Hide Cards toolbar control toggles Pages, Filters, legends, etc. — [Reorganizing the Workspace](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_reorganize.htm); [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm)

### Inferences
- A faithful replica’s “feel” depends more on the shelf/card geometry (Columns/Rows across the top, Marks + Filters + Pages stacked left of the canvas, Data pane left) than on pixel-perfect chrome.
- Pages and Analytics are first-class named regions in Desktop but are often omitted in early clones; omitting them is a deliberate v1 scope cut, not an accurate Tableau layout.

### Gaps
- Exact pixel layout, default pane widths, and menu bar item lists for Desktop 2024.x–2026.x were not captured from live Tableau Desktop binaries in this pass (docs describe regions functionally, not as a design-system spec).
- Official Format pane layout details (full control tree) were only lightly covered via secondary glossary references, not a dedicated Format Help deep-read.

---

## How do Data Source page, worksheet, dashboard, and story views differ in layout and controls?

### Takeaway
Navigation is tab-based: Data Source is a preparation page with left pane + relationship canvas + data/metadata grids; worksheets expose Data/Analytics + shelves; dashboards swap Side Bar to Dashboard/Layout and drop sheets/objects onto a layout canvas; stories use Story/Layout panes and a story-point navigator.

### Cited Findings
- A workbook contains sheets; a sheet is a worksheet, dashboard, or story (Excel-like model). — [Workbooks and Sheets](https://help.tableau.com/current/pro/desktop/en-us/environ_workbooksandsheets.htm)
- **Worksheet:** single view plus shelves, cards, legends, and Data + Analytics panes in the Side Bar; built by dragging fields onto shelves. — [Workbooks and Sheets](https://help.tableau.com/current/pro/desktop/en-us/environ_workbooksandsheets.htm)
- **Dashboard:** collection of views from multiple worksheets; Side Bar shows Dashboard and Layout panes. — [Workbooks and Sheets](https://help.tableau.com/current/pro/desktop/en-us/environ_workbooksandsheets.htm)
- **Story:** sequence of worksheets or dashboards; Side Bar shows Story and Layout panes. — [Workbooks and Sheets](https://help.tableau.com/current/pro/desktop/en-us/environ_workbooksandsheets.htm)
- Create via bottom icons (New Worksheet / New Dashboard / New Story), menus (Worksheet / Dashboard / Story), toolbar New Worksheet drop-down, or shortcuts (e.g., Ctrl+M / Command-M for new worksheet). — [Workbooks and Sheets](https://help.tableau.com/current/pro/desktop/en-us/environ_workbooksandsheets.htm)
- **Data Source page:** reachable after connect or via Data Source tab from anywhere; four areas—left pane (connections/tables), canvas logical layer (relationships), canvas physical layer (joins/unions via double-click table), data grid (first 1,000 rows), metadata/semantics grid (fields as rows). — [Data Source Page](https://help.tableau.com/current/pro/desktop/en-us/environment_datasource_page.htm)
- Data Source left pane differs by source type (file worksheets vs. server/database/schema/tables; cube sources omit left pane). — [Data Source Page](https://help.tableau.com/current/pro/desktop/en-us/environment_datasource_page.htm)
- **Dashboard authoring:** New Dashboard icon → drag views from Sheets list at left onto dashboard canvas; can Swap Sheets; objects include Horizontal/Vertical containers, Text, Image, Web Page, Blank, Navigation, Download, Extension, Pulse Metric. — [Create a Dashboard](https://help.tableau.com/current/pro/desktop/en-us/dashboards_create.htm)
- Dashboard interactivity: “Use as Filter” on a sheet; Desktop actions for multi-sheet filter/navigate/URL. — [Create a Dashboard](https://help.tableau.com/current/pro/desktop/en-us/dashboards_create.htm)
- **Story authoring:** New Story tab → choose story size → double-click or drag a sheet into a story point → Add a caption → Update after filter/sort changes → Layout tab for navigator style and next/previous arrows. — [Creating a Story](https://help.tableau.com/current/pro/desktop/en-us/story_create.htm)
- Story points remain connected to original sheets; modifying originals updates story points that use them. — [Creating a Story](https://help.tableau.com/current/pro/desktop/en-us/story_create.htm)
- Sheets used in a dashboard or story cannot be deleted (can be hidden); workbook must always keep at least one worksheet. — [Workbooks and Sheets](https://help.tableau.com/current/pro/desktop/en-us/environ_workbooksandsheets.htm)
- Undo/Redo history spans worksheets, dashboards, and stories for the current session; Clear Sheet removes fields/formatting/filters/sorts but not history. — [Workbooks and Sheets](https://help.tableau.com/current/pro/desktop/en-us/environ_workbooksandsheets.htm)

### Inferences
- Switching “mode” is primarily Side Bar + center canvas replacement, not a full app rewrite: global menu/toolbar/status/tabs stay; shelf chrome disappears on Data Source / Dashboard / Story.
- For a replica, the critical navigation contract is: Data Source tab | sheet tabs | +Worksheet / +Dashboard / +Story icons, with Side Bar content keyed to active tab type.

### Gaps
- Full Story Workspace page (“The Story Workspace”) was referenced from the Workspace overview but not fetched as a standalone deep page in this pass.
- Device layouts, tiled vs floating layout mechanics, and dashboard size presets were only partially covered via Create a Dashboard (objects and basic flow).

---

## What are the primary drag-and-drop interaction rules (pills, blue/green, dimension vs measure) and how do they appear visually?

### Takeaway
Tableau’s signature interaction is dragging fields from the Data pane onto shelves/Marks/Filters/Pages (or using double-click / Show Me); pills are blue when discrete (headers) and green when continuous (axes), while dimension vs measure is a separate role—dimensions segment LOD, measures aggregate by default.

### Cited Findings
- Ways to start a view: drag fields to cards/shelves; double-click fields in Data pane; multi-select fields then pick Show Me; drop on “Drop field here” grid for a tabular start. — [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm); [Start Building a Visualization by Dragging Fields](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_dragging.htm)
- Dimensions group/segment (qualitative); measures are aggregated (quantitative); dragging a measure applies an aggregation. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- **Blue = discrete** → headers on Rows/Columns; **green = continuous** → axes on Rows/Columns. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm); [Visual Cues and Icons in Tableau Desktop](https://help.tableau.com/current/pro/desktop/en-us/tips_visualcues.htm)
- Four field type combinations exist: discrete dimensions, continuous dimensions (often dates; string/Boolean cannot be continuous), discrete measures, continuous measures. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- Measure “pill” typically shows aggregation prefix such as `AVG([Age])` or `SUM(Sales)`; dates often show granularity like `WEEK([Order Date])`. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- Data pane icons: blue icons = discrete fields; green icons = continuous; `=` prefix = calculated field or copy. — [Visual Cues and Icons in Tableau Desktop](https://help.tableau.com/current/pro/desktop/en-us/tips_visualcues.htm)
- Adding dimensions to Rows/Columns/Marks (Color/Size/etc.) increases level of detail and often mark count (status bar reflects mark count). — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- Discrete on Color → categorical palette; continuous on Color → quantitative continuous color legend. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- Filter dialogs differ by type: discrete dimension → member checkboxes; continuous measure → choose aggregation first; continuous dimension (non-date) → range filter; dates → discrete or continuous filter path. — [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm); [Filter Data from Your Views](https://help.tableau.com/current/pro/desktop/en-us/filtering.htm)
- Structure-for-analysis summary: fields are dimension|measure and discrete|continuous; default measure aggregation is SUM; discrete → labels/panes; continuous → axes. — [Structure Data for Analysis](https://help.tableau.com/current/pro/desktop/en-us/data_structure_for_analysis.htm)
- Getting-started tutorial names Undo, Data pane, Columns/Rows shelves, Marks card, and Show/Hide Cards as core workspace controls for first drag-drop. — [Get Started – Drag and drop](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-drag.htm)
- Gray field on Filters = context filter; italics = action-controlled filter; delta icon = table calculation; plus/minus = hierarchy drill; cylinder+orange check = secondary (blend) source. — [Visual Cues and Icons in Tableau Desktop](https://help.tableau.com/current/pro/desktop/en-us/tips_visualcues.htm)

### Inferences
- “Feels like Tableau” requires both (1) pill chrome with blue/green and aggregation labels and (2) structural consequences: discrete → headers/panes, continuous → axes, Marks encodings change mark appearance without moving axes.
- Converting discrete↔continuous and dimension↔measure are orthogonal operations and must not be collapsed into one toggle in a replica.

### Gaps
- Complete Show Me matrix (exact minimum field counts per chart icon) is documented via hover text in-product; Blueprint “Visual Best Practices” lists chart intents but not a full numeric requirements table. — [Visual Best Practices](https://help.tableau.com/current/blueprint/en-us/bp_visual_best_practices.htm); [Use Show Me to Start a View](https://help.tableau.com/current/pro/desktop/en-us/buildauto_showme.htm)
- No primary-source screenshot of stock Tableau Desktop 2025/2026 chrome was available in-project (local PNGs are OpenViz).

---

## What menus, toolbar buttons, and side panes are essential for a v1 replica vs advanced features?

### Takeaway
v1 should prioritize Data pane + Columns/Rows + Marks + Filters + sheet/dashboard tabs + Undo/Redo/Save/Clear/Swap/Sort/Show Mark Labels/Show Me; Analytics, Pages, Format, Stories, dashboard objects/actions, and Desktop-only advanced toolbar items are later layers.

### Cited Findings
- Official Desktop toolbar buttons include: Start (Tableau icon), Undo, Redo, Save, New Data Source, Pause Auto Updates, Run Update, New Worksheet (drop-down for dashboard/story), Duplicate, Clear, Swap, Sort Asc/Desc, Highlight, Group Members, Show Mark Labels, Fix Axes, Fit, Show/Hide Cards, Presentation Mode, Share/Publish, Show Me. — [The Tableau Workspace – Toolbar Button Reference](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm)
- Undo is unlimited back to last workbook open, even after save; Redo unlimited; Clear has a drop-down for filters/formatting/sizing/axis ranges. — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm); [Workbooks and Sheets](https://help.tableau.com/current/pro/desktop/en-us/environ_workbooksandsheets.htm)
- Swap moves Rows fields to Columns and vice versa (also swaps Hide Empty Rows/Columns settings). — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm)
- Show Me highlights appropriate viz types; orange outline = recommended best match. — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm); [Use Show Me to Start a View](https://help.tableau.com/current/pro/desktop/en-us/buildauto_showme.htm)
- Presentation Mode hides shelves/toolbar/Data pane leaving the view (Desktop). — [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm); [Salesforce Help – missing workspace areas](https://help.salesforce.com/s/articleView?id=001458026&language=en_US&type=1)
- Analytics pane objects (advanced): Constant/Average line, Median with Quartiles, Box Plot, Totals, Average/Median with 95% CI, Trend Line, Forecast (Desktop-only), Custom Reference Line/Band/Distribution/Box Plot. — [Analytics Pane](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_analytics_pane.htm)
- Dashboard advanced objects: Navigation, Download, Extension, Pulse Metric, Show/Hide buttons, Web Page security settings. — [Create a Dashboard](https://help.tableau.com/current/pro/desktop/en-us/dashboards_create.htm)
- **2025.1 Desktop:** Custom Themes under Format menu (export/apply workbook styling). — [Tableau 2025.1 New Features](https://www.tableau.com/2025-1-features)
- **2025.2:** “Show Me 2.0” — all visualization types available upfront without selecting fields first; “Choose for me” from data source fields (also documented as starting in version 25.2 on Help). — [Tableau 2025.2 New Features](https://www.tableau.com/2025-2-features); [Use Show Me to Start a View](https://help.tableau.com/current/pro/desktop/en-us/buildauto_showme.htm)
- **2025.2 / 2026 Tableau Next “Workspaces”** features (Workspaces hub, Dynamic Grouping by Workspace, Adaptive Dashboards) are framed as Tableau Next experiences, not classic Desktop shelf/workspace redesigns. — [Tableau 2025.2 New Features](https://www.tableau.com/2025-2-features); [Tableau New Features (2026.3 page)](https://www.tableau.com/products/new-features)
- Desktop release train exists through 2026.3 (e.g., 2026.3 released Oct 23, 2026 per support downloads page). — [Tableau Desktop releases](https://www.tableau.com/support/releases)
- Secondary glossary: Format pane appears on left when open and controls worksheet-wide and field formatting; “pills” = fields on shelves. — [Provisio Tableau Glossary](https://provisiosupport.helpdocsite.com/tableau/tableau-glossary) (secondary; prefer Help for behavior)

### Inferences
- **v1 essential (parity for “feels like Tableau”):** Data Source tab + worksheet shelves (Columns, Rows, Marks, Filters) + Data pane Dimensions/Measures + blue/green pills + Show Me + bottom sheet tabs (+ Sheet / + Dashboard) + Undo/Redo/Save/Clear/Swap/Sort/Labels + basic dashboard sheet composition.
- **v1 optional / v2:** Pages shelf + page control; Analytics pane; Format pane / Custom Themes; Stories; Parameter controls; Measure Names/Values; context filters; Presentation Mode; Publish/Share; dashboard actions and advanced objects; Forecast.
- OpenViz’s current chrome (from project shots) already approximates a reduced toolbar (Undo/Redo/Save/+Data/+Sheet/+Dashboard/Swap/Sort/Clear/Show Me) and omits Analytics/Pages/Format/Story authoring chrome—aligned with a sensible v1 cut relative to official Desktop.

### Gaps
- No authoritative Tableau “minimum viable Desktop UI” checklist; v1 vs advanced split above is an inference for replica scoping, not an official product tier.
- Exact Desktop menu trees (File/Edit/Data/Worksheet/Dashboard/Story/Analysis/Map/Format/Server/Window/Help) were not exhaustively inventoried from Help in this pass.
- Distinguishing which 2024–2026 “new features” change classic Desktop chrome vs Cloud/Next requires case-by-case reading; many marketing items are not workspace-layout changes.

---

## What official docs describe the workspace and Show Me recommendations?

### Takeaway
Primary citable docs are Help pages under Desktop “Workspace / Build Views”: Environment Workspace, Shelves and Cards, Data Pane, Dimensions/Measures Blue-Green, Visual Cues, Show Me, Data Source Page, Workbooks and Sheets, Create Dashboard, Create Story, and Analytics Pane; Show Me recommendations are orange-outlined and requirement-driven by selected/in-view fields (with Show Me 2.0 / Choose for me from 25.2).

### Cited Findings
- Workspace overview + toolbar + status bar + Side Bar: [The Tableau Workspace](https://help.tableau.com/current/pro/desktop/en-us/environment_workspace.htm)
- Shelves/cards (Columns, Rows, Marks, Filters, Pages, legends): [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm)
- Drag-to-build: [Start Building a Visualization by Dragging Fields](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_dragging.htm)
- Data pane organization: [Work with Data Fields in the Data Pane](https://help.tableau.com/current/pro/desktop/en-us/datafields_understanddatawindow.htm)
- Blue/green and dimension/measure roles: [Dimensions and Measures, Blue and Green](https://help.tableau.com/current/pro/desktop/en-us/datafields_typesandroles.htm)
- Icon/pill visual cues: [Visual Cues and Icons in Tableau Desktop](https://help.tableau.com/current/pro/desktop/en-us/tips_visualcues.htm)
- Show Me usage and recommendations (orange outline; hover shows minimum requirements; line for date+measure; scatter for multi-dim + two measures): [Use Show Me to Start a View](https://help.tableau.com/current/pro/desktop/en-us/buildauto_showme.htm)
- Chart-type guidance / Show Me panel intents (line, bar, heat map, highlight table, treemap, gantt, bullet, scatter, histogram, maps, box-and-whisker): [Visual Best Practices](https://help.tableau.com/current/blueprint/en-us/bp_visual_best_practices.htm); [Choose the Right Chart Type](https://help.tableau.com/current/pro/desktop/en-us/what_chart_example.htm)
- Data Source page regions: [Data Source Page](https://help.tableau.com/current/pro/desktop/en-us/environment_datasource_page.htm)
- Sheet types and creation: [Workbooks and Sheets](https://help.tableau.com/current/pro/desktop/en-us/environ_workbooksandsheets.htm)
- Dashboard layout/controls: [Create a Dashboard](https://help.tableau.com/current/pro/desktop/en-us/dashboards_create.htm)
- Story layout/controls: [Creating a Story](https://help.tableau.com/current/pro/desktop/en-us/story_create.htm)
- Analytics pane: [Analytics Pane](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_analytics_pane.htm)
- Getting-started labeled workspace tour: [Get Started tutorial – drag and drop](https://help.tableau.com/current/guides/get-started-tutorial/en-us/get-started-tutorial-drag.htm)
- Product getting-started viz-design (dimensions/measures, continuous/discrete): [The Best Visualization for You | Tableau](https://www.tableau.com/learn/get-started/desktop-viz-design)
- Trailhead Desktop interface elements (status bar example with marks/rows/columns/totals): [Understand Tableau Interface Elements](https://trailhead.salesforce.com/content/learn/modules/the-tableau-desktop-workspace/get-to-know-tableau-desktop)
- Show Me conceptual “macro” framing (marketing/education): [Show Me As Macro](https://www.tableau.com/drive/showme-as-a-macro)
- Release notes / feature pages for recent Show Me and Format changes: [2025.1](https://www.tableau.com/2025-1-features), [2025.2](https://www.tableau.com/2025-2-features), [New features hub](https://www.tableau.com/products/new-features), [Desktop downloads/releases](https://www.tableau.com/support/releases)

### Inferences
- For the report writer, the single best “anchor” URL for named regions is `environment_workspace.htm`; the single best interaction-rules URL is `datafields_typesandroles.htm`; the single best shelves URL is `buildmanual_shelves.htm`; the single best Show Me URL is `buildauto_showme.htm`.

### Gaps
- Official in-app training video “Navigating the Workspace in Tableau Desktop” is linked from Help but was not transcribed here.
- Exact Show Me icon grid order and enablement rules per field-count combination were not reverse-engineered from a live Desktop build.

---

## Visual inventory of local project screenshots (OpenViz replica chrome)

### Takeaway
Local PNGs under `app/shots/` and `checks/round-2/` depict the **OpenViz Rust replica**, not stock Tableau Desktop; they already mirror many Tableau region names (Data pane, Columns/Rows, Marks, Filters, Show Me, sheet tabs) while simplifying dashboard Side Bar, menus, and omitting Pages/Analytics/Format/Story authoring panes.

### Cited Findings
- **Note on provenance:** Image paths are local project files; claims below describe what is visible in those PNGs (OpenViz branding). They are not official Tableau screenshots. Paths: `/home/l/Documentos/tableaudesktop-2026-3-0/app/shots/*.png` and `/home/l/Documentos/tableaudesktop-2026-3-0/checks/round-2/screenshot.png`.

#### `app/shots/Data_Source.png`
- Menu: File, Edit, Data, Worksheet, Dashboard, Help (no Story/Analysis/Format/Window/Server menus visible).
- Toolbar: Undo, Redo, Save, + Data, + Sheet, + Dashboard, Swap, Sort asc/desc, Clear, Show Me.
- Left: “Connections” pane with sample connection, Open CSV / Use sample / Create calculated field; fields listed with Dimension (blue-ish) / Measure (green-ish) role labels.
- Center: spreadsheet data preview with type icons (`#`, `Abc`, calendar), row index, Go to worksheet.
- Bottom: Data Source tab active; worksheet + Overview tabs; status “Sample - Shop Orders • 2,400 rows • 10 fields.”

#### `app/shots/Monthly_Sales.png` / `Profit_vs_Sales.png` / `checks/round-2/screenshot.png` (worksheet mode)
- Left Data pane: Dimensions (Order ID, Order Date, Region, Product Line, Channel) with Abc/calendar icons; Measures (Sales, Profit, Quantity, Discount, Days to Ship) with `#` icons; Search fields; + Calculation.
- Shelves: Columns and Rows with blue/green pills (e.g., `MONTH(Order Date)` blue + `SUM(Sales)` green for line chart; `SUM(Sales)`/`SUM(Profit)` green for scatter; `Region` blue + `SUM(Sales)` green for bar).
- Filters card: empty drop target “Drop fields here to filter.”
- Marks card: Automatic or Circle mark type; Color, Size, Label, Detail; pills on Marks (e.g., Channel, Product Line, Region, QUARTER(Order Date)); mark size slider / show mark labels in some shots.
- Right: Show Me grid (~24 icons), some enabled/highlighted; helper text counting dimensions/measures and explaining blue vs green.
- Canvas: titled viz + legend; tip text about dragging fields.
- Bottom tabs: Data Source, worksheets, Overview/Board dashboard; + sheet / + dashboard / + story icons present even when Story UI is otherwise absent.
- Status bar: data source name, row count, field count (replica summary—not Tableau’s marks/rows/columns/SUM readout).

#### `app/shots/Overview.png` / `app/shots/board.png` (dashboard mode)
- Left Side Bar labeled “Dashboard” (not Tableau’s Sheets list + Objects + Layout): checklist of sheets shown, Columns numeric layout control, Show sheet titles, Order with up/down.
- Canvas: multi-viz grid (2×2 or 3-column) with per-tile titles and Open buttons.
- Bottom: dashboard tab active; same global toolbar/menus as worksheet mode.
- `board.png` status also shows opened workbook path `target/test-files/test.ovw`.

### Inferences
- Replica already encodes the core Tableau interaction grammar (Data pane → shelves/Marks → blue/green pills → Show Me → tabs) sufficiently for worksheet authoring demos.
- Largest fidelity gaps vs official Desktop visible in these shots: missing Pages shelf; missing Analytics tab; simplified/nonstandard Dashboard pane (checkbox grid instead of drag-from-Sheets + Objects); abbreviated menus; status bar content differs from Tableau’s marks/rows/columns/aggregates messaging; Data Source page lacks relationship canvas / metadata grid / 1,000-row semantics of official Data Source Page.

### Gaps
- No local screenshot of an official Tableau Desktop install for side-by-side chrome comparison.
- Story view UI is only hinted by a +Story tab icon; no story-point navigator screenshot in the provided set.
- `app/shots/excel.png` and `now.png` were present on disk but not included in the assignment’s required read list and were not inventoried here.
