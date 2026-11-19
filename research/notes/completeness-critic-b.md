# Completeness Critic B: new web searches against feature-inventory.md

Date: 2026-11-19. I read `research/feature-inventory.md` in full (2642 lines). Then I ran about 40 new web searches and fetched
public pages to find features or behaviors the inventory misses or gets wrong.

**Clean-room rule:** I did not download or inspect the GEOlayers product, its ZXP, the trial, `scriptingApiExamples.zip`, or any
"free download" or crack site. Those sites appeared in search results, and I ignored them.

**Tags:** [S] = SOURCED (URL given). [I] = INFERRED (my reasoning). [S-weak] = only a search-engine snippet, because the page
itself was unreachable.

## 0. Source availability (what worked, what didn't)

- **aescripts forums:** every `aescripts.com/forums/discussion/...` URL now returns **404** (the forum is gone), including the
  pages cited in the inventory as AESF (comment/41477, /24493, discussion/950, discussion/2017).
  - The Wayback Machine is not reachable from the fetch tool.
  - So forum facts can only come from search snippets [S-weak].
- **aescripts learn/blog/FAQ and toolfarm pages:** readable through the `r.jina.ai` reader proxy.
- **GitHub (vendor org `GEOlayers`):** readable through the API.
  - The org has 3 repos: `Help`, `geodata`, `development`.
  - **New:** `Help` has 1 closed issue and 1 open PR (see §1).
- **YouTube:**
  - Public watch pages (title, description, chapters) were readable until YouTube rate-limited me with a Google "sorry" captcha page. I
    stopped there and did not try to get around it.
  - Transcript retrieval was **denied by the permission system**, so I did not pursue it.
  - So "GEOlayers 3 for Beginners: Every Button Explained" (DX_qi37vy0Y) is still description-only.
- **indietips.com** (3D-terrain article) is unreachable: DNS failed, and the proxy returned 422. Only the search snippet is available.
- **Not fetched on purpose:** `https://geolayers3-v2-prod.web.app/` (the new host of the examples zip, per PR #2). That host may serve
  product assets, and the hard rules forbid inspecting the product. **The user should decide whether a public help page there is
  acceptable.**

## 1. NEW: Scripting API details from the vendor Help repo (issue #1 / PR #2)

**Source:** https://github.com/GEOlayers/Help/issues/1 (closed 2026-10-25) and https://github.com/GEOlayers/Help/pull/2 (open,
created 2026-10-25, by user `volition74`). The PR says its text matches the **bundled help.html of GEOlayers 1.19/1.20** and that
the signatures were validated against it. It is a third-party transcription of vendor docs and is not merged.

I read the PR head README at
`https://raw.githubusercontent.com/volition74/Help/11a8d7cfe3435a48a6de0ffb206dda6c834337ed/README.md` and diffed it against
`sources/help-readme.md`. The differences are below. [S]

- **`geolayers3.snapLayerToMapcomp(comp, layer, coordinates, [options])`**
  - Description: "snap any layer to a Mapcomp. Snapped layers will follow the mapcomp's viewport transformations and
    optionally animate along a specified path."
  - `comp`: Mapcomp name, Mapcomp, or containing comp.
  - `layer`: the layer to snap.
  - `coordinates`: optional `[longitude, latitude]`, or an array of such arrays for a path.
  - `options`:
    - `scaleWithMap` (default **true**)
    - `rotateWithMap` (default **true**)
    - `rotateAlongPath` (default **true**)
    - `bearingOffset` (degrees, default **0**)
    - `threeDLayer` (default **true**)
    - `duration` (seconds; the path-animation duration when an array of coordinates is given)
  - Returns the snapped layer, or undefined.
  - This resolves **API-16 / U-14 (signature unknown)** and partly **PIN-08 ("difference from pin unknown")**.
- **`geolayers3.addLabel(comp, labelTemplateComp, labelData, [coordinates, [options]])`**
  - `labelTemplateComp` "Can also be null to add a null layer instead."
  - `coordinates`: optional `[lon, lat]`, or an array of them for a path to animate along.
  - `options.duration`: seconds of path animation.
  - This resolves the "exact shape of the array is unknown" note in **API-12 / LBL-24**. The coordinates are a **separate 4th
    argument**, not part of `labelData`.
- **`draw()` / `drawBrowserSelection()` callback:** "called after the data has been **drawn**". This confirms that C-11 is a
  copy-paste error.
- **`modalActions()` description:** "let the user choose one of a certain number of actions". This also confirms C-11.
- **Scripting-examples URL** changed from `geolayers3-live.firebaseapp.com` to `geolayers3-v2-prod.web.app`. The issue says the
  two archives are identical. Hard rule: do not download it.
