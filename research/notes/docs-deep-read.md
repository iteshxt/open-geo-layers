# GEOlayers 3 — deep read of LOCAL sources (clean-room feature decomposition)

Scope: only the local files below were read. Nothing from the product itself (ZXP, installer, scriptingApiExamples.zip) was downloaded or inspected.

Sources (abbreviations used throughout):
- **HELP** = `/home/iteshxt/Documents/1.Projects/adobe-extension/research/sources/help-readme.md` (official public help; image links point to GitHub user-images, so the original is a public GitHub README; exact repo URL not recorded in the file → unknown)
- **CL** = `/home/iteshxt/Documents/1.Projects/adobe-extension/research/sources/changelog.md` (captured from https://aescripts.com/geolayers/ "Version History → Show All", per its header). Cited as `CL vX.Y.Z`.
- **EX:<name>** = the example scripts `geodata-repo__*.jsx.txt` / `development-repo__*.jsx.txt` in the same folder.

Tags: **[S]** = SOURCED (stated in the cited source). **[I]** = INFERRED (my reasoning; must be verified before relying on it). "unknown" = not stated anywhere in local sources — do not invent.

---

## 0. Things the local sources do NOT tell us (gaps to fill from other research)
- The list of buttons that appear when hovering a Mapcomp in the Mapcomp list — HELP L27 ends with "the following buttons appear:" and the list is missing (the original used icons that were lost in the text capture). **unknown**
- Icon names in the main window ("click the little arrow top left", "clicking  top left" — an icon was stripped in HELP L25). **unknown**
- Exact signature/options of `geolayers3.snapLayerToMapcomp(...)` (CL v1.20.0). **unknown**
- Exact full lists of: imagery styles, color presets, label templates shipped, shape layer styles shipped, sample projects, toolbar buttons, preference keys. Only partial lists derivable (below).
- Units of the `duration` draw option (default 10) — seconds is likely [I] but unknown.
- File formats beyond those explicitly named. "Almost every file can be imported" (CL 1.4.2) without a list.
- FreeForm Pro / Plexus / Mir 3D-landscape support: stated in the task context, **not in local sources** (only Helium appears, CL 1.5.0).

---

## 1. Core concept: Mapcomps
| # | Fact | Tag / source |
|---|---|---|
|1.1| Mapcomp = Composition filled with map imagery and/or geographical labels; the basic building block of each project. | [S] HELP L10-11 |
|1.2| Create via a new project from the **projects window**, or the little arrow top-left in the main window → **"+ Add Mapcomp"**. | [S] HELP L13 |
|1.3| Creation dialog step 1 "Composition options and location": Name, pixel size, duration, framerate — **prefilled from the currently selected composition**. Navigate the **preview map** to the location; **search bar** to jump to a place; button **Next**. | [S] HELP L17-18 |
|1.4| Step 2 "Map Style": imagery **presets** below the preview map, **color presets**, or **change individual colors**; button **Create**. More detailed styling after creation. | [S] HELP L19-20 |
|1.5| Mapcomp is created **inside the currently selected composition** (that comp becomes the "containing comp"). | [S] HELP L21 |
|1.6| **Anchor layer**: helper layer above the Mapcomp layer in the containing comp; matches the Mapcomp's view animation; used for parenting. | [S] HELP L21; API `getMapcompAnchor` HELP L90-94 |
|1.7| Sample projects as a start point; **projects panel** opened from the context- or flyout menu of the GEOlayers panel. | [S] HELP L23 |
|1.8| **Mapcomp list**: shows all Mapcomps in an opened project, opened from top-left of main window; hover reveals per-Mapcomp buttons (list unknown). | [S] HELP L25-27 |
|1.9| View controls of a Mapcomp: **Latitude, Longitude, Zoom, Bearing, Pitch** (keyframeable). | [S] HELP L64, L82 |
|1.10| Mapcomp Settings: **Max Zoom** (CL 1.1.2); min zoom exists (CL 1.4.0 "Mapcomps with min/max zoom", "Empty previews for Mapcomps with min zoom"); **Motion Blur** (CL 1.0.7); **Duration** — also changes related Mapcomps' durations (CL 1.2.4). | [S] |
|1.11| **Mapcomp sync** (preview map ↔ selected Mapcomp sync, can be turned off). | [S] name in CL 1.4.0 bugfix "mapcomp sync is off"; behaviour [I] |
|1.12| **Linked Mapcomps** exist (e.g. duplicates / related Mapcomps); list can **collapse linked Mapcomps** (CL 1.3.0); removing one with others linked asks to remove all linked (CL 1.5.0); duration change propagates to related (CL 1.2.4). | [S] |
|1.13| **Confirm dialog** before removing a Mapcomp (CL 1.2.6). | [S] |
|1.14| Automatic Mapcomp naming with leading zeros: "Mapcomp 01, Mapcomp 02…" (CL 1.3.0). | [S] |
|1.15| Per-Mapcomp **language** setting (labels/geocoding): geocode() "By default the selected Mapcomp's setting will be used". | [S] HELP L149 |
|1.16| Square/portrait maps can be **pitched more** (pitch limit depends on aspect) (CL 1.0.4). | [S]; formula unknown |
|1.17| **MapPivot layer** — a layer carrying expressions (CL 1.1.8 bugfix "empty expressions on MapPivot layer"). Likely the pivot/null used to drive bearing/pitch rotation inside the Mapcomp. | name [S]; purpose [I] |
|1.18| Overzoom: Mapcomps zoomed below imagery minzoom → handled (CL 1.2.4); raster above max zoom uses **overzoomed textures** (CL 1.1.2). | [S] |
|1.19| Expressions link Mapcomp, anchor, labels, pinned layers; legacy expression engine must be supported (CL 1.2.0 bugfix); AE pref "Default Position Properties to Separate Dimensions" must be handled (CL 1.5.0). | [S] |
|1.20| Troubleshooting for projects corrupted by AE "Recovering" Mapcomps (CL 1.14.0). | [S] |
|1.21| **Provider Attribution** (a text element/layer with paragraph alignment, RTL-aware) (CL 1.6.1); attribution required per providers' terms (HELP L401). | [S] |

## 2. Finalize
- Finalize = renders final imagery for Mapcomps. API `finalize(comps, callback[, options])`, options booleans **onlyCurrentFrame, previewQuality, onlyWorkArea, purgeImageryCache** [S] HELP L42-49. These are almost certainly the same toggles as in the UI finalize dialog [I].
- **Directly queue Renders after finalization** (add to AE Render Queue) [S] CL 1.2.6.
- DEM tile requests without valid response must not stall finalization [S] CL 1.2.5; "finalization stalls" bugfix CL 1.0.1.
- Imagery cache exists (purge option) [S]; tiles rendered with **web workers** (memory leak fix) [S] CL 1.1.8; **Tilemerger** merges raster tiles, supports **TMS** servers (y-flipped) [S] CL 1.0.9; invalid urls at longitude < -180 fixed [S] CL 1.0.4 (→ tile x must be wrapped modulo 2^z [I]).
- "Rejected server requests can lead to black holes in tiles" fixed → need retry/fallback per tile [S] CL 1.4.0.
- Imagery texture filtering (blurriness) [S] CL 1.1.2.
- **Default Imagery Quality** preference [S] CL 1.15.1 (values unknown).

## 3. Imagery styles (Mapcomp imagery)
| Style / item | Tag / source |
|---|---|
| **Basic** (MapTiler vector) — water visibility separate for oceans, lakes, rivers (CL 1.1.8); Disputed Borders toggle (CL 1.7.0) | [S] |
| **Advanced** (MapTiler vector, new in 1.0.4) — water toggles (1.1.8); extra **rail** color (1.1.6); **street density** setting (value 5 exists, CL 1.2.6) ; Disputed Borders toggle (1.7.0) | [S]; street density range unknown (≥5) |
| **Lite** (MapTiler) — free for non-commercial projects (CL 1.4.0); Disputed Borders toggle (1.7.0) | [S] |
| **MapTiler Satellite** (V2 since 1.5.1; blurry fix 1.2.5) | [S] |
| Stylized imagery styles are those "with the MapTiler icon" | [S] HELP L390 |
| **Bing** imagery (aerial implied [I]); **Dark, Light, Gray** variants + **language support** (CL 1.1.4) | [S] |
| **CartoDB** imagery; supports entering an **API key** (CL 1.19.0) | [S] |
| **Stamen** — removed 1.6.4 (moved to Stadia Maps) | [S] |
| **Natural Earth Imagery**, **Natural Earth Water Mask**, **Natural Earth Mapcomp Labels** (CL 1.1.9); **Natural Earth Admin Boundaries Imagery** (CL 1.2.0); Inner Boundaries data fix (1.7.2); data update (1.6.6) | [S] |
| **Choropleth** Mapcomp imagery style for large datasets (CL 1.1.8) | [S] |
| **Circles** imagery for DataViz (CL 1.2.2) | [S] |
| **Lines** imagery style (Dataviz, CL 1.20.0) | [S] |
| **Universal Raster Style** with "Default Tiles" (CL 1.6.6) | [S] |
| **Custom Imagery Styles** — multiple can be imported/exported in preferences (CL 1.2.2); tile URI templates support **{year}, {month}, {date}, {dateISO}** (CL 1.4.0) plus standard {x}{y}{z} [I]; **Map Bounds** for raster styles (1.4.0) | [S] |
| **Local tile directories** picked in Imagery setup (CL 1.1.8); min/max zoom read from **metadata.json** (CL 1.5.2) | [S] |
| **Watermask Mapcomps** from the toolbar (MapTiler) | [S] HELP L393 |
| **Mapcomp labels** (labels baked into imagery) in different languages (MapTiler) / Natural Earth labels; dots (1.5.1); RTL (1.13.1); label renaming (1.2.1) | [S] |
| Elevation / DEM tiles (CL 1.2.5) — used for 3D landscapes heightmap [I] | [S] DEM exists |
| Raster vs vector distinction: "Raster Styles" (CL 1.1.2, 1.4.0) | [S] |
| Preview map displayed in default language (CL 1.1.8) | [S] |
| Globe and **Equal Earth** map projection via Plugin (prerelease build 984, CL 1.20.0) | [S] (mechanism unknown) |

## 4. Feature Browser (vector data panel)
- Browser holds features & **Feature Collections**, **favorites** (clearBrowser keeps favorites) [S] HELP L208-209.
- Add sources: file, geocoding request, Overpass query, GeoJSON object, OSM id, geojson url [S] HELP L172-198.
- **Import File** button; **CTRL/CMD-click Import** → import from a remote URL [S] CL 1.2.2, 1.4.2 ("Almost every file").
- **Remove** button; **SHIFT-click Remove** → clear whole browser [S] CL 1.3.0.
- **Export** button; **SHIFT-click Export** → all features except favorites written to `features.geojson` beside the project [S] CL 1.3.0.
- **Filter** (fast, shows only necessary collections) [S] CL 1.0.9. **Sort by Name** inside collections [S] CL 1.2.2.
- **Click preview map to find features** (MapTiler/GEOlayers Mapdata) [S] CL 1.0.4, HELP L391; **click preview map to select features in the Browser** [S] CL 1.1.0; click selects line between two vertices [S] CL 1.1.5.
- **Find Features at Coordinate** incl. **Inner Boundaries** feature per country [S] CL 1.2.0.
- **Current View features** (feature import for current view, MapTiler) [S] HELP L394; Current View **Transport** features with separate ramps [S] CL 1.3.0; fails if view can't be geocoded → fixed [S] CL 1.2.3.
- **Admin Bounds** for higher zooms [S] CL 1.17.0.
- **Routes**: Car, Bike, Pedestrian routes; imported **without Maneuver Points** by default (pref) [S] CL 1.4.0, 1.0.4.
- Geometry tools (non-blocking, cancelable): **Merge**, **Subtract**, **Create Lines from Polygons**, **Simplify Feature Topologies** [S] CL 1.2.1; **Merge Features in Collections by name** [S] CL 1.1.8; **Line Mesh from Polygons with common borders** [S] CL 1.0.7; **Feature Mesh** (one feature + collection → one-to-all lines; otherwise all-to-all) [S] CL 1.7.1; **Points-inside-Polygons** tools [S] CL 1.0.9.
- **Import features from Shape Layers** (polygon, line, point) [S] CL 1.3.0.
- **Feature Properties panel**: drag & center label position (CL 1.1.3); open http links (CL 1.0.9); duration of timed data (CL 1.1.4); **collective** feature properties (CL 0.9.0); Collection properties **export/import CSV/TSV** (CL 1.1.5); one-click **data-driven styles** from a collection's properties panel (CL 1.0.9) [S].
- **Update url-sourced Collections** with one click [S] CL 1.4.2.
- **Default Features** loaded on extension start (prefs), can be http/https [S] CL 1.1.7, 1.2.2.
- GeoJSONs next to the project auto-load when opened via projects panel [S] CL 1.0.9.
- Global **Lakes / Rivers** datasets downloadable (CL 1.0.6); **Natural Earth Physical Areas Land & Water** (CL 1.1.9); **Natural Earth States and Provinces Lines** (CL 1.2.0) [S].
- OSM by id (node/way/rel) [S] HELP L190-193, CL 1.1.7, 1.17.0.
- Large OSM downloads must not freeze panel; Nominatim results without geometry fall back to point [S] CL 1.0.6.
- Features with empty coordinates arrays handled (CL 1.5.3); multipoint geometries (CL 1.2.2) [S].
- **coordinate_elevation** property added on import of 3D line features [S] CL 1.11.1.
- Feature cache with configurable paths (hidden pref) [S] CL 1.0.7.
- Automatic label point: centered inside **largest polygon** [S] CL 1.2.0.

## 5. Feature Search
- Search bar (Nominatim/OSM by default) [S] HELP L18, L151; MapTiler Search [S] CL 1.5.3+.
- **Multiple sources** for Feature Search [S] CL 1.2.2: **Natural Earth Search API, Browser Search API, Browser Downloads Search API, File Search API (http urls; zipped shapefiles CL 1.5.0), Folder Search API, GitHub Search API (public repos)**.
- **Fallback online search service** if default fails; settable as default in prefs [S] CL 1.0.2.
- **Preload features for search on startup** [S] CL 1.2.2.
- **Feature preview** in search; can show **bounding boxes** (pref) [S] CL 1.2.2.
- Download a search result [S] CL 1.6.3.
- MapTiler Search loads full geometry from MapTiler internal dataset [S] CL 1.13.1; supports **worldviews** (pref) [S] CL 1.18.0/1.18.1.

## 6. Drawing (features → shape layers)
Options (UI prefs saved in .aep CL 1.1.8; API defaults HELP L234-251):
| Option | Default | Meaning |
|---|---|---|
| shapeLayerStyle | — | index number, name or style object [S] |
| drawInsideMapcomp | false | draw in Mapcomp instead of containing comp [I from name] |
| autoStrokeWidth | false | stroke width compensated for zoom [S name CL 1.1.7/1.1.8; behaviour I] |
| simplify | true | simplify geometry [S] |
| simplyfyByZoomRange (sic) | false | "Simplify Geometry by max Zoom" CL 1.2.0 [I link] |
| simplificationZoomOffset | 0 | offset applied to zoom used for simplification [I] |
| useTopoSimplificationIfPossible | false | topology-preserving simplification CL 1.2.0 [I link] |
| individualLayers | false | each feature on its own layer [S CL 1.1.4/1.1.8] |
| autoChangeRenderer | false | switch to CINEMA 4D renderer for extrusions [I from CL 1.5.2] |
| addTrimPaths | false | add Trim Paths (draw-on) [I] |
| duration | 10 | unknown units (likely seconds) [I] |
| dateBorderCopy | false | world copies across date line [S CL 1.2.0/1.15.2] |
| dashThreshold | 1 | "Dash Zoom Threshold" CL 1.1.7 [I link] |
| absoluteZScale | false | extrusion independent from zoom CL 1.0.5 [I link] |
| castsShadows / acceptsShadows | false | AE 3D material options [I] |
| lockLayerTransforms | false | pref "lock transform properties" CL 1.2.6/1.15.2 [S] |
- Features drawn with **time offsets** (data-driven) [S] CL 1.2.3.
- Draw **Mapcomp View** script [S] CL 1.1.6.
- Extruded features → CINEMA 4D renderer [S] CL 1.5.2; point features with C4D renderer [S] CL 1.1.2.
- **Create Arrows** from the toolbar [S] CL 1.1.7.
- **3D buildings** from toolbar [S] HELP L395.
- Multi-polygon content structure (faster) [S] CL 1.2.5.
- Line features from CSV can cross the date border [S] CL 1.2.5.

## 7. Shape Layer Styles
- Strokes with **outlines**; stroke **join defaults to round** (CL 1.0.7); **dashed** strokes w/ **round caps** (1.2.5); **Dash Speed** (1.2.0); **Preserve Transparency** (1.2.0); **layer blending modes** (1.1.2); **extrusion** (1.0.7) [S].
- **Pick** style from layers or shape content groups (CL 1.0.9); **swap** style on drawn layers (1.0.1, 1.0.7, 1.1.4) [S].
- **Thumbnails** show stroke width relative to min/max of the displayed set [S] CL 1.0.3.
- **Filter by name** (1.0.4), **by Mapcomp** via right-click on filter bar (1.1.6), filter as fullscreen panel (1.2.6) [S].
- **Data-driven styles**: one-click creation (1.0.9); color previewed with gradient (1.2.1); interpolation curve **'Circle Area'** for points (1.0.7); **animated** (1.0.8); keyframes with non-linear timing (1.2.4); **Property Diagrams** (1.2.6) [S].
- Styles based on a URL source can be updated with one click [S] CL 1.4.2.
- **Randomize color** [S] CL 1.0.4 bugfix.

## 8. Labels
### 8a Label Templates
- Label Template = a composition used as template; template requires string fields like "name" [S] HELP L104-116.
- Kinds include **Icon** and **Locator** templates [S] CL 1.0.6; default templates have **pointers** following the label anchor point [S] CL 1.4.1.
- **LabelColors comp** per template — revealed by SHIFT-select [S] CL 1.0.6; ALT-select reveals template in AE project [S] CL 1.0.6/0.9.0.
- **"Label Anchor Point" layer** in template with effect controls to default to **Scale with Map** / **Rotate with Map** [S] CL 1.0.4.
- Label layers update **markers** from source comp (responsive timing) [S] CL 1.0.4.
- **Animated label text** [S] CL 1.0.8; **time offsets** (data-driven) [S] CL 1.2.3.
- **Swap** template on labels regardless of selected Mapcomp [S] CL 1.2.3; AVLayer assets swapped (1.14.1).
- Asks for **missing data** when a feature lacks template fields [S] CL 1.1.5.
- Button in **Mapcomp Label settings** to **create Label Templates** [S] CL 1.1.2.
- **Custom Label Template Project** (loadable) [S] CL 1.2.2; importProject(file, true) [S] HELP L36-40.
- Filter by name/Mapcomp (shared with styles) [S].
- Unique layer names per place [S] CL 1.5.4. RTL handling [S] CL 1.12.2.
### 8b Label renaming
- **Label Renaming** preference (dictionaries), applies to Mapcomp labels and Template labels; dictionaries loadable from files [S] CL 1.2.1, 1.2.5.
### 8c Languages
- Welsh (1.6.1), Irish Gaelic & Scottish Gaelic (1.5.0), Traditional Chinese (1.12.2), RTL languages (1.12.2/1.13.1); Natural Earth languages (1.1.3); Bing languages (1.1.4) [S].

## 9. Pinning / snapping
- **Pin** layers (incl. text layers, anchors) to map via **toolbar pin button**; click again to **unpin** [S] CL 1.1.3, 1.0.4, 1.0.1.
- Pinned layers get separate **Latitude, Longitude, Height** properties [S] CL 1.1.0.
- Pinned layers' animated position via **"Position Progress"** slider [S] CL 1.1.2.
- **"(pinned)"** suffix in layer name [S] CL 1.4.0.
- **rotate-with-map** option on pinned/snapped layers [S] CL 1.2.6, 1.18.0.
- API `snapLayerToMapcomp(...)` [S] CL 1.20.0; `getLayersGeoPosition(layer)` [S] HELP L119-124.

## 10. Animation
- **Animate View to Feature** (0.9.0) with keyframe easing (1.2.5) [S].
- Animate view **along** one or multiple selected features [S] CL 1.0.9/1.1.0.
- Layers animated along features (street-scale accuracy) [S] CL 1.1.0.
- Prompt to adjust **duration** of any GEOlayers-created animation [S] CL 1.2.3.
- API setViewAtTime / fitViewAtTime / removeMapcompControlKeys [S].
- addLabel with coordinate array → label animated along path [S] CL 1.19.0.

## 11. 3D
- **3D Landscapes**: heightmap with the AE **"Calculations"** effect (CL 1.5.2), **Helium** plugin support (1.5.0); work-area duration (1.0.1); AE 17.5 offsets (1.1.8) [S]. Other plugins (FreeForm Pro, Plexus, Mir) — task context only.
- **Create 3D Scene & Camera** script [S] CL 1.12.2.
- **Globe** sample using **Video Copilot Orb** [S] CL 1.0.4. **Elevation** sample [S] CL 1.1.2.
- Extrusions, CINEMA 4D renderer, shadows [S].

## 12. Toolbar & scripts
- Run scripts from toolbar [S] CL 1.0.4; **ALT/OPT-click** shows description; **CTRL/CMD-click** reveals file [S] CL 1.2.0.
- Default scripts (exact file names where given): `add_line_pattern.jsx` (CL 1.2.0; `add_line_patterns.jsx` in CL 1.4.0 → exact name uncertain), `blend_in_and_out.jsx`, `north_needle.jsx` (1.2.0), `scale_bar.jsx`, `shapes_to_arrows.jsx` (1.1.7), `blink.jsx`, `stagger_layers.jsx` (1.0.7), `precompose_feature_shape_layer.jsx` (1.4.0), "Draw Mapcomp View" (1.1.6), "Distance Circle" (1.14.0), "Create 3D Scene & Camera" (1.12.2) [S].
- `autorun.jsx` runs automatically when opening a project [S] CL 1.0.9.
- **Quick Start Buttons** [S] CL 1.2.0.

## 13. Preferences (known)
Default comp specs (size, duration, framerate) (1.1.5); default language (1.1.8); default features on start (1.1.7); route maneuver points (1.4.0); lock shape-layer transform properties (1.2.6); label renaming dictionaries (1.2.1/1.2.5); feature-preview bounding boxes (1.2.2); default search service / fallback (1.0.2); custom imagery styles import/export (1.2.2); GEOlayers Mapdata enable/disable (green icon) (1.0.7); MapTiler worldview (1.18.0); Default Imagery Quality (1.15.1). Hidden prefs: Feature Cache Paths (1.0.7), purge Undo Cache each host call (1.5.3). [all S]

## 14. Misc / licensing / infrastructure
- MapTiler Cloud integration (1.1.9); Flex plan required after trial for MapTiler features [S] HELP L396.
- Overpass endpoint chosen automatically by response time & available slots [S] CL 1.5.6.
- Nominatim requests queued per usage policy [S] HELP L151.
- Log files [S] CL 1.1.6. UI animations [S] CL 1.1.3. About window with build nr & prerelease indicator [S] CL 1.3.0; About & Licensing panel [S] 1.11.1; licensing via aescripts (aesp) [S] 1.6.1/1.15.1; auth errors (1.8.0, 1.16.0).
- Undo groups closed manually per host call [S] 1.5.3. Host method "initEngine" [S] 1.14.1. "#" in file paths [S] 1.14.1.
- AE support: v22 (1.3.0), 25 (1.10.0), v27 (1.18.0) [S].
- Panel = CEP with ExtendScript host ("host call", "host method") [S terminology] ; web workers in panel [S].

---

## 15. Scripting API — complete reference (HELP L29-381 + CL)
Global object: `geolayers3`. **The GEOlayers panel must be open** for any API call [S] HELP L30. Async methods use Node-style callbacks `function(error, data)`; error objects have `.message` [S EX scripts].

Argument type "comp" everywhere = **a Mapcomp name, a Mapcomp, or a comp that contains a Mapcomp** [S].

| Function | Args & options (defaults) | Returns | Since |
|---|---|---|---|
| `importProject(file[, isLabelTemplateProject])` | file: File → .aep/.aet; isLabelTemplateProject: Boolean (project only has Label Templates). Sorts into GEOlayers project structure, uniquifies comp names, updates expressions. | — (unknown) | 1.1.6 |
| `finalize(comps, callback[, options])` | comps: one/array of Mapcomp names, Mapcomps, containing comps, or **undefined = all Mapcomps**; callback(error, mapcomps) async after all done; options booleans: onlyCurrentFrame, previewQuality, onlyWorkArea, purgeImageryCache (defaults unknown, presumably false [I]) | async | — |
| `duplicate(comp[, options])` | options.newName (when duplicating a Mapcomp) / options.newContainingCompName (when duplicating a containing comp); updates related expressions | duplicate comp or undefined | — |
| `setViewAtTime(comp, view[, forceKeyframe[, time]])` | view {latitude, longitude, zoom, bearing, pitch} floats; forceKeyframe: keyframe even if not yet keyframed; time in **containing comp**, default current time | applied view or undefined | — |
| `fitViewAtTime(comp, bbox[, forceKeyframe[, time]])` | bbox = GeoJSON bbox [lonMin, latMin, lonMax, latMax] WGS84 | applied view or undefined | — |
| `removeMapcompControlKeys(comp)` | removes keys from Lat/Lon/Zoom/Bearing/Pitch | — | — |
| `getMapcomps()` | — | all Mapcomps | — |
| `getMapcompAnchor(mapcomp)` | — | anchor layer in containing comp | — |
| `getLabelTemplateComps()` | — | Label Template comps or [] | — |
| `addLabel(comp, labelTemplateComp, labelData)` | labelTemplateComp: comp, template name, or integer index; **null → adds a null layer** (1.20.0). labelData: required latitude/lat & longitude/lon floats + template string fields ("name"…); **coordinate array** to animate along paths (1.19.0; exact shape unknown) | label layer or undefined | — |
| `updateLabelData(labelLayer, newLabelData)` | string fields | label layer or undefined | — |
| `getLayersGeoPosition(layer)` | layer in a Mapcomp's containing comp | [longitude, latitude] or undefined | — |
| `snapLayerToMapcomp(...)` | **unknown** | unknown | 1.20.0 |
| `fetchText(url, callback[, initObject])` | callback(error, text); initObject = WHATWG fetch init | async | — |
| `fetchJson(url, callback[, initObject])` | callback(error, json) | async | — |
| `geocode(searchString, callback[, options])` | options.language ISO 639 string (default: selected Mapcomp's language); callback(error, features[]) GeoJSON; Nominatim, queued per usage policy | async | 1.0.1 |
| `watch(fileOrFolder, callback[, options])` | callback(file) or (changedFilesInFolder, allFilesInFolder); options.interval ms default **10000**; replaces existing watch; deferred while busy | — | 1.0.1 |
| `watchCsv(csvFile, callback[, options])` | .csv/.tsv/.txt; callback(changedRows, allRows); options.interval 10000, options.delimiter default "," | — | 1.0.1 |
| `unwatch()` | stops current watch (only one watch at a time [I from "canceled"]) | — | 1.0.1 |
| `addToBrowser(addObj[, callback[, options]])` | addObj: File, URL with extension, GeoJSON object, {type:"geocode",query}, {type:"overpass",query}, {type:"osm",id:"node1234/way4321/rel1243"}, {type:"geojsonurl",url}; options: returnData (false), pointGeometries, normalizeToFeatureArray, name, namingProp | async | 1.0.4 (osm 1.1.7, geojsonurl 1.0.9, naming 1.2.1) |
| `clearBrowser([callback])` | removes all but favorites | async | 1.0.4 |
| `removeBrowserSelection([callback])` | | async | 1.2.0 |
| `getBrowserSelection([callback[, options]])` | options.fullGeometry (false → point only) | async: features[] | 1.2.1 |
| `drawBrowserSelection(comp[, callback[, options]])` | drawing options table §6 | async | 1.2.0 |
| `draw(comp, addObj[, callback[, options]])` | addObj as addToBrowser; drawing options §6 | async | 1.2.0 |
| `filesInDir(folder[, filterRegEx[, includeSubDirectories]])` | doc says "Returns a string" but text says File objects — doc error; File[] is the meaning [I] | File[] | 1.1.6 |
| `readFile(file[, encoding])` | file: File or filename string; encoding default UTF8 | string | — |
| `readFileCsv(file[, encoding])` | .csv/.tsv | Array of rows (Array of columns) | — |
| `readFileJson(file[, encoding])` | .json | object | — |
| `writeFile(file, content[, encoding])` | | — | — |
| `userFolder()` | GEOlayers User Folder | Folder | — |
| `userFile(fileName)` | 'basename.extension' inside user folder | File | — |
| `modalEditObject(obj, callback[, options])` | flat obj of strings/numbers/booleans; callback(error, editedObj); options.title, options.message | async | 1.1.6 |
| `modalActions(actions, callback[, options])` | actions: string[] → buttons; callback(error, actionsIndex) — undefined on cancel; options.title, message | async | 1.1.6 |
| `githubBrowser([options])` | title, defaultUser, defaultRepo, fixedRepo, flatten, filterText, actionBtnText (default "Import to GEOlayers"), actionFilter(item) (default: geojson files), action(item, url) (default: import to Feature Browser) | — | 1.2.0 (non-'master' default branch fix 1.2.2) |

Documentation quirks to replicate as aliases [I]: option key `simplyfyByZoomRange` (typo) — accept both spellings; addLabel accepts `lat`/`lon` aliases.

## 16. What the example scripts reveal
- **Overpass**: queries in Overpass QL with `[out:json][timeout:N]` header and `out body;>;out skel qt;` (recurse down to fetch way nodes) [S EX:*].
- `{{bbox}}` placeholder → replaced with the **current view's** bbox (comments: "inside the current view", "You should zoom to city level") [S comments; substitution format (Overpass south,west,north,east) I].
- `{{geocodeArea:<name>}}` (Overpass-Turbo shortcut) used in EX:Autobahn_Input; EX:Autobahn_BAB replaces it with `area[name='Deutschland']->.searchArea` — suggests `{{geocodeArea}}` may not be (reliably) supported by GEOlayers [I]. Support status **unknown**.
- Queried tags: `building`, `building:part` (ways/relations), `amenity=charging_station` (nodes), `amenity=hospital` (nwr), `route=railway` relations, `highway=motorway` + `ref="A <n>"`, generic `amenity=<x>` with `nwr` [S].
- Timeouts used: 25, 45, 50, 60 s [S].
- **GeoJSON object import with data join**: EX:COVID scripts `fetchJson` an ArcGIS GeoJSON (`.../FeatureServer/2/query?outFields=*&where=1%3D1&f=geojson` and an opendata.arcgis.com `.geojson`), mutate `feature.properties` (computed fields `confirmed:latest`, `deaths:latest`, `deathsrate:latest`, set `name` from fallbacks), set collection-level `data.properties.source` (URL) and `data.properties.GEOlayersDisplayName` (display name in Browser), then `addToBrowser(data)` [S].
  - [I] `GEOlayersDisplayName` is a reserved FeatureCollection property that names the collection in the Browser; `source` likely drives "update from url" and/or is shown as http link in properties panel.
  - [I] `name` is the default feature-naming property (cf. option `namingProp`).
  - [I] `key:suffix` property naming (e.g. `confirmed:latest`) looks like the convention for **timed data** (`property:<date>`) consumed by animated data-driven styles/labels (CL 1.0.8, 1.1.4); `latest` used as a non-date key. Needs verification.
- `githubBrowser` used to browse `aourednik/historical-basemaps` with fixedRepo & flatten [S].
- ExtendScript `prompt()` used for input; panel API calls are fire-and-forget with optional callback [S].
- Vendor repos named "geodata" and "development" hold community/useful scripts [S filenames] — likely the source of GitHub Search API / toolbar script library [I].

## 17. Changelog line-by-line index (every line → section above)
1.20.0: Lines imagery (§3), snapLayerToMapcomp (§15), addLabel null (§15), Globe/Equal Earth (§3)
1.19.0: CartoDB API key (§3), addLabel coordinate array (§15)
1.18.1/1.18.0: MapTiler worldview (§5/§13), AE v27 (§14), snapped layers rotate-with-map (§9)
1.17.0: OSM id stability, admin bounds higher zoom (§4)
1.16.0: Auth errors (§14)
1.15.2: world copies + lock transforms (§6)
1.15.1: Default imagery quality (§2), license framework (§14)
1.14.1: AVLayer assets swap in label templates (§8a), initEngine (§14), "#" paths (§14)
1.14.0: Distance Circle (§12), corrupted project troubleshooting (§1)
1.13.1: MapTiler full geometry (§5), point-only bug (§5), RTL Mapcomp labels (§3)
1.12.2: Create 3D Scene & Camera (§11/§12), RTL Label Templates (§8), Trad. Chinese (§8c)
1.11.1: coordinate_elevation (§4), About & Licensing (§14), label swap crash (§8a)
1.10.0 AE25; 1.9.0 OSM search; 1.8.0 auth; 1.7.2 NE inner boundaries (§14/§5/§3)
1.7.1 Feature Mesh (§4); 1.7.0 Disputed borders (§3); 1.6.6 NE update + Universal Raster (§3); 1.6.4 Stamen removal (§3); 1.6.3 search result download (§5); 1.6.2 subtract/merge (§4); 1.6.1 Welsh, attribution RTL, aesp (§8c/§1/§14)
1.5.7 preview map blank; 1.5.6 Overpass auto service (§14); 1.5.5 MapTiler search (§5)
1.5.4 unique label names, NE States/Provinces polygons, File search w/o url (§8a/§4/§5)
1.5.3 MapTiler search geometry, empty coords, undo groups, hidden pref purge undo (§5/§4/§14/§13)
1.5.2 metadata.json, C4D renderer for extrusion, Calculations effect/project color space (§3/§6/§11)
1.5.1 MapTiler Satellite V2, Mapcomp label dots (§3)
1.5.0 Helium, remove linked, Gaelic, zipped shapefiles, separate dimensions (§11/§1/§8c/§5/§1)
1.4.2 remote import (CTRL/CMD), url styles update, url collections update (§4/§7)
1.4.1 pointers follow anchor (§8a)
1.4.0 MapTiler Lite free NC, "(pinned)", precompose script, route maneuver pref, map bounds raster, tile URI date vars, freezes/black holes/min-max zoom/add_line_patterns fixes
1.3.0 import from shape layers, collapse linked, leading zeros, SHIFT remove, SHIFT export, transport ramps, AE22, About build nr
1.2.6 Property Diagrams, queue renders, confirm remove, lock transforms, fullscreen filter, street density 5, pinned rotation
1.2.5 round caps, multipolygon structure, csv lines across date border, renaming dictionaries from files, satellite blur, easing, DEM stall
1.2.4 Mapcomp duration in settings, .tcx, non-linear DD keyframes, zoom interpolation, low-zoom loading
1.2.3 duration prompt, draw time offsets, label time offsets, swap labels any Mapcomp, current view geocode
1.2.2 multiple search sources (6 APIs), preload, preview bbox, CTRL/CMD import url, Circles imagery, sort by name, http default features, custom imagery im/export, ";" CSV, multipoint, etc.
1.2.1 DD gradient preview, async geometry tools, addToBrowser naming, getBrowserSelection, renaming for template labels
1.2.0 Quick Start, simplify by max zoom, dash speed, preserve transparency, date line copies, NE admin bounds imagery, NE states/provinces lines, inner boundaries, NE search, label point in largest polygon, topo simplification, ALT/CTRL toolbar, 3 default scripts, 4 API fns
1.1.9 MapTiler Cloud, NE imagery/watermask/labels, NE physical areas
1.1.8 preview default language, drawing options saved in aep, water toggles, Choropleth, merge by name, local tile dirs, GeoJSON sequence, MapPivot, web workers
1.1.7 Create Arrows, Dash Zoom Threshold, default features, scale_bar, shapes_to_arrows, addToBrowser osm
1.1.6 filter by Mapcomp (right-click), Draw Mapcomp View, rail color, 4 API fns, log files
1.1.5 default comp specs, CSV/TSV property import/export, ask missing data, click line between vertices
1.1.4 Bing Dark/Light/Gray + language, timed data duration, individual layers, blending on swap, trimmed linestring label position
1.1.3 drag/center label pos, unpin, NE languages, UI animations
1.1.2 blending modes, Position Progress, Max Zoom, create Label Templates button, Elevation sample, C4D points, date border connect, texture filtering, overzoom
1.1.1 init/swap fixes; 1.1.0 click-select in browser, pinned Lat/Lon/Height, animate along feature
1.0.9 pick styles, points-in-polygons, fast filter, one-click DD, http links, animate along multiple, geojson url, autorun.jsx, Tilemerger TMS, auto-load GeoJSONs
1.0.8 Coronavirus sample, animated DD styles, animated label text
1.0.7 stroke outlines/round join, Motion Blur, blink/stagger, line mesh, Circle Area, cache paths, Mapdata toggle, extrusion anchor
1.0.6 Icon & Locator templates, SHIFT/ALT select, lakes/rivers, Nominatim point fallback, large OSM
1.0.5 3D building extrusion zoom-independent
1.0.4 Globe/Orb sample, click-map find, Advanced style, filter by name, markers, Label Anchor Point, toolbar scripts, more pitch, addToBrowser/clearBrowser, route/pin/randomize/tilemerger fixes
1.0.3 sample download button, thumbnail stroke widths, CSV name trim
1.0.2 fallback search; 1.0.1 geocode/watch/watchCsv/unwatch + fixes; 1.0.0 release; 0.9.x animate view to feature, ALT reveal template, collective properties
