# MapForge — Feature Specification (parity build spec)

*MapForge* is a placeholder working name. This is the build spec for a personal, non-distributed hobby recreation of the **feature set** of a commercial After Effects map extension. The product name, logo, wording that names the original, and its bundled assets are **not** reused. Where the original's UI shows its own name (for example "Import to …", "restart …", a project folder name), MapForge uses its own name.

Source of truth for behaviour: `research/feature-inventory.md` (444 items, revised 2026-11-19; updated the same day from the "Every Button Explained" transcript, source EBE, which added 14 items). **Every inventory ID has exactly one requirement below with the same ID**, so parity can be tracked by ID. Architecture and milestones: `docs/ARCHITECTURE.md`. Decisions only the user can make: `docs/OPEN_QUESTIONS.md`.

## How to read this spec

Each requirement has:

- **Req**: what MapForge must do. Details, options and defaults live in the inventory item with the same ID; the spec does not restate every detail.
- **Accept**: how we verify it.
  - `Unit:` automated on Linux (Vitest/Node with the mock AE host, or Playwright/Chromium for the panel and renderer).
  - `AE:` manual or scripted check inside After Effects on Windows/macOS (the user runs these later; a scripted AE test runner, `tests/ae/*.jsx`, prints PASS/FAIL to the Info panel and a log file).
- **Evidence**: the inventory tag. `SOURCED` = behaviour documented. `PARTLY INFERRED` = core feature documented, some details guessed. `INFERRED` = guessed; build a reasonable version, then confirm against the original during hands-on testing before freezing.
- **MS**: the milestone (M0–M10) that delivers it.
- **Free substitute**: used when the original relies on a paid provider or a paid/native plugin.

Parity is reached when every ID is checked off with its `Accept` passing, and every PARTLY INFERRED/INFERRED item has been confirmed or corrected against the original (OPEN_QUESTIONS Q-01).

## Global requirements (apply to every item)

- G-1 **Naming**: no GEOlayers name or branding in UI, files, folders, API namespace or metadata. Concept terms that are generic (Mapcomp, containing comp, anchor, Finalize, Feature Browser, Label Template, Shape Layer Style) are kept for 1:1 workflow parity unless the user decides otherwise (OPEN_QUESTIONS Q-03).
- G-2 **API namespace**: the ExtendScript global is `mapforge` with the original's function names, argument order, defaults, return values and callback contracts. A `geolayers3` alias is **not** shipped by default; the vendor example corpus is tested by rewriting the prefix in the test harness (Q-04).
- G-3 **Data compatibility**: read the original's reserved GeoJSON property `GEOlayersDisplayName` as well as our `MapForgeDisplayName` (Q-05).
- G-4 **Undo**: every user action is one undo group, always closed (LIC-10).
- G-5 **Both expression engines**: every generated expression runs under Legacy ExtendScript and JavaScript engines (MCS-24).
- G-6 **Provider etiquette**: identifying User-Agent, per-host rate limits (Nominatim/OSRM 1 req/s), disk caching, attribution rendered into the comp; never bulk-download from tile.openstreetmap.org.
- G-7 **No keys required to start**: with no keys at all, MapForge must create, animate and finalize a map using free sources (Natural Earth + OpenFreeMap + AWS Terrarium + Nominatim + Overpass + OSRM).

## Free substitutes for paid / unavailable providers

| Original dependency | Used for (IDs) | MapForge default | Optional (user-supplied key) |
|---|---|---|---|
| MapTiler Cloud vector styles Basic/Advanced/Lite | IMG-03/04/05, IMG-08, MCS-04 | OpenFreeMap styles (OpenMapTiles schema; no key) re-styled to match | MapTiler Free key (non-commercial, logo) |
| MapTiler Satellite V2 | IMG-06 | EOX Sentinel-2 cloudless (non-commercial) | MapTiler Free satellite-v2 |
| MapTiler labels (multilingual) | MLB-01/02/03 | OpenFreeMap `name:xx` + Natural Earth `name_xx` | MapTiler |
| MapTiler click-find / current-view import / admin bounds | UI-15, FB-10, FB-12, FB-13 | OpenFreeMap vector tiles + Overpass + Natural Earth | MapTiler |
| MapTiler watermask | IMG-17 | OpenFreeMap water layer / NE water polygons | MapTiler |
| MapTiler 3D buildings | 3D-03 | OpenFreeMap `building.render_height` or Overpass | MapTiler |
| MapTiler Search (+ worldview) | SRC-06, PRF-13 | Nominatim + Natural Earth (+ Photon fallback) | MapTiler geocoding |
| DEM / 3D terrain | IMG-31, 3D-05, 3D-08 | AWS Terrarium tiles (no key) | MapTiler terrain-rgb-v2 |
| Bing imagery | IMG-12 | Not available to new users (Basic keys retired 2025); EOX/CARTO dark/light/gray look-alikes | Bing Enterprise key or Azure Maps key |
| CARTO | IMG-13 | CARTO free key (non-commercial) | — |
| Routing (unknown provider) | RTE-01 | FOSSGIS OSRM car/bike/foot | ORS / GraphHopper key |
| Paid 3D plugins Helium / FreeForm Pro / Plexus / Mir | 3D-05/06 | Supported if the user owns them; free path: terrain mesh exported as GLB into an AE 3D model layer (AE 24+) | — |
| Native "via Plugin" globe / Equal Earth | 3D-10 | MapLibre v5 globe projection in the renderer; Equal Earth via d3-geo raster reprojection (no C++ plugin: cannot be built or tested on Linux) | — |
| Video Copilot Orb (free) | 3D-11 | Used as-is (user installs) | — |



## 1. Panel / UI & Mapcomp List (UI)

### UI-01 · Main dockable panel "GEOlayers 3"
- **Req:** Ship a dockable CEP panel 'MapForge' under Window > Extensions, with flyout and context menus (flyout entries per UI-19); the scripting API works only while the panel is open.
- **Accept:** AE: panel opens from Window > Extensions > MapForge, docks, survives AE restart; calling any `mapforge.*` function with the panel closed returns a clear error. Unit: mock-host boot test.
- **Evidence:** SOURCED · **MS:** M0

### UI-02 · Panel layout (top → bottom)
- **Req:** Lay out the panel top→bottom: header (list button, Mapcomp name button UI-25, '✓ Finalize'), toolbar, Search field, preview map, preview footer, Browser header, feature tree, and the feature action bar (FB-46) when features are selected.
- **Accept:** Unit: Playwright screenshot at 420×720 shows the seven fixed regions in this order, and the action bar appears below the tree only with a selection; AE: same layout inside the docked panel.
- **Evidence:** SOURCED · **MS:** M1

### UI-03 · Mapcomp bar / Mapcomp List opener
- **Req:** Top-left header control opens the Mapcomp List.
- **Accept:** Unit: click opens the list view; AE: works with 0, 1 and 20 Mapcomps.
- **Evidence:** SOURCED · **MS:** M1

### UI-04 · Mapcomp List
- **Req:** Mapcomp List shows every Mapcomp with thumbnail, name, imagery on/off toggle (UI-27), 'A' labels toggle, 'Preview' pill, lock, and the 'Prefer for …' settings (UI-26); footer icon + 'Back'. Clicking a row selects it and closes the List.
- **Accept:** AE: create 3 Mapcomps → 3 rows; clicking a row selects it, closes the List and the header shows its name; renaming in AE updates the row on refresh.
- **Evidence:** SOURCED · **MS:** M1

### UI-05 · Mapcomp List hover buttons
- **Req:** Row hover reveals Settings, Duplicate, Remove and ✓ Finalize-this-Mapcomp buttons. Default on-screen order = EBE narration order (Settings, Duplicate, Remove, Finalize); EBE has no screenshots, so confirm the order against STADIA / hands-on.
- **Accept:** Unit: hover shows the 4 buttons with tooltips (order: narration order, to confirm); AE: each button triggers MCS-09, MCS-22, UI-07, FIN-01 for that row only.
- **Evidence:** SOURCED · **MS:** M1

### UI-06 · "+ Create new Mapcomp" row
- **Req:** '+ Create new Mapcomp' row starts the two-step create dialog.
- **Accept:** Unit: click opens step 1 (MCS-03). AE: completing the dialog adds exactly one Mapcomp.
- **Evidence:** SOURCED · **MS:** M1

### UI-07 · Remove Mapcomp
- **Req:** Removing a Mapcomp asks for confirmation; if linked Mapcomps exist the dialog offers removing all linked ones.
- **Accept:** AE: delete with Cancel changes nothing; delete confirms removes Mapcomp, containing-comp layer and anchor; linked set → option removes all; one Undo restores.
- **Evidence:** SOURCED · **MS:** M1

### UI-08 · "A" icon = Mapcomp Labels on/off per Mapcomp
- **Req:** 'A' icon toggles automatic Mapcomp labels per Mapcomp.
- **Accept:** AE: toggling on adds that Mapcomp's labels, toggling off removes/hides them on next render, other Mapcomps unaffected; state saved in the .aep.
- **Evidence:** SOURCED · **MS:** M3

### UI-09 · "Preview" pill and lock icon in list rows
- **Req:** Show a 'Preview' pill and a lock per row. The lock locks the Mapcomp (EBE); default: it toggles the AE lock of the Mapcomp layer. Preview pill meaning to confirm (default: not finalized).
- **Accept:** AE: pill visible before finalize and hidden after; lock toggles AE layer lock of the Mapcomp layer. Re-check against original (OPEN_QUESTIONS).
- **Evidence:** PARTLY INFERRED · **MS:** M2

### UI-10 · Collapse linked Mapcomps
- **Req:** Linked/controller Mapcomps group under their controller and collapse.
- **Accept:** Unit: list of 1 controller + 2 linked renders as a collapsible group; AE: state matches project after reopen.
- **Evidence:** SOURCED · **MS:** M1

### UI-11 · Interactive preview map
- **Req:** Interactive preview map (pan, zoom, pitch, bearing) showing the selected Mapcomp's view frustum; never blank. Top controls: top-left 'Find features at coordinate' (FB-11), a Mapcomp View overlay toggle (hides the overlay; EBE "It will still uh keep everything locked up"; I: link unaffected), and a preview-imagery switch (e.g. the Mapcomp's final imagery).
- **Accept:** Unit: Playwright drags/zooms/rotates, frustum polygon matches projection of comp corners within 1 px; overlay toggle hides the frustum (I: link state unchanged; confirm in AE); imagery switch changes the preview style; regression test for blank map after panel reload.
- **Evidence:** PARTLY INFERRED · **MS:** M1
- **Free substitute:** MapLibre GL JS with free style (OpenFreeMap) when no MapTiler key.

### UI-12 · Preview footer
- **Req:** Preview footer, left to right: Keyframe button (keys Lat/Lon/Zoom/Bearing/Pitch of the selected Mapcomp at CTI, or of its controller when linked), 'Link Mapcomp View' chain-link (UI-13), zoom − / slider (fine, drag) / zoom + (big steps), reset rotation (north up, flat).
- **Accept:** Unit: zoom buttons step, slider drags fine zoom, reset sets bearing 0 and pitch 0; chain-link toggles link state (UI-13). AE: Keyframe adds one key on each of the 5 controls at CTI; on a linked Mapcomp the keys land on the controller.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### UI-13 · Mapcomp sync / "Link and Unlink control map with preview map"
- **Req:** 'Link Mapcomp View' toggle: linked, the preview follows the Mapcomp view at CTI and moving it writes the view (keys for changed parameters once keyframed); unlinked, the preview is free; re-linking snaps the preview back to the Mapcomp's keyed view.
- **Accept:** AE: with link on, scrubbing the timeline moves the preview; after Keyframe, moving CTI and panning adds keys only on the changed controls. With link off, panning changes nothing; re-link snaps back. Min/max-zoom Mapcomps behave with link off.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### UI-14 · Preview map language
- **Req:** Preview map labels use the default language preference.
- **Accept:** Unit: pref 'de' → preview place labels in German (name:de) where available.
- **Evidence:** SOURCED · **MS:** M3

### UI-15 · Preview map click actions
- **Req:** Clicking the preview finds features at that point and selects matching Browser features, including lines clicked between vertices.
- **Accept:** Unit: click on a known polygon selects it in the tree; click on a line segment midpoint selects the line.
- **Evidence:** SOURCED · **MS:** M4
- **Free substitute:** Find-at-point via OpenFreeMap vector tiles + Natural Earth when no MapTiler key.

### UI-16 · Preview overlays
- **Req:** Preview overlays the hovered search result/collection outline with 'Name (count)' label; optional bounding boxes.
- **Accept:** Unit: hover 'Deutschland' collection shows outline and 'Deutschland (3)'; PRF-05 on draws bbox.
- **Evidence:** SOURCED · **MS:** M4

### UI-17 · Toolbar row layout
- **Req:** Toolbar row in EBE narrated order (SOURCED: functions and order): Pin, Create Controller Mapcomp, Create 3D Landscape, Draw Diagram, Scripts; label area (template dropdown, a manual-label button (icon unknown) that shows a crosshair while active, Swap Label Template); Draw Arrow, Create a Shape Layer to Draw On, Shape Layer Style button, Swap Shape Layer Style; each with a tooltip. Premium buttons (INFERRED: Watermask, 3D buildings) only with a MapTiler premium plan (UI-28); their slot is INFERRED (default: between Create Controller Mapcomp and Create 3D Landscape, from the STADIA icon order). Icon ↔ function matching is INFERRED (narrated order vs 2023 STADIA icons; U-01); MapForge uses its own icons.
- **Accept:** Unit: without a premium-plan MapTiler key the 10 non-premium controls render in this order with tooltips; with a premium-plan key the two premium buttons appear; each is wired to its action per TB-15. Tooltip text and premium-icon positions to confirm (U-01, C-21).
- **Evidence:** PARTLY INFERRED · **MS:** M7

### UI-18 · Label Template selector dropdown (toolbar centre)
- **Req:** Label Template selector dropdown listing templates plus 'Null Layer', with keyword filter, swap, add new template, import templates and download default templates; the selected template is used for labelling; ⇄ swaps the template on selected labels.
- **Accept:** AE: dropdown lists project templates; filter narrows by keyword; add/import/download each add templates to the list; choosing 'Null Layer' then adding a label creates a null at the location; ⇄ swaps (LBL-12).
- **Evidence:** PARTLY INFERRED · **MS:** M6

### UI-19 · Flyout (≡) / context menu
- **Req:** Flyout menu with, in order: Support (Support dialog, LIC-19), Projects, Preferences, Help; plus troubleshooting entries (location to confirm). About & Licensing: (I) default = sections of the Support dialog (LIC-03/LIC-19); a separate About entry stays possible until confirmed hands-on.
- **Accept:** AE: entries appear in this order and each opens its view; flyout events handled via CSInterface.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### UI-20 · Fullscreen sub-panels and animated UI
- **Req:** Sub-views (list, filters, results, settings) take the full panel with a Back button and animated transitions.
- **Accept:** Unit: navigating into and back restores previous scroll/selection; transitions ≤300 ms and can be disabled.
- **Evidence:** SOURCED · **MS:** M0

### UI-21 · Generic modal dialogs
- **Req:** Generic modals: flat-object editor and action-button chooser, each with title and message; reused by missing-label-data and duration prompts.
- **Accept:** Unit: modal renders strings/numbers/booleans with correct inputs; Cancel/Apply return values per API-30/31.
- **Evidence:** PARTLY INFERRED · **MS:** M0

### UI-22 · Quick Start Buttons
- **Req:** Quick Start buttons when the project has no Mapcomp (contents to confirm).
- **Accept:** Unit: empty project shows buttons; each creates a working Mapcomp preset.
- **Evidence:** SOURCED · **MS:** M1

### UI-23 · Known UI strings / errors to reproduce
- **Req:** Reproduce the known user-facing strings and errors (with MapForge wording where the original names itself).
- **Accept:** Unit: snapshot test of string table; AE: trigger each error path (unlicensed style, failed tiles, large draw, '#' path) and see the message.
- **Evidence:** SOURCED · **MS:** M2