- **Not documented anywhere:** the PR left out "Dataviz – Lines Imagery Style", because the bundled help has no usage docs for it.
- The general-help section of the README "stops partway through the Mapcomps documentation". This matches the truncated "As you
  hover over a Mapcomp the following buttons appear:" (HELP L27).

[I] snapLayerToMapcomp's options (scale with map, rotate with map, rotate along path, bearing offset, 3D layer, path duration) are
very likely the same toggles the UI exposes when snapping or pinning a layer, so they should be UI checkboxes too. This needs to
be verified.

## 2. NEW: two vendor example scripts not in the local corpus

The vendor repo `GEOlayers/geodata` (tree via `https://api.github.com/repos/GEOlayers/geodata/git/trees/main?recursive=1`)
contains **9** files. The local `sources/` folder has only 7 of them. Missing [S]:

- `RKI COVID19 Bundeslaender Data import.jsx`:
  `geolayers3.addToBrowser("https://opendata.arcgis.com/datasets/ef4b445a53c1406892257fe63129a8ea_0.geojson", {namingProp:"LAN_ew_GEN", name:"RKI Bundesländer " + (new Date).toDateString()})`
- `RKI COVID19 Landkreise Data import.jsx`:
  `geolayers3.addToBrowser("https://opendata.arcgis.com/datasets/917fc37a709542548cc3be077a786c17_0.geojson", {namingProp:"GEN", name:"RKI Landkreise " + (new Date).toDateString()})`
- Both carry the attribution comment "Bundesamt für Kartographie und Geodäsie / Robert Koch-Institut".

**Parity impact [I]:**
- These vendor scripts pass the **options object as the 2nd argument**, where HELP documents `addToBrowser(addObj[, callback[,
  options]])`. To stay compatible with vendor scripts, `addToBrowser` (and probably `draw` / `drawBrowserSelection` /
  `getBrowserSelection`) must accept `(addObj, options)` when the 2nd argument is an object rather than a function. Otherwise
  these scripts are silent no-ops. API-23 does not mention this.
- These scripts also show `namingProp` used with non-`name` properties, and ArcGIS Hub `.geojson` URLs used as the source.
- TB-16 and the EX list should add both RKI scripts.

## 3. NEW / refined: 3D Landscape creation UI

- [S-weak] The indietips.com write-up of Boone's "How To Create 3D Terrain in Adobe After Effects" (https://indietips.com/?p=68726;
  only the search snippet was readable) says:
  - In the GEOlayers 3 panel, "Before we even create a map composition, select this button that says 'Create 3D Landscape.'"
  - It then picks a resolution, **"Ultra HD 4K"**, and selects **Helium** as the renderer.
  - The resulting project "differs significantly from regular GeoLayers projects, especially in how you navigate and control the
    map view".
  - Terrain height is set by an **elevation slider in the Effect Controls of a pink layer named "Helium"**.
  - Double-clicking the camera enables depth of field.
- [S] aescripts learn page (via proxy) for YT:WUhJe_9DQ4Y. Chapters: "Create 3D Landscape Mapcomp", "The Controls", "Increase the
  Elevation Height", "Enable Depth of Field", "Add Lights", "Fix the Background", "Create a Simple Loop Animation". The description
  says the terrain works with native AE cameras and casts shadows from lights.
  https://aescripts.com/learn/post/create-3d-terrains-in-after-effects-using-geolayers-and-helium
- **Gaps vs the inventory:**
  - 3D-05 says "one-click creation". The sources instead show a **"Create 3D Landscape" panel button**, a **resolution preset choice
    (e.g. Ultra HD 4K)** and a **3D-plugin choice** (Helium / FreeForm Pro / Plexus / Mir).
  - The landscape has its own navigation mode.
  - Each plugin gets a named layer ("Helium") that carries the elevation control.
  - "Fix the Background" suggests a background issue or step specific to the Helium setup [I].

## 4. NEW / refined: Mapcomp label and attribution look (help image)

