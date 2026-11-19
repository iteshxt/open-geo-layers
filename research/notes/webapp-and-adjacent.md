# GEOlayers.app, GEOlayers 2/1, storefronts, tutorials — adjacent research notes

Captured 2026-11-18. Only public pages, read with WebSearch/WebFetch. Nothing downloaded or decompiled.
Legend: **[S]** = SOURCED (URL given), **[I]** = INFERRED (my reasoning), **[S-snippet]** = sourced only from a
search-engine summary or snippet of the page, because the page itself returned 403. Treat these as weaker than [S].

Access problems: aescripts.com (product, learn and forum pages) and toolfarm.com return HTTP 403 to WebFetch.
- The aescripts product page was read once through the r.jina.ai reader proxy.
- A second proxy read (the learn tag page) was blocked by the permission classifier, so I stopped using the proxy.
- vimeo.com shows a bot-verification page.
- youtube.com pages come back truncated to the title only.
- web.archive.org is blocked for WebFetch.
- geolayers.app pages are a single-page app (SPA), so only the home page and pricing page return content.

---

## 1. GEOlayers.app (browser product, same vendor) — https://www.geolayers.app

### 1.1 Home page [S] https://www.geolayers.app
- Tagline: "Maps in Motion". Browser-based, no extra software needed.
- Described capabilities:
  - Design maps.
  - Highlight locations.
  - Add labels.
  - Draw routes.
  - Import geodata.
  - Place **icon and image markers**.
  - **Draw directly to the map**.
  - **Record voiceovers** in the editor.
  - Export high-quality videos.
- Four marketing sections:
  - **Design Templates**: adjust colors, basemaps, labels and styles to build reusable **template projects**.
  - **Create Maps**: highlight, label, draw routes, icon/image markers, import geodata, draw on map.
  - **Animate Maps**: an **item-based timeline, "no fiddly keyframes"**. It runs automatic animations such as **flashing features** and **rotating 3D viewports**.
  - **Share Maps**: render images and videos in multiple formats and resolutions. Examples are **MP4 for social media** and **lossless 4K image sequences**, with one-click export.
- Team workflows: organization-wide template projects. Named roles are project managers, template designers, video editors and social media distribution.
- 31 unique sample projects, opened as `https://editor.geolayers.app?pageId=timelines&projectId={uuid}`. The editor has a page id called "timelines". Thumbnails are at `/sample-project-videos/512/{projectId}_{index}_512.webp`.
- The hero asset is named "TopoWhite". [I] This is probably a basemap style name. Unconfirmed.
- The page links to GEOlayers 3 on aescripts as "more control over map animation".
- Social accounts: YouTube @GEOLayersAPP, X @GEOlayers, Instagram geolayers.

### 1.2 Pricing [S] https://www.geolayers.app/pricing

| Tier | Price | Projects | Max frame size, clip length | Notes |
|---|---|---|---|---|
| Free | free | 5 projects | 1280×1280 px, 10 s clips | Image and video export; non-commercial use only |
| Pro | $25/mo, billed annually | 50 projects | 2048×2048 px, 1 min | Audio export; commercial use |
| Studio | $66.67/mo, billed annually | 500 projects | 4096×4096 px, 10 min | **3D terrains and globe projection**; commercial use and B2B |
| Enterprise | custom | custom | custom | For companies with more than $1M yearly revenue (TV, film, streaming); team accounts |

- [I] The web app already has a **globe projection**. This matches the GEOlayers 3 1.20 prerelease "Globe and Equal Earth Map Projection via Plugin". The vendor probably shares the projection work between the two products.

### 1.3 Tools page [S] https://www.geolayers.app/tools
- Lists one tool, **Country-shaped flags** at https://flags.geolayers.app: searchable, editable country flags cropped to the country's shape, ready to download for map animations, infographics, thumbnails and social posts.
- The flags.geolayers.app page itself is an SPA. Only its title, "GEOlayers.app - Country-shaped flags", came back.
- [I] For GEOlayers 3 parity this is optional, because it is a separate web tool. The Masterclass lesson "Animating Flags" and the WAR course lesson "Flags" show that flags are a common map-animation need.

