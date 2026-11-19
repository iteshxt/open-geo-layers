# GEOlayers 3 — Community source mining (raw notes)

Captured 2026-11-18. Scope: community/third-party sources (aescripts forums, Adobe Community, Reddit,
Creative COW, Toolfarm, sigmaeditor, Mettle, MapTiler docs, Stadia Maps docs, course outlines, X posts).
Clean-room: no product binaries, ZXPs, installers or vendor example zips were downloaded or inspected.
Warez/"free download"/"crack" sites appeared in search results (vfxmed, aeblender, filecr, psdly, gfxplugin,
riztagar, iggtech, allmacworld, anyplugins, edx.ipeindia, avitmarine, several gumroad resellers, a Scribd
"Installation Method" doc). They were deliberately NOT opened or used.

Legend: **[SOURCED]** = stated by the cited source. **[INFERRED]** = my reasoning. **[GL2]** = source is about
GEOlayers 2 (may or may not carry over to v3).

---

## 0. Access log (what worked / what failed)

| Source | Result |
|---|---|
| https://aescripts.com/forums/discussion/comment/41477/ | WebFetch HTTP 403. Content recovered only from search-engine snippet (see §2.1). |
| https://aescripts.com/forums/discussion/950/geolayers/p10 | 403. Snippets only. |
| https://aescripts.com/geolayers/ (product page) | 403. |
| https://aescripts.com/faq/article/view/faq/geolayers3-faq/ and /knowledgebase/index/view/faq/geolayers3-faq | 403. Snippets only. |
| https://aescripts.com/learn/post/... (3D cameras, Helium terrain, label templates, 18 tips) | 403. Snippets only. |
| toolfarm.com (?p=33496, ?p=79982, ?p=101740, /buy/markus_bergelt_geolayers/) | 403 (blocked). |
| sigmaeditor.com article | WebFetch "Command failed with no output" (twice). Search snippet only. |
| mettle.com sliced landscape (GL3) + 360 landscapes (GL2) | OK (step titles only; details are in video). |
| docs.maptiler.com (2 GEOlayers pages), maptiler.com/cloud/geolayers/ | OK. |
| docs.stadiamaps.com GEOlayers quickstart | OK. |
| community.adobe.com threads (8 threads) | OK. |
| reddit.com | WebFetch refuses domain; site: searches returned zero Reddit hits. No Reddit data. |
| creativecow.net | Searches found no GEOlayers-3-specific thread (only generic Collapse Transformations threads). |
| web.archive.org | WebFetch refuses domain. |
| vimeo.com | Bot-verification page; only titles. |
| youtube.com | Only titles (descriptions/chapters truncated). |
| boonelovesvideo.teachable.com course landing page | OK (full curriculum). Lecture pages locked. |
| udemy course | 403. |
| curl with browser UA | Denied by the permission system (not attempted further). |

---

## 1. Finalize / imagery / tile caching

- **[SOURCED]** aescripts GEOlayers 3 FAQ (entry still headed for GEOlayers 2): "black or missing areas after finalization"
  → check the **work area** in the timeline of the **mapcomp**; finalization only covers the work area; fix = set work area,
  finalize the mapcomp again. Source: https://aescripts.com/faq/article/view/faq/geolayers3-faq/ (search snippet).
  Consistent with API option `onlyWorkArea` (help-readme.md).
- **[SOURCED]** Same FAQ: "blank window after first run" → remove with ZXP installer and reinstall; antivirus/firewall can
  block licensing in the background. (search snippet of FAQ)
- **[SOURCED][GL2]** aescripts forum "GEOlayers" thread p10: dev reply "GEOlayers finalize only the frames inside the workarea";
  users with blurry/black tiles told to delete tiles / **purge tile cache**; one user purged "tiles, the GEOlayers cache, and
  the AE cache" without success; preview looked fine on another tile server but blurry after finalizing.
  https://aescripts.com/forums/discussion/950/geolayers/p10 (snippet).