- [S] HELP README embedded image
  (https://user-images.githubusercontent.com/19971466/233547961-62f27e76-a9e3-4a64-815d-2f6af515c708.png, 858×235) shows a
  London Mapcomp:
  - Mapcomp labels are **UPPERCASE, letter-spaced, dark grey with a light halo**, e.g. "LONDON", "WESTMINSTER", "CITY OF
    LONDON", "HACKNEY".
  - Place labels have **place dots** (MLB-04).
  - Label sizes are hierarchical (city > district).
  - The attribution reads **"© MAPTILER © OPENSTREETMAP CONTRIBUTORS" in uppercase at the bottom-RIGHT**.
- **Contradiction with MCS-14 / LIC-08** ("attribution … observed bottom left", "italic text layer"). Vimeo thumbnails 528729028
  (Natural Earth) and 520372999 (MapTiler) show italic attribution at the bottom-left.
  - [I] Either attribution position is per-Mapcomp (the dropdown), or the help image's attribution is baked into the MapTiler
    style's own imagery.
  - Add this as a contradiction and verify it.

## 5. NEW (weak): behaviors from forum snippets

All of these come from search-engine summaries of now-404 aescripts forum pages. [S-weak]

- **"Animate view along feature" follows a smoothed path.** A user reports "the view is following a smoother version of the
  path to not have it wiggle too much". The search hit was in the GEOlayers forum comment set (comment/33880 or /39509; the exact id
  is unclear).
  - Parity: VIEW-06 needs a **path-smoothing step** before keyframing the view. Whether there is a smoothing parameter is unknown.
- **GPX routes can "adapt the view according to that route with one click"** (forum reply). This supports VIEW-06 / DAT-05 for
  imported tracks.
- **"Attribution info" button in Mapcomp settings** links to each provider's terms (forum thread "GEOlayers Mapdata", discussion/2017,
  snippet). This gives the exact name of the ⓘ in MCS-14: **"attribution info"**.
- The legacy Mapdata thread says "GEOlayers comes with 6 months mapdata subscription… after 6 months GEOlayers will continue to work,
  but … styleable maps will not work." This is already C-08.

## 6. Course lessons with no inventory item

The MC curriculum was fetched again via the proxy from https://boonelovesvideo.teachable.com/courses/1932860/lectures/44411996.
No lesson is a free preview. [S] These lesson titles map to **no** inventory ID. Each may hide a GEOlayers feature or may be a
pure AE technique [I]:

- "Improve Playback Performance" (Best Practices; 7:05 per n/community). [I] This is likely preview quality, Mapcomp sync, or
  turning off Mapcomp labels. Relates to FIN-07 / UI-13.
- "Animating Flags" (Advanced Techniques)
- "Stylize With Adjustment Layers" (Map Design and Layout)
- "How to Use Mattes" (Map Design and Layout). [I] Probably the watermask Mapcomp used as a track matte (IMG-17).
- "Simple Transitions" and "Animate Labels" (Animate!). [I] Possibly label intro/outro markers (LBL-09).
- From Boone's "Create Your First Map in GEOlayers" (YT:dBqZs9uT6Hw, 2024-11-25) chapters [S]:
  - "Choose the Mapcomp Basemap Imagery"
  - "How to Navigate the GEOlayers Panel"
  - "Download Map Data"
  - "Finalization Explained"
  - "How to Organize Your Map Features" (I: collections/favorites)
  - "How to Draw Out Features"
  - "Animate Map Features"
  - **"Revision Workflow"**. [I] Re-finalizing after changes; maybe duplicate or relink. The content is unknown.
- From a conflict-choropleth tutorial (YT:6hRMiQY6__c, 2024-12-31) [S]: "get subnational boundries in geolayers" (FB-13 / FB-14
  admin bounds) and "manipulating label templates in geolayers 3".

## 7. GEOlayers 2-era features (Vimeo user `geolayers`, page 3) that v3 may or may not keep

