# Completeness Critic A — feature-inventory.md vs changelog.md + help-readme.md

Date: 2026-11-19. Method: read `research/feature-inventory.md` in full (2642 lines), then walked
`sources/changelog.md` line by line (1.20.0 → 0.9.0) and `sources/help-readme.md` section by section,
plus the 11 vendor example scripts, and checked each item against the inventory (text + grep).
Clean-room: nothing from the product itself was downloaded or inspected.

Overall verdict: the inventory is very close to complete. Almost every changelog line and every
HELP API function/argument/option/default maps to an inventory ID. The gaps below are small and
mostly come from bugfix lines and from argument details of the file-I/O API functions.

## A. Changelog walk — coverage table (line → inventory ID)

| CL line | Item | Inventory |
|---|---|---|
| L5 | Dataviz Lines Imagery Style | DAT-18, IMG-30 |
| L6 | snapLayerToMapcomp | API-16, PIN-08 |
| L7 | addLabel null → null layer | API-12, UI-18, LBL-04 |
| L8 | Globe / Equal Earth via Plugin, prerelease build 984 | 3D-10, IMG-32 |
| L11 | CartoDB API key | IMG-13, PRF-15 |
| L12 | addLabel coordinate array | API-12, LBL-24 |
| L15 | MapTiler worldview wrong countries | SRC-06 |
| L18-20 | AE v27, worldview pref, snapped-layer rotation | LIC-12, PRF-13, PIN-05/08 |
| L23-24 | OSM id stability, Admin Bounds higher zooms | FB-15, FB-13 |
| L27 | Auth error handling | LIC-04 |
| L30 | world copies + lock transforms | DRW-14 |
| L33-34 | Default Imagery Quality, License framework | PRF-12, LIC-01 |
| L37-39 | AVLayer swap, initEngine, "#" in path | LBL-12, API-42, LIC-11 |
| L42-43 | Distance Circle, Recovering troubleshooting | TB-11, MCS-26 |
| L46-48 | MapTiler full geometry, point-only bug, RTL Mapcomp labels | SRC-06, MLB-05 |
| L51-53 | 3D Scene & Camera, RTL templates, Traditional Chinese | 3D-12/TB-13, LBL-23, MLB-03 |
| L56-58 | coordinate_elevation, About & Licensing, swap crash | FB-39, LIC-03, LBL-12 |
| L60-62 | AE25, OSM search results, auth | LIC-12, SRC-05, LIC-04 |
| L63 | NE Inner Boundaries messed up | **mis-mapped to IMG-16** (see W-1) |
| L66 | Feature Mesh one-to-all | FB-26 |
| L69 | Disputed borders Basic/Advanced/Lite | IMG-11 |
| L72-73 | NE data update, Universal Raster default tiles | IMG-15, IMG-19 |
| L76 | Stamen removed | IMG-14 |
| L78-79 | download search result, subtract/merge | SRC-10, FB-20/21 |
| L82-84 | Welsh, attribution RTL, aesp | MLB-03, MCS-14, LIC-01 |
| L86 | Preview Map blank (bugfix) | UI-11 (feature present; not cited) |
| L87 | Overpass server auto-selection | FB-16 |
| L88 | MapTiler results not displayed | SRC-06 (feature present) |
| L91-93 | unique label names, NE States lines-only, File source w/o url | LBL-19, FB-14, SRC-07 |
| L96-99 | MapTiler point-only, empty coords, undo groups, hidden undo pref | SRC-06, FB-38, LIC-10, PRF-18 |
| L102-104 | metadata.json, C4D renderer, color space | IMG-23, DRW-11, 3D-05 |
| L107-108 | Satellite V2, Mapcomp label dots | IMG-06, MLB-04 |
| L111-115 | Helium, remove linked, Irish/Scottish Gaelic, zipped shp, separate dims | 3D-06, UI-07, MLB-03, SRC-07, MCS-24 |
| L118-120 | CTRL/CMD remote import, url styles, url collections | FB-03, IMG-27/SLS-14, FB-19 |
| L122 | pointers follow anchor | LBL-03 |
| L125-135 | Lite free, (pinned), precompose, maneuver pts, bounds, date vars, bugfixes | IMG-05, PIN-02, TB-10, RTE-01/PRF-07, IMG-22, IMG-20, FB-38, FIN-11, UI-13, MCS-27, TB-03 |
| L138-145 | import from shape layers, collapse linked, leading zeros, SHIFT remove/export, transport ramps, AE22, About build | FB-30, UI-10, MCS-08, FB-04/05, FB-12, LIC-12, LIC-02 |
| L148-155 | Property Diagrams, queue renders, confirm remove, lock transforms, fullscreen filter, filter load bug, street density 5, pinned rotation | DAT-14, FIN-06, UI-07, PRF-08, SLS-12, PRF-19, IMG-04, PIN-05 |
| L158-164 | round caps, multipolygon, CSV lines date border, renaming files, satellite blur, easing, DEM stall | SLS-04, DRW-23, DRW-25, PRF-09, FIN-12, VIEW-05, FIN-13 |
| L167-171 | duration in settings, .tcx, non-linear DDS keys, zoom interpolation, minzoom | MCS-11, DAT-01, DAT-11, VIEW-13, MCS-27 |
| L174-178 | duration prompt, draw/label time offsets, swap any Mapcomp, current-view geocode | VIEW-11, DRW-20, LBL-11, LBL-12, FB-12 |
| L181-192 | multi-source search + all 6 search APIs, preload, preview/bbox, CTRL import, Circles, sort by name, http default features, im/export imagery styles, ";" CSV, multipoint / Mapcomp View zooms / custom template project / non-master repos | SRC-07, SRC-09, SRC-04/PRF-05, FB-03, DAT-16, FB-07, PRF-03, PRF-10, DAT-01, FB-38, TB-12, LBL-16, API-32 |
| L195-199 | gradient, non-blocking ops, naming options, getBrowserSelection, renaming templates, preview update | DAT-08, FB-20/21/24/28, API-23, API-27, LBL-20, MCS-04 |
| L202-216 | Quick Start, simplify by max zoom, dash speed, preserve transparency, date-line copies, NE admin imagery, NE states lines, inner boundaries, **improved NE search by name**, label point, topo simplification, ALT/CTRL scripts, 3 default scripts, 4 API fns, legacy dashes | all mapped except **"Improved Natural Earth data search by name"** (M-2) |
| L219-221 | MapTiler Cloud, NE imagery/watermask/labels, NE physical areas | IMG-07, IMG-15/17, MLB-02, FB-14 |
| L224-231 | preview language, drawing options in .aep, water visibility, choropleth, merge by name, local tiles, GeoJSON sequence, bugfixes | UI-14, DRW-02, IMG-03/04, DAT-15, FB-22, IMG-23, DAT-02; **3D Landscape heightmap/label offset only cited, not described** (M-4) |
| L234-238 | arrows, dash zoom threshold, default features, scale_bar/shapes_to_arrows, OSM id | RTE-06, DRW-15, FB-17, TB-06/07, API-24 |
| L241-245 | filter by Mapcomp, Draw Mapcomp View, rail color, 4 API fns, log files | SLS-12/LBL-18, TB-12, IMG-04, API-30/31/33/04, LIC-09 |
| L248-251 | default comp specs, CSV/TSV props, missing data, click line | PRF-01, FB-35, LBL-13, UI-15 |
| L254-256 | Bing Dark/Light/Gray + lang, timed duration, bugfixes | IMG-12, FB-33, DRW-10, DRW-22, SLS-11, LBL-21; **"pinned layers from older versions" unmapped** (M-3) |
| L259-262 | label position drag, unpin, NE language, UI animations | FB-33, PIN-01, MLB-03, UI-20 |
| L265-270 | blend modes, Position Progress, Max Zoom, Create Label Templates button, Elevation sample, bugfixes | SLS-05, PIN-04, MCS-13, MCS-19/LBL-14, PRJ-02, DRW-24, RTE-03, FIN-12, IMG-22 |
| L272 | panel won't init; swap template error | (robustness; LBL-12) |
| L275-277 | click preview selects, pinned Lat/Lon/Height, bugfixes | UI-15, PIN-03, VIEW-06, FB-21, PIN-10 |
| L280-289 | pick style from layer, points-in-polygons, filter speed, one-click DDS, http links, animate along multiple, geojson urls, autorun, TMS, auto-load GeoJSONs | SLS-09, FB-29, FB-06, DAT-07, FB-33, VIEW-06, API-24, TB-14, IMG-21, FB-18 |
| L292-294 | Coronavirus sample, animated DDS, animated label text | PRJ-02, DAT-10, LBL-10 |
| L297-304 | stroke outlines/round join, motion blur, blink/stagger, line mesh, Circle Area, cache paths, Mapdata icon, swap extrusion | SLS-03, MCS-18, TB-08/09, FB-25, DAT-09, PRF-17, PRF-14, SLS-07 |
| L307-310 | Icon & Locator, SHIFT/ALT select, Lakes/Rivers, Nominatim/OSM fixes | LBL-03, LBL-07/08, FB-14, SRC-05/FB-38 |
| L313 | building extrusion independent of zoom | 3D-03, DRW-16 |
| L316-325 | Globe sample, click find, Advanced, filter by name (**Label Templates half missing**), marker update, Label Anchor Point, run scripts, pitch, addToBrowser/clearBrowser, bugfixes | 3D-11, FB-10, IMG-04, SLS-12 (styles only — M-1), LBL-09, LBL-06, TB-01, MCS-20, API-23/25, RTE-01, PIN-01, SLS-13, FIN-10 |
| L328-330 | sample download button, thumbnails, CSV trim | PRJ-03, SLS-01, DAT-03 |
| L333 | fallback search | SRC-08, PRF-06 |
| L336-337 | geocode/watch fns, bugfixes | API-19..22, 3D-05, PIN-01, MCS-06, FIN-13, SLS-11 |
| L339-341 | releases; animate view to feature; ALT reveal; collective props | VIEW-05, LBL-08, FB-33 |

