# MapForge — Architecture

*MapForge* is a placeholder working name for a personal, non-distributed hobby After Effects extension that recreates, feature for feature, a commercial AE map-animation extension. No name, branding or code of the original is reused.

Inputs: `research/notes/ae-architecture.md` (n/arch), `research/notes/providers.md` (n/providers), `research/feature-inventory.md` (inventory IDs), `docs/FEATURE_SPEC.md` (requirements with the same IDs). Tags: **[S]** sourced in those notes, **[I]** our design inference that must be verified in AE.

---

## 1. Platform decision: CEP 12 panel + ExtendScript host (not UXP) — for AE in 2026

**Decision: build a CEP 12 HTML extension (Chromium 99 + Node.js 17.7.1) with an ExtendScript host layer.** Keep a host-adapter seam so a later UXP port only replaces the host layer.

Reasons:

1. **UXP for After Effects is beta-only.** It reached public beta in After Effects (Beta) 27.1 (UXP 9.4) in November 2026. Every AE UXP API member is "Min Version 27.0" [S n/arch §1.1]. A release-AE user (25–27) cannot run a UXP plugin today.
2. **UXP cannot host the map renderer.** UXP is "not a browser": no WebGL, no full DOM/CSS, no Node [S n/arch §0, §1.1]. The preview map, the vector-style renderer and the tile workers all need WebGL2, Web Workers and OffscreenCanvas, which CEP's Chromium 99 has [S/I n/arch §1.2]. The original's developer named missing WebGL and Web Workers as the reason UXP was impossible for this product [S].
3. **CEP is what the original uses** [S], so behaviours such as an always-open panel being required for the scripting API (API-01) and a global ExtendScript object (API-01/42) map 1:1.
4. **Lifetime is sufficient for a hobby build.** Adobe's timeline: CEP disabled by default in December 2028 and removed from AE in December 2029; ExtendScript scripts are not affected [S n/arch §0]. A PlayerDebugMode install keeps working until then. (I: "disabled by default" probably means it can be re-enabled.)
5. **Node in the panel** gives unrestricted file I/O (finalize image sequences, cache, prefs, user folder), `child_process` for reveal-in-Finder/Explorer, and zip handling — all needed by FIN, PRF, TB and API-33…39.

Consequences and mitigations:

- All AE DOM work goes through a `HostAdapter` interface (TypeScript) with one implementation today (`ExtendScriptHost`, via `evalScript` + CSXSEvent). A future `UxpHost` would call `require("aftereffects")`; the renderer would then move to a helper window (local browser/Electron over WebSocket), as Adobe staff suggested [S/I n/arch §0].
- Re-evaluate when UXP AE leaves beta **and** offers a WebGL-capable webview; until then UXP is out of scope.

## 2. Runtime overview

```
┌──────────────────────────── After Effects (Win/Mac) ───────────────────────────────┐
│                                                                                    │
│  CEP panel "MapForge" (Chromium 99 + Node 17.7.1, mixed context)                   │
│  ┌──────────────┐ ┌───────────────┐ ┌──────────────────┐ ┌──────────────────────┐ │
│  │ Panel UI     │ │ Preview map   │ │ Feature Browser  │ │ Finalize pipeline    │ │
│  │ (Svelte)     │ │ (MapLibre GL) │ │ + geo ops        │ │ render → files       │ │
│  └──────┬───────┘ └──────┬────────┘ └────────┬─────────┘ └──────────┬───────────┘ │
│         │      core services (TS): store · providers · search · styles · prefs   │ │
│         │      workers: tile render / mosaic / encode (OffscreenCanvas)          │ │
│         │      node: fs, cache, user folder, zip, child_process                  │ │
│         └───────────── bridge (evalScript ⇄ CSXSEvent, JSON / temp files) ───────┘ │
│                                         │                                          │
│  ExtendScript engine (ES3, shared global namespace)                                │
│  ┌───────────────────────────────────────────────────────────────────────────────┐ │
│  │ host.jsx bundle: global `mapforge` API · AE DOM ops · shape writer ·           │ │
│  │ expression installer · label templates · project metadata (XMP) · polyfills    │ │
│  └───────────────────────────────────────────────────────────────────────────────┘ │
│  AE project: Mapcomps, containing comps, anchors, MapPivot, imagery footage,       │
│  expressions (both engines), label precomps, shape layers                          │
└────────────────────────────────────────────────────────────────────────────────────┘
          │ HTTPS (rate-limited, cached)
          ▼  OpenFreeMap · Natural Earth (bundled) · Nominatim · Overpass · OSRM ·
             AWS Terrarium · CARTO · EOX · optional MapTiler / Bing-Azure / Mapbox / any XYZ
```

**Bridge** [S pattern n/arch §2; I design]:

- Panel → host: `evalScript("mapforge._host.op(<json>)")` with double-stringified JSON. Payloads over ~64 KB (GeoJSON, frame lists, shape vertex arrays) are written by Node to a temp JSON file and passed by path.
- Host → panel (async API calls from user scripts): ExtendScript stores the callback under an id and dispatches a `CSXSEvent` (`com.mapforge.request`) via `PlugPlugExternalObject`; the panel does the work and calls back `mapforge._resolve(id, json)`. CSXSEvent in AE is not documented in the cookbook → verify first thing in M0.
- A single **busy queue** in the panel serializes host-mutating jobs; `watch` callbacks are deferred while busy (API-44); Nominatim/OSRM have their own 1 req/s queues.
- AE has no project-opened or selection-changed events for CEP → poll `app.project.file` and the selection at a modest interval and on panel focus (UI-04 refresh, FB-18, TB-14) [S n/arch §2.6].

## 3. Repository layout

```
mapforge/
  cep.config.ts            # Bolt CEP: panel id com.mapforge.panel, host AEFT [25.0,99.9], CEF flags
  src/
    js/                    # panel (TypeScript)
      ui/                  # Svelte components per view (list, settings, browser, prefs, dialogs…)
      map/                 # preview map wrapper (MapLibre), frustum overlay, sync
      render/              # tile engine, style compiler, workers, finalize
      geo/                 # projection, geo ops, import/export, overpass, routing
      providers/           # provider registry, URL templates, scheduler, cache
      search/              # search sources
      data/                # dataviz, data-driven styles, timed data
      labels/              # Mapcomp label pass; label-template client side
      threeD/              # DEM, heightmaps, plugin adapters, GLB mesh
      api/                 # async API handlers (fetch*, geocode, watch, addToBrowser, draw…)
      host/                # HostAdapter interface + ExtendScriptHost + MockHost
      store/               # app state, prefs, project metadata client
      shared/              # types & JSON schemas shared with jsx (bridge contracts)
    jsx/                   # ExtendScript (TypeScript → ES3)
      index.ts             # initEngine(), global `mapforge`, polyfills
      api/                 # sync API (getMapcomps, setViewAtTime, duplicate…)
      ae/                  # comp/layer/effect/keyframe helpers
      shapes/              # shape-layer writer
      expr/                # generated expression strings (from src/shared/expr)
      labels/              # template instantiation, swap, markers
      meta/                # XMP project metadata
    shared/expr/           # projection + rig math, ES3-safe, compiled to both TS and expression text
  assets/
    styles/                # built-in imagery styles (JSON) + thumbnails
    shapestyles/           # default Shape Layer Styles
    templates/             # default label-template project (.aep authored once in AE) + JSON manifest
    scripts/               # default toolbar scripts (.jsx)
    naturalearth/          # bundled NE subsets (GeoJSON/TopoJSON/PMTiles)
    presets/               # optional .ffx pseudo-effect presets (authored in AE)
  tests/
    unit/                  # Vitest (Node) — geo, providers, expressions, bridge contracts
    host/                  # ES3 bundle run in node:vm against the mock AE DOM
    panel/                 # Playwright (Chromium) — UI, MapLibre render golden images
    ae/                    # in-AE test runner scripts (user runs on Win/Mac)
    fixtures/              # GeoJSON, GPX, KML, CSV, shapefiles, vendor example scripts, AE property-tree dumps
  tools/
    ae/dump-property-tree.jsx  # run once in AE to capture real match names for mocks
    install/ (win.ps1, mac.sh) # copy build + set PlayerDebugMode CSXS.12/13
```

## 4. Module breakdown

Each module lists its job, key design, and the inventory prefixes/IDs it owns.

### 4.1 Panel UI (`src/js/ui`) — UI-*, MCS-03/04/09–19, SRC-01–04, FB-01–09/33, PRF-*, LIC-02/03/05, UI-21/24
- Svelte 4 + TypeScript via Bolt CEP (Vite, HMR, browser preview). [I: Svelte chosen for small bundle and simple stores; React would also work.]
- View stack with full-panel sub-views and Back (UI-20); animated transitions (CL 1.1.3) with a reduce-motion switch.
- Theme from `getHostEnvironment().appSkinInfo` + `ThemeColorChanged` event.
- Flyout and context menus via `setPanelFlyoutMenu` / `setContextMenuByJSON` (UI-19).
- Modifier-key behaviours read from `MouseEvent.shiftKey/altKey/ctrlKey/metaKey` (FB-03/04/05, TB-01, LBL-07/08).
- Generic modals (UI-21) implemented in-panel (not separate CEP windows) so `modalEditObject`/`modalActions` (API-30/31) reuse them.
- Strings in one table (`ui/strings.ts`) — UI-23 snapshot test.

### 4.2 Preview map (`src/js/map`) — UI-11–16, VIEW-02, MCS-20
- `maplibre-gl` v5 (pin an exact version; verify WebGL2 on CEF 99). Free default style: OpenFreeMap; MapTiler when a key exists.
- Draws the selected Mapcomp's view frustum (trapezoid + diagonals) using the same projection module as the expressions (one source of truth).
- **Sync** (UI-13): when linked, the host pushes view samples at CTI (poll ~5 Hz while panel focused); preview moves write `setViewAtTime` with key-if-keyed semantics (VIEW-03).
- Click → `queryRenderedFeatures` + Browser hit test (UI-15, FB-10); hover outlines (UI-16).
- Aspect-dependent pitch clamp `maxPitch(aspect)` (MCS-20) — formula to calibrate against the original (OPEN_QUESTIONS).