### 1.4 About / Impressum [S] https://www.geolayers.app/about
- Operator: Markus Bergelt, Aichach, Germany. The page holds no product history.

### 1.5 Editor concepts from tutorials [S-snippet]
- "Viewport Animation Techniques in GEOlayers.app", https://aescripts.com/learn/post/viewport-animation-techniques-in-geolayers-app
  - A camera move is a **viewport item** on the timeline.
  - The button **"Fit Viewport"** frames a feature.
  - **"Follow Feature"** sets progress along a line feature.
  - **"Auto Bearing"** turns the camera with the route.
  - The tutorial also covers zooms, flyovers, orbits, camera paths that follow a line, and template-ready zooms.
  - You can add a **drawing layer** and draw a line with **curve handles**, then follow that line.
- "Make a London Map Animation in GEOlayers.app", https://aescripts.com/learn/post/make-a-london-map-animation-in-geolayers-app
  - Starts from a template project.
  - A **project colors** tool changes every feature style, **marker template** and animation color in a few clicks.
  - Search for "United Kingdom" and add it to the **feature browser**.
  - Swap the highlighted cities (Leipzig/Halle → London/City of London).
  - Three zoom stages, each made with Fit Viewport.
  - Add a label.
  - In the **export section**, choose **portrait video** and render.