## B. HELP walk — coverage

- Basic / Mapcomps (L1-27): MCS-01..05, UI-03..06, PRJ-01, IMG-10 — all covered.
- Scripting API (L29-381): all 39 functions present (API-04..39 + API-16 from CL). Every draw option
  and default (17 options) present in DRW-03..19. addToBrowser options (5) present. githubBrowser options (9) present.
  watch / watchCsv defaults (10000 ms, ",") present.
  Gaps are argument-level only: M-5 (readFileCsv/readFileJson/writeFile accept "Extend Script File Object or filename
  string", encoding default UTF8, writeFile `content` string), M-6 (filesInDir argument semantics), M-7 (modalEditObject
  callback also fires on cancel; cancel payload unknown).
- MapTiler Data (L385-398): LIC-06, IMG-02/03, FB-10, MLB-02, IMG-17, FB-12, 3D-03 — covered.
- Disclaimer (L400-407): LIC-08 — covered.

## C. Missing items (detail)

- **M-1 Filter Label Templates by name.** Evidence: CL 1.0.4 (L319) "Filter Label Templates and Shape Layer Styles by name".
  SLS-12 has the name filter for styles; no LBL item states a name filter for Label Templates (LBL-18 is only the
  by-Mapcomp matrix). Add to LBL (or make SLS-12 explicitly shared). SOURCED.