### 4.3 Feature Browser & geo operations (`src/js/geo`, `src/js/store`) — FB-*, SRC-07/10/14, DAT-01–06/13/19–21, API-23–27, API-40/41
- In-memory feature store (normalized: collections → features; ids; favorites; selection), persisted to the user folder cache (FB-40), virtualised tree.
- Heavy ops in a geo worker, cancelable via `AbortController` (CL 1.2.1 "non-blocking & cancelable"):
  - union/difference/intersection: `polyclip-ts` (robust; turf's own boolean ops wrap it) — FB-20/21/22
  - topology-preserving simplification: `topojson-server` + `topojson-simplify` + `topojson-client` — FB-28, DRW-09
  - shared-border line mesh: `topojson-client.mesh` — FB-25
  - label point: `polylabel` — FB-37
  - measure, buffers, point-in-polygon, great circles: `@turf/*` — FB-26/27/29/41, TB-11
  - date-line handling: split/shift helpers (own code) — FB-27, DRW-14/25
- Importers: `@tmcw/togeojson` (KML, GPX, TCX), `papaparse` (CSV/TSV, `;` delimiter, header trimming), `shpjs` (zipped shapefile + .prj via `proj4`), `topojson-client` (TopoJSON), `fflate` (zip), `osmtogeojson` (Overpass OSM-JSON → GeoJSON, FB-44).
- Overpass client: macro expansion (`{{bbox}}`, `{{center}}`, `{{geocodeArea:}}`, `{{geocodeId:}}`, …), mirror scoring via `/api/status`, back-off on 429/504 (FB-16, API-40) [S semantics n/providers §3; I algorithm].
- OSM by id via Overpass `node|way|rel(id);out body;>;out skel qt;` (FB-15).

### 4.4 Providers & search (`src/js/providers`, `src/js/search`) — IMG-12/13/18–23/25, SRC-05–09/12/13, RTE-01, LIC-04/06/18
- **Provider registry** entries: `{id, kind: xyz-raster | vector-style | bing-metadata | azure | local-dir | wms | dem-terrarium | dem-mapbox-rgb, urlTemplate, subdomains, scheme xyz|tms, tileSize 256|512, minzoom, maxzoom, bounds, retina, attribution, keySlot, capabilities{language, worldview}}` [I n/providers §14].
- **URL template engine**: `{z} {x} {y} {-y} {s} {q|quadkey} {r} {year} {month} {date} {dateISO}`; uppercase `{X}/{Y}` accepted (IMG-20). Date variables evaluated at the frame's comp time.
- **Scheduler**: per-host concurrency caps; 1 req/s for Nominatim and OSRM; identifying UA; retry with back-off; 403 → auth-error UI; 429 → request-limit UI and free fallback (LIC-18).
- **Disk cache**: `<cacheRoot>/<providerId>/<z>/<x>/<y>.<ext>` + `index.json` (ETag/expiry). Purge (FIN-08) and configurable paths (PRF-17).
- **Search sources**: Nominatim, MapTiler (key), Natural Earth (bundled index with diacritic folding, SRC-14), Browser, Browser Downloads, File (http, zipped shp), Folder, GitHub (REST trees + raw), Photon (fallback, SRC-08), coordinate parser (SRC-13). Results grouped by source (SRC-02). Search on Enter only (Nominatim policy).
- **Routing**: FOSSGIS OSRM car/bike/foot; maneuver points from `steps[].maneuver` (RTE-01, PRF-07).

### 4.5 Tile + style renderer (`src/js/render`) — IMG-*, FIN-09–12, MLB-*, DAT-15–18
- **Vector styles** (Basic/Advanced/Lite clones, MapTiler imports, style.json URLs): a style compiler turns MapForge style settings (color roles, water toggles, rail color, street density, disputed borders, fonts, language, hillshade, feature groups — IMG-03/04/08/11) into a MapLibre style JSON. Rendered by an **offscreen MapLibre instance** with `preserveDrawingBuffer:true`, `fadeDuration:0`, `pixelRatio` from Quality, `maxCanvasSize` up to GPU limit; large frames rendered in tiles and stitched [I n/arch §4.2A].
- **Raster styles** (Universal Raster, satellite, CARTO, Bing/Azure, local dirs, Seamless Texture): the **tilemerger** in workers fetches tiles, composites the needed extent with overzoom (parent tile upscaled above maxzoom; IMG-22), TMS flip, x-wrap (FIN-10), Hue/Opacity/Black adjustments, then hands the mosaic to MapLibre as an image/raster source or to the frame compositor [I n/arch §4.2B].
- **Dataviz styles**: Choropleth, Circles, Heatmap, Lines are MapLibre layer templates (`fill`, `circle`, `heatmap`, `line`) fed by a connected GeoJSON source (DAT-15–18).
- **Mapcomp labels** (MLB): default representation = a separate baked label pass (symbol layers only, transparent background) imported as its own footage layer inside the Mapcomp, so labels can be toggled (UI-08), motion-blurred (MLB-09) and blended separately. Label fade/"animation" is computed deterministically per frame by our own placement pass (collision + fade state carried across frames) rather than MapLibre's wall-clock fade. [I; OPEN_QUESTIONS MLB-10.]
- Encoding in workers: `OffscreenCanvas.convertToBlob` (PNG/JPEG); 16-bit heightmaps via `fast-png`.

### 4.6 Finalize pipeline & cache (`src/js/render/finalize`) — FIN-*, API-05, MCS-16/17, IMG-31
1. Host samples Lat/Lon/Zoom/Bearing/Pitch for every frame in range (`valueAtTime`, batched to a JSON file) — respecting `onlyCurrentFrame`/`onlyWorkArea`.
2. Planner groups frames by identical view (static spans render once; duplicates written as hard links where the filesystem allows) and plans tile requests (Quality → zoom offset, FIN-05).
3. Renderer produces frames per §4.5; failed tiles fall back to parent tiles, never black holes; failures counted ("N Tile(s) couldn't be rendered", FIN-11).
4. Writer: temp file → atomic rename → PNG validation (FIN-14), into `<project folder>/MapForge Imagery/<mapcomp-id>/<pass>/frame_00001.png` by default (portable with the project and Collect Files; FIN-18), cache root separate.
5. Host imports/replaces the image sequence (`ImportOptions.sequence`, `replaceWithSequence`), conforms frame rate, places it in the Mapcomp; Blend Duration cross-fade between zoom bands is applied with opacity expressions (MCS-17); motion blur toggled on the imagery layer (MCS-18).
6. Optional: add to Render Queue (FIN-06); callback to API caller (FIN-15).
- Before finalize the Mapcomp holds a low-resolution preview pass (FIN-01, I).
- Purge: imagery cache by Mapcomp or all (FIN-08).

### 4.7 ExtendScript host layer (`src/jsx`) — API-01–16/28/29/33–39/42/43, MCS-05–11/21–27, LIC-10/11
- TypeScript compiled to a single ES3 file (Bolt CEP's ExtendScript build) with JSON2 and ES5 polyfills (`Array.prototype.forEach/map/filter/indexOf`, `Object.keys`) because user scripts rely on them (API-42) [S].
- `initEngine()` re-installs `mapforge` defensively (namespaced, idempotent) so other scripts' globals cannot break it (CL 1.14.1).
- Every host call: `app.beginUndoGroup` / `endUndoGroup` in `finally` (LIC-10); optional `app.purge(PurgeTarget.UNDO_CACHES)` (PRF-18).
- Mapcomp creation (MCS-05): Mapcomp comp, containing comp (if needed), Mapcomp layer, `<name> Anchor` null, `MapPivot` null, controls, folder `MapForge Items`.
- **View controls** (VIEW-01): default = five Slider Controls named Latitude, Longitude, Zoom, Bearing, Pitch on a guide null "Map Controls" inside the Mapcomp; optional single pseudo effect via a `.ffx` preset authored once in AE (`assets/presets`) [S technique n/arch §3.3; layer/location is an OPEN QUESTION].
- `autoFixExpressions` after renames/duplicates (MCS-10, MCS-22, PRJ-04).
- Errors for paths containing `#` (LIC-11).

### 4.8 Expressions library (`src/shared/expr`) — MCS-06/07/24, PIN-*, LBL-06, VIEW-13, TB-05/06/11/12
- One ES3-safe module with Web-Mercator and rig math [S formulas n/arch §5.2]: `mercX/mercY` (clamp ±85.051129°), world size `tileSize·2^zoom`, normalized-space subtraction before scaling (no large-zoom precision errors), antimeridian wrap, bearing rotation, pitch via MapPivot X-rotation, pixels-per-metre for Height, inverse projection for `getLayersGeoPosition`.
- The same functions are (a) imported by the panel TS code and (b) serialized into expression text (function bodies injected), so preview, finalize and AE expressions agree.
- Generated expressions obey both engines: no `let/const`, arrows, template literals, `includes`; all `if` with braces and `else`; `thisLayer`, `thisComp` [S rules].
- Rigs:
  - **Anchor** (MCS-06): position/scale/rotation from the controls relative to a reference geo point and zoom; re-based when zoom range > ~8 levels.
  - **MapPivot** (MCS-07): pitch X-rotation, bearing Z-rotation about comp centre.
  - **Pinned layer** (PIN-01–05/13): Latitude/Longitude/Height sliders, Rotate/Scale-with-Map checkboxes, Position Progress slider, rig version marker for migration.
  - **Snapped layer** (PIN-08/API-16): scaleWithMap, rotateWithMap, rotateAlongPath, bearingOffset, threeDLayer, path + duration.
  - Zoom clamp against bezier overshoot (VIEW-13).

### 4.9 Label template system (`src/jsx/labels`, `src/js/labels`) — LBL-*, UI-18, API-12–14
- A template = any comp registered in project metadata (or found in the `Label Templates` folder) with text layers whose names match data fields (`name`, …) and an optional `Label Anchor Point` null carrying Scale/Rotate-with-Map checkbox effects (LBL-06) and a `LabelColors` comp (LBL-07).
- Instantiation: duplicate the template comp per label (so text can differ), set text from data, add as layer `Label '<name>'` in the containing comp, attach pin/snap rig at the feature's label point, copy template markers (LBL-09), unique names (LBL-19), time offsets (LBL-11), timed text keys (LBL-10), path animation (LBL-24).
- Swap (LBL-12): replace source comp / AVLayer assets, keep data and rig.
- Missing data prompt via UI-21 modal (LBL-13). Renaming dictionaries applied before text set (LBL-20).
- Default template project authored once in AE (OPEN_QUESTIONS) and shipped in `assets/templates`.

### 4.10 Shape-layer writer (`src/jsx/shapes`) — DRW-*, SLS-*, RTE-04–07, FB-30/32
- Panel projects geometry to local pixel space at a reference zoom relative to the anchor's reference point, simplifies (current zoom, max zoom, offset, topology), splits date-line copies, and writes a compact JSON (`paths: [{closed, v, i, o}]`) to a temp file.
- Host builds `ADBE Vector Group` → `ADBE Vector Shape - Group` per ring (holes via even-odd fill rule or merge paths), fills/strokes/dashes/trim/extrusion per the Shape Layer Style, parents to the anchor, sets blend mode, preserve transparency, material options, renderer switch (DRW-11), transform locks (DRW-19).
- Style expressions: auto stroke width `w / scale`, dash threshold/speed, extrusion with `absoluteZScale`.
- Reverse path: shape layer → feature (FB-30) via the inverse projection.
- Batch performance: one undo group, suppress dialogs, `setValuesAtTimes` for time offsets.

### 4.11 Data & DataViz (`src/js/data`) — DAT-07–18/22, FB-35/36, LBL-10/11, DRW-20
- Data-driven style builder: property → color (ramps via `chroma-js`; gradient preview DAT-08) / size with curves (`linear`, `circle area` = sqrt, plus log) (DAT-09).
- Timed data model: per-feature time series (`prop:<date>` and GeoJSON sequences, DAT-02/10 — convention to confirm), key generation with easing (DAT-11).
- Property Diagrams (DAT-14): bar/line charts generated as shape layers.
- Large-dataset guard (DRW-21/DAT-22) suggests dataviz imagery styles.

### 4.12 3D landscape (`src/js/threeD`, host helpers) — 3D-*, IMG-31, DRW-27
- DEM fetch (AWS Terrarium default, MapTiler terrain-rgb with key), decode to metres, stitch, export a 16-bit heightmap PNG + texture pass; heightmap comp uses AE's Calculations effect (3D-05) without changing project colour space.
- **Plugin adapters** (Helium, FreeForm Pro, Plexus, Mir): create the plugin-named control layer and wire heightmap + texture; match names must be captured from the user's machine (`tools/ae/dump-property-tree.jsx`) because the plugins are paid and Win/Mac only.
- **Free path**: build a terrain mesh (`@gltf-transform/core`) and import as an AE 3D model layer (AE 24+) [I].
- Landscape edge/sidewalls (3D-09), alignment tests (3D-16).
- Globe / Equal Earth (3D-10): render via MapLibre v5 `projection: globe` per frame and d3-geo `geoEqualEarth` raster reprojection; no native C++ plugin (cannot be built or tested on Linux).
- 3D Scene & Camera script (3D-12/TB-13): real AE camera whose zoom = 1.5 × comp height matches MapLibre's camera distance [S constant n/arch §5.2].

### 4.13 Scripting API surface (`mapforge` global) — API-*
Same names, argument order, defaults, return values and callbacks as the original's documented list, under our namespace:

`importProject, finalize, duplicate, setViewAtTime, fitViewAtTime, removeMapcompControlKeys, getMapcomps, getMapcompAnchor, getLabelTemplateComps, addLabel, updateLabelData, getLayersGeoPosition, snapLayerToMapcomp, fetchText, fetchJson, geocode, watch, watchCsv, unwatch, addToBrowser, clearBrowser, removeBrowserSelection, getBrowserSelection, drawBrowserSelection, draw, filesInDir, readFile, readFileCsv, readFileJson, writeFile, userFolder, userFile, modalEditObject, modalActions, githubBrowser`

- Sync functions run purely in ExtendScript; async ones bridge to the panel (§2).
- Compatibility shims: `simplyfyByZoomRange` + `simplifyByZoomRange`; `lat/lon` aliases; `addToBrowser(url, options)` overload (API-23); `geolayers3` alias only if the user opts in (OPEN_QUESTIONS Q-04).
- The 13 vendor example scripts are the parity test corpus (TB-16).

### 4.14 Preferences & storage (`src/js/store/prefs`) — PRF-*, FB-40, LIC-09, API-38/39
- User folder: `~/Documents/MapForge/` (Win/Mac via Node `os.homedir()`), containing `prefs.json` (schema-versioned, hidden prefs included), `styles/` (imagery and shape styles, import/export bundles), `scripts/` (user toolbar scripts), `dictionaries/` (label renaming), `logs/`, `cache/` (default cache root).
- Provider keys stored in `prefs.json` (hobby scope; plain JSON, not synced).

### 4.15 Project metadata (`src/jsx/meta`) — PRF-19, MCS-21, LBL-18, SLS-12, DRW-02, UI-09/10
- Stored in `app.project.xmpPacket` under namespace `http://ns.mapforge.local/project/1.0/` (prefix `mapforge`) via `AdobeXMPScript` [S technique n/arch §2].
- Contents: schema version; Mapcomp registry keyed by `item.id` (controls layer, anchor, pivot, link group/controller, style, quality, attribution, labels flag, imagery paths, rig version); drawing options; template/style ↔ Mapcomp assignment matrix; filters.
- Fallback mirror in a comment of the controls layer so a Mapcomp copied into another project can be re-registered (PRJ-04, MCS-26 repair tool).

## 5. Technology choices

| Concern | Choice | Notes |
|---|---|---|
| Scaffold / build | Bolt CEP (Vite + TS + Svelte), `cep.config.ts` | ES3 ExtendScript output, HMR browser preview, `zxp`/`zip` scripts [S n/arch §1.5] |
| AE typings | `types-for-adobe` | ExtendScript DOM types |
| Map rendering | `maplibre-gl` v5 (pinned exact version) | preview, finalize vector frames, globe |
| Vector basemap (free) | OpenFreeMap styles/tiles; optional Protomaps PMTiles via `pmtiles` | OpenMapTiles schema matches MapTiler-era style features |
| Geo ops | `@turf/*`, `polyclip-ts`, `polylabel`, `topojson-server/simplify/client`, `d3-geo`, `d3-geo-projection`, `proj4` | |
| Import/export | `@tmcw/togeojson`, `papaparse`, `shpjs`, `fflate`, `osmtogeojson` | KML/GPX/TCX, CSV/TSV, shapefile, zip, Overpass |
| Image encode | OffscreenCanvas `convertToBlob`; `fast-png` (16-bit) | no native modules in CEP (Node ABI 17.7.1) |
| Color | `chroma-js`; own `.ase` parser (small binary format) | IMG-08/09, DAT-07 |
| 3D mesh | `@gltf-transform/core` | free 3D landscape path |
| Testing | Vitest, Playwright (Chromium; one pinned ~99 build + current), `pixelmatch`, Node `vm` mock host | §6 |
| Lint | ESLint (TS) + ESLint `ecmaVersion: 3` on built jsx and generated expressions | |

Every library must run in Chromium 99 / Node 17.7.1 (no newer syntax in shipped bundles: Vite `build.target: "chrome99"`).

## 6. Developing and testing on Linux without After Effects

After Effects and ExtendScript do not run on Linux, so the design keeps everything except the thin AE DOM layer testable in Node/Chromium.

1. **Panel in a normal browser**: `yarn dev` serves the panel; `window.__adobe_cep__` absent → `MockHost` implements `HostAdapter` in TS (in-memory project model: comps, layers, effects, keys, expressions) and a Node-less `fs` shim (IndexedDB) so the full UI runs.
2. **Host code under Node**: the built ES3 `host.jsx` runs inside `node:vm` with a hand-written mock AE DOM (`app`, `CompItem`, `LayerCollection`, `PropertyGroup.addProperty(matchName)` registry, `Property.setValueAtTime` storing keys, `Shape`, `MarkerValue`, `ImportOptions`, `File`/`Folder` over a temp dir). Match names are validated against real property-tree dumps the user captures once with `tools/ae/dump-property-tree.jsx`.
3. **Expressions**: generated expression strings run in `vm` with mocks for `thisComp`, `thisLayer`, `comp()`, `effect()`, `toComp()`, `time`, `value`, under both an ES3-restricted evaluator (Legacy) and modern JS; results compared with MapLibre `map.project()` from Playwright for random views (≤0.5 px tolerance).
4. **Rendering**: Playwright Chromium with SwiftShader WebGL renders frames for golden-image tests (`pixelmatch`); a pinned Chromium-99 run catches API/syntax gaps of CEF 99.
5. **Bridge contracts**: JSON Schemas in `src/shared` validated in both directions in unit tests (mock) and later in AE.
6. **Providers**: recorded HTTP fixtures (no live calls in CI); a separate opt-in live smoke test honouring rate limits.
7. **In-AE verification (user, later)**: install script copies `dist/cep` to the CEP extensions folder and sets PlayerDebugMode for CSXS.12 and CSXS.13; `.debug` exposes DevTools on port 8088; `tests/ae/run-all.jsx` executes the AE acceptance checks from FEATURE_SPEC and writes `mapforge-ae-test.log`.

## 7. Build, packaging, signing

- `yarn build` → `dist/cep/` (manifest, panel bundle, `jsx/host.jsx`, assets). ExtendScript stays plain ES3 (`.jsx`); JSXBIN is optional and skipped.
- Manifest: `ExtensionBundleId="com.mapforge"`, panel `com.mapforge.panel`, host `AEFT [25.0,99.9]`, `CSXS 11.0+` required runtime, CEF flags `--enable-nodejs --mixed-context --allow-file-access --allow-file-access-from-files --disable-site-isolation-trials` [S n/arch §1.3]. Extension path must not contain `#`.
- **Hobby install (default): unsigned folder + PlayerDebugMode** (Win registry `HKCU\Software\Adobe\CSXS.12` and `.13` `PlayerDebugMode=1`; mac `defaults write com.adobe.CSXS.12 PlayerDebugMode 1` + `killall cfprefsd`).
- Optional ZXP: self-signed certificate with ZXPSignCmd ≥ 4.1.3 on Win/Mac (or the Docker wrapper on Linux, older version) — only if the user wants a one-file install. No Marketplace publishing (personal use).
- Known risk: AE 25.6–26.3 on macOS 26 reported "Unsupported Manifest version ''" for newly installed extensions [S n/arch §1.2]; test the manifest version early (M0).

## 8. Phased roadmap (milestones to 100% parity)

Each milestone ends with: all its IDs' `Unit:` checks green on Linux, a build the user can install, and a short AE checklist (the `AE:` checks of its IDs) for the user to run. Hands-on comparison sessions against the original (to resolve INFERRED/unknown items) are scheduled after M2, M6 and M10.

| MS | Theme | Exit criteria | Items |
|---|---|---|---|
| M0 | Foundations | Panel loads in AE 25–27; bridge round-trip incl. CSXSEvent; `mapforge` global with file utilities and modals; mock host + test harness; expression engine compatibility proven | 26: UI-01, UI-20, UI-21, MCS-24, PRF-19, API-01, API-02, API-03, API-17, API-18, API-30, API-31, API-33, API-34, API-35, API-36, API-37, API-38, API-39, API-42, API-43, LIC-09, LIC-10, LIC-11, LIC-12, LIC-13 |
| M1 | Mapcomp core & view rig | Create/duplicate/remove/link Mapcomps; controls, anchor, MapPivot rigs; preview map with sync; view API | 52: UI-02, UI-03, UI-04, UI-05, UI-06, UI-07, UI-10, UI-11, UI-12, UI-13, UI-19, UI-22, UI-25, MCS-01, MCS-02, MCS-03, MCS-04, MCS-05, MCS-06, MCS-07, MCS-08, MCS-09, MCS-10, MCS-11, MCS-12, MCS-13, MCS-20, MCS-21, MCS-22, MCS-23, MCS-25, MCS-27, MCS-28, MCS-29, VIEW-01, VIEW-02, VIEW-03, VIEW-04, VIEW-12, VIEW-13, VIEW-14, PIN-09, 3D-01, 3D-02, PRF-01, API-06, API-07, API-08, API-09, API-10, API-11, API-15 |
| M2 | Imagery styles, tile engine & Finalize | All style kinds render; finalize writes and imports sequences; cache; attribution; key handling with free fallbacks | 65: UI-09, UI-23, UI-27, UI-28, MCS-14, MCS-15, MCS-16, MCS-17, MCS-18, IMG-01, IMG-02, IMG-03, IMG-04, IMG-05, IMG-06, IMG-07, IMG-08, IMG-09, IMG-10, IMG-11, IMG-12, IMG-13, IMG-14, IMG-15, IMG-16, IMG-17, IMG-18, IMG-19, IMG-20, IMG-21, IMG-22, IMG-23, IMG-24, IMG-25, IMG-26, IMG-27, IMG-28, IMG-29, IMG-33, FIN-01, FIN-02, FIN-03, FIN-04, FIN-05, FIN-06, FIN-07, FIN-08, FIN-09, FIN-10, FIN-11, FIN-12, FIN-13, FIN-14, FIN-15, FIN-16, FIN-17, PRF-10, PRF-11, PRF-12, PRF-15, API-05, LIC-04, LIC-05, LIC-06, LIC-18 |
| M3 | Mapcomp labels | Automatic labels with languages, RTL, dots, renaming, motion blur | 15: UI-08, UI-14, MLB-01, MLB-02, MLB-03, MLB-04, MLB-05, MLB-06, MLB-07, MLB-08, MLB-09, MLB-10, MLB-11, PRF-02, PRF-09 |
| M4 | Search, Feature Browser, data import, async API | All search sources, Browser tools and geo ops, importers, Overpass, watch/fetch/geocode APIs | 97: UI-15, UI-16, UI-24, SRC-01, SRC-02, SRC-03, SRC-04, SRC-05, SRC-06, SRC-07, SRC-08, SRC-09, SRC-10, SRC-11, SRC-12, SRC-13, SRC-14, FB-01, FB-02, FB-03, FB-04, FB-05, FB-06, FB-07, FB-08, FB-09, FB-10, FB-11, FB-12, FB-43, FB-13, FB-14, FB-15, FB-16, FB-17, FB-18, FB-19, FB-20, FB-21, FB-22, FB-23, FB-24, FB-25, FB-26, FB-27, FB-28, FB-29, FB-30, FB-31, FB-32, FB-33, FB-34, FB-35, FB-36, FB-37, FB-38, FB-39, FB-40, FB-41, FB-42, FB-44, FB-45, FB-46, FB-47, SLS-15, RTE-09, DAT-01, DAT-02, DAT-03, DAT-04, DAT-05, DAT-06, DAT-13, DAT-19, DAT-20, DAT-21, PRF-03, PRF-04, PRF-05, PRF-06, PRF-13, PRF-16, PRF-17, API-19, API-20, API-21, API-22, API-23, API-24, API-25, API-26, API-27, API-32, API-40, API-41, API-44, API-45 |
| M5 | Drawing & Shape Layer Styles | Draw options with defaults, style library/editor/swap, extrusion & 3D buildings | 49: UI-26, DRW-01, DRW-02, DRW-03, DRW-04, DRW-05, DRW-06, DRW-07, DRW-08, DRW-09, DRW-10, DRW-11, DRW-12, DRW-13, DRW-14, DRW-15, DRW-16, DRW-17, DRW-18, DRW-19, DRW-20, DRW-21, DRW-22, DRW-23, DRW-24, DRW-25, DRW-28, SLS-01, SLS-02, SLS-03, SLS-04, SLS-05, SLS-06, SLS-07, SLS-08, SLS-09, SLS-10, SLS-11, SLS-12, SLS-13, SLS-14, SLS-17, SLS-18, SLS-19, 3D-03, 3D-04, 3D-15, API-28, API-29 |
| M6 | Label Templates & pinning/snapping | Default template project, add/swap/assign templates, pin/snap rigs, addLabel/snap APIs | 47: UI-18, MCS-19, LBL-01, LBL-02, LBL-03, LBL-04, LBL-05, LBL-06, LBL-07, LBL-08, LBL-09, LBL-10, LBL-11, LBL-12, LBL-13, LBL-14, LBL-15, LBL-16, LBL-17, LBL-18, LBL-19, LBL-20, LBL-21, LBL-22, LBL-23, LBL-24, LBL-25, LBL-26, LBL-27, PIN-01, PIN-02, PIN-03, PIN-04, PIN-05, PIN-06, PIN-07, PIN-08, PIN-10, PIN-11, PIN-12, PIN-13, PIN-14, 3D-14, API-12, API-13, API-14, API-16 |
| M7 | View animations, routes, arrows, default scripts | Animate-to/along/between, flyovers, routes, arrow tool, all toolbar scripts, vendor script corpus | 38: UI-17, VIEW-05, VIEW-06, VIEW-07, VIEW-08, VIEW-09, VIEW-10, VIEW-11, VIEW-15, DRW-26, SLS-16, RTE-01, RTE-02, RTE-03, RTE-04, RTE-05, RTE-06, RTE-07, RTE-08, RTE-10, RTE-11, TB-01, TB-02, TB-03, TB-04, TB-05, TB-06, TB-07, TB-08, TB-09, TB-10, TB-11, TB-12, TB-14, TB-15, TB-16, PRF-07, PRF-08 |
| M8 | DataViz | Data-driven styles, timed data, Property Diagrams, dataviz imagery styles | 13: IMG-30, DAT-07, DAT-08, DAT-09, DAT-10, DAT-11, DAT-12, DAT-14, DAT-15, DAT-16, DAT-17, DAT-18, DAT-22 |
| M9 | 3D | 3D landscapes (plugin adapters + free GLB path), DEM, globe/Equal Earth, 3D scene & camera | 14: IMG-31, IMG-32, DRW-27, 3D-05, 3D-06, 3D-07, 3D-08, 3D-09, 3D-10, 3D-11, 3D-12, 3D-13, 3D-16, TB-13 |
| M10 | Projects, prefs, help, parity audit | Projects panel & samples, remaining prefs, About/Help, troubleshooting & portability; every inventory ID checked → **100% parity** | 28: MCS-26, FIN-18, PRJ-01, PRJ-02, PRJ-03, PRJ-04, PRJ-05, PRJ-06, PRJ-07, PRJ-08, PRJ-09, PRJ-10, PRJ-11, PRF-14, PRF-18, PRF-20, PRF-21, API-04, LIC-01, LIC-02, LIC-03, LIC-07, LIC-08, LIC-14, LIC-15, LIC-16, LIC-17, LIC-19 |

Total: 444 inventory IDs (430 + 14 added from the EBE transcript on 2026-11-19), each assigned to exactly one milestone (generated from `docs/FEATURE_SPEC.md`).

## 9. Key risks

| Risk | Impact | Mitigation |
|---|---|---|
| WebGL2/MapLibre misbehaves in CEF 99 on some GPUs (null context on dual-GPU) | Preview/finalize broken | M0 spike; fallback to WebGL1 context; raster-only CPU path for finalize |
| CSXSEvent from ExtendScript unreliable in AE | Async API broken | Fallback: panel polls a host-side request queue via `evalScript` every 100 ms |
| View-control form unknown (pseudo effect vs sliders) | Expression compatibility with user expressions | Decide early (OPEN_QUESTIONS Q-27); keep effect lookup by name in one helper |
| Disk usage of per-frame sequences | Large projects | Static-span dedupe via hard links, JPEG for opaque passes, Quality pref |
| Paid plugins / exact UI unknown | Parity of 3D and UI details | Hands-on sessions; property-tree dumps from the user's machine |
| Provider ToS/rate limits | Blocked tiles | Free defaults that allow caching; scheduler; no OSM tile bulk use |
| CEP end of life (2028/2029) | Extension stops loading | HostAdapter seam; re-evaluate UXP when it gains a webview |
