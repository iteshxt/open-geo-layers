# GEOlayers 3 — video tutorial mining (raw notes)

Collected 2026-11-18/19. Clean-room research only: public video **metadata** (titles, descriptions, chapter
timestamps, durations), public course curricula, public thumbnails/screenshots published by third parties
(Stadia Maps docs). No product files, ZXP, trials or vendor example zips were touched.

Legend: **[SOURCED: url]** = directly stated/visible in that source. **[INFERRED]** = my reasoning, verify later.

## 0. Method / what worked, what did not

| Source | Result |
|---|---|
| Vimeo oEmbed API `https://vimeo.com/api/oembed.json?url=…` | OK — titles, descriptions, durations for all six requested IDs |
| Vimeo simple API v2 `https://vimeo.com/api/v2/geolayers/videos.json?page=1..3` | OK — full list of 60 newest videos on vimeo.com/geolayers (all GEOlayers 3 tutorials included) |
| vimeo.com page HTML (WebFetch) | FAILED — "Verify to continue" bot check |
| Vimeo captions / player config | FAILED (non-JSON / challenge); not pursued further |
| YouTube watch-page HTML (public `ytInitialPlayerResponse.videoDetails`) | OK — descriptions + chapter timestamps for ~130 videos |
| YouTube captions (timedtext) | FAILED — empty response (needs PO token). Innertube `get_transcript` was blocked by the session permission classifier; not attempted again |
| recapio.com digest | FAILED — page only says "Generating AI summary…" |
| youtubetotranscript.com, youtube-transcript.io | FAILED — HTTP 403 |
| youtubetranscript.com | FAILED — empty shell |
| backup.hollywoodcamerawork.com, blog.naimichael.com (repost/summary mirrors) | FAILED — connection refused |
| avitmarine.com mirrors | FAILED — 404 |
| baks.verde.ag mirror | FAILED — TLS cert mismatch |
| aescripts.com/learn/* and toolfarm.com | FAILED — Cloudflare "Just a moment…" challenge (not bypassed) |
| boonelovesvideo.teachable.com course pages | OK — full curricula with lecture durations; lecture bodies locked |
| docs.stadiamaps.com/tutorials/getting-started-with-geolayers | OK — text + 6 screenshots taken "from the original GEOlayers 3 Vimeo guide" (Route Loop project) — **best source of exact UI labels** |
| Vimeo HD thumbnails (i.vimeocdn.com …-d_1920x1080) | OK — 29 thumbnails; a few show real UI (Assign Label Templates matrix, New Search results, MapTiler licence dialog) |

**No full transcript of any GEOlayers 3 video could be obtained.** All UI names below come from screenshots,
descriptions, chapter titles and course lecture titles.

---

## 1. Official GEOlayers tutorials (Vimeo: vimeo.com/geolayers — author "GEOlayers" = Markus Bergelt)

Source for whole table: Vimeo API v2 [SOURCED: https://vimeo.com/api/v2/geolayers/videos.json] and oEmbed.
Same videos mirrored on YouTube by aescripts + aeplugins (playlist "GEOlayers 3 Tutorials"
PLZAr8tT8TcsRKduHeuKtHEzMHNrUTybbq) and by the official GEOlayers YouTube channel (UCvDbc3BYv3vuyZrZf2AxWPw,
handle @GEOlayersAPP).

| Vimeo id | Date | Title | Dur | Description (verbatim topics) |
|---|---|---|---|---|
| 872558334 | 2023-10-09 | GEOlayers 3 - Continue to use Stamen Styles with Stadia Maps | 72s | (no description) |
| 654002705 | 2021-12-07 | GEOlayers 3 - Import from MapTiler Cloud | 90s | Learn how to import and update maps from cloud.maptiler.com |
| 533574071 | 2021-04-06 | GEOlayers 3 - Data-driven Time Offset | 347s | Visualize timing and magnitude of earthquakes with USGS data (earthquake.usgs.gov/earthquakes/feed/v1.0/geojson.php) |
| 528729028 | 2021-03-25 | GEOlayers 3 - Flyover Animations | 225s | Learn about Mapcomp View features and one-click flyover animations |
| 520372999 | 2021-03-06 | GEOlayers 3 - Motion Blur | 67s | Add Motion Blur to your map animations with a few clicks |
| 519954560 | 2021-03-05 | GEOlayers 3 - Local Tile Directories | 218s | Reproject geotiffs and create local tile directories. NASA Visible Earth; MapTiler Desktop |
| 519605279 | 2021-03-04 | GEOlayers 3 - Globe Project | 495s | Work with the Globe project; requires Video Copilot Orb |
| 517178231 | 2021-02-26 | GEOlayers 3 - New Search | 330s | "the search has got superpowers"; Historical Basemaps repo github.com/aourednik/historical-basemaps |
| 507479009 | 2021-02-02 | GEOlayers 3 - Geometry Simplification | 263s | why and how GEOlayers simplifies geometries for shape layers |
| 479822829 | 2020-11-16 | GEOlayers 3 - License Maptiler Imagery | 71s | how to integrate Maptiler Imagery |
| 475011262 | 2020-11-03 | GEOlayers 3 - Arrows | 140s | create animated arrows with a few clicks |
| 468890858 | 2020-10-16 | GEOlayers 3 - Scale Bar Script | 79s | add scale bars to your maps |
| 468873758 | 2020-10-16 | GEOlayers 3 - Draw Mapcomp View Script | 63s | add a certain Mapcomp's View to an overview map |
| 466519449 | 2020-10-09 | GEOlayers 3 - Assign Label Templates to Mapcomps | 78s | assign certain Label Templates to only certain Mapcomps; "A well hidden feature for power users and template creators" |
| 434592647 | 2020-07-02 | GEOlayers 3 - Line Patterns | 311s | create line patterns to highlight countries |
| 433581148 | 2020-06-29 | GEOlayers 3 - Edit Feature Properties with Spreadsheet | 131s | edit data of a feature collection with a spreadsheet editor |
| 425849140 | 2020-06-04 | GEOlayers 3 - Bikepark Tracks | 587s | use the 3D Landscape sample project |
| 404914688 | 2020-04-07 | GEOlayers 3 - Coronavirus Timeline Data Visualization | 398s | use the Coronavirus animated Timeline Sample Project |
| 399625603 | 2020-03-22 | Coronavirus animated timeline | 20s | created with GEOlayers 3 v1.0.8 |
| 397404357 | 2020-03-13 | GEOlayers 3 - Coronavirus Data Visualization | 275s | quickly import current Coronavirus data and visualize it |
| 376376225 | 2019-11-29 | GEOlayers 3 - Timed Data - Part 2 | 541s | more dataviz techniques |
| 373569335 | 2019-11-16 | GEOlayers 3 - Timed Data | 448s | racetrack visualization that matches on-board footage |
| 373569122 | 2019-11-16 | GEOlayers 3 - Subtract Polygon Features | 148s | Highlight only the land portion of a downloaded Feature |
| 367072536 | 2019-10-17 | GEOlayers 3 - Mapbox | 54s | integrate Mapbox Studio Styles |
| 361319119 | 2019-09-20 | GEOlayers 3 - Sliced Landscape | 742s | create a 3D Landscape setup; add sidewalls; draw Features on a 3D Landscape; add and displace a Label |
| 359745657 | 2019-09-13 | GEOlayers 3 - Route Loop | 704s | create Route by connecting Features; draw animated Shape Layers; animate a Layer as an arrowhead along a Features' path; automatically animate mapcomp views along and between features |
| 358244424 | 2019-09-06 | GEOlayers 3 - Data-driven Shape Layer Styles | 640s | Import Country Features; Use the Feature Browser; Investigate Features; Create Shape Layer Styles; Draw Features |
| 357897696 | 2019-09-04 | GEOlayers 3 - Mapstyling | 801s | Use Adobe Swatch Files; Adjust Colors; Change Fonts and Language; Show and hide groups of geographical Features; Split your Map into different Layers like Labels and Imagery; Link Mapcomps. "Note that for mapstyling directly inside After Effects a MapTiler Cloud Plan is required." |
| 352731294 | 2019-08-08 | GEOlayers 3 - Getting started | 416s | Create a Mapcomp; Basic map styling; Find Locations; Animate your map; Finalize your Mapcomp |
| 338398513 | 2019-05-25 | GEOlayers 3 | 67s | teaser: "An Extension for Adobe After Effects to design and animate maps" |
| 351912966…351914106 | 2019-08-04 | showcase clips: Zugspitze, Galathea Depth, Singapore, Snow Globe, New York Comic, Madrid - Rome, Halftone, Flood Animation, Empire State Building, Berlin - NY | 5–30s | (sample-project showreels — likely correspond to bundled sample projects [INFERRED]) |

Older GEOlayers 2 videos on the same channel (context only, not GEOlayers 3): "New Feature Download" (Natural Earth
+ OSM features for current view), "Folded Map" sample, "Walkthrough: Mount Everest Tour" (GEOlayers 2 + Trapcode Mir 2),
"create a server-profile for mapbox custom maps", "Elevation 4K" template, "Online Label Templates", "GEOlayers 2
Tutorial 09.3 WMS", "Swap Feature Styles", "Create/Animate Car & Plane Routes", "Box Labels in Hong Kong".
GEOlayers 2 concepts that clearly carried over into 3: server profiles → imagery styles, online label templates,
swap styles, car/plane routes [INFERRED from changelog + titles].

Thumbnail title cards confirm taglines: "Maps for After Effects", "Animate in 3D Space"
[SOURCED: Vimeo thumbnails 338398513, 352731294].

### aescripts YouTube mirrors (description adds)
- "GEOlayers 3 Tutorial: Getting Started" BHp4J4zVJSQ (416s), "…: Mapstyling" d7cIDAZLkxg (801s), "…: Data driven Shape Layer Styles" 0bZgFC_1FEE, "…: Route Loop" fJFMsVBpuSY, "…: Sliced Landscape" EBB5bb1P9nA, "…: Timed Data Part 1/2" 5r4WlQv6u9A / MZv5PsidP_Q, "…: Line Patterns" PtSZNvTZCtY ("line patterns to highlight features"), "…: Bikepark Tracks" yP5SJISXtls, "…: Assign Label Templates to Mapcomps" krDpSgT2qEg, "…: How to add scale bars" ha3yhVuQrsw, "…: Animating Arrows" fUD6QdB3ipc, "…: Powerful New Search Bar Features" N2IQs94rx_Q ("The search bar in GEOlayers just got a lot more powerful"), "Working with the Globe Sample Project in GEOlayers 3" m5bMw8Fq9q4 ("GEOlayers comes with many sample projects… geo-accurate 3D globe animations… requires the free ORB plugin from Video Copilot"), "Using Georeferenced Imagery and Local Tile Directories" gxtlqJodDkI ("NASA Visible Earth collection as well as local tile directories"), "How to add Motion Blur to Map Animations" XKd_cNsDF-g ("built-in support for adding motion blur to map and label layers"), "Create a Map Flyover Animation using GEOlayers" fZ17qbEFvko ("map flyover animation in one click"), "Data-Driven Time Offset Animation" MiEl9pWO-zA, "Create a 3D Sliced Landscape in After Effects" DgzHjJcI4Rk (GEOlayers 3 + Mettle FreeForm Pro + FLUX, host Charles Yeager), shorts x9kEDk7mO68 (Arrow tool), qPBe2AE7Nu8 (3D Terrains with Helium), qy92SmN8Dqo (animated clouds), JqJH3i71iRQ (USSR → 15 countries). "GEOlayers Trailer" 1LRVN4YAwq4 (2015, v1: "HTTP-tileservers for up-to-date imagery and highly detailed vector-data").
[SOURCED: youtube.com/watch?v=<id> descriptions]

Mettle article step headings for the Sliced Landscape tutorial: "Create 3D Landscape." → "Keyframe GEOlayers 3 landscape." → "Finalize GEOlayers 3." → "Create landscape edge." → "Adjust landscape elevation." (then FLUX / FreeForm Pro steps)
[SOURCED: https://www.mettle.com/how-to-create-a-3d-sliced-landscape-geolayers-3-freeform-pro/]

---

## 2. EXACT UI LABELS from screenshots (highest-value section)

### 2a. Stadia Maps doc screenshots (Route Loop project, AE 2023, GEOlayers 3)
[SOURCED: https://docs.stadiamaps.com/tutorials/getting-started-with-geolayers/ images geolayers-main.png,
geolayers-mapcomp-settings.png, geolayers-mapcomp-add-new.png, geolayers-mapcomp-url.png,
geolayers-mapcomp-api-key.png, geolayers-mapcomp-apply.png]

**Main panel ("GEOlayers 3" panel, with ≡ flyout menu)** top→bottom:
1. Header row: list icon (☰, opens Mapcomp list) | current Mapcomp name centred ("Route Loop") | **"✓ Finalize"** button (top-right, outlined blue).
2. Toolbar row of icons (left group, 7 icons): pin, a pill/toggle-shaped icon, a sliders/list icon, a cube, a mountain/triangle pair, a bar-chart, a document/script icon. Centre: a label-template selector showing **"Null Layer ▾"** with a template icon, followed by a swap (⇄) icon. Right group: a curved "share/arrow" icon, a framed-pen icon, a colour swatch square, and another ⇄ icon.
   - Exact meaning of each icon is NOT labelled in the screenshot → UNKNOWN. [INFERRED mapping, to verify: pin = "Pin selected layers" (changelog: "Unpin pinned layers/labels by clicking pin button in toolbar again"); cube = 3D Scene / extrusion; mountain = 3D Landscape; bar chart = DataViz / Property Diagrams; document = Scripts (changelog: "Run scripts from the toolbar"); curved arrow = Create Arrows (changelog 1.1.7 "Create Arrows from the toolbar"); framed-pen = draw onto layer / "Create a Shape Layer to Draw On"; colour swatch + ⇄ = current Shape Layer Style + swap style; "Null Layer ▾" + ⇄ = current Label Template + swap label.]
   - In the "New Search" thumbnail the same centre selector reads **"10 Region 2 ▾"** → confirms the centre dropdown is the **Label Template selector**, and "Null Layer" is one of its entries (matches API 1.20.0 "addLabel supports null as labelTemplateComp argument, to add a null layer"). [SOURCED: Vimeo thumbnail 517178231]
   - Swatch in New Search thumbnail is pink/magenta; in Stadia shot dark grey → swatch reflects the currently selected Shape Layer Style [INFERRED].
3. Search field: magnifier + placeholder **"Search..."**; an × clears it.
4. Preview map (dark basemap) — shows the Mapcomp's camera frustum as a blue trapezoid with diagonals (view of the pitched/bearing-rotated mapcomp). Top-left: crosshair/target icon. Top-right: a camera icon (blue) and an image icon. [INFERRED: camera = "Show Mapcomp view"/sync preview to mapcomp view; image = toggle imagery preview.] MapMagic video lists "Show Mapcom view button", "Link and Unlink control map with preview map", "Zoom level limitation", "Mercator projection explanation", "Changing the pitch and the bearing" [SOURCED: youtube kzUK7jjBIlg description].
5. Preview footer: diamond/compass-like icon, **chain-link icon (blue = linked)**, zoom-out (−) magnifier, horizontal zoom slider, zoom-in (+) magnifier, a north/compass (ⓘ-like) icon at far right.
6. Feature Browser header row: **"+"** (add/import), funnel icon + **"Filter..."** field, then geometry-type toggles: **● (points)**, **> (lines)**, **■ (polygons, blue filled)**, a dashed-square (bounding box / current view?) icon, and a stacked-shapes icon at far right.
7. Feature tree: **"☆ Favorite Features"** (collapsed) and **"📄 Features"** (collapsed) — default collections.

**Mapcomp list** (opened via ☰):
- Each row: thumbnail, Mapcomp name ("Route Loop"), an imagery icon (blue picture), an **"A"** icon (Mapcomp Labels toggle — forum confirms "toggling the little 'A' icon in the mapcomp list"), a **"Preview"** pill badge, a lock icon at right.
- Hover buttons under the name: **Settings** (sliders icon, tooltip "Settings"), duplicate (two-sheets icon), delete (trash), ✓ (finalize this Mapcomp [INFERRED]).
- **"+ Create new Mapcomp"** row.
- Footer: a small icon bottom-left (sort / link? unknown) and **"Back"** button bottom-right.

**Mapcomp Settings** dialog (title "Mapcomp Settings"):
- **Name** (e.g. "Route Loop")
- **Duration** (e.g. "30 seconds")
- **Language** dropdown (e.g. "English - United States")
- **Max Zoom** slider (value 16)
- **Min Zoom** slider (value 0)
- **Attribution** dropdown ("bottom left") + a checkbox + ⓘ info icon
- **Imagery** section (picture icon, crossed-tools icon at right = edit/clear?) with a grid of imagery-style thumbnails (~30 shown: transparent/checkerboard ones, satellite, Natural Earth, dark/light basemaps, black-white water masks, dotted (Circles/heatmap dataviz), purple, error "!" tiles, loading spinners) and a **"+"** tile to add a new imagery style.
- **Quality** slider (80%)
- **Blend Duration** slider (0.3 s)
- **Imagery Motion Blur** toggle
- **Cancel** / **Apply** buttons.

**Mapcomp Imagery** editor (title "Mapcomp Imagery"; header icons: save, import, export):
- Live preview with zoom slider (− / + / compass).
- **Name**: "Universal Raster" (default name of the custom raster style)
- **Raster Source** group:
  - **URI** text field (default value `https://a.tile.openstreetmap.org/{z}/{x}/{y}.png`), a **"..."** button (pick local file/folder [INFERRED; changelog 1.1.8 "Pick local tile directories in Imagery setup"]), and a scheme dropdown **"xyz ▾"** (other option presumably "tms" — changelog "Tilemerger supports TMS servers" [INFERRED]).
  - **Min Zoom** 0, **Max Zoom** 16
  - **Tile Size** 512 px
  - **Bounds** -180°, -85.051129°, 180°, 85.051129° (Web-Mercator limits)
- Adjustment sliders: **Hue** 0, **Opacity** 1, **Black** 0 (list continues below the fold — unknown).
- **Cancel** / **Apply**.
- Stadia workflow: append `?api_key=…` to URI; rename (e.g. "Stadia Maps - Stamen Toner"); Apply saves it into the style library; Apply again on Mapcomp Settings applies to the Mapcomp; "GEOlayers should now re-render the Mapcomp" [SOURCED: Stadia doc text]. Stadia tile URL used: `https://tiles.stadiamaps.com/tiles/stamen_toner/{z}/{x}/{y}@2x.png`.

**After Effects side (what GEOlayers creates)** — visible in geolayers-main.png:
- Project panel: comp **"containing Route Loop"**, folder **"GEOlayers 3 Items"**, folder "Solids".
  → The Mapcomp ("Route Loop") lives inside a containing comp named "containing <Mapcomp name>" when created from scratch [SOURCED screenshot; naming rule INFERRED].
- Timeline of "containing Route Loop" (top→bottom):
  1 "Hue" (adjustment/solid layer)  
  2 "Label 'Geneva'" (precomp layer — Template Label)  
  3 "Label 'Lyon'"  
  4 "★ Arrowhead" (shape layer)  
  5 "★ Lyon - Geneva (93.224 mi)" (shape layer — connected route; **length appended to name in miles**) parent "8. Route Loop Anchor"  
  6 "★ Geneva" (shape, blend Multiply) parent "8. Route Loop Anchor"  
  7 "★ Lyon" (shape, blend Multiply) parent "8. Route Loop Anchor"  
  8 "Route Loop Anchor" (red label colour; parent "9. Route Loop")  
  9 "Route Loop" (the Mapcomp precomp layer)
  → Naming conventions: label layers `Label '<name>'`; anchor `<Mapcomp> Anchor`; drawn features named after feature name; route between features `<A> - <B> (<distance> mi)`.
- Comp is 1920×1080-ish, 25 fps ("00148 (25.00 fps)"), renderer **Classic 3D**, view "Active Camera".

### 2b. Vimeo thumbnails showing UI
- **Assign Label Templates to Mapcomps** (466519449): panel shows "Renderer: Classic 3D" and a matrix: checkboxes **"Select all"**, **"Select none"**; columns = Mapcomp names (rotated): "Main Map", "Main Labels", "Overview Ma…"; rows = label templates **"01 Place Top", "02 Place Bottom", "03 Place Left", "04 Place Right", "05 Region", "06 Place Top 2", "07 Place Bottom 2", "08 Place Left 2", "09 Place Right 2", "10 Region 2", "11 Locator", "11 Locator 2", "Capital Citi…"** → these are the **default Label Template names** (numbered, two design families + Locator). [SOURCED: https://i.vimeocdn.com thumbnail of vimeo 466519449]
- **New Search** (517178231): search typed "Germany"; results grouped under source header **"Natural Earth"**: "■ Deutschland — Sovereign Country", "> Deutschland — Inner Boundaries", "📁 Deutschland — States & Provinces" (folder icon = collection). Row hover actions: **fit-to-bounds (dashed frame) icon, tag (label) icon, pen (draw) icon, ⤓ download (add to browser) icon**. Further results (OSM/Nominatim style) "● Germany — Hamlet: Houston County, Texas, Vereinigte Staaten von Amerika", "● Germany — Hamlet: Owen, Clark County, Indiana…". Preview map shows selected collection outline with label **"Deutschland (3)"** (collection name + feature count). Back button bottom-right. Mapcomp name "Mapcomp 1" (default naming). [SOURCED: thumbnail 517178231]
  → Geometry icons: ● point, > line, ■ polygon, 📁 collection.
- **License Maptiler Imagery** (479822829): dialog **"Data not licensed"** with MapTiler logo: "In order to use this Imagery you'll need to license data powered by MapTiler. 1. Click the "License Data" button below 2. Check your browser and sign up for a FLEX plan 3. Copy your MapTiler API Key and paste it right here: [Paste your API Key here] 4. Hit Apply and restart GEOlayers" — buttons **Cancel / License Data**; underneath, imagery thumbnails strip with "+" and dialog footer **Cancel / Create** (this is the create-Mapcomp style step). [SOURCED: thumbnail 479822829]
- **Edit Feature Properties with Spreadsheet** (433581148): Google Sheet named "United States of America Properties" containing columns of multilingual names (Alabama/Алабама/…/Chinese), numeric ids (e.g. 1159315233 = Natural Earth ne_id [INFERRED]), "admin_boundary state", label coords, an index column "19 Alabama"… → CSV/TSV export of collection properties round-trip. [SOURCED thumbnail]
- **Local Tile Directories** (519954560): shows MapTiler Desktop start screen options "Folder with tiles", "…age"(Image), "MBTiles", "Google Earth KML" — external tool used to generate tiles. [SOURCED thumbnail]
- **Import from MapTiler Cloud** (654002705): cloud.maptiler.com/maps "Standard maps": Basic, Bright(?), Outdoor, Satellite Hybrid, Streets, Toner, Topo, Topographique, Voyager; country maps (Czechia CZ Basic EPSG:5514 …, France). [SOURCED thumbnail]
- **Data-driven Time Offset** (533574071): timeline UI overlay with dates "2021-03-30 / 2021-03-31 / 2021-04-06" and a progress bar — earthquake circles appear by date. [SOURCED thumbnail]
- **Draw Mapcomp View Script** (468873758): overview map of Europe with a red rectangle showing a detail Mapcomp's view. **Scale Bar Script** (468890858): white bar labelled "90 m" bottom-right over satellite imagery. [SOURCED thumbnails]
- Attribution strings seen on finalized maps: "© MapTiler © OpenStreetMap contributors", "Map tiles by Stamen Design, CC BY 3.0 — Map data © OpenStreetMap", Bing: "© 2019 Microsoft Corporation / © 2019 DigitalGlobe / ©CNES (2019) Distribution Airbus DS / Mapzen", "Natural Earth", "2019 Novel Coronavirus (nCoV) Data Repository, provided by JHU CCSE / © OpenStreetMap contributors © CARTO". Attribution is italic text bottom-left by default. [SOURCED thumbnails]

---

## 3. Boone Loves Video (Jason Boone) — GEOlayers 3 MasterClass curriculum (verbatim)
[SOURCED: https://boonelovesvideo.teachable.com/p/geolayers-3-masterclass] — $349, AE 2023, 11 sections / 59 lectures.

**Hello There**: Welcome to the Course (1:48) · A Tour of the Interface (12:08) · MapTiler Features (Not Required for Course) (8:54) · Contact, Support and Updates  
**QuickStart Guide - Make a Map in 30 Minutes**: Create a Mapcomp (6:26) · Find Data (3:21) · Draw Features (3:43) · Add Labels (4:23) · Animate the Map (6:06) · Final Touches (5:47) · The Disputed Area Dataset (1:34)  
**Create Mapcomps**: How a Mapcomp Works (5:47) · Mapcomp Settings (6:02) · Controller Mapcomps (4:31) · Duplicate Mapcomps (3:45) · Where to Find License Information (1:17)  
**Map Features**: Search for Features (2:54) · Feature Properties (3:41) · Download Datasets (2:47) · Create Custom Features (6:51) · Import and Export (2:35)  
**Draw Features**: How Features are Drawn (2:19) · Layer Styles (4:49) · Create Custom Layer Styles (3:56) · Control the Geometry (6:11) · Draw Features in the Mapcomp (2:17)  
**Label Features**: How Labels are Drawn (3:23) · Fine Tune Label Placement (4:07) · Create Custom Label Templates (8:06) · Pin Layers to a Map or Feature (4:07) · Parent Layers to a Map (2:59) · Mapcomp Label Templates (2:17)  
**Animate!**: Animate the Mapcomp View (5:40) · Simple Transitions (6:51) · Fine Tune Motion with the Graph Editor (3:11) · Animate Paths and Routes (7:10) · Animate Labels (9:50)  
**Map Design and Layout**: Create a Locator/Inset Map (5:29) · Draw a Scale Bar and North Needle (2:44) · How to Add Texture (3:27) · How to Use Mattes (4:23) · Stylize With Adjustment Layers (3:04)  
**Advanced Techniques**: How to Draw on Shape Layers (0:55) · How to Draw Arrows (2:43) · Georeference Existing Maps (4:56) · How to Use Shadows (6:05) · How to Add Feature Properties (3:31) · How to Work With Mapbox (2:23) · Animating Flags (11:52)  
**Project Breakdowns**: Census Regions of the United States (29:21) · Make a Map Like Vox (27:17) · Create a Choropleth Map (32:58) · Make a Map Like RealLifeLore (31:12) · Recreate the Johnny Harris 3D Hawaii Map (3rd Party Plugin Required) (11:19) · Mapping Vasco Da Gama's Route to India (17:46) · Animating the Panama Canal (33:05)  
**Best Practices**: 3 Ways to Finalize Imagery (1:49) · Improve Playback Performance (7:05) · Troubleshooting (9:28)

Notable feature signals: "Controller Mapcomps" (one Mapcomp drives others = linked Mapcomps), "Where to Find License Information" (attribution/licence info per imagery), "Mapcomp Label Templates" (label templates for automatic Mapcomp labels vs. Template Labels), "3 Ways to Finalize Imagery" [INFERRED: full / only current frame / preview quality or work area — matches API finalize options].

### GEOlayers 3 WAR! curriculum (verbatim titles, $349, AE 2025)
[SOURCED: https://boonelovesvideo.teachable.com/p/geolayers-3-war and /courses/2552093]
Hello There (Welcome, Contact…) · Research, Planning + Preparation (The Project Brief, Collecting Maps, Battle Map Breakdown) · Create a Basemap (The Elements of a Basemap, 7 Ways to Create a Basemap, Making Custom Basemaps with Mapbox, Administrative Boundaries, Mapping Cities, Towns and Villages) · Create Map Features (Basic Georeferencing, Advanced Georeferencing, Georeferencing with Graticules, Digitizing, Advanced Digitizing, How to Manage Your Map Features) · Iconography (Where to Find Icons, Where to Find Vector Imagery, Font-based Icons, Portraits) · Symbology (Military Symbols, Flags, How to Visualize Disputed Areas) · The Power of Labels (Battle Map Fonts, Pin and Parent Elements, Custom Label Templates, Animate Labels, Label Locations Quickly, **Measure Distance and Area**) · Invasions and Frontlines (3 Ways to Add Arrows, Put Symbols on a Path, Visualize an Invasion with Shape Layers, …with Solid Layers, How to Rasterize Vector Layers) · Battle Elements (Fire, Smoke, Reticals and Crosshairs, Smoke Blasts, Gunfire, Muzzle Flash, Troops, Boat Wake) · Project Breakdowns (Operation Overlord Daily Frontlines, Make Labels Like Knowledgia, Germany Advances, Map the Sunken Uboats of WWII, Make a Basemap Like Caspian Report).
Other Teachable products: Adobe After Effects Masterclass (€299), bundles "After Effects / GEOlayers Masterclass" (€449), "GEOlayers 3" (2 courses, $499). No separate "QuickStart" product exists — QuickStart is section 2 of the MasterClass. [SOURCED: https://boonelovesvideo.teachable.com/]

---

## 4. Boone Loves Video YouTube — chapter lists (feature-relevant lines)
All [SOURCED: https://www.youtube.com/watch?v=<id> description]

- **5B8p13M0AAI "New to GEOlayers 3? Watch This First!"** (18:06): 0:00 Intro · 0:35 Setup Your Basemap · 2:31 How to Navigate the Map · 2:59 Download Map Features · 4:12 Shape Layer Styles · 4:55 Draw Out a Country · 5:36 How to Search Map Features · 5:55 Add a Label with a Template · 7:02 Add a Label Manually · 8:02 Animate the Map · 9:33 Fine Tune the Animation and Labels · 12:26 How to Make Your Map Pop · 17:29 The Final Animation.
- **DX_qi37vy0Y "GEOlayers 3 for Beginners: Every Button Explained"** (24:49, 2026-06-29): "comprehensive look at the user interface" — no chapters. (Key target for a future transcript.)
- **dBqZs9uT6Hw "Create Your First Map in GEOlayers Step-by-Step"** (21:19): Setup a Mapcomp · Choose the Mapcomp Basemap Imagery · How to Navigate the GEOlayers Panel · Download Map Data · **Finalization Explained** · How to Organize Your Map Features · How to Draw Out Features · Label Features · Style Your Map · Animate the Map View · Animate Map Features · How to Export Your Map Animation · **Revision Workflow** · The Final Animation.
- **0SoBCtbF3B0 "18 GEOlayers Tips in 18 Minutes"** (2022): **Finalize One Frame** · **Turn Off Labels** · Quickly Create and Animate a Route · Quickly Animate the Camera View · **Change Preview Settings to Work in Real Time** · Create a Custom Layer Style · Applying a Label · **Download Label Templates** · Manually Place Label Templates · Control Labels Precisely · Scaling Labels · Download Geodata · Pinning Layers · Custom Label Templates · Adding Arrows · Scale Bar · Make Overlays Pop · Imagery Motion Blur.
- **2Ga-6DD2epo "20 GEOlayers Tips in 10 Minutes"** (2024): Use a Free Basemap in Commercial Projects · **Create Layer Style Presets** · Where to Find Historical Boundaries · Control Feature Styles with Data · Create Label Templates · Georeference Maps · Add Seamless Textures to Your Map · **Filter in the Browser via Data** · **Create Custom Map Feature from a Layer** · **Merge and Subtract Features** · **Split a Map Feature Into Individual Polygons** · **Feature Collection Naming and Sorting** · **Isolate Points with Polygons** · **The Multi-Mapcomp Workflow** · Finalize Like a Pro · **Create a Shape Layer to Draw On** · **How to Add Data to a Feature** · **Turn Something into a Compass** (north needle script) · MapTiler Madness.
- **aELgIVzP_HE "GEOlayers GOD MODE"** (2025): **Find Features at Coordinate** · How to Automate Animations · Use MapBox Studio · OpenStreetMap Trick · Multi-Mapcomp Workflow · **Default Scripts** · **Create Shape Layer Styles from an Image** · **Choropleth Mapcomp Trick** · Animate a Time Series.
- **y05ttPEuQow "Master GEOlayers Labeling: 8 Pro Tips"** (2025): **How to Add a New Label Template in GEOlayers** · **Use a Label Anchor Point Null Layer** · Animate Your Label In and Out · **Add Responsive Design - Time** (label in/out markers) · **Create Labels for Multiple Positions** · Create a Light/Dark Mode Rig (LabelColors) · Use Essential Graphics · **Create a Label Template Project**.
- **OTzPcJTSlpU "How to Create Custom Label Templates in GEOlayers 3"** (8:53) — MasterClass module. **D32kKyXBCyo "Create Map Label Templates for GEOlayers 3"** (6:41).
- **KM3lFJEa3JA "The Free Basemap in GEOlayers"** (2024): **Universal Raster Mapcomp Profile** · Small Scale Explained · **How to Override Zoom Level** · Natural Earth Data · **How to Make a Watermask Mapcomp** · Natural Earth Vector Data · Open Street Map Explained · **How to Find Attribution Information for Any Map**. ("All versions of Natural Earth raster + vector map data are in the public domain.")
- **pxHxlnEzt8E "MapTiler + GEOlayers 3 EXPLAINED"** (11:04): what you get with a premium MapTiler account (maptiler.com/cloud/geolayers).
- **LEaMUpjupG0 "How to Get DETAILED BORDERS (4 Tips)"**: Tip 1 **Simplification** · Tip 2 Use OSM · Tip 3 Create a New Feature · Tip 4 **Watermask Mapcomp (MapTiler premium feature)**.
- **TH9WoaOU3Go "How to Map Administrative Boundaries in GEOlayers 3"** — country borders, provinces, neighbourhoods.
- **NDqpEe3G2yc "NEW! 3D Scene Feature in GEOlayers"** (2025-01, matches 1.12.2 "Create 3D Scene & Camera Script"): How to Create a New 3D Scene in GEOlayers 3 · The Setup Explained · **How to Create Multiple 3D Scenes** · Creating 3D Snow on a Map · Adding 3D Text · **3D Renderers Explained** · How to Import 3D Models in After Effects. ("You can finally use 3D cameras in GEOlayers.")
- **zS11R5Cdszc "Use 3D Models in GEOlayers"** (2025): 3D File Formats in AE · GLB is the Best · How to Animate the 3D Model in GEOlayers 3 · Environment Light · Shadow Catcher Layer · **GEOlayers' Native Script for 3D Workflows**.
- **WUhJe_9DQ4Y "3D TERRAIN… GEOlayers 3 + Helium"** (2023): **Create 3D Landscape Mapcomp** · **The Controls** · **Increase the Elevation Height** · Enable Depth of Field · Add Lights · Fix the Background · Create a Simple Loop Animation. ("works with native After Effects cameras, and will cast shadows from lights").
- **s-bmbI-P-5g "3D TERRAIN Map Route Animation"** (2021, for Eva zu Beck).
- **rmITTGwVENo "Up Your ARROW Game in GEOlayers 3"** (2023): **Draw Arrow Tool** · Retiming · Repositioning · **Make the Arrow a Map Feature** · **Animate Along Arrow Path** · Animate the Zoom, Pitch and Bearing · Add a Shadow · Get the 3D Look.
- **oilXRAW0_CM "Create Branching Animated ARROWS"**: … Bring Arrow into GEOlayers 3 · Add a Stroke Layer Style · How to Move the Arrows.
- **D_X3xN5Uc_Y "Data-Driven Styling in GEOlayers 3"** (2024): Set up a Mapcomp · Populated Places Dataset from Natural Earth · Import into GEOlayers 3 · Layer Styles · **Isolate Points Inside a Polygon** · **Data-Driven Styles** · **How to Explore Feature Properties of a Data Feature Collection** · **The Best Way to Create a Data-Driven Style**. (Mentions a separate "raster technique" = Choropleth/Circles/Heatmap imagery.)
- **3OU3eQsudeM "Animate Your GEOlayers Map View Like a Pro (5 Tips)"**: Download Map Features · **Animate to a Feature** · Constant Movement with LoopOut · Markers for Visual Reference · **Motion Blur to the MapComp Imagery**.
- **g4zTDhlMYzg "9 Map Animation Moves Every GEOlayers User Should Know"** (2025): Manually Animate View to a Feature · **Animate View to a Feature** · **Animate View Along Features** · **Animate View Between Features** · The Never-Ending Flyover · The Never-Ending Revolution · Ping Pong · **Manually Animating to Mapcomp View Features** · Animate View Along a Custom Feature Path.
- **-yfNEqew-a4 "Trigger Animations with the Map Zoom Level"**: isolate the Mapcomp **Zoom** parameter, conditional expression, slider controls, animation preset (shows Zoom is an expression-readable effect control).
- **M_iUcgEILw8 "LATITUDE and LONGITUDE Labels in GEOlayers"**: MapComps Explained · source-text expression reading lat/lon · decimals · Guide Layers · Create Your Own Map Feature Path · Animate a Shape Along Your Map Feature Path · Degrees/Minutes/Seconds format.
- **EtsCl787YR8 "How to Georeference Maps in GEOlayers 3"**: Bring it into AE · Move Anchor Point to Location · Navigate GEOlayers Map to Location · Adjust Opacity, Scale and Rotation · **Parent to Map and Create Map Feature** · Puppet Pin Tool.
- **tZ7SVVmgwyE "How to Georeference Photos in GEOlayers 3"**: Copy/Paste the Coordinates · **Paste Coordinates in GEOlayers 3** · Georeference a Null Object · Fine Tune the Position.
- **kA-w8W4eqCM "How to Map Your Photos in GeoLayers"** (2026): georeference photos in bulk (uses ExifTool → CSV import [INFERRED]).
- **Q-Z2qs21tx8 "Create Custom Routes for Your Map (KML)"**: Google My Maps → KML → GEOlayers (KML import confirmed).
- **38ynpb1N4PE "Advanced Map Route Visualization Techniques"** (2026): GPX file + gpx.studio (GPX import).
- **-3TPi_9-DS4 "RECORD and VISUALIZE Your GPS Tracks"**: Transfer GPS Files · Animate GPS Routes on a Map.
- **1z_f9kwvSCg "Tips, Tricks + Troubleshooting GEOlayers 3"** (2023) and **gfXl9-7oZjY "How to Troubleshoot GEOlayers"** (2026): How to Contact Support · **The Help Menu** · **"Get Dimensions Error"** · **Mapcomp Not Showing** · **Mapcomp Not Linking** · **How to Import a Project** · Fix Imagery Issues.
- **qwKj1lOyNtU "Make Better Map Animations With INSET Maps"** (2026) — inset/locator map via second Mapcomp + Draw Mapcomp View.
- **Vq6q-X3KVgo "Crazy Cool GEOlayers Trick"** (2025): The Problem with Vector Shape Layers · **How to Export to a Geojson** · **Create a New Choropleth Mapcomp** · **Connect Your Geojson** · Benefits (raster instead of vectors).
- **JcCTGlszHcw "How to Map the Continents in GEOlayers"**: Download the data · Create a New GEOlayers Mapcomp · **Rename the Feature Collection** · Draw the Continent Features · Customize Features Drawn · **Create a Choropleth Map Comp**.
- **JTQ4aniKPek "How to Rasterize Vector Shapes"**: Get a Map Feature · **How to Simplify Geometry** · Precomping · **Precompose Feature Shape Layer** (default script precompose_feature_shape_layer.jsx) · Auto-tracing the Alpha.
- **v36dD4_lZ4U "Add Texture (Method 2)"**: **Create a New Texture Mapcomp** · **Choose the Seamless Texture Profile** ("GEOlayers has a built in image profile called Seamless textures, which allows you to create an image tileset with your texture") · Finalize · Change the Blend Mode · Create a Seamless Texture in Photoshop.
- **nUu1XaCys34 "Add Texture (Method 1)"**: Create the Mapcomp · Overlay the Texture · **Attach the Texture to the Map** · CC RepeTile.
- **3ih7TuYiZow "How to Visualize Distance on a (GEOlayers) Map"** (2025-09, matches 1.14.0 "Distance Circle default Script"): Download a Map Feature · Center Up the Mapcomp View · **Run the Distance Circle Script** · Style the Circle · **Increase the Vertices** · **The Parameters** · Duplicating the Circles.
- **h-mJiAIydXI "Walking Distance Map"** (2022): Add a Scale Bar · Find Pixel Width for 1k (scale bar script).
- **XTN0qAKfmtg "OpenStreetMap ➡️ AE (4 Ways)"**: Method 3 **Overpass Turbo to After Effects (with GEOlayers 3)** · Method 4 **QGIS to After Effects (with GEOlayers 3)**.
- **qwvzGTaH-AM "Mapping All of the Bakeries…"** (2022): Download OpenStreetMap Data · Overpass Turbo · **Import Geodata into GEOlayers** · Draw Out the Map Markers.
- **y34htfkMmxc "How to Create Historical Maps"** (2022): Historical Basemaps Repository · **Connecting the Repository** (GitHub Search API) · Search for the Maps · Drawing the USSR / Holy Roman Empire.
- **17tHaEJuZNA "Columbus 1492 Voyage"** (2022): Georeference an Image · Trace the Route · **Create Feature from Layer** · Stylize the Map · **Automating Animations with Geodata** · **Animate Layer Along Feature Path** · **Position Progress Effect** · **Control Label Position** · **Change the Label** (swap) · **Change the Base Map**.
- **4kWcuolF16Q "A24 Civil War Map"**: Download USA State Polygons · **Add Map Data Column** · Create a Polygon for Each Force · Set up the Mapcomps · **Duplicating Mapcomps**.
- **Duglh9VTLes "From USSR to 15 Countries"** (2025): Create a Basemap · Download Country Map Features · **How to Add Map Feature Properties in GEOlayers 3** · **Create a Layer Style Preset** · How to Draw out the Map Features · Draw out Country Borders · De Facto vs De Jure Country Admin Boundaries · Where to Find and Download Disputed Country Boundaries.
- **5b7loF9HJr8 "Russo-Ukrainian War"**: Admin Boundaries · Add Labels for Countries · **How to Create a Shape Layer Style in GEOlayers** · How to Map Disputed Areas · **How to Georeference in GEOlayers** · Create a New Map Features.
- **TXjU-WVPwa4 "Historic Battle Maps"** (43 min): Aligning the Reference Map · **Create a Mapcomp View** · Draw out the Countries / Ocean / Rivers · Label … with Label Templates · Adding Animated Arrows · Retiming the Animation.
- **jkAOSag3QFg "Map Like GeoGlobeTales"**: Create the Mapcomp · **How to Create a Square Map** · Animate the GEOlayers Mapview · Render out in Portrait 9:16.
- **dJpOurDy5sI "Cuban Missile Crisis"**: Create a Mapcomp · **Create a Custom Basemap** · **Optimize the Land Polygon** · **Label World Cities**.
- **sxh0aGwXfQI "North Korea Testing Nukes"**: Launch GEOlayers 3 · **Add Null Object to Launch Site** · Draw the Rocket Path Trajectory · Move the Path in 3D Space.
- **pAsBSu5Vw8I "Mission Impossible Map"**: Felt → **Import GeoJson in After Effects** · Desaturate the Basemap · Connecting the Lines · Name Labels · Animating the Lines and Labels.
- **3C6Ch4vyjx8 "WW2 Map"**: Bring geojson into AE · Animate the Range Circles · Animate the Map View.
- **WGpxJzjDjvU "How to Map Disputed Areas"**: Natural Earth de facto / de jure boundaries.
- **GOc7np11Tio / 0Www84yQAdo "Animate the Frontline of a War"**: Download Map Features · Create Korean Peninsula Polygon · Draw out Country Shape Layers · Connect the Matte to the Map.
- **1ayTEAQfbiE "NASA Solar Eclipse"**: KML data → animate centre line, path outline, umbra.
- **ez-nrD4VIPQ flags**, **K8dL8VdEUpc clouds**, **mapfwee2cL4 icons on path (NATOkit font)**, **7quJARsob9s Composition Border**, **K0-DhsMP4cA cinematic breakdown**, **o7MBcdHWgL4 U-boats over time (time series)**, **2yM6Oc-mIQ8 Kings and Generals basemap**, **FD6LcXbFes8 CIA repeating-line borders (line patterns)**, **Ah5JDAG1Heo Mapping Paris arrondissements/quartiers (2020)**.

---

## 5. Other third-party channels (descriptions only)
[SOURCED: youtube.com/watch?v=<id>]
- **Thong "GeoLayers 3 Complete Interface Tour"** rJ9asneXYrE (10:19, 2026): explores "the **Map Comp Bar**, **Toolbar**, **Search Bar**, **Map Preview**, **Feature Browser**, **Layer Styles**, **Feature Collections**". Thong "What Is GeoLayers 3 and Project Setup & Preferences" UxP_B6Lco00 (2:18): "interface, project settings, and essential preferences".
- **MapMagic Tutorials "Exploring the buttons"** kzUK7jjBIlg (6:26, 2024): "Finalize the map; Move around the map and zoom; Changing the pitch and the bearing; Change control map imagery; **Show Mapcom view button**; **Link and Unlink control map with preview map**; **Zoom level limitation**; **Mercator projection explanation**; How to Change Mapcom". MapMagic "How to use labels in Geolayers3?" 6_uGtZQ_wuM: use labels, make your own label, change existing labels.
- **QuickSkills "Pin Selected Layers vs. Parent to World Map"** Dm_1hGYNExQ: difference between parenting to the World Map comp (layer stays flat) and **"Pin Selected Layers to Map"** (rotates/zooms with the map, part of 3D environment). QuickSkills "5 Tips & Tricks…" PPFpNW8dYFc: "split features, remove messy attributions, and use the '**Subtract**' tool to create custom territories". QuickSkills 3D path (8W_PP8bRQpQ), missile attack (H4D2plm3a6U), flags (m9qivtpM6HU), historical maps (7BF1ElxVMaU), ChatGPT historical map (NjQwUgKQkk8).
- **Explainer VFX** 9TYySccpjdA: "**linked map comps** in geolayers 3".
- **Razker Studio** "How To Export GeoLayer 3 Project Correctly" 21RUQrIzYyU; "Data Driven Styles with AI" gY15luTCN18 (Gemini-generated population + coordinates → data-driven colours and scales).
- **Digi Tech Team** "Custom Labels" HRld3wwV-30: "custom pre-comps for labels", "Tracking labels to specific map coordinates".
- **MapMotion Studio**: Add Terrain (tFZ6y7R0E7Q), Vintage Maps (V_1NncQGAfU), "Unlock Unlimited GeoLayers Map Styles" (ns-pyGCcJt4 — Mapbox Studio custom styles via tile URL).
- **CREATE NEW** "Invalid MapTiler API Key error — official solution" N0Lqfk0iJ_M (error string "Invalid MapTiler API Key").
- **Artexora Solutions** GeoGlobeTales breakdown TjBKV6cKj9w (1h42): GeoJSON import, parenting objects with maps, GEOlayers Arrow Animation.
- **Flat Pack FX** (WqJa957IHXM 3D map routes; cKZzpTI5agI super detailed 3D map; sells a "GEOlayers mini course").
- Full courses (non-English, chapters not given): EZEdit Hindi 2h04 (BKo_6nQJ_fQ), Ritesh Goswami Hindi 1h28 (28EMvIj_e74), Bangla playlist PLLSiTLVFwO41kYZ2WmivKRSA8MvSW2NW5.
- School of Motion, Ukramedia, Sonduck, Jake In Motion: **no GEOlayers-specific tutorials found** on YouTube search (only generic AE content / School of Motion "Motion Mondays" news).
- GEOlayers.app (browser app, separate product by same vendor) has its own tutorials on the official channel (Viewport Animation, Timeline items, Change Project Colors, Draw shapes, Enterprise Workflows, Trim & Animate Lines, Cut Holes into Country Shapes, 3D Terrain in One Setting, Satellite Texture) — **out of scope for GEOlayers 3** but naming may overlap.

---

## 6. Consolidated feature list derived from videos (cross-checked with changelog.md)

See structured output. Key additions the changelog/help do NOT spell out:
1. Exact Mapcomp Settings fields (Name, Duration, Language, Max Zoom, Min Zoom, Attribution position+toggle, Imagery grid, Quality %, Blend Duration s, Imagery Motion Blur).
2. Exact custom raster style fields (Name, URI, "..." picker, xyz/tms, Min/Max Zoom, Tile Size, Bounds, Hue, Opacity, Black, save/import/export icons). Default custom raster = "Universal Raster" with OSM tile URL, max zoom 16, tile size 512, Mercator bounds.
3. Default Label Template set names (01 Place Top … 11 Locator 2, Capital Cities…).
4. Label Template ↔ Mapcomp assignment matrix with Select all/none.
5. "Data not licensed" MapTiler gate dialog (FLEX plan, paste API key, restart GEOlayers).
6. Search results grouped by source with per-result actions (zoom-to, label, draw, add-to-browser) and geometry/collection icons; collection count shown "(3)".
7. Feature browser default collections "Favorite Features" + "Features"; geometry-type filter toggles.
8. AE-side naming conventions: "containing <name>" comp, "GEOlayers 3 Items" folder, "<Mapcomp> Anchor", "Label '<name>'", route "<A> - <B> (<dist> mi)".
9. Image profiles: "Universal Raster", "Seamless Texture" profile, "Watermask" Mapcomp, "Choropleth Mapcomp" (connect a GeoJSON), Texture Mapcomp.
10. Mapcomp View features (feature that stores/visualizes a Mapcomp's view; "Create a Mapcomp View").

## 7. Open questions (need transcripts or hands-on observation)
- Exact meaning/tooltips of each toolbar icon (7 left + 4 right icons) — "Every Button Explained" (DX_qi37vy0Y) and "A Tour of the Interface" lecture would answer; transcript not reachable.
- Full list of imagery styles in the grid (names) — thumbnails only.
- Remaining sliders below "Black" in the imagery editor (likely Saturation/Brightness/Contrast/White?) — unknown.
- What the crossed-tools icon in Mapcomp Settings > Imagery does.
- Attribution dropdown options besides "bottom left"; meaning of its checkbox.
- Whether "Preview" pill in Mapcomp list toggles preview quality vs indicates unfinalized state.
- Distance units: "mi" seen in route name — is it a preference (metric/imperial)?
- Controls exposed on the Mapcomp layer / anchor (effect names) beyond Latitude/Longitude/Zoom/Bearing/Pitch and "Position Progress".
- 3D Scene script options; Distance Circle "Parameters"; Arrow tool options.
