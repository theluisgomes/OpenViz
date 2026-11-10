# Visualizações, encoding Marks e superfície de charting (replica Rust)

## What does Show Me recommend for given dimension/measure combinations, and what is the full-ish list of viz types?

### Takeaway
Show Me enables chart icons whose minimum field requirements match the current Data-pane selection (or fields already in the view); hovering an icon shows those minimums, and Tableau outlines its preferred recommendation (e.g. line for date + measure). Official Help documents the interaction model but does not publish a single canonical table of all ~24 Show Me icons—requirements must be reconstructed from hover text, chart-specific “Build a …” pages, and secondary exam guides.

### Cited Findings
- Show Me creates a view from fields already in the view plus fields selected in the Data pane; open it via the toolbar Show Me control. — [Use Show Me to Start a View](https://help.tableau.com/current/pro/desktop/en-us/buildauto_showme.htm)
- Multi-select fields with Ctrl (Command on Mac), then pick a viz type; hovering each viz type shows a description of its minimum field requirements. — [Use Show Me to Start a View](https://help.tableau.com/current/pro/desktop/en-us/buildauto_showme.htm)
- With a date dimension and a measure selected, Tableau suggests a line view (orange outline); a line is described as often optimal for measure change over time. — [Use Show Me to Start a View](https://help.tableau.com/current/pro/desktop/en-us/buildauto_showme.htm)
- Starting in version 25.2, with a data source selected and little/no worksheet data, choosing a viz type then **Choose for me** can auto-build a viz from data-source fields. — [Use Show Me to Start a View](https://help.tableau.com/current/pro/desktop/en-us/buildauto_showme.htm)
- Secondary study material states Show Me shows **24 core visualization thumbnail icons**; icons are colored when selected fields meet minimums and grayed-out otherwise; clicking assigns fields to shelves/Marks automatically. — [OpenExamPrep: Building Visualization Types & Show Me](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
- Community/certification cheat sheets also state Show Me includes **24 charts** and list hover-style minimums (e.g. Gantt: 1 Date, 1+ Dimensions, 0–2 Measures; Packed bubbles: 1+ Dimensions, 1 or 2 Measures). — [Towards AI Tableau charts cheat sheet](https://pub.towardsai.net/a-comprehensive-cheat-sheet-on-tableau-charts-a-road-to-tableau-desktop-specialist-certification-d2d433872cef)
- Approximate Show Me catalog reconstructed from official Build pages + study guides (not an official enumerated list): text table, heat map, highlight table, symbol map, filled map, pie, horizontal bars, stacked bars, side-by-side bars, treemap, circle views / side-by-side circles, continuous lines, discrete lines, dual lines, continuous/discrete area, dual combination, scatter, histogram, box-and-whisker, Gantt, bullet graph, packed bubbles. — [OpenExamPrep Show Me matrix](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me); [Build a Pie Chart](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_pie.htm); [Build a Histogram](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_histogram.htm); [Build a Packed Bubble Chart](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_bubbles.htm)
- Example minimums cited by study guide / Build docs:
  - Text table: 1+ Dimensions, 1+ Measures — [OpenExamPrep](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
  - Heat map: 1+ Dimensions, 1–2 Measures — [OpenExamPrep](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
  - Highlight table: 1+ Dimensions, 1 Measure — [OpenExamPrep](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
  - Horizontal bars: 0+ Dimensions, 1+ Measures — [OpenExamPrep](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
  - Lines / areas: 1 Date, 0+ Dimensions, 1+ Measures — [OpenExamPrep](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
  - Symbol map: 1 geographic dimension, 0–2 measures; filled map: 1 geographic dimension, 0–1 measure — [OpenExamPrep](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
  - Scatter: 0+ Dimensions, 2 continuous measures — [OpenExamPrep](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
  - Histogram: 1 continuous measure (and no dimensions for Show Me availability) — [Build a Histogram](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_histogram.htm)
  - Pie: at least one dimension and one or two measures; aggregate fields such as Profit Ratio do not contribute to those requirements — [Build a Pie Chart](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_pie.htm)
  - Treemap (Show Me hover phrasing via secondary source): try 1 or more Dimensions, 1 or 2 Measures — [Pluralsight Tableau Playbook – Treemap](https://www.pluralsight.com/resources/blog/guides/tableau-playbook-treemap); building blocks also in [Build a Treemap](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_treemap.htm)
  - Box plot (Show Me): 0+ Dimensions, 1+ Measures — [OpenExamPrep](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
  - Bullet graph: 1+ Dimensions, 2 Measures — [OpenExamPrep](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
- Marks card **Automatic** mark selection (distinct from Show Me icons, but drives defaults): Text when dimensions are innermost on both Rows and Columns; Shape when measures are innermost on both; Bar when dimension + measure (unless date → Line); Line when date + measure. — [Change the Type of Mark in the View](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- Full Marks **type** dropdown (not the same as Show Me thumbnails): Automatic, Bar, Line, Area, Square, Circle, Shape, Text, Map, Pie, Gantt Bar, Polygon, Density. — [Change the Type of Mark in the View](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)

### Inferences
- A credible replica should implement Show Me as a **constraint solver** over selected field roles (dimension/measure/date/geographic/continuous), not a fixed chart-type picker: enable/disable icons, show hover requirements, and auto-place pills on Columns/Rows/Marks.
- Local OpenViz screenshots (`app/shots/Profit_vs_Sales.png`, `Monthly_Sales.png`) show a Show Me flyout with many thumbnail icons and status text like “3 dimension(s), 2 measure(s)”—matching Tableau’s selection-count UX even if the replica’s icon set is incomplete.
- Prefer official “Build a …” pages for pill placement when they conflict with exam blogs; use blogs only for the Show Me hover minimum matrix Tableau Help does not centralize.

### Gaps
- Tableau Help’s Show Me article does **not** list all 24 icons or a complete requirements matrix in one place; exact hover wording for every icon was not scraped from a live Desktop instance.
- Unconfirmed exact Show Me labels for “circle views,” “side-by-side circles,” “dual lines,” and “dual combination” beyond secondary guides.
- Version-to-version drift (e.g. 25.2 “Choose for me”) means a 2026.3 Desktop build may differ slightly from Help `current/` text.


## How do bar, line, area, scatter, text/crosstab, heatmap, treemap, pie, packed bubble, Gantt, box plot, histogram, maps, dual axis, and combined axis work in terms of required pills?

### Takeaway
Tableau charts are compositions of **shelf pills** (Columns/Rows) plus **Marks encodings** (Color, Size, Label/Text, Detail, Tooltip, Shape, Path, Angle)—not standalone chart widgets. Dual axis layers two independent axes (up to four total) with per-measure Marks cards; combined/blended axis merges measures onto one scale via Measure Names/Values.

### Cited Findings

#### Encoding channels (Marks card)
- Drag fields to Marks card properties to encode marks; click property buttons to edit; dimension order on Marks is hierarchical top→bottom and affects sort. — [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- Shelves reference: encode with color, size, shape, text, and detail; many properties accept **multiple fields** (Label, Detail, Tooltip, Color); **Size and Shape only one field at a time**; holding Shift while dragging Color adds without replacing. — [Shelves and Cards Reference](https://help.tableau.com/current/pro/desktop/en-us/buildmanual_shelves.htm)
- **Color**: discrete field → categorical colors / breakout; continuous field → value-based coloring; Edit Colors for palettes. Default mark color is blue (black for text). — [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- **Size**: slider behavior differs by mark type (circles grow; bars widen; lines thicken; polygons cannot resize; pies scale overall); discrete Size separates marks like Detail then sizes; continuous Size maps value range to mark size; legends + Edit Sizes. — [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- **Label / Text**: dimension → member names as labels and mark separation; measure → measure values as labels; Text is the crosstab target. — [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- **Detail**: dimension on Detail separates marks / adds granularity **without** changing table structure (unlike Rows/Columns). — [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- **Tooltip**: drag fields; Edit Tooltip for static/dynamic text, Show Tooltips, Responsive vs On Hover, command buttons, selection by category; ATTR asterisk when mark aggregates multiple dimension members. — [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- **Shape**: dimension → unique shapes (+ shape legend); measure → binned discrete shapes; default ~10 shapes then repeat; custom shapes from Tableau Repository. — [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- **Path**: available for Line or Polygon; line types Linear / Step / Jump; line patterns Solid / Dashed / Dotted (from 23.2; dashed/dotted incompatible with continuous size encoding); Path can encode drawing order via dimension or measure. — [Control the Appearance of Marks](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_markproperties.htm)
- **Angle**: appears when mark type is Pie; measure on Angle sets wedge angular measure. — [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm); [Build a Pie Chart](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_pie.htm)

#### Chart-type pill recipes
- **Bar**: Automatic when dimension + measure are innermost on Rows/Columns (date → Line instead); or Marks = Bar; marks stack by default; Color dimension → stacked bars. — [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm); [Build a Bar Chart](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_bar.htm)
- **Line**: Automatic for date/continuous dimension vs measure(s); or Marks = Line; Path for line type/pattern. — [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- **Area**: Marks = Area (or Automatic with Stack Marks on); Columns: dimension (often date); Rows: measure; Color: dimension for stacked series. — [Quick Start: Area Charts](https://help.tableau.com/current/pro/desktop/en-us/qs_area_charts.htm); [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- **Scatter**: measures as innermost fields on both Rows and Columns → Automatic Shape (open circle); Circle mark for filled circles; Detail dimensions set mark grain (e.g. product/customer). — [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm); [Use Show Me – scatter example](https://help.tableau.com/current/pro/desktop/en-us/buildauto_showme.htm)
- **Text / crosstab**: Automatic Text when dimensions innermost on Rows and Columns; place measure on Text; Worksheet > Duplicate as Crosstab. — [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- **Heat map / highlight table**: Square marks with measure on Color (and often Size for “heat map”); highlight table = text table with color fill. Official highlight/heat build guidance referenced from Square mark docs. — [Change the Type of Mark – Square](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm); requirements in [OpenExamPrep](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
- **Treemap**: Mark Automatic or Square; Color: dimension or measure; Size: measure; Label/Detail: dimension(s); Size and Color are crucial; measure elsewhere has no effect. — [Build a Treemap](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_treemap.htm)
- **Pie**: Marks = Pie (never Automatic); Color = dimension (slices); Angle = measure; Show Me also moves measure to Size. — [Build a Pie Chart](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_pie.htm); [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- **Packed bubble**: Marks = Circle; Detail = dimension; Size = measure; Color = dimension or measure; Label optional. — [Build a Packed Bubble Chart](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_bubbles.htm)
- **Gantt**: Marks Automatic or Gantt Bar; Columns = continuous date/time; Rows = dimension(s); Size = continuous duration measure (length of bar). — [Build a Gantt Chart](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_gantt.htm); [Change the Type of Mark – Gantt](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- **Box plot**: Can be added from Analytics pane (scope always Cell); Show Me path uses 0+ dims / 1+ measures per study guide. — [Analytics Pane](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_analytics_pane.htm); [OpenExamPrep](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
- **Histogram**: Show Me when view has a **single measure and no dimensions**; replaces measure with continuous bin dimension and puts Count on Rows; building blocks: Automatic mark, Columns = bin, Rows = Count/CountD. — [Build a Histogram](https://help.tableau.com/current/pro/desktop/en-us/buildexamples_histogram.htm)
- **Maps**: Geographic role fields generate Latitude/Longitude; Symbol/point: locations on Detail, Automatic mark; filled: Map mark + measure/continuous dimension on Color; mapping workspace documents Detail/Color/Size/Label. — [Change the Type of Mark – Map](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm); [The Mapping Workspace](https://help.tableau.com/current/pro/desktop/en-us/maps_workspace.htm); [OpenExamPrep maps](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)
- **Density mark**: groups overlapping marks and color-codes intensity; Intensity slider; usable on maps/scatters. — [Change the Type of Mark – Density](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- **Dual axis**: two independent axes layered; drag measure to opposite side until black dashed line, or Dual Axis on pill; up to **four** layered axes (2 Columns + 2 Rows); Synchronize Axis for shared scale (same/compatible numeric types); Marks card splits per measure + All for combo charts. — [Add Axes for Multiple Measures](https://help.tableau.com/current/pro/desktop/en-us/multiple_measures.htm)
- **Combined / blended axis**: drop second measure **onto existing axis**; uses Measure Values on shared axis and Measure Names (often on Color); one Marks card; best when scales/units similar. — [Add Axes for Multiple Measures](https://help.tableau.com/current/pro/desktop/en-us/multiple_measures.htm)
- **Small multiples / panes**: multiple discrete pills on Rows/Columns create headers/panes (table structure); Analytics objects scope to Table / Pane / Cell. — [Analytics Pane](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_analytics_pane.htm); discrete date lines create headers/panes per [OpenExamPrep](https://open-exam-prep.com/study-guides/tableau-desktop/exploring-analyzing-data/visualizations-show-me)

### Inferences
- Replica core model: `ViewSpec { columns, rows, filters, marks: { type, encodings[] } }` with aggregation, discrete/continuous, and date-part/truncation on pills—then a renderer that maps mark type + encodings to geometry.
- Side-by-side bars vs stacked = same Bar mark + second dimension on Columns (headers) vs Color (stack)—layout logic, not a separate mark type.
- Local shots confirm recipes: scatter with Color=Product Line + Detail=Region,QUARTER(Order Date); line with MONTH(Order Date)×SUM(Sales)+Color=Channel; highlight table Region×Product Line with Text+Color=SUM(Profit) (`app/shots/*.png`).

### Gaps
- Official exhaustive Show Me field-placement rules for bullet graphs and dual combination were not fetched from a dedicated Help page.
- Exact Polygon mark data-shape requirements beyond “specially constructed data source” remain thin. — [Change the Type of Mark](https://help.tableau.com/current/pro/desktop/en-us/viewparts_marks_marktypes.htm)
- Continuous axis fixed bar width / histogram continuous sizing edge cases need implementation tests against Desktop.


## How do dashboards and stories compose sheets (layout containers, floating vs tiled, filter/highlight/URL actions)?

### Takeaway
Dashboards compose sheets and objects in a tiled grid and/or floating layers, optionally nested in horizontal/vertical layout containers, with Fixed/Range/Automatic sizing and optional Desktop/Tablet/Phone device layouts. Interactivity is action-driven (Filter, Highlight, URL, Go to Sheet, Parameter, Set) with a documented execution order. Stories are sequenced story points (sheets/dashboards with captions/filters), not freeform layouts.

### Cited Findings
- Dashboard size modes: **Fixed** (default; scroll if window smaller; better cache), **Range** (min–max; good for mobile vertical), **Automatic** (fill window; prefer tiled; unpredictable). — [Size and Lay Out Your Dashboard](https://help.tableau.com/current/pro/desktop/en-us/dashboards_organize_floatingandtiled.htm)
- **Layout containers**: Horizontal resizes widths; Vertical adjusts heights; Distribute Evenly; can nest views/objects; Item hierarchy on Layout pane. — [Size and Lay Out Your Dashboard](https://help.tableau.com/current/pro/desktop/en-us/dashboards_organize_floatingandtiled.htm)
- **Tiled vs floating**: Tiled = non-overlapping single-layer grid; Floating = can layer/overlap; Shift-drag to float; floating best with fixed dashboard size/position. — [Size and Lay Out Your Dashboard](https://help.tableau.com/current/pro/desktop/en-us/dashboards_organize_floatingandtiled.htm)
- Layout pane: x/y position, size, Show title, grid (Dashboard > Show Grid), reorder layering (front = top of hierarchy for floating in Desktop). — [Size and Lay Out Your Dashboard](https://help.tableau.com/current/pro/desktop/en-us/dashboards_organize_floatingandtiled.htm)
- Device layouts (Desktop authoring): Default is parent; device layouts (desktop/tablet/phone) are children—items must exist on Default first; Phone can Auto-Generate; Desktop/Tablet layouts manual/independent; Device Preview; published layouts chosen by smallest iframe dimension (≤500 phone, 501–800 tablet, >800 desktop). — [Create Dashboard Layouts for Different Device Types](https://help.tableau.com/current/pro/desktop/en-us/dashboards_dsd_create.htm); [Embed Dashboards](https://help.tableau.com/current/pro/desktop/en-us/embed_dashboard.htm)
- Container + filter resize: Use as Filter + Exclude all values can collapse target sheets under titles when nothing selected. — [Size and Lay Out Your Dashboard](https://help.tableau.com/current/pro/desktop/en-us/dashboards_organize_floatingandtiled.htm)
- **Action types**: Filter, Highlight, Go to URL, Go to Sheet, Change Parameter, Change Set Values. — [Actions](https://help.tableau.com/current/pro/desktop/en-us/actions.htm)
- **Action order**: Parameter → Set → Filter → Go to sheet → Highlight → Go to URL; within type, alphabetical by action name. — [Actions](https://help.tableau.com/current/pro/desktop/en-us/actions.htm)
- Dashboard-specific: filter/highlight can impact multiple views; URL actions target Web Page objects; Go to Sheet for navigation; triggers include hover/select/menu. — [Actions and Dashboards](https://help.tableau.com/current/pro/desktop/en-us/actions_dashboards.htm)
- **Stories**: a story is a sheet that is also a sequence of story points (each a sheet/dashboard view with optional filters/annotations/captions); Layout tab for navigator style; stories do **not** support dynamic features such as zone visibility, axis titles, or axis ranges. — [Stories](https://help.tableau.com/current/pro/desktop/en-us/stories.htm); [Creating a Story](https://help.tableau.com/current/pro/desktop/en-us/story_create.htm)

### Inferences
- Table-stakes dashboard engine for a replica: layout tree (tiled containers + floating absolute boxes), sheet viewports, legends/filters as objects, and an action runtime with the Tableau order of operations.
- Local OpenViz dashboards (`Overview.png`, `board.png`) use simplified tiled grids (columns = 2 or 3) rather than full Tableau container nesting—good MVP, not feature-parity.
- Device layouts are high effort; Fixed + tiled grid covers most Desktop authoring credibility first.

### Gaps
- Full list of dashboard Objects (Image, Text, Blank, Navigation, Download, Extension, Web Page, etc.) and Format Dashboard Objects details were not exhaustively fetched.
- Zone visibility / dynamic zone show-hide behavior outside Stories note was not researched in depth.
- Exact Story point copy-on-write semantics for filters vs shared sheet state need Desktop verification.


## What Analytics and Format features are table stakes for a credible Desktop-like replica?

### Takeaway
Analytics table stakes: constant/average reference lines, reference bands/distributions, totals, trend lines, and (if aiming high) forecast + clusters + box plots—scoped Table/Pane/Cell. Format table stakes: worksheet Font, Alignment, Shading (incl. banding), Borders/dividers, Lines (grid/zero/trend/reference), plus formatting for titles, legends, filters, highlighters, parameters, and tooltips; workbook-level defaults are a plus.

### Cited Findings

#### Analytics pane
- Drag analytics objects into the view; destinations include Table / Pane / Cell scope; dual-axis views expose per-axis drop targets. — [Analytics Pane](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_analytics_pane.htm)
- Summarize/custom objects include: Constant Line, Average Line, Median with Quartiles, Box Plot, Totals (Subtotals, Column/Row Grand Totals), Average with 95% CI, Median with 95% CI, Trend Line (Linear, Logarithmic, Exponential, Polynomial), Forecast (Desktop-only; constraints), Custom Reference Line/Band/Distribution, Custom Box Plot. — [Analytics Pane](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_analytics_pane.htm)
- Reference lines/bands/distributions/boxes attach to continuous axes; lines at constant or computed values; bands shade between values; distributions by percentages/percentiles/quantiles/std. dev. — [Reference Lines, Bands, Distributions, and Boxes](https://help.tableau.com/current/pro/desktop/en-us/reference_lines.htm)
- Forecast requires ≥1 measure; not supported with table calcs, disaggregated measures, percent calcs, grand/subtotals, Exact Date aggregation, multidimensional sources; nulls constrain. — [Analytics Pane](https://help.tableau.com/current/pro/desktop/en-us/environ_workspace_analytics_pane.htm)
- Clusters: drag Cluster onto view → creates Clusters group on Color (existing Color field moved to Detail); k-means; 2–50 clusters or auto up to 25; Variables dialog. — [Find Clusters in Data](https://help.tableau.com/current/pro/desktop/en-us/clustering.htm)

#### Format pane
- Worksheet-level Format (Desktop): Font, Borders, Shading, Alignment, Lines, Filters, etc.—applies only to current view; workbook-level formatting exists separately. — [Format at the Worksheet Level](https://help.tableau.com/current/pro/desktop/en-us/formatting_worksheet.htm); [Format at the Workbook Level](https://help.tableau.com/current/pro/desktop/en-us/formatting_workbook.htm)
- Fonts: pane vs header; special totals/grand totals fonts. — [Format at the Worksheet Level](https://help.tableau.com/current/pro/desktop/en-us/formatting_worksheet.htm)
- Alignment: horizontal, vertical, direction (normal/up/down), wrap for headers. — [Format at the Worksheet Level](https://help.tableau.com/current/pro/desktop/en-us/formatting_worksheet.htm)
- Shading: pane/header/worksheet backgrounds; row/column banding with band size and nested level. — [Format at the Worksheet Level](https://help.tableau.com/current/pro/desktop/en-us/formatting_worksheet.htm)
- Borders: cell/pane/header; row/column dividers with style/width/color/level. — [Format at the Worksheet Level](https://help.tableau.com/current/pro/desktop/en-us/formatting_worksheet.htm)
- Lines: grid lines, zero lines, trend lines, reference lines, drop lines. — [Format at the Worksheet Level](https://help.tableau.com/current/pro/desktop/en-us/formatting_worksheet.htm)
- Format interactive controls: legends, filters, highlighters, parameters; copy/paste worksheet formatting (Desktop; excludes reference lines/annotations). — [Format at the Worksheet Level](https://help.tableau.com/current/pro/desktop/en-us/formatting_worksheet.htm)
- Individual parts: titles, captions, tooltips, legends, filters, highlighters, parameter controls. — [Format Individual Parts of the View](https://help.tableau.com/current/pro/desktop/en-us/formatting_specific_titlecaption.htm)
- Mark labels have dedicated show/hide/format (font, match mark color, alignment)—Desktop-richer. — [Show, Hide, and Format Mark Labels](https://help.tableau.com/current/pro/desktop/en-us/annotations_marklabels_showhideworksheet.htm)

### Inferences
- MVP Analytics credibility: Average/Constant reference line + Totals + Linear trend covers most “Desktop-like” demos; Forecast/Clusters are differentiating but expensive (models + UI).
- MVP Format: axis titles, grid/zero lines, number formats, fonts for headers/pane, color legends, tooltip text—banding/dividers matter most for crosstabs.
- Without Table/Pane/Cell scoped averages, small-multiples analytics feel broken.

### Gaps
- Annotations, drop lines, and axis edit dialog (fixed vs automatic range, tick marks) were not fully documented in this pass.
- Exact exponential-smoothing forecast options UI (periods, CI width, Describe Forecast) relies partly on secondary blogs vs Help Forecasting article (not fully fetched).
- Number formatting / custom date formats / locale—critical for BI—were out of scope of Format worksheet page fetch.


## Which open charting approaches in Rust (egui plot, plotters, custom GPU, Vega-Lite interop) map best to Tableau’s mark model?

### Takeaway
No Rust crate implements Tableau’s shelf + Marks grammar natively. Best mapping is a **custom mark engine** (scene graph / GPU or immediate-mode primitives) driven by a Vega-Lite–like encoding IR; use egui_plot for interactive Cartesian prototypes, plotters for offline/high-quality primitives, charming/ECharts or Vega-Lite interop for declarative chart catalogs—not as a 1:1 Marks replica.

### Cited Findings
- **plotters**: pure-Rust drawing library for plots with bitmap/SVG/WASM/GUI backends; not limited to fixed chart types—you compose figures from primitives (lines, circles, meshes); widely used but described elsewhere as low-level / less actively developed. — [plotters GitHub](https://github.com/plotters-rs/plotters); [Zach Wong – Rust charting overview](https://zachrwong.info/writing/plotting-benchmarks/)
- **egui_plot**: immediate-mode 2D plotting for egui (v0.38.0 as of 2026-10-19); integrates with egui GUI apps; ecosystem notes list it among primary Rust plotting crates. — [crates.io egui_plot](https://crates.io/crates/egui_plot); [egui_plot GitHub](https://github.com/emilk/egui_plot)
- **charming**: Rust visualization library powered by Apache ECharts (declarative chart types). — [lib.rs visualization](https://lib.rs/visualization); [Charming vs Plotters](https://awesomedataviz.com/compare/charming-vs-plotters/)
- Vega-Lite related Rust crates exist (`vega_lite_4`, `vega_lite_5`, `vl-convert`, experimental `kanvas`) enabling declarative specs / conversion rather than Tableau Marks cards. — [lib.rs visualization](https://lib.rs/visualization)
- Ecosystem commentary: Plotters is mature/low-level (build chart types from primitives); performance can dominate Python libs for large scatters; newer libs (Kuva, Charton) target scientific PNG pipelines. — [Zach Wong – Rust charting overview](https://zachrwong.info/writing/plotting-benchmarks/)

### Inferences
| Approach | Fit to Tableau Marks model | Best use in a Desktop replica |
|---|---|---|
| **Custom GPU / egui painter** | Highest—encode Color/Size/Shape/Path/Angle as shaders/meshes; dual axis & small multiples as layout | Production viz canvas if UI is egui/wgpu |
| **egui_plot** | Medium—lines, bars, scatter, interactions; weak on treemap/pie/packed/Gantt/maps/density | Early worksheet Cartesian charts + brushing |
| **plotters** | Medium—flexible primitives; weaker interactive dual-axis/dashboard embedding | Export, tests, golden-image renders |
| **charming / ECharts** | Low–medium—rich chart catalog, not shelf grammar | Optional “Show Me → ECharts type” bridge |
| **Vega-Lite interop** | Medium–high conceptually (encoding channels ≈ Marks) but different layout/facet model | Internal IR or interchange; still need custom shelves UX |

- Closest conceptual cousin to Marks encodings is **Vega-Lite’s encoding channels** (x, y, color, size, shape, text, detail/tooltip, order/path, theta for pie)—map Tableau shelves → VL encoding, then render via custom or `vl-convert`; Tableau-specific facets (Measure Names/Values, dual axis Marks cards, density, Gantt size-as-duration) still need extensions.
- For OpenViz-like egui Desktop shell (local shots), **egui_plot + custom mark painters** is the pragmatic stack: egui_plot for axes/interaction; custom code for treemap, packed bubbles, highlight tables, pie angle, maps.

### Gaps
- No published production Tableau-clone architecture in Rust was found; feasibility notes are engineering inference from crate capabilities, not benchmarks against Tableau.
- GPU libraries (wgpu, lyon, vello) and map stacks (geo/proj tile renderers) were not evaluated in depth.
- charming/ECharts and Vega-Lite do not document parity with Tableau Analytics (clusters, exponential-smoothing forecast) out of the box.
)