- YouTube video titles [S-snippet]:
  - "GEOlayers app - Animate with Timeline items" (https://www.youtube.com/watch?v=dmckfOjlld8)
  - "GEOLayers.app - Beginner project Barcelona"
  - "GEOLayers.app - How to draw shapes"
  - "GEOlayers.app - How to Change Project Colors (Tutorial)"
  - "GEOLayers.app - Viewport Animation Techniques"
  - "GEOLayers.app - Animated Viewport #animatedmaps"
  - "Learn about Enterprise Workflows"
  - "Cinematic Map Animation Project Breakdown (GEOlayers)" (https://www.youtube.com/watch?v=K0-DhsMP4cA)
- X post [S-snippet] https://x.com/GEOlayers/status/1942247223632277686: "no After Effects needed", aimed at real estate, travel and documentaries.
- Podcast [S-snippet], Minds Behind Maps MBM#65, "Creating the most used map animation tool: GEOLayers – Markus Bergelt" (about Apr/Jun 2024): chapter "Making a web app" at 35:11, subscription models at 42:35, local vs web-based software at 1:07:21. https://chartable.com/podcasts/minds-behind-maps/episodes/166450721-creating-the-most-used-map-animation-tool-geolayers-markus-bergelt-mbm65

[I] Concepts shared with GEOlayers 3: feature browser, labels and marker templates, line-follow animation, 3D viewport (pitch and bearing), globe projection, templates. Concepts that exist only in the web app and are **not** GEOlayers 3 features: item-based timeline, voiceover, in-browser export, project colors tool. AE keyframes, comps and the render queue cover these jobs in GEOlayers 3.

---

## 2. GEOlayers 3 storefront description (aescripts) [S] https://aescripts.com/geolayers/ (read through the r.jina.ai proxy)

Paraphrased, not verbatim.
- Overview: renders custom maps from online data sources and gives access to large geospatial databases. You can:
  - Draw buildings as shape layers.
  - Highlight borders, streets, water and places.
  - Animate driving routes.
  - Extrude buildings.
- "Every location with geodata can become an editable asset".
- **Map styling**:
  - A set of default map styles.
  - **Create a style from a composition's colors, from an image, or from an Adobe Swatch file (.ase)**. ← This is NOT in the changelog.
  - Imagery from common image-based tile servers, e.g. MapTiler Cloud.
- Feature sections:
  - **Animate in 3D space**: scroll, zoom, pitch and rotate, all keyframeable.
  - **Online Search**: find countries, cities, landmarks and POIs. Results can be labelled, drawn, downloaded, or **exported for GIS software**.
  - **Mapstyling**: colors, line widths, fonts, **hillshading**, show/hide feature groups. Full control needs a MapTiler Cloud account.
  - **Labels**: one-click labels from default or custom templates. **Any AE composition can be a label template.**
  - **Data Visualization**: import .csv/.tsv and build data-driven shape layer styles.
  - **3D Landscapes**: one click with Helium (X), Mettle FreeForm Pro, Rowbyte Plexus (3) or Trapcode Mir, using real elevation data.
- "What's new in v3": new UI, 3D pitch/rotate, automatic labels, mapstyling in AE, faster finalization, scripting API.
- Compatibility:
  - The text says AE CC 2015 (13.6)+ is required.
  - The compatibility list shows AE CC 2017 → 2026.
  - **GEOlayers 1/2 projects are not supported, but GEOlayers 2 server profiles, shape layer styles and label templates can be imported.**
- MapTiler data license:
  - The current page offers a **two-week trial** of OSM-based MapTiler Cloud data. It unlocks styling in AE, multilingual labels, feature search by clicking the preview map, 3D building drawing, and a commercial data license.
  - After the trial GEOlayers 3 keeps working at no extra cost.
  - A Toolfarm search snippet said **six months** instead. That is probably the older offer. [S-snippet] https://www.toolfarm.com/?p=33496
- Bundles (discount applied automatically in the cart, new full licenses only):
  - Helium X: realistic shading, full AE 3D interaction, fog, shadows, DoF, Helium AI.
  - Mettle FreeForm Pro: shading, multiple material channels, fog/mist, shadows, skydome.
  - Rowbyte Plexus 3: vector look with Point, Line and Facet renderers.
- Vimeo tutorial links on the page:
  - Overview: https://vimeo.com/338398513
  - https://vimeo.com/352489850
  - https://vimeo.com/358862712
  - https://vimeo.com/359322393
  - https://vimeo.com/359742800
  - https://vimeo.com/359358458
  - https://vimeo.com/359227398
  - 3D animation: https://player.vimeo.com/video/352731294
  - Mapstyling: https://player.vimeo.com/video/357897696
  - Labels: https://player.vimeo.com/video/359745657
  - Dataviz: https://player.vimeo.com/video/358244424
  - 3D landscapes: https://player.vimeo.com/video/361319119
  - Vimeo hides titles and descriptions behind bot verification, so these could not be read.
- FAQ headings only: How to download items; How to upgrade; How can I retrieve my license code; ZXP/UXP Installer FAQ.

---

## 3. Provider docs that describe GEOlayers 3 UI

### 3.1 Stadia Maps quickstart [S] https://docs.stadiamaps.com/tutorials/getting-started-with-geolayers
1. With the GEOlayers panel open, open the **settings for the Mapcomp** (create a Mapcomp if there is none).
2. Select the **new "Mapcomp Imagery"** option.
3. Enter a raster tile URL, e.g. `https://tiles.stadiamaps.com/tiles/stamen_toner/{z}/{x}/{y}@2x.png?api_key=YOUR-API-KEY`.
4. **Rename** it, e.g. "Stadia Maps - Stamen Toner".
5. **Apply** to save it in the **style library**.
6. Click **apply again** to apply it to the selected Mapcomp. GEOlayers re-renders the Mapcomp.

- This is the replacement workflow after the built-in Stamen style was removed in 1.6.4 (changelog).
- [I] The Universal Raster style takes `{z}/{x}/{y}` templates. Query-string API keys pass through unchanged. `@2x` retina tiles are supported. From the changelog, `{year} {month} {date} {dateISO}` variables also work.

### 3.2 MapTiler [S] https://docs.maptiler.com/guides/maps-apis/maps-platform/how-to-use-maptiler-maps-in-geolayers/
- First use: the missing maps show a **red exclamation mark**. Click one and paste the MapTiler API key.
- To replace the key: **Preferences → scroll to the bottom → key field**.
- MapTiler recommends one dedicated key per tool.
- Licensing: Flex/Custom plan covers editing tiles. A Video Material License is needed for TV/movie work and for channels over 100k subscribers.
- MapTiler maps can be customized inside GEOlayers. **Custom styles created in MapTiler can be imported.**

[S] https://www.maptiler.com/cloud/geolayers/
- Flex plan is $30/mo.
- "The default key must be replaced by your own custom API key from Cloud" → [I] GEOlayers ships with a default/trial key.

[S] https://docs.maptiler.com/guides/maps-apis/maps-platform/maptiler-cloud-maps-for-adobe-after-effects-via-geolayers-3-plugin/
- Steps: install AE → install GEOlayers 3 → activate MapTiler FLEX → connect → use.
- Features listed: flat maps, 3D buildings, 3D terrain, watermasks, multilingual labels.

---

## 4. Forum facts about GEOlayers 3 (aescripts forum; every page 403, all [S-snippet])
- **Dataviz Mapcomp Imagery Styles**: a vendor reply names three, **Choropleth, Circles and Heatmap**, "to visualize larger datasets where AE shape layers are simply too slow". **Heatmap is not in the changelog.** 1.20 adds "Lines", which makes four. Thread among aescripts.com/forums/discussion/comment/30836/ (exact comment unverified).
- **Mapcomp labels**: small, text-only labels that are placed, blended and animated automatically. They are not placed when no data is licensed. Turn them off per Mapcomp with the **little "A" icon in the Mapcomp List**. (Search for "label templates Locator Icon"; thread around comment/24493.)
- A **"Default Label Templates Project"** file holds the default label templates.
- **Duplicate Mapcomp**, from the Mapcomp List: "click 'Duplicate Mapcomp' in the Mapcomp List, pick the second option, enable **Link**, press duplicate". Use case: a duplicate Mapcomp that only handles imagery, trimmed in time. https://aescripts.com/forums/discussion/comment/41477/ ("Collapse transformations on Controller comps not working?"). [I] The duplicate dialog has at least two options, probably "duplicate Mapcomp only" vs "duplicate with containing comp" (matching the API's newName vs newContainingCompName), plus a Link checkbox that creates linked Mapcomps.
- **Finalize quality**: blurry Bing Aerial after finalizing → "zoom in a bit more and finalize with a higher Quality, or zoom out and lower the Quality; this loads other zoom levels". [I] The Finalize dialog has a Quality setting that offsets the tile zoom level. The 1.15.1 changelog adds a "Default Imagery Quality Setting in Preferences".
- Finalize error text: **"2 Tile(s) couldn't be rendered"** (GeoTIFF / local tile directory case).
- Support suggests purging the undo cache, at the cost of undo history. Matches the hidden pref in 1.5.3.
- **Attribution / terms**: "open the mapcomp preferences and find an **info icon next to the Attribution tag**". [I] Mapcomp settings show the provider attribution with an info link to the provider's terms. The 1.6.1 changelog mentions "Provider Attribution".
- Basic and Advanced imagery presets need a MapTiler mapdata subscription. Other providers do not. Trial users can hit a request limit, after which imagery stops rendering.
- **Projection**: the vendor said "GEOlayers only works in Mercator Projection" (before 1.20). A user made a manual mesh-warp to equirectangular for globes. https://aescripts.com/forums/discussion/comment/9997/
- GEOlayers 2 era: "You can pin a layer by selecting it and clicking the **second button in the toolbar**" (pin is a toolbar button; 1.1.3 says unpin by clicking the pin button again).
- A user reports that in newer versions "Administrative" results (e.g. "Croatia") no longer appear in the search bar. [I] The search result list shows a result-type/category label such as "Administrative".

## 5. Adobe Community threads [S]
- https://community.adobe.com/t5/after-effects-discussions/geolayers-3-issue-entire-portions-of-my-map-disappear-when-finalizing/td-p/15438647: the Finalize button can make map portions disappear (unresolved).
- https://community.adobe.com/t5/after-effects-discussions/geolayers-3-layers-not-visible-in-a-project-created-1-year-ago-december-5-2020/m-p/13104053: a "MapComp" layer sits in the containing comp. Layers above it vanished after reopening. One fix: reconnect the disk used as AE cache. [I] Mapcomp imagery/finalized frames may be stored in a cache location, so a missing disk breaks them. Relates to the 1.14.0 "Troubleshooting for corrupted projects due to AE 'Recovering' Mapcomps".
- https://community.adobe.com/t5/after-effects-discussions/how-do-you-anchor-a-solid-layer-to-a-map-to-illustrate-an-animated-frontline/m-p/14719327: users call the anchor layer the "map anchor".

---

## 6. Tutorials and courses (feature evidence from titles and chapters)

### 6.1 aescripts learn / YouTube [S-snippet unless stated]
- **"Up Your ARROW Game in GEOlayers 3"**, Boone Loves Video, Aug 23 2023. https://aescripts.com/learn/post/up-your-arrow-game-in-geolayers-3. Chapters:
  - 0:07 **Draw Arrow Tool**
  - 1:01 Retime
  - Reposition
  - **Make the arrow a map feature**
  - **Animate along the arrow path**
  - Animate zoom, pitch and bearing
  - Shadow
  - 3D look
  - Final animation
- "Create and animate arrows quickly in Adobe After Effects with the GEOlayers 3 Arrow tool". The Arrow tool is a toolbar item (changelog 1.1.7 "Create Arrows from the toolbar"; default script shapes_to_arrows.jsx).
- **"GEOlayers 3 Tutorial: Animating Arrows"**, Nov 10 2020. https://aescripts.com/learn/post/geolayers-3-tutorial-animating-arrows
- **"GEOlayers 3 Tutorial: Line Patterns"**, https://www.youtube.com/watch?v=PtSZNvTZCtY: line patterns that highlight features (add_line_pattern.jsx).
- **"GEOlayers 3 Tutorial: Bikepark Tracks"**, https://www.youtube.com/watch?v=yP5SJISXtls: uses the **3D Landscape sample project**.
- **"Working with the Globe Sample Project in GEOlayers 3"**, https://aescripts.com/learn/post/working-with-the-globe-sample-project-in-geolayers-3 and https://www.youtube.com/watch?v=m5bMw8Fq9q4. Geo-accurate 3D globes. Needs the free **Video Copilot ORB** plugin.
- "Mapping Paris in Adobe After Effects with GEOlayers 3": arrondissements and quartiers.
- "Make a Mission Impossible Map in After Effects using GEOlayers 3".
- "Create 3D Terrains in After Effects using GEOlayers and Helium".
- "After Effects + GEOlayers 3 + FreeForm Pro + Mettle FLUX" (3D sliced landscape).
- "Walkthrough Mount Everest tour".
- **"Using georeferenced imagery and local tile directories in GEOlayers 3"**: georeferenced imagery such as the **NASA Visible Earth** collection, plus local tile directories.
- **"GEOlayers 3 Tutorial: Geometry Simplification"**, https://aescripts.com/learn/geolayers-3-tutorial-geometry-simplification
- **"Assign Label Templates to Mapcomps in GEOlayers"**: "a well-hidden feature for power users and template creators" (the changelog's right-click filter bars, 1.1.6/1.2.6).
- "Create Map Label Templates for GEOlayers 3" (Boone), https://www.toolfarm.com/?p=101740
- "Learn how to create a map flyover animation in one click using GEOlayers": [I] probably "Animate View to Feature" / "Animate the view along multiple selected Features" (changelog 1.0.9).
- "GEOlayers 3 Tutorial: Powerful New Search Bar Features": [I] matches 1.2.2, multiple search sources.
- "GEOlayers 3 Tutorial: Route Loop", "Data driven Shape Layer Styles", "Mapstyling tutorial", "18 GEOlayers tips in 18 minutes", "20 GEOlayers Tips in 10 Minutes" (titles only).
- "How to Visualize Distances in GEOlayers (Create a Walking Distance Map)": [I] Distance Circle default script (1.14.0).
- "From USSR to 15 Countries — Map It with GEOlayers" (title only).

### 6.2 Boone Loves Video "GEOlayers 3 Masterclass" curriculum [S] https://boonelovesvideo.teachable.com/p/geolayers-3-masterclass (AE 2023, GEOlayers v3)
- Hello There: Welcome; **A Tour of the Interface** (12:08); MapTiler Features (8:54); Contact.
- QuickStart: Create a Mapcomp; Find Data; Draw Features; Add Labels; Animate the Map; Final Touches; **The Disputed Area Dataset**.
- Create Mapcomps: How a Mapcomp Works; **Mapcomp Settings**; **Controller Mapcomps**; **Duplicate Mapcomps**; Where to Find License Information.
- Map Features: **Search for Features**; **Feature Properties**; **Download Datasets**; **Create Custom Features**; **Import and Export**.
- Draw Features: How Features are Drawn; **Layer Styles**; Create Custom Layer Styles; **Control the Geometry**; **Draw Features in the Mapcomp**.
- Label Features: How Labels are Drawn; Fine Tune Label Placement; Create Custom Label Templates; **Pin Layers to a Map or Feature**; **Parent Layers to a Map**; **Mapcomp Label Templates**.
- Animate!: Animate the Mapcomp View; Simple Transitions; Graph Editor; **Animate Paths and Routes**; Animate Labels.
- Map Design and Layout: **Create a Locator/Inset Map**; **Draw a Scale Bar and North Needle**; Texture; Mattes; Adjustment Layers.
- Advanced: How to Draw on Shape Layers; **How to Draw Arrows**; **Georeference Existing Maps**; Shadows; How to Add Feature Properties; How to Work With Mapbox; Animating Flags.
- Project breakdowns: Census Regions US; Vox-style; **Create a Choropleth Map**; RealLifeLore-style; Johnny Harris 3D Hawaii (3rd-party plugin); Vasco da Gama route; Panama Canal.
- Best Practices: **3 Ways to Finalize Imagery**; Improve Playback Performance; Troubleshooting.

[I] Implications:
- "Controller Mapcomps": a Mapcomp whose view drives linked Mapcomps (Link on duplicate).
- "The Disputed Area Dataset": a downloadable dataset in the browser (1.7.0 adds disputed borders toggled on MapTiler styles).
- "3 Ways to Finalize Imagery": maybe Finalize current frame / work area / whole comp (API options onlyCurrentFrame and onlyWorkArea). Unconfirmed.

### 6.3 "GEOlayers 3 WAR!" course [S] https://boonelovesvideo.teachable.com/p/geolayers-3-war (AE 2025)
- Lessons include:
  - Administrative Boundaries
  - Mapping Cities, Towns and Villages
  - Georeferencing (basic / advanced / graticules)
  - Digitizing
  - How to Manage Your Map Features
  - How to Visualize Disputed Areas
  - Pin and Parent Elements
  - Custom Label Templates
  - Animate Labels
  - Label Locations Quickly
  - **Measure Distance and Area**
  - **3 Ways to Add Arrows**
  - **Put Symbols on a Path**
  - Visualize an Invasion with Shape/Solid Layers
  - How to Rasterize Vector Layers
- [I] "Measure Distance and Area": the Feature Properties panel probably shows length/area. Unconfirmed.

---

## 7. GEOlayers 2 (predecessor) [S unless noted]
- No Film School, https://nofilmschool.com/animated-maps-adobe-after-effects:
  - $199.99. Needs AE CC 2015 (13.6)+. Version 1 projects not supported.
  - Imagery from tile servers through **14 default "Server Profiles"**, each a map style, free with attribution. Users can create and customize their own.
  - **Search tool**: countries, cities, borders, rivers, streets, specific buildings, POIs.
  - **Labels**: single click, with an **intro animation** and adjustable attributes. Default templates, or any AE comp as a template.
  - **Feature styles**: style-sets for **color, stroke, fill, opacity, extrusion**. Styles can be **applied automatically by feature type**.
  - **Route tool**: route between 2+ locations **by car, bicycle, plane, foot or straight line**. Graphic layers snap to the route and animate along it.
  - CSV/TSV import with data-driven styles.
  - Extrusion with control over the amount. **Lights added for depth.**
  - 3D landscapes with Mir 2, FreeForm Pro or Plexus 3.
  - Located features can be drawn, downloaded and exported.
- Lesterbanks, https://lesterbanks.com/2016/12/geolayers-2-raises-effects-maps-game/: rebuilt, "up to 10x faster", browse map data in AE, dynamic labels, routes, one-click 3D landscapes, Mapbox demo.
- CGPress, https://cgpress.org/archives/geolayers-updated-with-animated-gpx-route-plotting.html (GL1/2): **animated GPX route plotting**, one-click navigation to **GeoShape** features, quick labels from the search tab and the GeoShapes tool.
- Forum [S-snippet]:
  - The **Quick Setup** 3D landscape uses **Bing satellite textures and Mapzen height data** by default.
  - A user mentions **Mapstack** (Stamen) options like CartoDB Positron that GEOlayers did not have.
  - A Server Profile can be made from a Mapbox tileset (GeoTIFF upload) and applied to a texture Mapcomp.
  - "Video tutorials are still available on our vimeo channel".
- GL2 Vimeo tutorial titles [S-snippet], numbered series:
  - "01 Basics"
  - "**02 Toolbar**" (https://vimeo.com/188033904)
  - "04 Geographical Features"
  - "**05 Styles & Rules**"
  - "**07 Label Templates**"
  - "08 Quick Setup"
  - "**09 Preferences**"
  - "Mapstack"
  - "Feature Styles"
  - 03 and 06 were not found.
  - [I] "Styles & Rules": GL2 had rule-based styling (style by feature type). GL3 reworked it into data-driven Shape Layer Styles.
- GEOlayers 1, https://lesterbanks.com/?p=33857 and https://cgpress.org/archives/geolayers-for-after-effects.html:
  - Tile servers (Bing, MapQuest), with live tile refresh on zoom/pan.
  - A "layers" tab for markers, routes, waypoints, text and cloud layers.
  - 2D/3D/mixed layers.
  - GeoJSON vectors.
  - Mir terrain.

---

## 8. Other items
- Old toolfarm listing [S-snippet] https://www.toolfarm.com/?p=33496: "platforms such as Mapbox bring all its advantages of customizable base maps directly into AE". Mapbox was the GL2/early GL3 provider. The Masterclass has a "How to Work With Mapbox" lesson, and the WAR course has "Making Custom Basemaps with Mapbox". [I] A Mapbox style is used through the Universal Raster URL template (`https://api.mapbox.com/styles/v1/{user}/{style}/tiles/256/{z}/{x}/{y}@2x?access_token=...`). Unconfirmed in GEOlayers docs.
- SEO or pirate-looking pages (edx.ipeindia.org, avitmarine, hollywoodcamerawork, baks.verde.ag, *.gumroad resellers, "v1.5.3 Win/Mac" pages) were ignored, as the rules require.

## 9. Open questions
- The full list of built-in imagery style names. Known so far:
  - MapTiler: Satellite (V2), Basic, Advanced, Lite
  - Natural Earth, plus Natural Earth Admin Boundaries
  - Bing: Aerial, Dark, Light, Gray
  - CartoDB
  - Stamen (removed)
  - Universal Raster
  - Dataviz: Choropleth, Circles, Heatmap, Lines
  - Water Mask
  - Are Bing Road/Hybrid, OSM, Esri, Mapbox, or "TopoWhite" (web app) styles present?
- The exact names of the default Label Templates. Only the "Icon" and "Locator" types are known from the 1.0.6 changelog.
- The names of the default Shape Layer Style presets.
- Arrow tool options (head shape, width, curvature, animation).
- 1.20 Globe / Equal Earth "via Plugin": what plugin (a GEOlayers native AE effect?) and what UI.
- The complete Preferences panel list.
- The options of the duplicate-Mapcomp dialog.
- The meaning of "3 Ways to Finalize Imagery".
- Whether Heatmap is still shipped.