- **M-2 Improved Natural Earth search by name.** Evidence: CL 1.2.0 (L210) "Improved Natural Earth data search by name".
  SRC-07 lists the NE Search API (1.2.2) but nothing about name-matching quality (I: fuzzy / alternate-name / multilingual
  NE name columns matching). SOURCED existence; algorithm unknown.
- **M-3 Backward compatibility with pinned layers from older versions.** Evidence: CL 1.1.4 (L256) "Bugfixes: ... pinned layers
  from older versions". Implies pinned-layer rigs created by earlier GEOlayers builds must keep working / be migrated (relevant
  because 1.1.0 changed pinned layers to separate Lat/Lon/Height). No PIN item covers rig versioning/migration. SOURCED existence;
  mechanism INFERRED.
- **M-4 3D Landscape heightmap + label alignment.** Evidence: CL 1.1.8 (L231) "Bugfixes: 3D Landscape heightmaps/labels offset in AE 17.5".
  3D-05 cites 1.1.8 but its text never states the requirement that heightmaps and labels on a landscape must line up
  with the texture (no offset). SOURCED.
- **M-5 File I/O API argument details.** Evidence: HELP L317-339 — readFileCsv/readFileJson/writeFile each take "Extend Script File
  Object or filename string" and "encoding: String, defaults to UTF8"; writeFile takes "content: String to write to the file".
  API-35/36/37 omit these (only API-34 states them). SOURCED.