- **[SOURCED]** Adobe Community, Jul 31 2025 "Geolayers 3 issue: entire portions of my map disappear when finalizing":
  everything fine until **"Finalize" button**; whole map sections vanish; no fix posted.
  https://community.adobe.com/t5/after-effects-discussions/geolayers-3-issue-entire-portions-of-my-map-disappear-when-finalizing/td-p/15438647
- **[SOURCED]** Search snippet of an aescripts forum comment: "every time I finalize a map, a big empty square appears over
  Ukraine" (re-finalizing/starting over did not help). https://aescripts.com/forums/discussion/comment/39509/ (via search summary; not directly verified).
- **[SOURCED]** Adobe Community bug report Mar 7 2026: AE 25/26 throw "AEGP Plugin PNGIO Support: PNGIO library error: bad
  adaptive filter value (5027::12)" and "IDAT: CRC error (5027::12)" when GEOlayers map tiles load; same project fine in AE 24.
  https://community.adobe.com/bug-reports-528/after-effects-25-26-pngio-error-5027-12-when-using-geolayers-3-works-in-ae-24-1552832
  - **[INFERRED]** GEOlayers writes imagery as **PNG files on disk** that AE imports as footage (tile/frame PNGs). A corrupted/partially
    written PNG (or a PNG variant AE 25's PNGIO is stricter about) breaks import. Our recreation must write fully-flushed,
    standard PNGs (atomic write: temp file → rename) and validate CRCs.
- **[SOURCED]** Masterclass lesson "3 Ways to Finalize Imagery (1:49)" under "Best Practices", plus "Improve Playback Performance (7:05)",
  "Troubleshooting (9:28)". https://boonelovesvideo.teachable.com/p/geolayers-3-masterclass
  - **[INFERRED]** The 3 ways most likely = finalize all frames / only work area / only current frame (matches API booleans
    `onlyCurrentFrame`, `onlyWorkArea`; plus `previewQuality`). Unverified.
- **[SOURCED]** "18 GEOlayers Tips in 18 Minutes" (Boone Loves Video, on aescripts learn): chapters include **"2:09 Finalize One Frame"**,
  "3:00" route-animation tip, "6:42 Create a Custom Layer Style", "Custom Label Templates", pinning, arrows, scale bars, a section on
  changing preview settings to work in real time, **"16:48 Imagery Motion Blur"**. https://aescripts.com/learn/post/18-geolayers-tips-in-18-minutes (snippets)
- **[SOURCED]** Changelog cross-refs: "Mapcomp Settings for Motion Blur" (1.0.7); "Default Imagery Quality Setting in Preferences" (1.15.1);
  "Directly queue Renders after finalization" (1.2.6). (changelog.md)
- **[SOURCED]** Changelog 1.4.0 mentions "**mapcomp sync**" ("Blocked preview map for Mapcomps with min/max zoom when mapcomp sync is off") →
  there is a toggle that syncs the panel preview map with the selected Mapcomp's view. (changelog.md; surfaced by search)
- **[SOURCED]** Vendor/retail copy: v3 has "Faster finalization and rendering" vs v2; "GEOlayers handles all imagery rendering" while
  you keyframe scroll/zoom/pitch/rotate. https://www.editingcorp.com/animated-maps-in-after-effects-with-geolayers-3/
- **[SOURCED][GL2]** Mettle 360° landscape tutorial (GEOlayers 2): step "Finalize All on GEOLayers" → a **Finalize All** action existed in v2.
  https://www.mettle.com/360-landscapes-with-freeform-pro-geolayers-2-after-effects-cc-2019-tutorial/
  **[INFERRED]** v3 equivalent = API `finalize(undefined, ...)` "finalize all Mapcomps in the project"; UI button name unknown.

## 2. Mapcomp structure, anchor layer, controller / linked mapcomps, collapse transformations

### 2.1 aescripts comment 41477 — "Collapse transformations on Controller comps not working?"
- **[SOURCED]** (search snippet; page itself 403): user has a **multiply-blended shape layer inside a controller comp** that ignores
  blending in the map comp **even with collapse transformations on**; both comps use **Classic 3D** renderer; "If i pre compose any
  element in the main comp, the collapse transformations & blending mode works but the controller comps do not." No resolution visible.
  https://aescripts.com/forums/discussion/comment/41477/
- **[INFERRED]** "Controller comps"/"Controller Mapcomps" are additional comps whose view is driven by expressions from a master
  Mapcomp (linked). Because they are nested comps with expressions-driven transforms (likely 3D layer + expression-linked
  position/scale/rotation to emulate pitch/bearing), AE blending across nested collapsed 3D comps behaves per AE rules
  (a collapsed precomp with 3D layers splits into 3D render groups; blend modes don't cross 3D-plane boundaries). Our recreation
  should document that blending modes inside controller comps only blend within that comp.

### 2.2 Controller Mapcomps / linked Mapcomps (what is known)
- **[SOURCED]** Masterclass "Create Mapcomps" section: "How a Mapcomp Works (5:47)", "Mapcomp Settings (6:02)", "**Controller Mapcomps** (4:31)",
  "**Duplicate Mapcomps** (3:45)", "Where to Find License Information (1:17)". (teachable course page)
- **[SOURCED]** Changelog: "Collapse linked Mapcomps in the Mapcomp List" (1.3.0) — UI collapse (tree) of linked mapcomps, NOT AE's Collapse
  Transformations switch; "Removing a Mapcomp with others linked to it gives the option to remove all linked ones too" (1.5.0);
  "Change Mapcomp duration via Mapcomp Settings (also changes related Mapcomps' durations)" (1.2.4); "Mapcomp naming supports
  leading zeros automatically (Mapcomp 01, Mapcomp 02...)" (1.3.0); "Confirm dialog before removing a Mapcomp" (1.2.6);
  "Troubleshooting for corrupted projects due to After Effects 'Recovering' Mapcomps" (1.14.0). (changelog.md)
- **[SOURCED]** Search snippet of aescripts forum reply: duplicate a mapcomp "with its containing comp" from the **mapcomp list** in GEOlayers;
  the duplicate is needed only to handle the imagery; one can trim the mapcomp layers and keep working in the original containing comp
  (still drawing features and adding labels). https://aescripts.com/forums/discussion/comment/24493/ (snippet, version uncertain)
- **[SOURCED]** Search snippet: dev advises opening/importing projects **through the GEOlayers panel** so it "takes care of all expressions,
  comp naming and so on". Matches API `importProject()` ("sort everything into its project structure, take care of unique comp names
  and update expressions"). (snippet; help-readme.md)
- **[SOURCED][GL2]** aescripts GEOlayers 2 thread: user with "many feature comps linked to a main control comp" asks how to choose which
  comp a new feature is placed into. https://aescripts.com/forums/discussion/1402/geolayers-2/p7 (snippet)

### 2.3 Anchor layer / parenting / "pseudo-3D"
- **[SOURCED]** Help: Mapcomp's **anchor layer** sits above the Mapcomp in the containing comp, matches the view animation, used to parent layers.
  (help-readme.md)
- **[SOURCED]** aescripts dev reply (snippet): "Easiest solution would be to import any cloud footage to the Mapcomp's containing Comp and
  parent it to the Mapcomp's Anchor layer"; for particle systems "parent this reference layer to the Anchor Layer of your Mapcomp".
  (aescripts forum, via search summary)
- **[SOURCED]** Adobe Community Aug 2023 ("AfterEffects GeoLayer3"): user parented everything to a layer named "**World map comp anchor**"
  (i.e., anchor layer naming pattern "<Mapcomp name> anchor" — INFERRED from the quote); when zoom/camera keyframes added, "everything
  goes out of place". Mylenium: plugin palette has tools to "sync motion to the camera and pin objects in 3D". (Mylenium's "baked data
  from Google Earth" claim is wrong/irrelevant — INFERRED; GEOlayers 3 uses tile servers, not Google Earth.)
  https://community.adobe.com/t5/after-effects-discussions/aftereffects-geolayer3/m-p/14021559
- **[SOURCED]** Adobe Community Aug 2023 "Problems with orient along path in 3d composition": GEOlayers 3 comp with a 3D camera fly-in;
  user suspects "GEOlayers doesn't use a camera, it's a sort of pseudo 3d with an anchor null".
  https://community.adobe.com/t5/after-effects-discussions/problems-with-orient-along-path-in-3d-composition/td-p/14019807
  - **[INFERRED]** Mapcomp pitch/bearing are rendered inside the imagery (pitched raster), and the anchor layer is a 3D null whose
    transform is expression-driven from Latitude/Longitude/Zoom/Bearing/Pitch so overlay layers follow. Separate camera-based 3D
    exists via the "Create 3D Scene & Camera" default script (1.12.2).
- **[SOURCED]** Adobe Community Jul 2024 frontline thread: Boone Loves Video tutorial anchors a **solid** (with Turbulent Displace) to the map
  with GEOlayers 3; non-plugin workaround = parent to null → parent null to map.
  https://community.adobe.com/t5/after-effects-discussions/how-do-you-anchor-a-solid-layer-to-a-map-to-illustrate-an-animated-frontline/m-p/14719231/highlight/true
- **[SOURCED]** Adobe Community Dec 2021 "Layering Error": GEOlayers layers are **3D layers**; overlapping layers at identical Z flicker in
  stacking; fix = offset Z by 1px or toggle 3D switch. https://community.adobe.com/questions-529/layering-error-in-after-effects-42201
  - **[INFERRED]** Drawn shape layers / labels placed by GEOlayers are 3D layers at the same Z plane; recreation should give each new
  overlay layer a tiny Z offset or keep them 2D-ordered to avoid z-fighting.
- **[SOURCED]** Pinning (changelog): pinned layers have separate Latitude, Longitude and Height properties; animated position via
  "**Position Progress**" slider; unpin by clicking the pin button again; "(pinned)" suffix in layer name. Masterclass lessons:
  "Pin Layers to a Map or Feature (4:07)", "Parent Layers to a Map (2:59)".

### 2.4 Missing mapcomp / corrupted projects
- **[SOURCED]** Adobe Community Nov 2021–Sep 2024 "GEOlayers 3 layers not visible in a project created 1 year ago": layers above the mapcomp
  missing; resetting transforms partially restores; Adobe called project "corrupt"; one fix: the external AE **cache drive** had disconnected,
  "MapComp could no longer be found"; reconnecting + re-setting cache fixed it.
  https://community.adobe.com/t5/after-effects-discussions/geolayers-3-layers-not-visible-in-a-project-created-1-year-ago-december-5-2020/m-p/14880531
  - **[INFERRED]** Mapcomp imagery (finalized and/or preview tiles) is stored as files referenced by the project; if the storage path is
    unavailable, footage goes missing and expressions referencing it fail → layers invisible. Store imagery paths relative to project or a
    known user folder and relink on open.
- **[SOURCED]** Changelog 1.14.0 "Troubleshooting for corrupted projects due to After Effects 'Recovering' Mapcomps".

## 3. Performance / limits

- **[SOURCED]** Adobe Community Mar 2021 "How to more efficiently load many point features": drawing ~3484 point features → GEOlayers shows a
  warning dialog "You're trying to draw 3484 features with 3484 ..." (text truncated in source); AE hangs; user used
  "**Simplify Geometry (Current Zoom)**" option. Advice: split project, cull layers.
  https://community.adobe.com/t5/after-effects/how-to-more-efficiently-load-many-point-features-with-geolayers-performance-q/m-p/11865951
  - **[INFERRED]** Point features are drawn as individual layers (or per-feature shape groups) → thousands of layers kill AE. GEOlayers
    warns above a threshold (exact threshold unknown). Recreation: same warning + offer "Circles Imagery for DataViz" (1.2.2) / Choropleth
    Mapcomp imagery style (1.1.8) as the scalable path (rendered into imagery rather than layers).
- **[SOURCED]** Changelog: "Mapcomps with lower zooms than imagery minzoom → extremely long loading" (fixed 1.2.4); "Multi Polygon shapes
  render faster (different shape layer content structure)" (1.2.5); "Labeling empty Feature Collections freezes the panel" (fixed 1.4.0);
  "large OSM downloads freeze panel" (fixed 1.0.6); merge/subtract/simplify are "non-blocking & cancelable" (1.2.1).
- **[SOURCED]** Masterclass "Improve Playback Performance (7:05)" — content locked.

## 4. Providers / imagery setup (community docs)

### 4.1 MapTiler
- **[SOURCED]** MapTiler docs "How to use MapTiler maps in GEOlayers": API key from MapTiler Cloud "administration menu"; create a dedicated key;
  **first-time entry: click the maps marked with a red exclamation mark, paste key into the prompt**; to replace: **open preferences, scroll
  to the bottom, enter new key in the field**; you can import custom styles generated in MapTiler; edit MapTiler maps with GEOlayers' style tools.
  Licensing: tile usage needs Flex/Custom plan; video material license needed for TV/movie and YouTube/Vimeo channels >100k subscribers.
  https://docs.maptiler.com/guides/maps-apis/maps-platform/how-to-use-maptiler-maps-in-geolayers/
- **[SOURCED]** MapTiler guide "MapTiler maps for Adobe After Effects via GEOlayers 3 plugin": features = flat maps, **3D buildings, 3D terrain,
  watermasks, labels in multiple languages**; steps install AE → install plugin → activate FLEX → connect → use.
  https://docs.maptiler.com/guides/maps-apis/maps-platform/maptiler-cloud-maps-for-adobe-after-effects-via-geolayers-3-plugin/
- **[SOURCED]** maptiler.com/cloud/geolayers: Flex $30/month; "default key must be replaced" with own key from Keys page; start by choosing a
  **Mapcomp style**, browsing MapTiler Cloud maps, or building your own; attribution required. https://www.maptiler.com/cloud/geolayers/
- **[SOURCED]** help-readme "MapTiler Data": stylized Mapcomp Imagery (styles with MapTiler icon), find features by clicking preview map,
  Mapcomp labels in different languages, **Watermask Mapcomps from the toolbar**, current-view feature import, **3D buildings from toolbar**.
- **[SOURCED][older]** aescripts "GEOlayers Mapdata" forum thread (2019–2020): license came with 6 months of mapdata; afterwards plugin works but
  "styleable maps" stop; imagery styles needing Mapdata have "**the little Badge top right in the thumbnail**"; e.g. Stamen "Toner Background"
  available without it. https://aescripts.com/forums/discussion/2017/geolayers-mapdata (snippet)

### 4.2 Custom raster tile servers (Stadia Maps doc = best public UI walkthrough)
- **[SOURCED]** Stadia "Quickstart: GEOlayers 3": with GEOlayers panel open, **select the settings for the Mapcomp** → select the new
  "**Mapcomp Imagery**" option → enter raster source URL, e.g. `https://tiles.stadiamaps.com/tiles/stamen_toner/{z}/{x}/{y}@2x.png?api_key=...`
  → update the **name** (e.g. "Stadia Maps - Stamen Toner") → apply to save the source to your **style library** → **click apply again** to
  update the selected Mapcomp → GEOlayers re-renders the Mapcomp. Image alt texts: "Opening the GEOlayers Mapcomp settings", "Adding a new
  Mapcomp Imagery source to GEOlayers", "Applying the new imgery source to your selected Mapcomp in GEOlayers".
  https://docs.stadiamaps.com/tutorials/getting-started-with-geolayers/
  - **[INFERRED]** Tile URL template uses `{z}`, `{x}`, `{y}` placeholders (plus `{year}`, `{month}`, `{date}`, `{dateISO}` per 1.4.0;
    TMS supported per 1.0.9 "Tilemerger supports TMS servers"). `@2x` retina tiles accepted as literal URL text.
- **[SOURCED][GL2]** aescripts thread p10 (2017): convert GL2-style variables `{{ZoomLevel}}`→`{z}`, `{{pY}}`→`{Y}`, `{{pX}}`→`{X}` (snippet).
- **[SOURCED]** Product copy (via editingcorp/gumroad snippets): "Old GEOlayers 2 **server profiles**, shape layer styles, and label templates can
  still be imported"; legacy GL1/GL2 projects NOT supported. Requires AE CC 2015 (13.6)+ (Toolfarm snippet says CC 2017+; conflict).
- **[SOURCED]** aescripts forum (snippet): user wanting GeoTIFF texture on heightmap advised to make a **local tile directory** from GeoTIFF with
  MapTiler; GEOlayers can use local tile dirs (changelog 1.1.8 "Pick local tile directories", 1.5.2 min/max zoom read from "metadata.json").
- **[SOURCED]** Stamen removed 1.6.4 (moved to Stadia); Mapbox supported as custom tile source ("How to Work With Mapbox (2:23)" lesson;
  product copy "tile-server imagery such as Mapbox").

## 5. 3D landscapes

- **[SOURCED]** Mettle "How to Create a 3D Sliced Landscape | GEOlayers 3 + FreeForm Pro": GEOlayers steps = "Create 3D Landscape",
  "Keyframe GEOlayers 3 landscape", "Finalize GEOlayers 3", "Create landscape edge", "Adjust landscape elevation"; then FreeForm Pro
  render as Points etc. https://www.mettle.com/how-to-create-a-3d-sliced-landscape-geolayers-3-freeform-pro/
- **[SOURCED][GL2]** Mettle 360° (GL2): "Open GEOLayers and Select 3D Landscape" → remove unnecessary layers → resize **3D Landscape comp**
  square → add camera → resize **Texture comps** and **Height Map comps** to 4500 px → "Finalize All" → circle mask on texture+height comps
  → "Adjust Value Range in Height Map Comp".
  - **[INFERRED]** 3D Landscape = a container comp holding a displacement plugin layer (FreeForm Pro / Mir / Plexus / Helium) fed by two
    Mapcomp-like comps: a **texture** mapcomp (imagery) and a **heightmap** mapcomp (DEM tiles → grayscale). Changelog 1.5.2 confirms the
    heightmap uses AE's "**Calculations**" effect; "DEM tile requests ... stall finalization" (1.2.5) confirms DEM tiles are fetched during finalize.
- **[SOURCED]** aescripts learn "Create 3D Terrains ... GEOlayers and Helium": chapters "Create 3D Landscape Mapcomp", "Increase the Elevation
  Height", "Enable Depth of Field", "Create a Simple Loop Animation"; terrain "works with native After Effects cameras, and will cast shadows
  from lights". (snippet)
- **[SOURCED]** Product copy: "With Trapcode Mir, Mettle FreeForm Pro, or Plexus 3, generate landscapes from real elevation data in one click"
  (editingcorp). Helium added 1.5.0.
- **[SOURCED]** Masterclass project "Recreate the Johnny Harris 3D Hawaii Map (3rd Party Plugin Required) (11:19)".

## 6. Labels / templates (community)
- **[SOURCED]** Any AE composition can be a label template (product copy). Lessons: "How Labels are Drawn", "Fine Tune Label Placement",
  "Create Custom Label Templates", "**Mapcomp Label Templates**" (2:17); YouTube "GEOlayers 3 Tutorial: Assign Label Templates to Mapcomps".
- **[SOURCED]** "Label Anchor Point" layer effect controls default Scale/Rotate with Map (1.0.4); "LabelColors" comp (SHIFT-select reveals).

## 7. Panel / install / environment issues
- **[SOURCED]** Panel opened via **Window > Extensions > GEOlayers 3** (CEP). Reports of panel not opening on macOS 13 / M1 (2023), no fix.
  https://community.adobe.com/t5/after-effects-discussions/geolayers-3-not-responding-after-trying-to-launch-it-through-quot-window-gt-extensions-gt-geolayers/m-p/13712683
  https://community.adobe.com/t5/after-effects-discussions/geo-layers-not-showing-in-after-effects-in-macbook-air-m1/m-p/14198877
- **[SOURCED]** X @GEOlayers: "ZXP/UXP Installer FAQ" post (Feb 2025); updates via aescripts manager app. https://x.com/GEOlayers/status/1892224844776779833
- **[SOURCED]** "Undo group mismatch" is a generic AE warning from scripts; changelog 1.5.3 "Close Undo Groups on each host call manually" +
  hidden pref to purge undo cache — confirms GEOlayers had undo-group issues.
- **[INFERRED]** Requires AE pref "Allow Scripts to Write Files and Access Network" (standard for CEP+ExtendScript file writes). Only seen in an
  untrusted third-party doc; not used as a source.
- **[SOURCED]** GEOlayers also markets a separate browser product "GEOlayers.app" (no AE needed) — X posts 2025. Out of scope for the AE extension.

## 8. Course outline as a UI-concept inventory (Boone Loves Video GEOlayers 3 Masterclass, AE 2023)
Source: https://boonelovesvideo.teachable.com/p/geolayers-3-masterclass
- Hello There: Welcome; **A Tour of the Interface (12:08)**; **MapTiler Features (Not Required for Course)**; Contact, Support and Updates
- QuickStart: Create a Mapcomp; Find Data; Draw Features; Add Labels; Animate the Map; Final Touches; **The Disputed Area Dataset**
- Create Mapcomps: How a Mapcomp Works; Mapcomp Settings; Controller Mapcomps; Duplicate Mapcomps; Where to Find License Information
- Map Features: Search for Features; Feature Properties; Download Datasets; Create Custom Features; Import and Export
- Draw Features: How Features are Drawn; Layer Styles; Create Custom Layer Styles; Control the Geometry; Draw Features in the Mapcomp
- Label Features: How Labels are Drawn; Fine Tune Label Placement; Create Custom Label Templates; Pin Layers to a Map or Feature;
  Parent Layers to a Map; Mapcomp Label Templates
- Animate!: Animate the Mapcomp View; Simple Transitions; Fine Tune Motion with the Graph Editor; Animate Paths and Routes; Animate Labels
- Map Design and Layout: Create a Locator/Inset Map; Draw a Scale Bar and North Needle; How to Add Texture; How to Use Mattes; Stylize With Adjustment Layers
- Advanced: How to Draw on Shape Layers; How to Draw Arrows; **Georeference Existing Maps**; How to Use Shadows; How to Add Feature Properties;
  How to Work With Mapbox; Animating Flags
- Project breakdowns: Census Regions of the US; Map Like Vox; Choropleth Map; Map Like RealLifeLore; Johnny Harris 3D Hawaii; Vasco Da Gama route; Panama Canal
- Best Practices: 3 Ways to Finalize Imagery; Improve Playback Performance; Troubleshooting

Notes: "Draw Features in the Mapcomp" = API option `drawInsideMapcomp` (default false → features drawn in the containing comp).
"How to Draw on Shape Layers" = 1.2.x "shape layers created to draw on" / individualLayers option. "How to Use Shadows" =
draw options `castsShadows`/`acceptsShadows`. "Disputed Area Dataset" relates to 1.7.0 disputed borders toggle. "Georeference Existing Maps" —
mechanism unknown (INFERRED: place an image layer and pin corners/parent to anchor).

## 9. Third-party descriptive claims (low weight)
- editingcorp: generate a style from the colors of a comp or image; import Adobe Swatch (.ase) files in one click; adjust colors, line widths,
  fonts, feature visibility; hillshading; online search for countries/cities/landmarks/POIs; export data for GIS; CSV/TSV data-driven styles;
  extruded buildings. https://www.editingcorp.com/animated-maps-in-after-effects-with-geolayers-3/
- ifmlearn tutorial page: map style settings include colors, opacity, borders, **hillshading**; manual coordinate input; label templates.
  https://ifmlearn.in/geolayers-3-map-animation-in-after-effects/
- sigmaeditor (snippet only): GEOlayers 3 named as the tool behind Vox / Johnny Harris style map videos; alternatives Google Earth Studio.