From the Vimeo API v2 (https://vimeo.com/api/v2/geolayers/videos.json?page=3) [S]. Whether each survives in v3 is unknown [I]:

- **"GEOlayers 2 Tutorial – 09.3 WMS"** (241651263): "connect GEOlayers 2 to Web Map Services". v3 imports GEOlayers 2 **server
  profiles** as imagery styles (IMG-28). If a GL2 profile can be WMS, v3 needs a WMS path or a defined failure for it. The inventory
  has **no WMS item**.
- **"Online Label Templates"** (242194872): "add online Label Templates to your projects with a few clicks". This supports LBL-17.
- **"Box Labels in Hong Kong"** (230256972): "Box Label Templates" family (GL2). Whether v3's default set includes "Box" templates
  is unknown.
- **"Create Car & Plane Routes" / "Animate Car & Plane Routes"** (230357893 / 230361037): GL2 plane routes. This supports the
  RTE-10 question.
- **"Swap Feature Styles"** (239417803): "apply a Feature Style to any Shape Layer". This is the GL2 precursor of SLS-11.
- **"Folded Map"** (248306385): "Unfolding paper map Sample Project". **"Elevation 4K"** (243509982): "4K Template Project for
  GEOlayers 2 using Trapcode Mir". PRJ-02 may want to note these GL2 samples. [I] v3's "Elevation" sample (CL 1.1.2) may descend
  from "Elevation 4K".
- **"New Feature Download"** (251186686): imports global Natural Earth datasets and OSM features for the current view. This is the
  GL2 precursor of FB-12 / FB-14.

## 8. Vendor YouTube channel (@GEOlayersAPP) and Markus's channel (@MarkusGEOlayers)

[S] I read the descriptions of all 22 + 29 listed videos (watch pages, 2023-2026).

- Almost all 2025-2026 uploads are about **GEOlayers.app** (web), which is out of scope per LIC-17:
  - "Globe Projection" (lcyUlF3iids, 2026-07-14)
  - "Switch Your Map to a Globe Projection – GEOlayers.app Tutorial" (UqHnzr2NbzQ, 2026-09-07)
  - "Greenland … One Click to a Globe | GEOlayers.app" (zeZSD_BJ8S4)
  - Symbol grids, emoji markers, lock layers, vignettes, voiceover, and more
- [I] GL3 1.20's prerelease "Globe and Equal Earth Map Projection via Plugin" (3D-10) arrived at the same time (Sep–Oct 2026) as
  the app's globe projection. The app videos are the best public description of the intended *behavior*: "turn a flat map into a
  3D Globe" with one setting. Use them as reference for U-12, but the AE mechanism stays unknown.
- "GEOlayers.app – Learn about Enterprise Workflows" (E5_Bo4hnwBc, 2025-12-01) says: "Your designer creates an interactive map in
  After Effects, saves it to GEOlayers.app" and "Direct access to projects from Adobe Premiere Pro with our new plugin". [I] This
  is marketing that probably refers to the app's own AE/Premiere plugin, not GEOlayers 3. No GEOlayers 3 changelog line mentions
  an app bridge, so treat it as **out of scope** and not missing.

## 9. Data and terms updates (provider-related)

- [S] https://www.maptiler.com/cloud/geolayers/ (fetched 2026-11-19):
  - The **Flex plan is $30/month**.
  - It "includes a video material license for individual creatives" with a **100,000-subscriber channel limit**.
  - TV, film and commercial streaming need a custom licence.
  - Attribution is required unless a custom licence says otherwise; discounts are offered for showing the MapTiler logo.
  - Setup: copy the key from the Keys page and paste it into GEOlayers; "The default key must be replaced".
  - Hobby use is unaffected (the inventory already recommends the Free plan).
- [S] The storefront snippet "Maps can be pitched and rotated in 3D and interact fully with the native After Effects 3D environment"
  (aescripts.com/geolayers via search summary). This adds weight to C-15: overlays and anchors must behave correctly with AE
  cameras and lights.

## 10. Searches that found nothing new (to avoid repeating them)

- "geolayers 3" + "rotate with map" / pinned
- "mapcomp settings"
- "controller mapcomp"
- "shape layer style"
- "equal earth" / "globe projection"
- prerelease globe 2026
- "watermask"
- "property diagram(s)"
- label template anchor / "scale with map"
- "feature browser"
- "animate view"
- "default scripts"
- "linked" mapcomp
- "create 3D scene"
- distance circle / north needle / scale bar
- "blend duration" / "imagery quality" / "imagery motion blur"
- hillshading
- choropleth / circles / heatmap
- texture / seamless / watermask mapcomp
- maneuver / routing provider
- find features / current view / favorites / label renaming
- right-click / alt-click / shift-click
- forum "mapcomp list"
- LabelColors / Label Anchor Point
- creativecow, reddit, provideocoalition reviews
- X/Twitter globe prerelease

**Property Diagrams (DAT-14)** still has **zero** public description beyond the changelog line. The same is true for "Lines Imagery
Style" (even the bundled help has no usage docs, per PR #2), "Heatmap", "Blend Duration" and the "Quality" slider.

## 11. Leads for the user (need a decision or hands-on check)

1. Allow or deny reading a public help page on `geolayers3-v2-prod.web.app`. It may be the bundled help.html with full current docs,
   but that host may also serve product code.
2. YouTube transcripts of DX_qi37vy0Y ("Every Button Explained"), rJ9asneXYrE (Thong tour), dBqZs9uT6Hw and WUhJe_9DQ4Y would answer
   U-01, U-02 and U-11. Transcript fetching was denied here, so the user can watch them or allow transcript access.
3. Hands-on checks in AE:
   - the snap/pin options UI
   - the "Create 3D Landscape" dialog (resolution presets, plugin list)
   - attribution position and style options
   - whether `addToBrowser(url, optionsObj)` works