### UI-24 · GitHub Browser panel
- **Req:** GitHub Browser panel: title, user/repo fields (lockable), 'Flatten folders', filter, action button (default 'Import to MapForge').
- **Accept:** Unit: browse aourednik/historical-basemaps (non-'master' default branch), filter, flatten, import a GeoJSON into the Browser.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### UI-25 · Header Mapcomp-name button (select / open / return)
- **Req:** The header Mapcomp name is a button that also closes the Mapcomp List. Click cycle: if the Mapcomp layer is not selected in the containing comp, select it; next click opens the Mapcomp; next click returns to the containing comp (if already selected, the first click opens it). Several containing comps: Q-55 (default: the one the Mapcomp was opened from, else the first found).
- **Accept:** AE: in a containing comp with 100 layers and nothing selected, click 1 selects the Mapcomp layer, click 2 opens the Mapcomp, click 3 returns to the containing comp; with the layer pre-selected, click 1 opens the Mapcomp. Unit: clicking it while the List is open closes the List.
- **Evidence:** SOURCED · **MS:** M1

### UI-26 · Preferred Mapcomp ("Prefer for …") in the Mapcomp List
- **Req:** Per-Mapcomp 'Prefer for label templates' and 'Prefer for shape layer styles' settings in the Mapcomp List; with Draw on preferred Mapcomp (DRW-28) on, the List shows 'Prefer for drawing features'. Default: one preferred Mapcomp per kind, stored in the .aep.
- **Accept:** Unit: setting a Mapcomp as preferred clears the previous one of that kind; label text switches to 'Prefer for drawing features' when DRW-28 is on. AE: preference survives save/reopen. Control form to confirm (Q-19).
- **Evidence:** PARTLY INFERRED · **MS:** M5

### UI-27 · Imagery on/off per Mapcomp (List row toggle)
- **Req:** The blue imagery icon in each List row toggles that Mapcomp's imagery on/off.
- **Accept:** AE: toggling off hides the Mapcomp's imagery (labels and overlays unaffected) and is saved in the .aep; toggling on restores it without re-downloading cached tiles.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### UI-28 · MapTiler-plan-dependent panel buttons
- **Req:** Buttons for MapTiler premium features (default: Watermask Mapcomp, 3D buildings; to confirm) are shown only with a MapTiler premium plan (a key with premium access; EBE ties them to "a premium data subscription plan", not to having a key). Button list INFERRED; how premium access is detected is INFERRED (e.g. probing a premium-only endpoint).
- **Accept:** Unit: with no key or a Free-plan key the buttons are hidden; with a key that has premium access they appear in the toolbar positions of UI-17; an invalid key shows the LIC-05 dialog instead.
- **Evidence:** PARTLY INFERRED · **MS:** M2
- **Free substitute:** With no MapTiler key, MapForge may still offer the free watermask/buildings paths (IMG-17, 3D-03) behind the same buttons as a declared hobby deviation (Q-10).


## 2. Mapcomp creation & settings (MCS)

### MCS-01 · Mapcomp concept
- **Req:** A Mapcomp is an AE comp filled with map imagery and/or labels, the unit of every project.
- **Accept:** AE: a created Mapcomp is a CompItem tagged as Mapcomp (metadata) and listed by getMapcomps().
- **Evidence:** SOURCED · **MS:** M1

### MCS-02 · Entry points to create a Mapcomp
- **Req:** Entry points: Projects 'New project', '+ Create new Mapcomp', Quick Start, toolbar Watermask, toolbar 'Create 3D Landscape', toolbar Create Controller Mapcomp (MCS-28); texture/choropleth via styles.
- **Accept:** AE: each entry point produces a valid Mapcomp (or 3D landscape) recognised by getMapcomps().
- **Evidence:** PARTLY INFERRED · **MS:** M1

### MCS-03 · Create dialog, step 1 "Composition options and location"
- **Req:** Create step 1: Name, width×height, duration, framerate prefilled from the selected comp (else PRF-01); navigate/search location; Next.
- **Accept:** AE: with a 1920×1080 25 fps comp selected, fields prefill those values; with nothing selected, prefs apply.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### MCS-04 · Create dialog, step 2 "Map Style"
- **Req:** Create step 2: imagery preset strip (+), color preset, individual colors, live preview; Cancel/Create; locked styles open LIC-05.
- **Accept:** Unit: picking a preset updates preview within 1 s; locked style without key opens the licence dialog.
- **Evidence:** SOURCED · **MS:** M1
- **Free substitute:** Free default style set (OpenFreeMap/Natural Earth) instead of MapTiler.

### MCS-05 · What creation produces in AE
- **Req:** Creation adds the Mapcomp as a layer in the selected comp (containing comp), an '<Mapcomp> Anchor' layer directly above, a 'containing <name>' comp when starting from scratch, all under a 'MapForge Items' folder.
- **Accept:** AE: structure, names, parenting and folder match; renderer left at AE default (Classic 3D) unless decided otherwise.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### MCS-06 · Mapcomp anchor layer
- **Req:** Anchor layer in the containing comp follows the Mapcomp view so layers can be parented; anchors can be pinned.
- **Accept:** AE: parent a solid to the anchor at Berlin; animate Lat/Lon/Zoom/Bearing; solid stays on Berlin within 1 px at 10 sampled frames. Unit: expression math vs MapLibre project().
- **Evidence:** PARTLY INFERRED · **MS:** M1

### MCS-07 · MapPivot layer
- **Req:** Internal 'MapPivot' layer carrying the pitch/bearing expressions.
- **Accept:** AE: MapPivot exists, expressions non-empty and error-free in both engines.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### MCS-08 · Automatic Mapcomp naming
- **Req:** Auto-name 'Mapcomp 01', 'Mapcomp 02'… unique with leading zeros.
- **Accept:** Unit + AE: creating 3 Mapcomps yields 01, 02, 03; deleting 02 then creating gives a unique name.
- **Evidence:** SOURCED · **MS:** M1

### MCS-09 · Mapcomp Settings dialog
- **Req:** 'Mapcomp Settings' dialog with fields MCS-10…19 and the link-view control (MCS-29); Cancel/Apply; Apply re-renders.
- **Accept:** AE: change any field, Apply → comp updated and imagery re-rendered; Cancel → no change.
- **Evidence:** SOURCED · **MS:** M1

### MCS-10 · Settings: Name
- **Req:** Settings: Name renames the Mapcomp and fixes all dependent expressions.
- **Accept:** AE: rename with pinned layers/labels present; no expression errors (autoFixExpressions).
- **Evidence:** SOURCED · **MS:** M1

### MCS-11 · Settings: Duration
- **Req:** Settings: Duration changes the Mapcomp and linked Mapcomps' durations.
- **Accept:** AE: set 30 s → Mapcomp, containing layer, and linked Mapcomps all 30 s.
- **Evidence:** SOURCED · **MS:** M1

### MCS-12 · Settings: Language
- **Req:** Settings: Language dropdown; default for geocode() and (I) Mapcomp labels.
- **Accept:** AE: set German → geocode() without language returns German names; labels re-render in German.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### MCS-13 · Settings: Max Zoom / Min Zoom
- **Req:** Settings: Max Zoom / Min Zoom sliders limiting imagery tile zoom (overzoom beyond max).
- **Accept:** AE: Max 12 at view zoom 15 → finalized tiles are z12 upscaled, not blank.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### MCS-14 · Settings: Attribution
- **Req:** Settings: Attribution position dropdown, show/hide checkbox, 'attribution info' button; render provider attribution as a text layer honouring RTL.
- **Accept:** AE: attribution text appears in chosen corner with provider string; hidden when unchecked; info button opens provider terms URL; RTL paragraph stays aligned.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### MCS-15 · Settings: Imagery section
- **Req:** Settings: Imagery grid with badges (subscription, missing key '!', loading) and '+' new style tile.
- **Accept:** Unit: grid lists library styles; key-missing style shows '!' and clicking prompts for key.
- **Evidence:** SOURCED · **MS:** M2

### MCS-16 · Settings: Quality
- **Req:** Settings: Quality slider (default from PRF-12) controlling imagery resolution/zoom offset.
- **Accept:** AE: Quality 50% vs 100% → finalized imagery at lower vs full tile zoom; measurable sharpness difference.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### MCS-17 · Settings: Blend Duration
- **Req:** Settings: Blend Duration (default 0.3 s) cross-fading imagery between zoom levels.
- **Accept:** AE: zoom animation across tile levels shows a cross-fade of the set duration, no pops.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### MCS-18 · Settings: Imagery Motion Blur
- **Req:** Settings: Imagery Motion Blur toggle.
- **Accept:** AE: on → fast pan frames show motion blur on imagery; off → none.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### MCS-19 · Mapcomp Label settings
- **Req:** Mapcomp Label settings area with a 'Create Label Templates' button.
- **Accept:** AE: button creates template comps derived from the Mapcomp label style (LBL-14).
- **Evidence:** PARTLY INFERRED · **MS:** M6

### MCS-20 · Pitch limit depends on aspect ratio
- **Req:** Pitch limit depends on aspect ratio (square/portrait pitch more).
- **Accept:** Unit: maxPitch(1920×1080) < maxPitch(1080×1080) < maxPitch(1080×1920); AE: UI clamps accordingly.
- **Evidence:** SOURCED · **MS:** M1

### MCS-21 · Linked / Controller Mapcomps
- **Req:** Linked/controller Mapcomps share view, durations, collapse together and can be removed together. A controller is a Mapcomp without imagery or labels (MCS-28); links are made in Settings (MCS-29); the Keyframe button on a linked Mapcomp keys the controller.
- **Accept:** AE: animate controller view → linked Mapcomps follow at every frame; duration change propagates; Keyframe on a linked Mapcomp adds keys on the controller, not on the linked comp.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### MCS-22 · Duplicate Mapcomp
- **Req:** Duplicate dialog with (at least) two scope options and a Link toggle (weak; linking is confirmed only via Settings, MCS-29); updates expressions.
- **Accept:** AE: duplicate Mapcomp-only and with-containing-comp both produce error-free copies; Link makes the copy follow the original.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### MCS-23 · Multi-Mapcomp layer split
- **Req:** Split a map into linked Mapcomps (imagery vs labels).
- **Accept:** AE: 'Main Map' imagery + 'Main Labels' labels-only linked Mapcomps stay aligned while animating.
- **Evidence:** SOURCED · **MS:** M1

### MCS-24 · Expression-rig robustness
- **Req:** Expression rig works in Legacy and JavaScript engines, with 'Separate Dimensions' pref, at large zoom and with odd Zoom easing.
- **Accept:** Unit: generated expressions lint as ES3 and run in vm with both mock engines; AE: test project passes in both engines with the pref on and off.
- **Evidence:** SOURCED · **MS:** M0

### MCS-25 · Overlay layers are 3D layers on one Z plane
- **Req:** Overlay layers are 3D layers on one Z plane; avoid z-fighting (1 px Z offsets where we stack).
- **Accept:** AE: stacked drawn layers show no flicker in Classic 3D and C4D.
- **Evidence:** SOURCED · **MS:** M1

### MCS-26 · Corrupted-project troubleshooting
- **Req:** Troubleshooting tool repairing projects corrupted by AE 'Recovering' Mapcomps; relink missing imagery.
- **Accept:** AE: simulate missing imagery folder → tool relinks or re-finalizes; corrupted metadata → repaired list.
- **Evidence:** PARTLY INFERRED · **MS:** M10

### MCS-27 · Minimum and over-zoom safety
- **Req:** Zooming below imagery minzoom must not load forever; min-zoom previews not empty.
- **Accept:** Unit: request planner returns finite tile set for zoom < minzoom; preview shows overzoomed/underzoomed fallback.
- **Evidence:** SOURCED · **MS:** M1

### MCS-28 · "Create new Controller Mapcomp" (toolbar)
- **Req:** Toolbar button that creates a Controller Mapcomp: a new comp named 'controller' (unique: 'controller 2'…), with the view controls but no imagery or labels, listed in the Mapcomp List. Default specs and view from the selected Mapcomp.
- **Accept:** AE: click → comp 'controller' appears in the project and in the List, renders no imagery/labels, has the 5 view controls; second click → 'controller 2'.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### MCS-29 · Settings: link view to a controller
- **Req:** Mapcomp Settings control that links the Mapcomp's view to a chosen Controller Mapcomp (default form: dropdown of controllers + 'None'; 'None' unlinks and bakes the current values).
- **Accept:** AE: choose 'controller' + Apply → the Mapcomp follows the controller's view at every frame and is grouped under it in the List (UI-10); choose 'None' → it keeps its current view and stops following.
- **Evidence:** PARTLY INFERRED · **MS:** M1


## 3. View animation (VIEW)

### VIEW-01 · View controls Latitude / Longitude / Zoom / Bearing / Pitch
- **Req:** Five keyframeable controls Latitude, Longitude, Zoom, Bearing, Pitch per Mapcomp, readable from expressions.
- **Accept:** AE: controls exist on the agreed layer; keyframing them animates imagery and overlays; user expression `...effect("Zoom")(1)` reads the value.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### VIEW-02 · Animate by navigating the preview
- **Req:** Footer Keyframe button keys all 5 view controls at CTI (on the controller if linked); afterwards, with Link Mapcomp View on, navigating the preview at another CTI keys the changed controls; manual keyframing also works.
- **Accept:** AE: Keyframe → 5 keys at CTI; move CTI, pan only → keys added on Lat/Lon only; graph-editor edits respected.
- **Evidence:** SOURCED · **MS:** M1

### VIEW-03 · Set view at time (API)
- **Req:** setViewAtTime semantics: static set when unkeyed and not forced; key added when keyed or forced; time in containing-comp time.
- **Accept:** Unit (mock host): three cases produce setValue vs setValueAtTime; AE: same.
- **Evidence:** SOURCED · **MS:** M1