- **M-6 filesInDir argument semantics.** Evidence: HELP L303-306 "folder: Extend Script Folder Object", "filterRegEx: Regular expression
  to test on the file name", "includeSubDirectories: Boolean, set to true to include files from sub directories". API-33 only states the return.
- **M-7 modalEditObject cancel path.** Evidence: HELP L355 "A function(error, editedObj) that is called after the user applied his edits or
  canceled." API-30 does not mention that the callback fires on cancel (payload on cancel unknown; I: editedObj undefined like modalActions).
- **M-8 Overpass OSM-JSON → GeoJSON assembly (INFERRED requirement).** Evidence: every vendor Overpass example ends with
  `out body;>;out skel qt;` and queries `relation[...]` (e.g. EX:Railroads_OSM `relation["route"="railway"]`, EX:Buildings_OSM building
  relations, EX:Autobahn_BAB `nwr`). That returns raw OSM elements, so GEOlayers must build GeoJSON (ways → lines/polygons, multipolygon
  and route relations). API-40/FB-16 cover the query macros but not this conversion. INFERRED.
- **M-9 Preview Map blank bugfix / MapTiler results-not-displayed bugfix not cited.** Evidence: CL 1.5.7 "Bugfix: Preview Map is blank";
  CL 1.5.5 "Bugfix: MapTiler search results no longer displayed". Features exist (UI-11, SRC-06) but citations are missing; low priority.

## D. Wrong or unsupported claims

- **W-1 IMG-16** says "Inner boundaries were fixed in 1.7.2" under *Admin Boundaries Imagery*. CL 1.7.2 (L63) reads
  "Natural Earth Inner Boundaries for some countries are messed up" — this matches the **Inner Boundaries feature** of
  'Find Features at Coordinate' (CL 1.2.0 L209, FB-11). Move the 1.7.2 citation to FB-11 (or cite both, marked INFERRED).
- **W-2 FIN-11** "Missing tiles fall back to a lower zoom" is tagged SOURCED (CL 1.4.0; AESF) but CL 1.4.0 only says rejected requests
  must not leave black holes, and CL 1.1.2 says overzoom must use overzoomed textures *instead of* lower-zoom tiles. Mark INFERRED
  or cite the exact forum snippet.
- **W-3 DAT-01** lists "TXT" as a sourced import format. HELP L165 mentions .txt only as an input to `watchCsv`. Re-scope to
  "watchCsv accepts .txt" or mark browser .txt import INFERRED.
- **W-4 PRF-02** "Default language — used by the preview map and new Mapcomps" is tagged SOURCED (CL 1.1.8), but CL 1.1.8 says only
  "Preview map displayed in default language set in prefs". The "new Mapcomps" part is INFERRED.