### VIEW-04 · Fit view to bbox
- **Req:** Fit view to bbox (API; UI: search-result 'fit the map view' and the 'Fit feature to view' menu's plain entry).
- **Accept:** Unit: fitView([5,47,15,55]) gives a view whose frustum contains the bbox with ≤10% margin; AE: both UI actions do the same without creating keys.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### VIEW-05 · Animate View to Feature
- **Req:** Animate View to Feature: keys from current view to fit the feature with eased keys; offered as a keyframe-marked entry of the 'Fit feature to view' menu (FB-46); entry name to confirm (Q-51).
- **Accept:** AE: one click → 2 keys per control, ease applied, flight path without overshoot below min zoom.
- **Evidence:** SOURCED · **MS:** M7

### VIEW-06 · Animate View Along Features
- **Req:** Animate View Along Features (one or several, smoothed path; works for imported GPX tracks). (I) Offered as a keyframe-marked entry of the Fit feature to view menu (FB-46); entry name to confirm (Q-51).
- **Accept:** AE: along a 50 km GPX track the view centre stays within N px of the smoothed line; no jitter frame-to-frame.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### VIEW-07 · Animate View Between Features
- **Req:** Animate View Between Features in sequence. (I) Offered as a keyframe-marked entry of the Fit feature to view menu (FB-46); entry name to confirm (Q-51).
- **Accept:** AE: selecting 3 features creates fly-to segments visiting each in selection order.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### VIEW-08 · One-click flyover animations
- **Req:** One-click flyover animations (button names to confirm). (I) Offered as a keyframe-marked entry of the Fit feature to view menu (FB-46); entry name to confirm (Q-51).
- **Accept:** AE: flyover action produces a continuous orbit/pan keyframe set; loopable.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### VIEW-09 · Mapcomp View features
- **Req:** Browser '+' > 'Mapcomp view' asks for a name (Apply) and stores the current view incl. pitch and bearing as a feature; animate to it; Draw Mapcomp View script.
- **Accept:** AE: create 'world view 1', change pitch/bearing, create 'world view 2', animate to view 1 → view (incl. pitch/bearing) restored exactly.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### VIEW-10 · Animate View Along a Custom Feature Path
- **Req:** Animate view along a custom feature path made from a drawn layer.
- **Accept:** AE: draw path → make feature (FB-30) → animate view along it.
- **Evidence:** SOURCED · **MS:** M7

### VIEW-11 · Duration prompt
- **Req:** Prompt for duration whenever an animation is created.
- **Accept:** Unit: every animation action opens the duration modal; Cancel aborts with no keys.
- **Evidence:** SOURCED · **MS:** M7

### VIEW-12 · Remove view keyframes
- **Req:** Remove all view keyframes (API + UI).
- **Accept:** AE: removeMapcompControlKeys leaves 0 keys on all five controls, values at current time kept.
- **Evidence:** SOURCED · **MS:** M1

### VIEW-13 · Zoom interpolation safety
- **Req:** Zoom interpolation safety: clamp, no expression errors on bezier overshoot.
- **Accept:** AE: extreme overshooting Zoom ease produces no expression error and no blank frame.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### VIEW-14 · Linked view propagation
- **Req:** Linked Mapcomps follow the controller's view (link set in Settings, MCS-29).
- **Accept:** AE: same as MCS-21 per frame.
- **Evidence:** SOURCED · **MS:** M1

### VIEW-15 · Imagery motion blur during view moves
- **Req:** Motion blur during view moves (see MCS-18).
- **Accept:** AE: covered by MCS-18 test.
- **Evidence:** SOURCED · **MS:** M7


## 4. Imagery styles & map styling (IMG)

### IMG-01 · Imagery style library
- **Req:** Imagery style library (vector or raster) applied per Mapcomp; import/export multiple styles.
- **Accept:** Unit: save/load/import/export round-trip of the style library JSON; AE: style applied to two Mapcomps independently.
- **Evidence:** SOURCED · **MS:** M2

### IMG-02 · Locked-style indicators
- **Req:** Locked-style badges (subscription badge, red '!' for missing key) and key prompt.
- **Accept:** Unit: no key → MapTiler styles show '!' and click opens key prompt; valid key → badge only.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### IMG-03 · Basic (MapTiler imagery style)
- **Req:** 'Basic' imagery style with ocean/lake/river toggles and disputed-borders toggle.
- **Accept:** AE: toggling oceans off removes ocean fill in finalized frames; disputed borders on/off changes boundary rendering.
- **Evidence:** SOURCED · **MS:** M2
- **Free substitute:** OpenFreeMap (OpenMapTiles schema) style 'Basic' clone when no MapTiler key.

### IMG-04 · Advanced (MapTiler imagery style)
- **Req:** 'Advanced' style: water toggles, rail color, street density (≥1..5+), disputed borders.
- **Accept:** AE: street density 5 renders without black polygons; rail color applies to rail lines.
- **Evidence:** SOURCED · **MS:** M2
- **Free substitute:** OpenFreeMap-based 'Advanced' clone.

### IMG-05 · Lite (MapTiler)
- **Req:** 'Lite' style with disputed-borders toggle.
- **Accept:** AE: Lite renders with no key requirement beyond MapTiler Free (or substitute).
- **Evidence:** SOURCED · **MS:** M2
- **Free substitute:** OpenFreeMap 'positron'-like style.

### IMG-06 · MapTiler Satellite (V2)
- **Req:** Satellite imagery style (V2).
- **Accept:** AE: finalized satellite frames are sharp (no blur) at z0–17.
- **Evidence:** SOURCED · **MS:** M2
- **Free substitute:** MapTiler Free satellite-v2 key or EOX Sentinel-2 cloudless (non-commercial).

### IMG-07 · Import from MapTiler Cloud
- **Req:** Import and update maps from MapTiler Cloud (user and standard maps).
- **Accept:** Unit: given a key, list maps and import one as a style; update pulls new style.json.
- **Evidence:** SOURCED · **MS:** M2
- **Free substitute:** Also allow any style.json URL (OpenFreeMap, self-hosted).

### IMG-08 · Mapstyling inside AE
- **Req:** In-AE map styling: colors, .ase swatches, fonts, language, line widths, hillshading, feature-group visibility.
- **Accept:** Unit: import an .ase file → palette applied to style layers; toggling 'roads' group hides roads; AE: finalized output reflects edits.
- **Evidence:** PARTLY INFERRED · **MS:** M2
- **Free substitute:** Hillshade from AWS Terrarium DEM when no MapTiler terrain.

### IMG-09 · Generate a style from a composition's colors or from an image
- **Req:** Generate a style from a composition's colors or from an image.
- **Accept:** Unit: given an image, k-means palette (k≈6) mapped to style roles; AE: from active comp colors.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### IMG-10 · Color presets and individual colors
- **Req:** Color presets and individual colors in create dialog and after creation.
- **Accept:** Unit: preset switch updates preview; individual color picker edits one role.
- **Evidence:** SOURCED · **MS:** M2

### IMG-11 · Disputed borders toggle
- **Req:** Disputed borders toggle on Basic/Advanced/Lite.
- **Accept:** Unit: style filter on boundary.disputed toggles; AE: visible difference over Kashmir.
- **Evidence:** SOURCED · **MS:** M2

### IMG-12 · Bing imagery
- **Req:** Bing imagery: Aerial, Dark, Light, Gray with language.
- **Accept:** Unit: with an Enterprise key, metadata request resolves quadkey URLs; language param applied.
- **Evidence:** PARTLY INFERRED · **MS:** M2
- **Free substitute:** Bing Basic keys are retired: offer Azure Maps tilesets (user key) and Esri/EOX as substitutes.

### IMG-13 · CartoDB imagery
- **Req:** CartoDB imagery styles with API key entry.
- **Accept:** Unit: key appended as ?key=; without key watermark warning shown.
- **Evidence:** SOURCED · **MS:** M2
- **Free substitute:** CARTO free key (non-commercial).

### IMG-14 · Stamen style (REMOVED 1.6.4)
- **Req:** No built-in Stamen style; document Stadia custom-URL migration.
- **Accept:** Doc + Unit: Stadia URL with api_key works as custom raster.
- **Evidence:** SOURCED · **MS:** M2

### IMG-15 · Natural Earth Imagery
- **Req:** Natural Earth Imagery (no subscription, world scale).
- **Accept:** AE: world-scale finalize with no network keys produces NE imagery.
- **Evidence:** SOURCED · **MS:** M2
- **Free substitute:** Bundled Natural Earth (public domain).

### IMG-16 · Natural Earth Admin Boundaries Imagery
- **Req:** Natural Earth Admin Boundaries Imagery.
- **Accept:** AE: admin-0/1 boundaries render correctly for all countries.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### IMG-17 · Watermask Mapcomps
- **Req:** Watermask Mapcomps: premium toolbar version and Natural Earth Water Mask.
- **Accept:** AE: watermask Mapcomp produces white water / black land (or agreed polarity) usable as track matte, aligned with base map.
- **Evidence:** SOURCED · **MS:** M2
- **Free substitute:** Watermask from OpenFreeMap water layer or Natural Earth/OSM water polygons.

### IMG-18 · "Mapcomp Imagery" editor (custom raster style)
- **Req:** 'Mapcomp Imagery' raster editor: Name, URI, '…' local picker, scheme xyz/tms, min/max zoom, tile size, bounds, Hue/Opacity/Black(+) sliders; save/import/export; Cancel/Apply.
- **Accept:** Unit: editor saves a style object with all fields; live preview reflects Hue/Opacity; AE: Apply then assign renders custom tiles.
- **Evidence:** PARTLY INFERRED · **MS:** M2
- **Free substitute:** Default URI should not be tile.openstreetmap.org for finalize (bulk ban); use OpenFreeMap raster or CARTO.

### IMG-19 · Universal Raster Style ("Default Tiles")
- **Req:** Built-in 'Universal Raster' profile with default tiles.
- **Accept:** AE: new Universal Raster Mapcomp renders without configuration.
- **Evidence:** PARTLY INFERRED · **MS:** M2
- **Free substitute:** Default tiles from a bulk-tolerant free source.

### IMG-20 · Tile URI template variables
- **Req:** Tile URI template variables {z}{x}{y}, {year}{month}{date}{dateISO}, @2x/query passthrough; optional {s},{q},{-y}, uppercase {X}{Y}.
- **Accept:** Unit: template engine table test (quadkey, TMS flip, date at comp time).
- **Evidence:** PARTLY INFERRED · **MS:** M2

### IMG-21 · XYZ / TMS schemes
- **Req:** XYZ and TMS schemes.
- **Accept:** Unit: tms flips y = 2^z−1−y.
- **Evidence:** SOURCED · **MS:** M2

### IMG-22 · Map Bounds, Min/Max zoom per raster style, overzoom
- **Req:** Map Bounds, per-style min/max zoom, overzoom with overzoomed textures.
- **Accept:** Unit: no requests outside bounds; above maxzoom parent tiles are upscaled (not lower-zoom stitched).
- **Evidence:** SOURCED · **MS:** M2

### IMG-23 · Local tile directories
- **Req:** Local tile directories with metadata.json min/max zoom.
- **Accept:** Unit: folder with metadata.json (string zooms) parsed; missing metadata defaults 0–22.
- **Evidence:** SOURCED · **MS:** M2

### IMG-24 · Georeferenced imagery workflow
- **Req:** Georeferenced imagery workflow (GeoTIFF → tiles → style).
- **Accept:** Doc + AE: gdal2tiles output folder used as imagery, aligned with base map within 1 px at z10.
- **Evidence:** SOURCED · **MS:** M2

### IMG-25 · Mapbox Studio styles
- **Req:** Mapbox Studio styles via raster URL with token.
- **Accept:** Unit: template with access_token works.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### IMG-26 · "Seamless Texture" image profile / Texture Mapcomp
- **Req:** 'Seamless Texture' imagery profile building a tileset from a user texture.
- **Accept:** AE: pick texture → Mapcomp tiles it seamlessly at all zooms; finalize → blend mode set.
- **Evidence:** SOURCED · **MS:** M2

### IMG-27 · URL-sourced style update
- **Req:** URL-sourced styles update with one click (imagery and shape styles, C-07).
- **Accept:** Unit: style with sourceUrl → update fetches and replaces; offline → error, old style kept.
- **Evidence:** SOURCED · **MS:** M2

### IMG-28 · Import GEOlayers 2 server profiles as imagery styles
- **Req:** Import GEOlayers 2 server profiles as imagery styles.
- **Accept:** Unit: parse sample GL2 profile (format to obtain) into a raster style. Blocked until a sample file exists (OPEN_QUESTIONS).
- **Evidence:** SOURCED · **MS:** M2

### IMG-29 · Imagery thumbnail grid (observed contents)
- **Req:** Ship a thumbnail grid with the built-in style set (≈30 tiles incl. transparent, satellite, NE, dark/light, watermasks, dataviz).
- **Accept:** Unit: built-in style catalogue count and thumbnails generated; names to confirm (U-04).
- **Evidence:** SOURCED · **MS:** M2

### IMG-30 · Dataviz imagery styles
- **Req:** Dataviz imagery styles: Choropleth, Circles, Heatmap, Lines.
- **Accept:** AE: each style renders a GeoJSON into imagery (see DAT-15..18).
- **Evidence:** SOURCED · **MS:** M8

### IMG-31 · Elevation / DEM tiles
- **Req:** Elevation/DEM tiles for 3D landscapes.
- **Accept:** Unit: Terrarium/terrain-rgb decode to metres matches reference values ±1 m.
- **Evidence:** SOURCED · **MS:** M9
- **Free substitute:** AWS Terrarium (free, no key).

### IMG-32 · Map projection
- **Req:** Web Mercator imagery; globe/Equal Earth via 3D-10.
- **Accept:** Unit: projection functions round-trip lat/lon within 1e-9.
- **Evidence:** SOURCED · **MS:** M9

### IMG-33 · WMS imagery (decision needed)
- **Req:** Decide WMS: support GetMap as a raster source, or define GL2 WMS profile import behaviour.
- **Accept:** Unit (if supported): WMS GetMap bbox per tile in EPSG:3857 returns aligned tiles.
- **Evidence:** PARTLY INFERRED · **MS:** M2


## 5. Finalize & tile rendering / caching (FIN)

### FIN-01 · Finalize
- **Req:** Finalize renders final imagery into the Mapcomp(s) from the header button (next to the name button), the List row ✓ (finalize that Mapcomp), or API.
- **Accept:** AE: after finalize, every frame of the Mapcomp shows full-quality imagery matching the controls; second finalize with no changes reuses cache.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### FIN-02 · Finalize options
- **Req:** Finalize options onlyCurrentFrame, previewQuality, onlyWorkArea, purgeImageryCache (defaults false). UI: plain click = whole composition; CTRL/CMD-click = onlyCurrentFrame (SOURCED); other options' UI per Q-29 default until confirmed.
- **Accept:** AE: plain click renders every frame; CTRL/CMD-click renders only the CTI frame; each other option measurably changes the rendered frame range/quality/cache.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### FIN-03 · "3 Ways to Finalize Imagery"
- **Req:** '3 ways to finalize': whole composition (plain click) and current frame (CTRL/CMD-click) are sourced; third (I: work area only) to confirm; finalize-all when no comp given.
- **Accept:** AE: each mode renders the expected frame count.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### FIN-04 · Work area matters
- **Req:** Respect the work area via onlyWorkArea; default (plain Finalize click) = whole comp (EBE narrator wording "entire composition" / "entire timeline"; work area not tested; I: not work-area-only).
- **Accept:** AE: onlyWorkArea=true renders only work-area frames; frames outside keep previous imagery. Confirm in AE (hands-on): does a plain click with a short work area render the whole comp?
- **Evidence:** PARTLY INFERRED · **MS:** M2

### FIN-05 · Quality → tile zoom
- **Req:** Quality maps to tile zoom offset.
- **Accept:** Unit: quality→zoomOffset table; AE: visible resolution change.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### FIN-06 · Queue renders after finalization
- **Req:** Option to add comps to the Render Queue after finalizing.
- **Accept:** AE: option on → containing comp added to RQ once.
- **Evidence:** SOURCED · **MS:** M2

### FIN-07 · Preview settings for real-time work
- **Req:** Lower preview quality for real-time work.
- **Accept:** AE: preview quality frames render ≥3× faster than final.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### FIN-08 · Imagery cache and purge
- **Req:** Disk imagery cache, purgeable.
- **Accept:** Unit: cache keyed by URL; purge removes files; AE: purge option forces refetch.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### FIN-09 · Tile rendering in Web Workers
- **Req:** Tile rendering in Web Workers without leaks.
- **Accept:** Unit: render 1000 tiles; heap and ImageBitmap count return to baseline.
- **Evidence:** SOURCED · **MS:** M2

### FIN-10 · Tilemerger
- **Req:** Tilemerger: stitch tiles, XYZ/TMS, wrap x across the antimeridian.
- **Accept:** Unit: mosaic for lon −190…−170 wraps correctly.
- **Evidence:** SOURCED · **MS:** M2

### FIN-11 · Missing / rejected tiles
- **Req:** No black holes on rejected tiles; (I) parent-tile fallback; report 'N Tile(s) couldn't be rendered'.
- **Accept:** Unit: 429/404 tiles filled from parent; count reported in UI.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### FIN-12 · Texture filtering and overzoom quality
- **Req:** Sharp imagery: correct texture filtering, overzoom, satellite.
- **Accept:** Unit: pixel-diff vs reference render under threshold.
- **Evidence:** SOURCED · **MS:** M2

### FIN-13 · DEM / server robustness
- **Req:** DEM/server failures never stall finalize (timeouts, retries).
- **Accept:** Unit: DEM server hangs → finalize completes with warning within timeout.
- **Evidence:** SOURCED · **MS:** M2

### FIN-14 · Imagery stored as PNG files imported as footage
- **Req:** Write PNGs atomically, validate, store on a stable path; import as footage.
- **Accept:** Unit: temp-then-rename writes; corrupted file detection; AE: no PNGIO errors on AE 25/26.
- **Evidence:** INFERRED · **MS:** M2

### FIN-15 · Async finalize with callback
- **Req:** Async finalize with callback(error, mapcomps).
- **Accept:** Unit (mock host): callback called once after all comps, with errors aggregated.
- **Evidence:** SOURCED · **MS:** M2

### FIN-16 · Known failure modes to avoid
- **Req:** Avoid known failures (missing portions, empty squares).
- **Accept:** AE: regression project with pitch 60 and bearing 45 shows full coverage.
- **Evidence:** SOURCED · **MS:** M2

### FIN-17 · Faster finalization than v2
- **Req:** Finalize speed: rendering happens while the user keyframes; finalize faster than naive per-frame fetch.
- **Accept:** Benchmark: 10 s 1080p25 Mapcomp finalizes in agreed budget on test machine.
- **Evidence:** SOURCED · **MS:** M2

### FIN-18 · Project portability: imagery storage, moving and collecting projects
- **Req:** Define imagery storage location and portability (Collect Files works; cache-drive changes recoverable).
- **Accept:** AE: Collect Files on a finalized project then open on a second machine → imagery present; moved cache → relink prompt.
- **Evidence:** PARTLY INFERRED · **MS:** M10


## 6. Mapcomp labels — automatic (MLB)

### MLB-01 · Mapcomp labels
- **Req:** Automatic Mapcomp labels placed, blended and animated for the view; default look uppercase letter-spaced grey with halo, dots, size by rank.
- **Accept:** AE: labels appear for cities in view, fade in/out with zoom, no overlaps; toggle with 'A'.
- **Evidence:** PARTLY INFERRED · **MS:** M3
- **Free substitute:** Natural Earth + OpenFreeMap place names (free).

### MLB-02 · Label data sources
- **Req:** Label sources: MapTiler (multilingual) and Natural Earth.
- **Accept:** Unit: source switch changes label set; NE works offline.
- **Evidence:** SOURCED · **MS:** M3
- **Free substitute:** OpenFreeMap/OpenMapTiles names as MapTiler substitute.

### MLB-03 · Languages
- **Req:** Languages incl. Welsh, Irish/Scottish Gaelic, Traditional Chinese, RTL.
- **Accept:** Unit: language list contains these; AE: Arabic labels render RTL correctly.
- **Evidence:** PARTLY INFERRED · **MS:** M3

### MLB-04 · Place dots
- **Req:** Place dots on Mapcomp labels.
- **Accept:** AE: dots render at place positions.
- **Evidence:** SOURCED · **MS:** M3

### MLB-05 · RTL rendering
- **Req:** RTL labels not reversed.
- **Accept:** AE: Hebrew/Arabic city names read correctly.
- **Evidence:** SOURCED · **MS:** M3

### MLB-06 · Label renaming applies to Mapcomp labels
- **Req:** Label renaming dictionaries apply to Mapcomp labels.
- **Accept:** Unit: dictionary 'Köln→Cologne' renames label.
- **Evidence:** SOURCED · **MS:** M3

### MLB-07 · Fonts and language for map labels via Mapstyling
- **Req:** Fonts and language of map labels via styling.
- **Accept:** AE: font change applies to Mapcomp labels.
- **Evidence:** SOURCED · **MS:** M3

### MLB-08 · Labels-only Mapcomp
- **Req:** Labels-only Mapcomp linked to an imagery Mapcomp.
- **Accept:** AE: labels-only Mapcomp has transparent background and aligns.
- **Evidence:** SOURCED · **MS:** M3

### MLB-09 · Label motion blur
- **Req:** Motion blur for label layers.
- **Accept:** AE: fast pan blurs labels when enabled.
- **Evidence:** PARTLY INFERRED · **MS:** M3

### MLB-10 · Representation in AE
- **Req:** Decide representation (AE text layers vs baked imagery); default: AE text layers in a labels precomp (I).
- **Accept:** AE: labels editable/inspectable per decision; no per-frame expression cost explosion (≥25 fps scrub on 200 labels in preview quality).
- **Evidence:** INFERRED · **MS:** M3

### MLB-11 · Mapcomp Label Templates
- **Req:** Mapcomp Label Templates (MCS-19/LBL-14).
- **Accept:** Covered by LBL-14 test.
- **Evidence:** SOURCED · **MS:** M3


## 7a. Search (SRC)

### SRC-01 · Search bar
- **Req:** Search field with clear ×, below the toolbar and in the create dialog; search on Enter. Misspelling handling: parity default = no fuzzy correction (EBE narrator remark, not demonstrated; weak; to confirm).
- **Accept:** Unit: typing does not send requests; Enter sends one; × clears. Parity default, to confirm: a misspelled query ('Germnay') is not corrected.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### SRC-02 · Results list
- **Req:** Results grouped under light-grey source headers (Natural Earth first, then Nominatim/OSM); rows with geometry icon, name, type subtitle; Back.
- **Accept:** Unit: 'Germany' shows the Natural Earth group first with country polygon, Inner Boundaries and collection rows, then the Nominatim group.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### SRC-03 · Result row hover actions
- **Req:** Row hover actions: fit, label, draw, add to browser.
- **Accept:** AE: each action works on a polygon result.
- **Evidence:** SOURCED · **MS:** M4

### SRC-04 · Feature preview
- **Req:** Hover previews outline + name/count; optional bbox.
- **Accept:** Unit: as UI-16.
- **Evidence:** SOURCED · **MS:** M4

### SRC-05 · OSM / Nominatim search
- **Req:** OSM Nominatim search: 1 req/s queue, geometry fallback to point, language = Mapcomp language.
- **Accept:** Unit: 5 rapid queries are spaced ≥1 s; result without geometry becomes a Point; UA header set.
- **Evidence:** SOURCED · **MS:** M4

### SRC-06 · MapTiler Search
- **Req:** MapTiler search with full geometry by id and worldviews; results always displayed.
- **Accept:** Unit (with key): country search returns polygon; worldview param sent.
- **Evidence:** SOURCED · **MS:** M4
- **Free substitute:** Nominatim + Natural Earth + Photon when no key.

### SRC-07 · Multiple search sources
- **Req:** Multiple search sources: NE, Browser, Browser Downloads, File (http, zipped shp), Folder, GitHub, OSM, MapTiler.
- **Accept:** Unit: each source adapter returns features for a fixture; File source without URL opens no popup.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### SRC-08 · Fallback online search service
- **Req:** Fallback search service when default fails; can be default.
- **Accept:** Unit: default returns error → fallback queried.
- **Evidence:** SOURCED · **MS:** M4
- **Free substitute:** Photon (komoot) as fallback.

### SRC-09 · Preload features for search on startup
- **Req:** Preload features for search on startup.
- **Accept:** Unit: pref on → sources indexed at boot.
- **Evidence:** SOURCED · **MS:** M4

### SRC-10 · Download a search result into the Browser
- **Req:** Download a search result into the Browser.
- **Accept:** Unit: ⤓ adds feature to Browser 'Features'.
- **Evidence:** SOURCED · **MS:** M4

### SRC-11 · Export search results for GIS software
- **Req:** Export search results for GIS software (GeoJSON).
- **Accept:** Unit: exported file validates as GeoJSON and opens in QGIS.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### SRC-12 · Historical basemaps via the GitHub source
- **Req:** Historical basemaps via GitHub source.
- **Accept:** Unit: connect aourednik/historical-basemaps, search 'Roman', import.
- **Evidence:** SOURCED · **MS:** M4

### SRC-13 · Coordinates input
- **Req:** Coordinate input ('lat, lon') in search.
- **Accept:** Unit: '48.137, 11.575' centres preview there and offers a point feature.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### SRC-14 · Natural Earth search by name (matching quality)
- **Req:** Natural Earth name search tolerant of case/diacritics and name_xx. Misspellings: parity default no fuzzy matching (EBE remark, weak; to confirm).
- **Accept:** Unit: 'koln', 'Köln', 'Cologne' all find Cologne. Parity default, to confirm: 'Colgone' does not.
- **Evidence:** PARTLY INFERRED · **MS:** M4


## 7b. Feature Browser (FB)

### FB-01 · Feature Browser
- **Req:** Feature Browser tree with nested collections; top-level '☆ Favorite Features' and '📄 Features'; users can create a feature collection (EBE); (I) from a '+' > 'Feature collection' entry, starting empty.
- **Accept:** Unit: tree renders 10k features virtualised; defaults present; a new collection appears (I: empty) and accepts 'add to feature collection'.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-02 · Browser header
- **Req:** Header: '+' ('Add features to browser') dropdown with Download features, Import file, Mapcomp view, Feature from layer, Feature collection; Filter…; point/line/polygon toggles; 'Preview selection on map' toggle (FB-45); one further icon (to confirm).
- **Accept:** Unit: '+' menu lists Download features, Import file first, then Mapcomp view, Feature from layer and (I) Feature collection (order of entries 3–5 to confirm), and each opens its flow; geometry toggles hide types; filter works.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-03 · Import File
- **Req:** Import File ('+' menu); CTRL/CMD-click imports from URL; files dropped onto the Browser are imported too.
- **Accept:** Unit: modifier opens URL prompt; supported formats parsed; a GeoJSON dropped on the Browser is imported.
- **Evidence:** SOURCED · **MS:** M4

### FB-04 · Remove features
- **Req:** Remove selected; SHIFT-click clears all except favorites.
- **Accept:** Unit: both behaviours.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-05 · Export features
- **Req:** Export selection as GeoJSON; SHIFT-click exports all but favorites to features.geojson beside the project.
- **Accept:** AE: SHIFT-export writes file next to .aep.
- **Evidence:** SOURCED · **MS:** M4

### FB-06 · Filter
- **Req:** Fast filter; filter by data.
- **Accept:** Unit: filter 10k features < 100 ms; property filter syntax (to define) works.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-07 · Collection naming and sorting
- **Req:** Sort by name (in the collection's Feature Properties view); rename collections; honour display-name property and let Feature Properties choose the name property.
- **Accept:** Unit: GEOlayersDisplayName/MapForgeDisplayName sets display name.
- **Evidence:** SOURCED · **MS:** M4

### FB-08 · Favorites
- **Req:** Favorites survive clear and SHIFT-export.
- **Accept:** Unit: clearBrowser keeps favorites.
- **Evidence:** SOURCED · **MS:** M4

### FB-09 · Selection
- **Req:** Multi-select (CTRL/CMD-click adds); preview click selects in tree; double-click zooms the preview to the feature; selection shows the action bar (FB-46).
- **Accept:** Unit: shift/ctrl multiselect; click selection sync; double-click fits the preview to the feature.
- **Evidence:** SOURCED · **MS:** M4

### FB-10 · Find Features by clicking the preview map
- **Req:** Find features by clicking the preview map.
- **Accept:** Unit: as UI-15.
- **Evidence:** SOURCED · **MS:** M4
- **Free substitute:** Vector tiles from OpenFreeMap + NE.

### FB-11 · Find Features at Coordinate
- **Req:** 'Find features at coordinate' button (top-left of the preview, approximate; I: crosshair icon by position match) shows the current view location's coordinate and lists all features there from large to small (country, Inner Boundaries, states collection, state, the point), with Back; Inner Boundaries correct for all countries.
- **Accept:** Unit: view centred on Kansas lists United States of America, Inner Boundaries, states, Kansas, then a point, in that order; Back returns; regression for previously broken countries.
- **Evidence:** SOURCED · **MS:** M4

### FB-12 · Current View features import
- **Req:** Current View features import with separate transport ramps.
- **Accept:** Unit: view over Munich imports roads/rail/water/buildings collections.
- **Evidence:** SOURCED · **MS:** M4
- **Free substitute:** Overpass or OpenFreeMap tiles instead of MapTiler.

### FB-43 · "Add Current View Feature"
- **Req:** 'Add Current View Feature' (view-extent feature), robust to geocode failure.
- **Accept:** Unit: works offline with fallback name.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-13 · Admin Bounds to browser (higher zooms)
- **Req:** Admin bounds for higher zooms to the browser.
- **Accept:** Unit: admin level 4–8 polygons for the view.
- **Evidence:** SOURCED · **MS:** M4
- **Free substitute:** Overpass boundary=administrative or geoBoundaries.

### FB-14 · Downloadable datasets
- **Req:** '+' > 'Download features' datasets: all countries (as one collection), NE Populated Places, lakes/rivers, NE land/water, states/provinces lines+polygons, disputed areas.
- **Accept:** Unit: each dataset loads with expected feature count.
- **Evidence:** SOURCED · **MS:** M4

### FB-15 · Get features by OSM id
- **Req:** Get features by OSM id (node/way/rel).
- **Accept:** Unit: rel id returns assembled multipolygon.
- **Evidence:** SOURCED · **MS:** M4

### FB-16 · Overpass queries into the Browser
- **Req:** Overpass queries with {{bbox}} (and other macros) and server auto-selection.
- **Accept:** Unit: macro expansion; server scoring by /api/status.
- **Evidence:** SOURCED · **MS:** M4

### FB-17 · Default Features on startup
- **Req:** Default features loaded on startup (files/URLs).
- **Accept:** Unit: pref list loaded at boot.
- **Evidence:** SOURCED · **MS:** M4

### FB-18 · Auto-load GeoJSONs next to the project
- **Req:** Auto-load GeoJSONs beside the project when opened via Projects panel.
- **Accept:** AE: open via panel → sibling GeoJSONs in Browser.
- **Evidence:** SOURCED · **MS:** M4

### FB-19 · Update URL-sourced collections with one click
- **Req:** One-click update of URL-sourced collections (store import URL separately).
- **Accept:** Unit: collection imported from URL updates on click; `source` property untouched.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-20 · Merge Features
- **Req:** Merge features (union), non-blocking, cancelable.
- **Accept:** Unit: union of two touching polygons; cancel mid-way leaves data intact.
- **Evidence:** SOURCED · **MS:** M4

### FB-21 · Subtract Features
- **Req:** Subtract features (difference), non-blocking, cancelable.
- **Accept:** Unit: country minus ocean = land part.
- **Evidence:** SOURCED · **MS:** M4

### FB-22 · Merge Features in Collections by name
- **Req:** Merge features in collections by name.
- **Accept:** Unit: features sharing a name dissolve.
- **Evidence:** SOURCED · **MS:** M4

### FB-23 · Split a feature into individual polygons
- **Req:** Split multipolygon into polygons.
- **Accept:** Unit: n parts → n features.
- **Evidence:** SOURCED · **MS:** M4

### FB-24 · Create Lines from Polygons
- **Req:** Create lines from polygons, non-blocking.
- **Accept:** Unit: polygon rings become LineStrings.
- **Evidence:** SOURCED · **MS:** M4

### FB-25 · Line Mesh from polygons with common borders
- **Req:** Line mesh from polygons with common borders.
- **Accept:** Unit: two adjacent countries → shared border line once + outer lines.
- **Evidence:** SOURCED · **MS:** M4

### FB-26 · Feature Mesh
- **Req:** Feature mesh: all-to-all lines; one feature + collection → one-to-all.
- **Accept:** Unit: 4 points → 6 lines; 1+4 → 4 lines.
- **Evidence:** SOURCED · **MS:** M4

### FB-27 · Connect features
- **Req:** 'Connect features' (2+ selected) opens a menu of route types (incl. plane route; full list Q-50); creates a new route feature (e.g. 'plane route, A to B') with distance and label position; lines cross the date border. (I) It may show the data source used ("it shows you how it is pulling this data"; meaning to confirm). The menu/dialog stays open after a route is created and is closed manually (EBE "let me close this"; I: sub-view with Back/close, UI-20).
- **Accept:** Unit: Tokyo→LA line crosses antimeridian correctly; Afghanistan + Albania → plane route feature in the Browser with a distance property; a failing type shows an error and creates nothing; after creating a route the menu stays open until closed.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-28 · Simplify Feature Topologies
- **Req:** Simplify feature topologies preserving shared borders.
- **Accept:** Unit: simplified neighbours have no gaps/overlaps.
- **Evidence:** SOURCED · **MS:** M4

### FB-29 · Points-inside-Polygons tools
- **Req:** 'Points inside polygons' (point collection + CTRL-clicked polygon) opens options; 'points inside of polygons' creates a new collection with the polygon and the points inside it; other options TBD (Q-53).
- **Accept:** Unit: populated places + Australia → new collection holding Australia and only the Australian points.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-30 · Import features from Shape Layers
- **Req:** '+' > 'Feature from layer' imports features from AE shape layers; an arrow layer yields an arrowhead shape and an arrow line.
- **Accept:** AE: drawn path on a draw-on layer → line feature with correct lat/lon within 1 px; arrow layer → 2 features; a path with fill only and a path with stroke only both convert to the same feature geometry (EBE: path "doesn't even have to be a stroke").
- **Evidence:** SOURCED · **MS:** M4

### FB-31 · Custom features from georeferenced layers
- **Req:** Custom features from georeferenced layers.
- **Accept:** AE: traced shape on georeferenced image → feature.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-32 · Add Null to Feature
- **Req:** Add Null to Feature.
- **Accept:** AE: null placed at feature label point, parented/pinned as per rig.
- **Evidence:** SOURCED · **MS:** M4

### FB-33 · Feature Properties panel
- **Req:** Feature Properties panel: single-feature layout; collection layout with min/max per property, Sort by Name and per-property treatment (timing, display name); http links, timed-data duration, draggable/centerable label point.
- **Accept:** Unit: collection view shows correct min/max for a numeric property; choosing a name property renames tree entries; link opens browser; dragging label point updates property.
- **Evidence:** SOURCED · **MS:** M4

### FB-34 · Add / edit feature properties
- **Req:** Add/edit feature properties.
- **Accept:** Unit: add column to collection; edit value.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-35 · Collection properties CSV/TSV export → spreadsheet → import
- **Req:** Collection properties CSV/TSV export/import (data join).
- **Accept:** Unit: export, edit, import round-trip changes values by key.
- **Evidence:** SOURCED · **MS:** M4

### FB-36 · One-click data-driven style from a collection's properties panel
- **Req:** One-click data-driven style from properties panel.
- **Accept:** Unit: covered by DAT-07.
- **Evidence:** SOURCED · **MS:** M4

### FB-37 · Automatic label point
- **Req:** Automatic label point inside largest polygon (pole of inaccessibility).
- **Accept:** Unit: label point inside C-shaped polygon.
- **Evidence:** SOURCED · **MS:** M4

### FB-38 · Robust import
- **Req:** Robust import of edge cases (empty coords, multipoint, no geometry, large downloads, empty collections).
- **Accept:** Unit: fixture suite; 50 MB OSM download keeps UI responsive.
- **Evidence:** SOURCED · **MS:** M4

### FB-39 · `coordinate_elevation` property
- **Req:** `coordinate_elevation` property on 3D lines.
- **Accept:** Unit: GPX with ele → property array.
- **Evidence:** SOURCED · **MS:** M4

### FB-40 · Feature cache
- **Req:** Feature cache with configurable paths.
- **Accept:** Unit: cached second load is offline-capable.
- **Evidence:** SOURCED · **MS:** M4

### FB-41 · Measure distance and area
- **Req:** Measure distance and area; route features carry a distance property; route layer names include distance.
- **Accept:** Unit: geodesic length/area within 0.5% of reference.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-42 · Feature Browser and search write into the same tree
- **Req:** Search and Browser share the tree ('Browser Downloads' source).
- **Accept:** Unit: downloaded results searchable.
- **Evidence:** SOURCED · **MS:** M4

### FB-44 · Overpass OSM-JSON → GeoJSON conversion
- **Req:** Convert Overpass OSM-JSON to GeoJSON (ways→lines/polygons, multipolygon/route relations assembled).
- **Accept:** Unit: osmtogeojson-equivalent output on vendor example queries.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-45 · "Preview selection on map" toggle
- **Req:** Browser toggle (default on) that previews selected features on the preview map; double-clicking a feature zooms the preview to it.
- **Accept:** Unit: toggle on + select → outline shown; toggle off → no outline; double-click fits the preview to the feature's bbox.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-46 · Feature action bar
- **Req:** Action bar below the Browser when features are selected; buttons depend on geometry and selection: Fit feature to view (menu; keyframe-marked entries animate), pin, label, draw, add to feature collection, feature properties, Connect features, Create lines from polygons, Merge/Subtract, Points inside polygons, Convert polygons to points, Draw property charts, Export, Remove. Default grouping (EBE, approximate): feature actions (fit, pin, label, draw, add to collection, properties, connect); geometry group (lines from polygons, merge/subtract, points inside polygons, convert to points); then charts/export/remove; rows vs groups to confirm. Icons and visibility rules: Q-52; Fit-feature-to-view entries: Q-51.
- **Accept:** Unit: single polygon shows the single-feature set; two polygons add Merge/Subtract; point collection + polygon adds Points inside polygons; nothing selected hides the bar; each button calls its feature (IDs above).
- **Evidence:** PARTLY INFERRED · **MS:** M4

### FB-47 · Convert polygons to points
- **Req:** Make one point per selected polygon at its label point (FB-37), keeping the polygon's properties, as a new collection. Default: its own action-bar button (placement vs Points-inside-polygons option: Q-53).
- **Accept:** Unit: 'all countries' → one point per country, each inside its largest polygon, with the same properties.
- **Evidence:** PARTLY INFERRED · **MS:** M4


## 8. Drawing features to shape layers (DRW)

### DRW-01 · Draw features as shape layers
- **Req:** Draw features as shape layers with current style into the containing comp, parented to the anchor, named after the feature.
- **Accept:** AE: drawn Lyon polygon sits exactly over Lyon at 10 frames of an animated view.
- **Evidence:** SOURCED · **MS:** M5

### DRW-02 · Drawing options UI, persisted in the .aep
- **Req:** Drawing options at the bottom of the Shape Layer Style panel (Auto Stroke Width, geometry/simplify, individual layers, draw inside Mapcomp, draw on preferred Mapcomp), persisted per .aep.
- **Accept:** Unit: options render below the style settings in the style panel. AE: change options, save, reopen → restored.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-03 · shapeLayerStyle
- **Req:** shapeLayerStyle by index, name or object.
- **Accept:** Unit: all three resolve.
- **Evidence:** SOURCED · **MS:** M5

### DRW-04 · drawInsideMapcomp (default **false**)
- **Req:** drawInsideMapcomp (default false), user-visible in the drawing options.
- **Accept:** AE: true puts layers inside the Mapcomp.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-05 · autoStrokeWidth / "Auto Stroke Width" (default **false**)
- **Req:** autoStrokeWidth (default false): constant visual stroke while zooming.
- **Accept:** AE: stroke px constant across zoom 4→10.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-06 · simplify (default **true**)
- **Req:** simplify (default true) for current zoom.
- **Accept:** Unit: vertex count drops; error < 0.5 px at current zoom.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-07 · simplyfyByZoomRange (sic) (default **false**)
- **Req:** simplyfyByZoomRange (alias simplifyByZoomRange, default false): simplify for max zoom of animation.
- **Accept:** Unit: uses max zoom over keys.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-08 · simplificationZoomOffset (default **0**)
- **Req:** simplificationZoomOffset (default 0).
- **Accept:** Unit: +2 keeps more vertices.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-09 · useTopoSimplificationIfPossible (default **false**)
- **Req:** useTopoSimplificationIfPossible (default false).
- **Accept:** Unit: neighbours share simplified border.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-10 · individualLayers (default **false**)
- **Req:** individualLayers (default false).
- **Accept:** AE: true → one layer per feature; false → groups in one layer.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-11 · autoChangeRenderer (default **false**)
- **Req:** autoChangeRenderer (default false) to CINEMA 4D for extrusions.
- **Accept:** AE: extruded draw with true sets renderer.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-12 · addTrimPaths (default **false**)
- **Req:** addTrimPaths (default false).
- **Accept:** AE: Trim End animates 0→100 over duration.
- **Evidence:** SOURCED · **MS:** M5

### DRW-13 · duration (default **10**)
- **Req:** duration (default 10 s).
- **Accept:** AE: animation length 10 s.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-14 · dateBorderCopy (default **false**)
- **Req:** dateBorderCopy (default false): world copies for date-line crossers.
- **Accept:** AE: Fiji polygon appears on both sides.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-15 · dashThreshold / "Dash Zoom Threshold" (default **1**)
- **Req:** dashThreshold (default 1).
- **Accept:** AE: dashes stable during zoom.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-16 · absoluteZScale (default **false**)
- **Req:** absoluteZScale (default false).
- **Accept:** AE: extrusion constant across zoom when true.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-17 · castsShadows (default **false**)
- **Req:** castsShadows (default false).
- **Accept:** AE: material option set.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-18 · acceptsShadows (default **false**)
- **Req:** acceptsShadows (default false).
- **Accept:** AE: material option set.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-19 · lockLayerTransforms (default **false**)
- **Req:** lockLayerTransforms (default false).
- **Accept:** AE: transforms locked by expression/lock.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-20 · Draw with time offsets
- **Req:** Draw with (data-driven) time offsets.
- **Accept:** AE: quake features appear at their timestamps.
- **Evidence:** SOURCED · **MS:** M5

### DRW-21 · Large-draw warning
- **Req:** Large-draw warning with count and choice.
- **Accept:** Unit: > threshold features → warning modal.
- **Evidence:** SOURCED · **MS:** M5

### DRW-22 · "Create a Shape Layer to Draw On"
- **Req:** Toolbar 'Create a Shape Layer to Draw On': an empty shape layer already parented to the map and set to 3D.
- **Accept:** AE: new layer is 3D and parented; shapes drawn on it stay put when pitch/bearing/zoom change; pen paths convert to features accurately at large scales.
- **Evidence:** SOURCED · **MS:** M5

### DRW-23 · Multi-polygon content structure
- **Req:** Fast multipolygon content structure; holes correct.
- **Accept:** AE: polygon with holes renders holes; 1k-part multipolygon draws < agreed time.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### DRW-24 · Point features with the CINEMA 4D renderer
- **Req:** Points render with C4D renderer.
- **Accept:** AE: point features visible in C4D.
- **Evidence:** SOURCED · **MS:** M5

### DRW-25 · Lines from CSV cross the date border
- **Req:** CSV lines cross date border.
- **Accept:** Unit: as FB-27 for CSV lines.
- **Evidence:** SOURCED · **MS:** M5

### DRW-26 · Rasterize vector shapes workflow
- **Req:** Rasterize workflow (simplify → precompose → auto-trace).
- **Accept:** AE: script precomposes drawn layer.
- **Evidence:** SOURCED · **MS:** M7

### DRW-27 · Draw onto a 3D Landscape
- **Req:** Draw onto a 3D landscape (displaced).
- **Accept:** AE: drawn route follows terrain.
- **Evidence:** SOURCED · **MS:** M9

### DRW-28 · "Draw on preferred Mapcomp"
- **Req:** Drawing option that sends drawn features to the Mapcomp marked 'Prefer for drawing features' in the Mapcomp List (UI-26) instead of the selected one; UI-only (no API option).
- **Accept:** AE: with Mapcomp B preferred and A selected, drawing a feature places the layer for B (in B's containing comp, or inside B with drawInsideMapcomp); option off → A.
- **Evidence:** PARTLY INFERRED · **MS:** M5


## 9. Shape Layer Styles (SLS)

### SLS-01 · Shape Layer Style library
- **Req:** Shape Layer Style library with thumbnails (stroke width relative); toolbar swatch showing the current style that opens the Shape Layer Style panel: presets at the top and drawing options at the bottom (SOURCED); Add style, import/export and the style settings with data-driven options in between, (I) placed in the panel or inside what Add style opens (to confirm, Q-24).
- **Accept:** Unit: thumbnails scale strokes min/max; swatch colour follows the current style; clicking it opens the panel with presets at the top and drawing options at the bottom.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### SLS-02 · Fill
- **Req:** Fill color and opacity.
- **Accept:** AE: applied to drawn layer.
- **Evidence:** INFERRED · **MS:** M5

### SLS-03 · Stroke
- **Req:** Stroke width/color, outlines, round join default.
- **Accept:** AE: outline stroke rendered; join round.
- **Evidence:** SOURCED · **MS:** M5

### SLS-04 · Dashes
- **Req:** Dashes with round caps and Dash Speed; works in legacy engine.
- **Accept:** AE: dashes animate at speed in both engines.
- **Evidence:** SOURCED · **MS:** M5

### SLS-05 · Layer blending modes
- **Req:** Blend modes in styles, applied on swap.
- **Accept:** AE: Multiply style → layer Multiply after swap.
- **Evidence:** SOURCED · **MS:** M5

### SLS-06 · Preserve Transparency
- **Req:** Preserve Transparency option.
- **Accept:** AE: switch set.
- **Evidence:** SOURCED · **MS:** M5

### SLS-07 · Extrusion
- **Req:** Extrusion; anchor adapts on swap.
- **Accept:** AE: swap to/from extruded keeps position.
- **Evidence:** SOURCED · **MS:** M5

### SLS-08 · Create custom styles / "Layer Style Presets"
- **Req:** 'Add style' in the style panel brings up a menu/dialog (EBE "bring up this"): new / import / export (form to confirm, Q-24); 'new' opens an editor to create custom styles / presets.
- **Accept:** Unit: editor creates, saves, reloads style.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### SLS-09 · Pick style from a layer or shape content group
- **Req:** Pick style from a layer or content group.
- **Accept:** AE: picked style equals source layer values.
- **Evidence:** SOURCED · **MS:** M5

### SLS-10 · Create Shape Layer Styles from an image
- **Req:** Create styles from an image.
- **Accept:** Unit: palette extraction yields N styles.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### SLS-11 · Swap Shape Layer Style
- **Req:** 'Swap shape layer style' button (name SOURCED, EBE; I: toolbar ⇄ next to the swatch; EBE also swaps "right here" in the style panel) applies the current style to selected drawn layers.
- **Accept:** AE: draw in style A, pick style B, swap → all selected layers updated incl. colour and blend mode.
- **Evidence:** PARTLY INFERRED (feature and button name SOURCED, toolbar placement INFERRED) · **MS:** M5

### SLS-12 · Filter styles
- **Req:** Filter styles by name; right-click filter by Mapcomp (matrix, saved in .aep).
- **Accept:** Unit + AE: as LBL-18.
- **Evidence:** SOURCED · **MS:** M5

### SLS-13 · Randomize color
- **Req:** Randomize color.
- **Accept:** Unit: produces distinct colors per feature.
- **Evidence:** PARTLY INFERRED · **MS:** M5

### SLS-14 · URL-sourced styles update with one click
- **Req:** URL-sourced shape styles update with one click.
- **Accept:** Unit: as IMG-27.
- **Evidence:** SOURCED · **MS:** M5

### SLS-15 · Data-driven styles
- **Req:** Data-driven styles (see DAT-07..12), incl. data-driven ranges opened from the style panel.
- **Accept:** Covered by DAT tests.
- **Evidence:** SOURCED · **MS:** M4

### SLS-16 · Line patterns
- **Req:** Line patterns via add_line_patterns script.
- **Accept:** AE: pattern applied to layer with existing effects.
- **Evidence:** SOURCED · **MS:** M7

### SLS-17 · Import GEOlayers 2 shape layer styles
- **Req:** Import GEOlayers 2 shape layer styles.
- **Accept:** Unit: sample GL2 style file parsed (needs sample).
- **Evidence:** SOURCED · **MS:** M5

### SLS-18 · Style rules by feature type (GL2 "Styles & Rules")
- **Req:** Style rules by feature type (decide).
- **Accept:** Unit (if built): rules map class→style on draw.
- **Evidence:** INFERRED · **MS:** M5

### SLS-19 · Import / export Shape Layer Styles
- **Req:** Import and export Shape Layer Styles (one or many) from the style panel; default format: one JSON file holding a list of styles.
- **Accept:** Unit: export 3 styles, delete them, import the file → 3 identical styles; name clash gets a unique name.
- **Evidence:** PARTLY INFERRED · **MS:** M5


## 10. Label Templates & template labels (LBL)

### LBL-01 · Template labels
- **Req:** Any comp can be a label template; text fields filled from feature properties; label layer 'Label '<name>''.
- **Accept:** AE: template with 'name' text → label shows feature name, layer named accordingly.
- **Evidence:** SOURCED · **MS:** M6

### LBL-02 · Default template set
- **Req:** Ship a default template set (01 Place Top … 11 Locator 2, Capital Cities, Icon) in a default template project.
- **Accept:** AE: default project loads all templates; each renders.
- **Evidence:** PARTLY INFERRED · **MS:** M6

### LBL-03 · Template kinds and pointers
- **Req:** Template kinds Place×4, Region, Locator, Icon; pointers follow anchor point.
- **Accept:** AE: moving label anchor bends pointer.
- **Evidence:** SOURCED · **MS:** M6

### LBL-04 · Template selector and "Null Layer" entry
- **Req:** Selector 'Null Layer' entry and addLabel(null).
- **Accept:** AE: null created.
- **Evidence:** SOURCED · **MS:** M6

### LBL-05 · Add a label
- **Req:** Add label (current template) from search result, Browser selection (action bar), or manually: a manual-label button (icon unknown) shows a crosshair; type text, position the crosshair, 'Add label' → pinned label at the crosshair. (I) Where the crosshair appears (preview map default) and how it is positioned (moving the map default) are INFERRED.
- **Accept:** AE: all three paths create a correct label; the manual label sits at the crosshair's geo position and is pinned.
- **Evidence:** PARTLY INFERRED · **MS:** M6

### LBL-06 · "Label Anchor Point" layer
- **Req:** 'Label Anchor Point' layer effect controls: Scale with Map / Rotate with Map defaults.
- **Accept:** AE: label scales/rotates per controls.
- **Evidence:** SOURCED · **MS:** M6

### LBL-07 · LabelColors comp
- **Req:** LabelColors comp; SHIFT-select reveals it.
- **Accept:** AE: SHIFT-click selects LabelColors in Project panel.
- **Evidence:** PARTLY INFERRED · **MS:** M6

### LBL-08 · ALT-select a template
- **Req:** ALT-select reveals template comp in Project panel.
- **Accept:** AE: reveal works.
- **Evidence:** SOURCED · **MS:** M6

### LBL-09 · Responsive timing via markers
- **Req:** Labels copy/update markers from template (responsive timing).
- **Accept:** AE: template marker change propagates on update.
- **Evidence:** PARTLY INFERRED · **MS:** M6

### LBL-10 · Animated label text
- **Req:** Animated label text from timed data.
- **Accept:** AE: text changes at data timestamps.
- **Evidence:** SOURCED · **MS:** M6

### LBL-11 · Label time offsets
- **Req:** Label time offsets (data-driven).
- **Accept:** AE: labels start at offsets.
- **Evidence:** SOURCED · **MS:** M6

### LBL-12 · Swap Label Template
- **Req:** 'Swap label template' button next to the dropdown swaps the selected labels to the chosen template regardless of Mapcomp; swaps AVLayer assets; no crash.
- **Accept:** AE: select a label, choose '03 Place Left', swap → template changed, position/data kept.
- **Evidence:** SOURCED · **MS:** M6

### LBL-13 · Missing-data prompt
- **Req:** Prompt for missing template data.
- **Accept:** AE: modal asks for missing field.
- **Evidence:** SOURCED · **MS:** M6

### LBL-14 · "Create Label Templates" button
- **Req:** 'Create Label Templates' button in Mapcomp Label settings.
- **Accept:** AE: creates templates.
- **Evidence:** SOURCED · **MS:** M6

### LBL-15 · Authoring a new template
- **Req:** Template authoring/registration via 'add a new template' in the template dropdown (mechanism to define).
- **Accept:** AE: user comp registered as template appears in selector.
- **Evidence:** PARTLY INFERRED · **MS:** M6

### LBL-16 · Label Template Projects
- **Req:** Load custom Label Template projects (importProject(..., true)); reachable as 'import' from the template dropdown.
- **Accept:** AE: templates from .aep appear.
- **Evidence:** SOURCED · **MS:** M6

### LBL-17 · Download online Label Templates
- **Req:** Download label templates from MapForge's default projects (the original, EBE: "the default [GEOlayers] projects"), from the label area (exact placement INFERRED).
- **Accept:** Unit: download from the configured URL/GitHub repo adds its templates to the dropdown.
- **Evidence:** PARTLY INFERRED · **MS:** M6
- **Free substitute:** Self-hosted/GitHub template repo.

### LBL-18 · Assign Label Templates (and Shape Layer Styles) to Mapcomps
- **Req:** Assign templates/styles to Mapcomps via matrix; persisted.
- **Accept:** AE: unchecked templates hidden when that Mapcomp selected.
- **Evidence:** SOURCED · **MS:** M6

### LBL-19 · Unique label layer names
- **Req:** Unique label layer names.
- **Accept:** AE: two 'Paris' labels get unique names; expressions intact.
- **Evidence:** SOURCED · **MS:** M6

### LBL-20 · Label renaming applies to template labels
- **Req:** Renaming dictionaries apply to template labels.
- **Accept:** Unit: as MLB-06.
- **Evidence:** SOURCED · **MS:** M6

### LBL-21 · Placement control
- **Req:** Placement control incl. trimmed linestrings.
- **Accept:** AE: label follows trimmed line end.
- **Evidence:** SOURCED · **MS:** M6

### LBL-22 · Scaling labels
- **Req:** Scaling labels.
- **Accept:** AE: scale control works.
- **Evidence:** PARTLY INFERRED · **MS:** M6

### LBL-23 · RTL and Traditional Chinese in templates
- **Req:** RTL and Traditional Chinese in templates.
- **Accept:** AE: renders correctly.
- **Evidence:** SOURCED · **MS:** M6

### LBL-24 · Labels animated along a path
- **Req:** Labels animated along a coordinate path with duration.
- **Accept:** AE: label moves along path over duration.
- **Evidence:** SOURCED · **MS:** M6

### LBL-25 · Import GEOlayers 2 label templates
- **Req:** Import GEOlayers 2 label templates.
- **Accept:** AE: sample GL2 template imports (needs sample).
- **Evidence:** SOURCED · **MS:** M6

### LBL-26 · Pinned labels
- **Req:** Pinned labels with '(pinned)' suffix and the pin effect controls (PIN-14); manual labels are pinned automatically; unpin.
- **Accept:** AE: pin/unpin toggles; a manual label shows the pin effects.
- **Evidence:** SOURCED · **MS:** M6

### LBL-27 · Filter Label Templates by name
- **Req:** Filter Label Templates by name.
- **Accept:** Unit: filter narrows list.
- **Evidence:** SOURCED · **MS:** M6


## 11. Pinning / snapping layers (PIN)

### PIN-01 · Pin Selected Layers to Map
- **Req:** First toolbar button 'Pin selected layers to map': pins selected layers (incl. text and anchors) by adding pin effects (PIN-14); clicking again with a pinned layer selected unpins and removes all pin effects.
- **Accept:** AE: pinned layer stays on its geo point through animation; unpin removes every pin effect and restores the layer; a layer with keyframed scale/position/content animation keeps that animation after pin and after unpin (EBE bouncing icon).
- **Evidence:** SOURCED · **MS:** M6

### PIN-02 · "(pinned)" suffix
- **Req:** '(pinned)' suffix.
- **Accept:** AE: suffix added/removed.
- **Evidence:** SOURCED · **MS:** M6

### PIN-03 · Latitude / Longitude / Height properties
- **Req:** Separate Latitude/Longitude/Height properties.
- **Accept:** AE: editing Lat moves layer correctly.
- **Evidence:** SOURCED · **MS:** M6

### PIN-04 · "Position Progress" slider/effect
- **Req:** 'Position Progress' slider along a path.
- **Accept:** AE: 0→100 moves along feature.
- **Evidence:** SOURCED · **MS:** M6

### PIN-05 · Rotate with map
- **Req:** Rotate/scale with map options, exposed as effect controls on the pinned layer (PIN-14, not a popover), respecting existing rotation.
- **Accept:** AE: toggling Rotate with Map / Scale with Map on the layer takes effect immediately; rotation offset preserved.
- **Evidence:** PARTLY INFERRED · **MS:** M6

### PIN-06 · Pin to a Feature
- **Req:** Pin to a feature from the feature action bar's pin button (FB-46; I: pins the selected AE layer to the feature's label point).
- **Accept:** AE: layer pinned at feature label point.
- **Evidence:** PARTLY INFERRED · **MS:** M6

### PIN-07 · Pin vs Parent
- **Req:** Support both pinning and parenting workflows.
- **Accept:** AE: both stay aligned.
- **Evidence:** SOURCED · **MS:** M6

### PIN-08 · Snap layer to Mapcomp ("snapped layers")
- **Req:** Snap layer to Mapcomp with options and path animation (API; the UI pin button builds the same rig).
- **Accept:** AE: snap with path → layer follows path over duration.
- **Evidence:** PARTLY INFERRED · **MS:** M6

### PIN-09 · Get a layer's geo position
- **Req:** getLayersGeoPosition → [lon,lat].
- **Accept:** Unit: inverse projection within 1e-6°.
- **Evidence:** SOURCED · **MS:** M1

### PIN-10 · Animate layers along features
- **Req:** Animate layers along features, accurate at street scale.
- **Accept:** AE: vehicle on street path within 1 px at z18.
- **Evidence:** SOURCED · **MS:** M6

### PIN-11 · Georeference images and photos
- **Req:** Georeference images/photos (anchor + navigate + parent + feature).
- **Accept:** AE: workflow completes; coordinate paste works.
- **Evidence:** PARTLY INFERRED · **MS:** M6

### PIN-12 · Anchor-based parenting for footage and particles
- **Req:** Anchor-based parenting for footage/particles.
- **Accept:** AE: parented footage follows map.
- **Evidence:** PARTLY INFERRED · **MS:** M6

### PIN-13 · Backward compatibility of older pinned layers
- **Req:** Migrate/tolerate older pinned-layer rigs.
- **Accept:** Unit: rig version marker; AE: v0 rig project opens and is upgraded without errors.
- **Evidence:** PARTLY INFERRED · **MS:** M6

### PIN-14 · Pin effect controls
- **Req:** Pinning adds effect controls: Scale with Map, Pixel Offset, Height, Bearing (offset), 3D, Rotate with Map (names and defaults to confirm; UI default per Q-33); same set on pinned labels; all removed on unpin.
- **Accept:** AE: each control works live (scale follows zoom; pixel offset shifts in screen space; Height lifts in 3D; bearing rotates; 3D switch; rotate follows map bearing); unpin leaves no effects behind.
- **Evidence:** PARTLY INFERRED · **MS:** M6


## 12. Routes & arrows (RTE)

### RTE-01 · Car / Bike / Pedestrian routes
- **Req:** Car/Bike/Pedestrian routes offered as route types in the Connect features menu (FB-27; I: each may show its data source, Q-50); maneuver points off by default.
- **Accept:** Unit: route A→B per mode returns LineString; pref adds points. AE/Unit: select 2 features → Connect features lists car/bike/foot (with a source label if Q-50 confirms one); choosing one creates a route feature.
- **Evidence:** PARTLY INFERRED · **MS:** M7
- **Free substitute:** FOSSGIS OSRM (free, 1 req/s).

### RTE-02 · Create a route by connecting features
- **Req:** Route by connecting features: route feature named like 'plane route, A to B' (FB-27); drawn layer '<A> - <B> (<distance> mi)'.
- **Accept:** AE: name format and distance correct; units per decision.
- **Evidence:** SOURCED · **MS:** M7

### RTE-03 · Connections across the date border
- **Req:** Connections across the date border.
- **Accept:** Unit: as FB-27.
- **Evidence:** SOURCED · **MS:** M7

### RTE-04 · Animated route drawing
- **Req:** Animated route drawing (trim + duration).
- **Accept:** AE: route draws on over duration.
- **Evidence:** SOURCED · **MS:** M7

### RTE-05 · Arrowhead/vehicle along route
- **Req:** Arrowhead/vehicle along route via Position Progress.
- **Accept:** AE: head aligned with trim end.
- **Evidence:** SOURCED · **MS:** M7

### RTE-06 · Arrow tool ("Create Arrows from the toolbar")
- **Req:** Toolbar 'Draw arrow': activates AE's pen tool; the drawn path becomes an arrow shape layer that is already keyframed, carries arrow effect controls, takes its colour from the current Shape Layer Style and is parented to the map.
- **Accept:** AE: click Draw Arrow, draw a path → arrow layer with keys (U shows them), style colour, parent = map; it stays put when pitch/bearing change; Feature from layer gives 2 features; editing a vertex of the arrow path with the pen tool (G) updates the arrow shape and its animation (EBE; I: animation driven by the path; confirm in AE).
- **Evidence:** PARTLY INFERRED · **MS:** M7

### RTE-07 · shapes_to_arrows.jsx
- **Req:** shapes_to_arrows.jsx.
- **Accept:** AE: path converted to arrow.
- **Evidence:** SOURCED · **MS:** M7

### RTE-08 · Branching arrows / "3 Ways to Add Arrows"
- **Req:** Branching arrows.
- **Accept:** AE: branch arrow from existing arrow.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### RTE-09 · Tracks from GPX / KML / TCX as routes
- **Req:** GPX/KML/TCX tracks as routes.
- **Accept:** Unit: parse fixtures.
- **Evidence:** SOURCED · **MS:** M4

### RTE-10 · Plane / straight-line route modes (GEOlayers 2)
- **Req:** Plane route type in Connect features (exists in GL3); straight-line mode to decide.
- **Accept:** Unit: plane route is a great-circle (I) line with a distance property.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### RTE-11 · Symbols on a path
- **Req:** Symbols on a path.
- **Accept:** AE: repeated symbols along path.
- **Evidence:** PARTLY INFERRED · **MS:** M7


## 13. Data import & DataViz (DAT)

### DAT-01 · Supported file formats
- **Req:** Import GeoJSON/JSON, sequences, CSV(, ;), TSV, TCX, KML, GPX, zipped shapefile, .aep/.aet; URL import.
- **Accept:** Unit: fixture per format.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### DAT-02 · GeoJSON file sequence import
- **Req:** GeoJSON file sequence → timed data.
- **Accept:** Unit: 3 files → 3 time steps.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### DAT-03 · CSV / TSV import
- **Req:** CSV/TSV points from lat/lon columns (auto-detect), trimmed names.
- **Accept:** Unit: column detection table.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### DAT-04 · Remote URL import
- **Req:** Remote URL import.
- **Accept:** Unit: URL with extension imports.
- **Evidence:** SOURCED · **MS:** M4

### DAT-05 · GPS tracks
- **Req:** GPS tracks as lines, elevation kept.
- **Accept:** Unit: GPX ele → coordinate_elevation.
- **Evidence:** SOURCED · **MS:** M4

### DAT-06 · Zipped shapefiles
- **Req:** Zipped shapefiles.
- **Accept:** Unit: shpjs import with .prj reprojection.
- **Evidence:** SOURCED · **MS:** M4

### DAT-07 · One-click data-driven styles
- **Req:** One-click data-driven style from a property.
- **Accept:** AE: population → circle size/color.
- **Evidence:** SOURCED · **MS:** M8

### DAT-08 · Gradient preview
- **Req:** Gradient preview in UI.
- **Accept:** Unit: gradient matches ramp.
- **Evidence:** SOURCED · **MS:** M8

### DAT-09 · Interpolation curves
- **Req:** Interpolation curves incl. 'Circle Area'.
- **Accept:** Unit: radius ∝ sqrt(value).
- **Evidence:** SOURCED · **MS:** M8

### DAT-10 · Timed data and animated data-driven styles
- **Req:** Timed data animating styles; duration shown.
- **Accept:** AE: time series animates.
- **Evidence:** PARTLY INFERRED · **MS:** M8

### DAT-11 · Non-linear keyframe timing
- **Req:** Non-linear keyframe timing for data keys.
- **Accept:** AE: easing option applied.
- **Evidence:** SOURCED · **MS:** M8

### DAT-12 · Data-driven time offsets
- **Req:** Data-driven time offsets.
- **Accept:** AE: as DRW-20.
- **Evidence:** SOURCED · **MS:** M8

### DAT-13 · Data join via CSV/TSV
- **Req:** CSV/TSV data join.
- **Accept:** Unit: as FB-35.
- **Evidence:** SOURCED · **MS:** M4

### DAT-14 · Property Diagrams
- **Req:** Property Diagrams from the toolbar 'Draw Diagram' button and the action bar 'Draw property charts' (charts as AE layers), e.g. a GPX elevation profile.
- **Accept:** AE: bar chart layers created from a property; a GPX track's elevation yields a profile chart.
- **Evidence:** SOURCED · **MS:** M8

### DAT-15 · Choropleth Mapcomp (imagery dataviz)
- **Req:** Choropleth imagery style from connected GeoJSON.
- **Accept:** AE: 3000 polygons render as imagery fast.
- **Evidence:** SOURCED · **MS:** M8

### DAT-16 · Circles imagery (dataviz)
- **Req:** Circles imagery style.
- **Accept:** AE: proportional circles rendered.
- **Evidence:** SOURCED · **MS:** M8

### DAT-17 · Heatmap imagery (dataviz)
- **Req:** Heatmap imagery style.
- **Accept:** AE: heatmap rendered.
- **Evidence:** PARTLY INFERRED · **MS:** M8

### DAT-18 · Lines imagery (dataviz)
- **Req:** Lines imagery style.
- **Accept:** AE: lines rendered.
- **Evidence:** SOURCED · **MS:** M8

### DAT-19 · Live data
- **Req:** Live data via watch/watchCsv.
- **Accept:** Unit: file change triggers callback within interval.
- **Evidence:** SOURCED · **MS:** M4

### DAT-20 · Scripted data join
- **Req:** Scripted data join flow.
- **Accept:** Unit: vendor COVID scripts run against mock fetch.
- **Evidence:** SOURCED · **MS:** M4

### DAT-21 · Filter by data
- **Req:** Filter by data.
- **Accept:** Unit: as FB-06.
- **Evidence:** SOURCED · **MS:** M4

### DAT-22 · Large-dataset guidance
- **Req:** Large-dataset guidance in UI.
- **Accept:** Unit: warning suggests dataviz style.
- **Evidence:** PARTLY INFERRED · **MS:** M8


## 14. 3D (3D)

### 3D-01 · Pitch and bearing ("Animate in 3D space")
- **Req:** Keyframeable pitch and bearing.
- **Accept:** AE: pitch 45/bearing 90 render matching preview.
- **Evidence:** SOURCED · **MS:** M1

### 3D-02 · Pseudo-3D rig (no camera)
- **Req:** Pseudo-3D rig without camera.
- **Accept:** AE: overlays align under pitch without a camera layer.
- **Evidence:** PARTLY INFERRED · **MS:** M1

### 3D-03 · Draw 3D buildings (toolbar)
- **Req:** Draw 3D buildings from toolbar; zoom-independent extrusion; C4D renderer.
- **Accept:** AE: buildings extruded with heights.
- **Evidence:** SOURCED · **MS:** M5
- **Free substitute:** Overpass building:levels/height or OpenFreeMap render_height.

### 3D-04 · Extruded features, shadows, lights
- **Req:** Extrusion, shadows, lights.
- **Accept:** AE: shadows render.
- **Evidence:** SOURCED · **MS:** M5

### 3D-05 · 3D Landscape Mapcomp
- **Req:** Toolbar 'Create 3D Landscape' dialog with Texture Size, Height Map Size and a required renderer-plugin choice; heightmap via Calculations; color space unchanged; work area set.
- **Accept:** AE: landscape created with chosen plugin; project color space unchanged.
- **Evidence:** PARTLY INFERRED · **MS:** M9
- **Free substitute:** AWS Terrarium DEM; free path: AE native 3D model (GLB mesh) when no paid plugin.

### 3D-06 · 3D landscape renderer plugins
- **Req:** Renderers Helium, FreeForm Pro, Plexus (3), Mir, listed in the Create 3D Landscape dialog.
- **Accept:** AE: each installed plugin setup works (user-owned plugins).
- **Evidence:** SOURCED · **MS:** M9
- **Free substitute:** GLB terrain mesh into AE 3D model layer (AE 24+) as free renderer (I).

### 3D-07 · Landscape controls
- **Req:** Landscape controls (elevation height etc.) on plugin-named layer.
- **Accept:** AE: elevation slider changes height.
- **Evidence:** PARTLY INFERRED · **MS:** M9

### 3D-08 · Elevation (DEM) source
- **Req:** DEM source.
- **Accept:** Unit: decode + stitch heightmap.
- **Evidence:** PARTLY INFERRED · **MS:** M9
- **Free substitute:** AWS Terrarium or MapTiler Free terrain-rgb.

### 3D-09 · Sliced landscape / landscape edge
- **Req:** Landscape edge/sidewalls.
- **Accept:** AE: sliced edge visible.
- **Evidence:** SOURCED · **MS:** M9

### 3D-10 · Globe and Equal Earth projection "via Plugin"
- **Req:** Globe and Equal Earth projections.
- **Accept:** AE: one setting switches flat → globe.
- **Evidence:** PARTLY INFERRED · **MS:** M9
- **Free substitute:** MapLibre v5 globe projection for render; d3-geo Equal Earth raster; no native C++ plugin (cannot build on Linux).

### 3D-11 · Globe sample project
- **Req:** Globe sample using VC Orb.
- **Accept:** AE: sample opens with Orb installed.
- **Evidence:** SOURCED · **MS:** M9

### 3D-12 · Create 3D Scene & Camera
- **Req:** Create 3D Scene & Camera script.
- **Accept:** AE: camera matches map view; overlays aligned.
- **Evidence:** SOURCED · **MS:** M9

### 3D-13 · 3D models in the map (compatibility, not a parity feature)
- **Req:** Compatibility with AE native 3D models (not a parity feature).
- **Accept:** AE: GLB in scene stays aligned with camera script.
- **Evidence:** PARTLY INFERRED · **MS:** M9

### 3D-14 · Pinned layer Height
- **Req:** Pinned layer Height.
- **Accept:** AE: height moves layer toward camera.
- **Evidence:** SOURCED · **MS:** M6

### 3D-15 · Renderer handling
- **Req:** Renderer handling (Classic default, C4D auto for extrusion).
- **Accept:** AE: as DRW-11.
- **Evidence:** SOURCED · **MS:** M5

### 3D-16 · Heightmap and labels aligned with the landscape texture
- **Req:** Heightmap and labels aligned with texture.
- **Accept:** AE: no offset at edges.
- **Evidence:** SOURCED · **MS:** M9


## 15. Toolbar default scripts (TB)

### TB-01 · Run scripts from the toolbar
- **Req:** Toolbar Scripts list of default scripts (line pattern, blend in/out, blink, distance circle, 3D scene, stagger, …) and user scripts; ALT/OPT-click shows description; CTRL/CMD-click reveals file.
- **Accept:** Unit: list shows default and user scripts; click runs; modifiers.
- **Evidence:** SOURCED · **MS:** M7

### TB-02 · Script location
- **Req:** Default scripts shipped (EBE); user scripts (user folder) listed and run from the same menu (I: same list); description from header comment.
- **Accept:** Unit: menu lists both.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### TB-03 · add_line_pattern.jsx / add_line_patterns.jsx
- **Req:** add_line_patterns.jsx (alias add_line_pattern.jsx).
- **Accept:** AE: works with existing effects.
- **Evidence:** SOURCED · **MS:** M7

### TB-04 · blend_in_and_out.jsx
- **Req:** blend_in_and_out.jsx.
- **Accept:** AE: opacity in/out keys.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### TB-05 · north_needle.jsx
- **Req:** north_needle.jsx.
- **Accept:** AE: needle rotates with bearing.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### TB-06 · scale_bar.jsx
- **Req:** scale_bar.jsx.
- **Accept:** AE: bar length correct ±2% at lat.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### TB-07 · shapes_to_arrows.jsx
- **Req:** shapes_to_arrows.jsx.
- **Accept:** AE: as RTE-07.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### TB-08 · blink.jsx
- **Req:** blink.jsx.
- **Accept:** AE: blink animation.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### TB-09 · stagger_layers.jsx
- **Req:** stagger_layers.jsx.
- **Accept:** AE: offsets layers.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### TB-10 · precompose_feature_shape_layer.jsx
- **Req:** precompose_feature_shape_layer.jsx.
- **Accept:** AE: precomposed.
- **Evidence:** SOURCED · **MS:** M7

### TB-11 · Distance Circle
- **Req:** Distance Circle with parameters and vertex count.
- **Accept:** AE: 100 km circle geodesically correct.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### TB-12 · Draw Mapcomp View
- **Req:** Draw Mapcomp View script.
- **Accept:** AE: rectangle follows detail Mapcomp view.
- **Evidence:** SOURCED · **MS:** M7

### TB-13 · Create 3D Scene & Camera
- **Req:** Create 3D Scene & Camera (see 3D-12).
- **Accept:** AE: as 3D-12.
- **Evidence:** SOURCED · **MS:** M9

### TB-14 · autorun.jsx
- **Req:** autorun.jsx on project open via panel.
- **Accept:** AE: script runs once.
- **Evidence:** SOURCED · **MS:** M7

### TB-15 · Built-in toolbar actions (non-script)
- **Req:** Built-in toolbar actions wired: Pin, Controller Mapcomp, 3D Landscape, Draw Diagram, Scripts, label dropdown/manual label/swap, Draw Arrow, Draw-On layer, style button/swap; Watermask and 3D buildings (INFERRED) only with a MapTiler premium plan (UI-28).
- **Accept:** AE: each action triggers its feature.
- **Evidence:** PARTLY INFERRED · **MS:** M7

### TB-16 · Vendor community script repos
- **Req:** Vendor example scripts (13) run unmodified apart from the namespace.
- **Accept:** AE: corpus passes (geolayers3.→mapforge. rewrite in test harness).
- **Evidence:** SOURCED · **MS:** M7


## 16. Projects, sample projects & templates (PRJ)

### PRJ-01 · Projects panel
- **Req:** Projects dialog (flyout entry under Support): recent projects, sample projects, new project, Import Project.
- **Accept:** AE: recent list shows the last opened projects; new project creates first Mapcomp; samples and Import Project open.
- **Evidence:** SOURCED · **MS:** M10

### PRJ-02 · Sample projects
- **Req:** Sample projects (Globe, timeline, Elevation, 3D Landscape, showcases), listed below recent projects, with usage directions as layer markers.
- **Accept:** AE: each sample opens and finalizes.
- **Evidence:** PARTLY INFERRED · **MS:** M10
- **Free substitute:** Re-authored samples using free data.

### PRJ-03 · Download-with-browser button for samples
- **Req:** Download-with-browser button for samples.
- **Accept:** Unit: opens URL.
- **Evidence:** SOURCED · **MS:** M10

### PRJ-04 · Open/import projects through the panel
- **Req:** Import Project (Projects dialog): sort into structure, uniquify names, fix expressions, never duplicate MapForge items (unlike AE drag-and-drop).
- **Accept:** AE: importing a MapForge project into another works with no expression errors and no duplicated MapForge folders/items.
- **Evidence:** SOURCED · **MS:** M10

### PRJ-05 · Project-open hooks
- **Req:** Project-open hooks (GeoJSON autoload, autorun).
- **Accept:** AE: as FB-18/TB-14.
- **Evidence:** SOURCED · **MS:** M10

### PRJ-06 · Label Template projects
- **Req:** Label Template projects.
- **Accept:** AE: as LBL-16.
- **Evidence:** SOURCED · **MS:** M10

### PRJ-07 · GEOlayers 2 asset import
- **Req:** GEOlayers 2 asset import (no GL1/GL2 projects).
- **Accept:** Unit: needs sample files.
- **Evidence:** SOURCED · **MS:** M10

### PRJ-08 · Meaning of "template" for parity
- **Req:** Recreate all 'templates' (labels, styles, imagery, samples, scripts).
- **Accept:** Audit: checklist complete.
- **Evidence:** INFERRED · **MS:** M10

### PRJ-09 · Quick Start Buttons
- **Req:** Quick Start Buttons.
- **Accept:** As UI-22.
- **Evidence:** SOURCED · **MS:** M10

### PRJ-10 · Tutorial lessons that may hide features (check during hands-on)
- **Req:** Check tutorial-only lessons for hidden features.
- **Accept:** Hands-on review recorded in OPEN_QUESTIONS.
- **Evidence:** PARTLY INFERRED · **MS:** M10

### PRJ-11 · "Revision Workflow" and "How to Export Your Map Animation"
- **Req:** Revision workflow / export chapters reviewed.
- **Accept:** Hands-on review recorded.
- **Evidence:** PARTLY INFERRED · **MS:** M10


## 17. Preferences (PRF)

### PRF-01 · Default comp specs
- **Req:** Default comp specs.
- **Accept:** Unit: used when no comp selected.
- **Evidence:** SOURCED · **MS:** M1

### PRF-02 · Default language
- **Req:** Default language.
- **Accept:** Unit: preview language.
- **Evidence:** PARTLY INFERRED · **MS:** M3

### PRF-03 · Default Features loaded on startup
- **Req:** Default features on startup (files/URLs).
- **Accept:** Unit: loaded.
- **Evidence:** SOURCED · **MS:** M4

### PRF-04 · Preload features for search on startup
- **Req:** Preload search features.
- **Accept:** Unit: as SRC-09.
- **Evidence:** SOURCED · **MS:** M4

### PRF-05 · Feature preview shows bounding boxes
- **Req:** Feature preview bounding boxes.
- **Accept:** Unit: as UI-16.
- **Evidence:** SOURCED · **MS:** M4

### PRF-06 · Fallback online search service as default
- **Req:** Fallback search as default.
- **Accept:** Unit: as SRC-08.
- **Evidence:** SOURCED · **MS:** M4

### PRF-07 · Routes: import Maneuver Points (default **off**)
- **Req:** Maneuver points (default off).
- **Accept:** Unit: as RTE-01.
- **Evidence:** SOURCED · **MS:** M7

### PRF-08 · Drawing: lock shape layer transform properties
- **Req:** Lock shape layer transforms.
- **Accept:** AE: as DRW-19.
- **Evidence:** SOURCED · **MS:** M7

### PRF-09 · Label renaming dictionaries
- **Req:** Label renaming dictionaries from files.
- **Accept:** Unit: load CSV/JSON dictionary.
- **Evidence:** SOURCED · **MS:** M3

### PRF-10 · Custom imagery styles: import / export multiple
- **Req:** Import/export multiple imagery styles.
- **Accept:** Unit: round-trip.
- **Evidence:** SOURCED · **MS:** M2

### PRF-11 · MapTiler API key field
- **Req:** MapTiler key field at bottom; invalid-key error.
- **Accept:** Unit: invalid key shows error.
- **Evidence:** SOURCED · **MS:** M2

### PRF-12 · Default Imagery Quality
- **Req:** Default Imagery Quality.
- **Accept:** Unit: new Mapcomp Quality = pref.
- **Evidence:** SOURCED · **MS:** M2

### PRF-13 · MapTiler Search worldview
- **Req:** MapTiler worldview.
- **Accept:** Unit: param sent.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### PRF-14 · Enable/disable GEOlayers Mapdata (green icon)
- **Req:** Legacy Mapdata toggle (decide; default omit).
- **Accept:** N/A unless decided.
- **Evidence:** SOURCED · **MS:** M10

### PRF-15 · CartoDB API key
- **Req:** CartoDB API key.
- **Accept:** Unit: stored and used.
- **Evidence:** SOURCED · **MS:** M2

### PRF-16 · Search sources configuration
- **Req:** Search sources configuration.
- **Accept:** Unit: add/remove source.
- **Evidence:** INFERRED · **MS:** M4

### PRF-17 · Hidden: Feature Cache Paths
- **Req:** Hidden: feature cache paths.
- **Accept:** Unit: path override.
- **Evidence:** SOURCED · **MS:** M4

### PRF-18 · Hidden: purge Undo Cache on each host call
- **Req:** Hidden: purge undo cache per host call.
- **Accept:** AE: app.purge called when on.
- **Evidence:** SOURCED · **MS:** M10

### PRF-19 · Per-project settings stored in the .aep
- **Req:** Per-project settings in .aep.
- **Accept:** AE: survive save/reopen.
- **Evidence:** SOURCED · **MS:** M0

### PRF-20 · Other preferences
- **Req:** Remaining preferences (to enumerate), incl. groups feature search, composition settings, animation handling, Mapcomp icons.
- **Accept:** Audit vs original prefs list.
- **Evidence:** SOURCED · **MS:** M10

### PRF-21 · Mapcomp icon visibility
- **Req:** Preferences to show/hide Mapcomp icons (default scope: each Mapcomp List row icon/toggle, UI-04/08/09/26/27).
- **Accept:** Unit: hiding an icon in Preferences removes it from every List row; setting persists across panel reloads.
- **Evidence:** PARTLY INFERRED · **MS:** M10


## 18. Scripting API (API) — global `geolayers3` (ExtendScript)

### API-01 · Global object and runtime
- **Req:** Global `mapforge` object in ExtendScript while panel open.
- **Accept:** AE: `typeof mapforge` is object after panel load, from any script.
- **Evidence:** SOURCED · **MS:** M0

### API-02 · `comp` argument convention
- **Req:** comp argument: name, Mapcomp or containing comp.
- **Accept:** Unit: resolver accepts all three.
- **Evidence:** SOURCED · **MS:** M0

### API-03 · Async convention
- **Req:** Node-style async callbacks.
- **Accept:** Unit: callback(error,data) contract.
- **Evidence:** SOURCED · **MS:** M0

### API-04 · `importProject(file[, isLabelTemplateProject])`
- **Req:** importProject(file[, isLabelTemplateProject]).
- **Accept:** AE: import sorts items, fixes expressions.
- **Evidence:** SOURCED · **MS:** M10

### API-05 · `finalize(comps, callback[, options])`
- **Req:** finalize(comps, callback[, options]); the UI CTRL/CMD-click maps to onlyCurrentFrame.
- **Accept:** Unit: undefined = all.
- **Evidence:** SOURCED · **MS:** M2

### API-06 · `duplicate(comp[, options])`
- **Req:** duplicate(comp[, options]).
- **Accept:** AE: returns duplicate.
- **Evidence:** SOURCED · **MS:** M1

### API-07 · `setViewAtTime(comp, view[, forceKeyframe[, time]])`
- **Req:** setViewAtTime.
- **Accept:** Unit: as VIEW-03.
- **Evidence:** SOURCED · **MS:** M1

### API-08 · `fitViewAtTime(comp, bbox[, forceKeyframe[, time]])`
- **Req:** fitViewAtTime.
- **Accept:** Unit: as VIEW-04.
- **Evidence:** SOURCED · **MS:** M1

### API-09 · `removeMapcompControlKeys(comp)`
- **Req:** removeMapcompControlKeys.
- **Accept:** AE: as VIEW-12.
- **Evidence:** SOURCED · **MS:** M1

### API-10 · `getMapcomps()`
- **Req:** getMapcomps().
- **Accept:** AE: returns all.
- **Evidence:** SOURCED · **MS:** M1

### API-11 · `getMapcompAnchor(mapcomp)`
- **Req:** getMapcompAnchor(mapcomp).
- **Accept:** AE: returns anchor layer.
- **Evidence:** SOURCED · **MS:** M1

### API-12 · `addLabel(comp, labelTemplateComp, labelData[, coordinates[, options]])`
- **Req:** addLabel(comp, template, data[, coordinates[, options]]).
- **Accept:** AE: all argument forms.
- **Evidence:** SOURCED · **MS:** M6

### API-13 · `getLabelTemplateComps()`
- **Req:** getLabelTemplateComps().
- **Accept:** AE: returns templates or [].
- **Evidence:** SOURCED · **MS:** M6

### API-14 · `updateLabelData(labelLayer, newLabelData)`
- **Req:** updateLabelData(layer, data).
- **Accept:** AE: text updated.
- **Evidence:** SOURCED · **MS:** M6

### API-15 · `getLayersGeoPosition(layer)`
- **Req:** getLayersGeoPosition(layer).
- **Accept:** Unit: as PIN-09.
- **Evidence:** SOURCED · **MS:** M1

### API-16 · `snapLayerToMapcomp(comp, layer, coordinates[, options])`
- **Req:** snapLayerToMapcomp(comp, layer, coordinates[, options]).
- **Accept:** AE: defaults applied; returns layer.
- **Evidence:** SOURCED · **MS:** M6

### API-17 · `fetchText(url, callback[, initObject])`
- **Req:** fetchText(url, cb[, init]).
- **Accept:** Unit: fetch init passthrough.
- **Evidence:** SOURCED · **MS:** M0

### API-18 · `fetchJson(url, callback[, initObject])`
- **Req:** fetchJson(url, cb[, init]).
- **Accept:** Unit: parse errors reported.
- **Evidence:** SOURCED · **MS:** M0

### API-19 · `geocode(searchString, callback[, options])`
- **Req:** geocode(query, cb[, options]).
- **Accept:** Unit: queued 1 req/s.
- **Evidence:** SOURCED · **MS:** M4

### API-20 · `watch(fileOrFolder, callback[, options])`
- **Req:** watch(fileOrFolder, cb[, options]).
- **Accept:** Unit: interval default 10000; replacement semantics.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### API-21 · `watchCsv(csvFile, callback[, options])`
- **Req:** watchCsv(file, cb[, options]).
- **Accept:** Unit: changedRows/allRows.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### API-22 · `unwatch()`
- **Req:** unwatch().
- **Accept:** Unit: stops.
- **Evidence:** SOURCED · **MS:** M4

### API-23 · `addToBrowser(addObj[, callback[, options]])`
- **Req:** addToBrowser(addObj[, callback[, options]]) incl. (url, options) overload.
- **Accept:** Unit: RKI-style call works.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### API-24 · `addObj` forms (shared by addToBrowser and draw)
- **Req:** addObj forms.
- **Accept:** Unit: each form.
- **Evidence:** SOURCED · **MS:** M4

### API-25 · `clearBrowser([callback])`
- **Req:** clearBrowser([cb]).
- **Accept:** Unit: favorites kept.
- **Evidence:** SOURCED · **MS:** M4

### API-26 · `removeBrowserSelection([callback])`
- **Req:** removeBrowserSelection([cb]).
- **Accept:** Unit.
- **Evidence:** SOURCED · **MS:** M4

### API-27 · `getBrowserSelection([callback[, options]])`
- **Req:** getBrowserSelection([cb[, options]]).
- **Accept:** Unit: fullGeometry option.
- **Evidence:** SOURCED · **MS:** M4

### API-28 · `drawBrowserSelection(comp[, callback[, options]])`
- **Req:** drawBrowserSelection(comp[, cb[, options]]).
- **Accept:** AE: draw options honoured.
- **Evidence:** SOURCED · **MS:** M5

### API-29 · `draw(comp, addObj[, callback[, options]])`
- **Req:** draw(comp, addObj[, cb[, options]]).
- **Accept:** AE: draws from URL.
- **Evidence:** SOURCED · **MS:** M5

### API-30 · `modalEditObject(obj, callback[, options])`
- **Req:** modalEditObject(obj, cb[, options]).
- **Accept:** Unit: cancel fires callback.
- **Evidence:** PARTLY INFERRED · **MS:** M0

### API-31 · `modalActions(actions, callback[, options])`
- **Req:** modalActions(actions, cb[, options]).
- **Accept:** Unit: undefined on cancel.
- **Evidence:** SOURCED · **MS:** M0

### API-32 · `githubBrowser([options])`
- **Req:** githubBrowser([options]).
- **Accept:** Unit: as UI-24.
- **Evidence:** SOURCED · **MS:** M4

### API-33 · `filesInDir(folder[, filterRegEx[, includeSubDirectories]])`
- **Req:** filesInDir(folder[, regex[, recurse]]) → File[].
- **Accept:** Unit (mock FS).
- **Evidence:** SOURCED · **MS:** M0

### API-34 · `readFile(file[, encoding])`
- **Req:** readFile(file[, encoding]).
- **Accept:** Unit.
- **Evidence:** SOURCED · **MS:** M0

### API-35 · `readFileCsv(file[, encoding])`
- **Req:** readFileCsv(file[, encoding]).
- **Accept:** Unit.
- **Evidence:** SOURCED · **MS:** M0

### API-36 · `readFileJson(file[, encoding])`
- **Req:** readFileJson(file[, encoding]).
- **Accept:** Unit.
- **Evidence:** SOURCED · **MS:** M0

### API-37 · `writeFile(file, content[, encoding])`
- **Req:** writeFile(file, content[, encoding]).
- **Accept:** Unit.
- **Evidence:** SOURCED · **MS:** M0

### API-38 · `userFolder()`
- **Req:** userFolder().
- **Accept:** Unit: returns MapForge user folder.
- **Evidence:** SOURCED · **MS:** M0

### API-39 · `userFile(fileName)`
- **Req:** userFile(name).
- **Accept:** Unit.
- **Evidence:** SOURCED · **MS:** M0

### API-40 · Overpass query macros
- **Req:** Overpass macros.
- **Accept:** Unit: as FB-16.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### API-41 · Reserved GeoJSON properties
- **Req:** Reserved GeoJSON properties.
- **Accept:** Unit: display name, name, coordinate_elevation honoured.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### API-42 · Engine bootstrap and ES5 polyfills
- **Req:** Engine bootstrap and ES5 polyfills; robust initEngine.
- **Accept:** AE: init survives a script that clobbers globals.
- **Evidence:** PARTLY INFERRED · **MS:** M0

### API-43 · Doc quirks to keep as aliases
- **Req:** Doc-quirk aliases.
- **Accept:** Unit: both spellings accepted.
- **Evidence:** PARTLY INFERRED · **MS:** M0

### API-44 · Busy queue
- **Req:** Busy queue: defer watch callbacks; queue geocodes.
- **Accept:** Unit: deferred during finalize.
- **Evidence:** PARTLY INFERRED · **MS:** M4

### API-45 · Doc-text clarifications confirmed by the bundled help
- **Req:** Callback timing per bundled help.
- **Accept:** Unit: draw callback after drawing.
- **Evidence:** SOURCED · **MS:** M4


## 19. Licensing / support / misc (LIC)

### LIC-01 · Commercial licensing
- **Req:** No commercial licensing (hobby build).
- **Accept:** N/A.
- **Evidence:** SOURCED · **MS:** M10

### LIC-02 · About window
- **Req:** About/version info with build number and prerelease flag (shown in the Support dialog's licence section, LIC-19).
- **Accept:** Unit.
- **Evidence:** SOURCED · **MS:** M10

### LIC-03 · About & Licensing panel
- **Req:** About & Licensing panel (licence info + Deactivate stub; third-party licences/attributions). (I) Default: as sections of the Support dialog (LIC-19), since EBE shows licence, version and credits there; a separate About entry stays possible until confirmed (UI-19).
- **Accept:** Unit: lists OSS licences.
- **Evidence:** PARTLY INFERRED · **MS:** M10

### LIC-04 · Auth error handling
- **Req:** Graceful auth errors (provider keys).
- **Accept:** Unit: 403 → message, no crash.
- **Evidence:** SOURCED · **MS:** M2

### LIC-05 · "Data not licensed" dialog (MapTiler gate)
- **Req:** 'Data not licensed' dialog (MapForge wording).
- **Accept:** Unit: text and buttons.
- **Evidence:** SOURCED · **MS:** M2

### LIC-06 · MapTiler integration terms
- **Req:** Provider key handling, no bundled default key.
- **Accept:** Unit: no key → free styles.
- **Evidence:** SOURCED · **MS:** M2
- **Free substitute:** MapTiler Free key optional.

### LIC-07 · GEOlayers Mapdata (legacy subscription)
- **Req:** Legacy Mapdata (omit).
- **Accept:** N/A.
- **Evidence:** PARTLY INFERRED · **MS:** M10

### LIC-08 · Disclaimer and attribution
- **Req:** Disclaimer and attribution.
- **Accept:** Unit: About shows disclaimer.
- **Evidence:** SOURCED · **MS:** M10

### LIC-09 · Log files
- **Req:** Log files.
- **Accept:** Unit: log written to user folder.
- **Evidence:** SOURCED · **MS:** M0

### LIC-10 · Undo hygiene
- **Req:** Undo hygiene.
- **Accept:** AE: each action one undo step.
- **Evidence:** SOURCED · **MS:** M0

### LIC-11 · "#" in file paths
- **Req:** '#' in paths: descriptive error.
- **Accept:** Unit.
- **Evidence:** SOURCED · **MS:** M0

### LIC-12 · AE version support
- **Req:** AE 25–27 support, CEP 12.
- **Accept:** AE: smoke test on each.
- **Evidence:** SOURCED · **MS:** M0

### LIC-13 · Install and update
- **Req:** Install via folder + PlayerDebugMode; optional self-signed ZXP; 'Check for updates' shows the latest version (stub or user-hosted version file).
- **Accept:** Doc: install steps verified. Unit: update check reads a version file and shows the latest number.
- **Evidence:** SOURCED · **MS:** M0

### LIC-14 · Help and troubleshooting
- **Req:** Help menu (docs, tutorial links, keyword search) and troubleshooting topics.
- **Accept:** Unit: links open; keyword search finds a topic.
- **Evidence:** PARTLY INFERRED · **MS:** M10

### LIC-15 · Panel not opening on some Macs
- **Req:** Panel opening on macOS (known issue).
- **Accept:** AE (mac): panel opens.
- **Evidence:** SOURCED · **MS:** M10

### LIC-16 · Bundles with Helium X / FreeForm Pro / Plexus 3
- **Req:** Plugin bundles (no functional impact).
- **Accept:** N/A.
- **Evidence:** SOURCED · **MS:** M10

### LIC-17 · GEOlayers.app (sibling web product)
- **Req:** GEOlayers.app out of scope.
- **Accept:** N/A.
- **Evidence:** SOURCED · **MS:** M10

### LIC-18 · MapTiler trial/default-key request limit
- **Req:** Request-limit handling (clear error + fallback).
- **Accept:** Unit: 429/403 from MapTiler → message and free fallback.
- **Evidence:** PARTLY INFERRED · **MS:** M2

### LIC-19 · Support dialog (four sections)
- **Req:** Panel menu 'Support' opens a dialog with four sections: licence information (version; Deactivate as a no-op stub), contact support (form with priority; hobby: saves a local report with logs, no vendor endpoint), check for updates (latest version, LIC-13), terms of use + third-party credits (LIC-08). Hobby treatment of all four sections is a user decision (Q-54).
- **Accept:** Unit: the four sections render in this order; contact form requires a priority and writes a report file; update check shows a version; credits list every bundled OSS licence.
- **Evidence:** PARTLY INFERRED · **MS:** M10


## Traceability summary

| Milestone | Items |
|---|---|
| M0 | 26 |
| M1 | 52 |
| M2 | 65 |
| M3 | 15 |
| M4 | 97 |
| M5 | 49 |
| M6 | 47 |
| M7 | 38 |
| M8 | 13 |
| M9 | 14 |
| M10 | 28 |
| **Total** | **444** |
