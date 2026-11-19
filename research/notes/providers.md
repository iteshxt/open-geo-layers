# GEOlayers 3: data providers and external services (raw research notes)

Captured 2026-11-18. Clean-room research only: public docs, changelog, vendor's public example scripts, provider API docs.
Nothing from the GEOlayers product binary/ZXP or the scriptingApiExamples.zip was used.

Legend: **[S]** = SOURCED (URL or local file given). **[I]** = INFERRED (my reasoning, unverified). **[?]** = unknown.

Local sources:
- `research/sources/help-readme.md` (HELP)
- `research/sources/changelog.md` (CL)
- `research/sources/geodata-repo__*.txt`, `development-repo__*.txt` (EX)

Blocked during research (HTTP 403 to WebFetch): aescripts.com product page, aescripts forum threads, aescripts learn/blog posts, toolfarm.com, docs.stadiamaps.com/faq. A direct curl to aescripts was refused by the sandbox, so I did not retry. Facts from those pages below come only from search-engine snippets and are marked accordingly.

---

## 0. Provider map: what GEOlayers uses each service for

| Provider | GEOlayers feature it powers | Status in GEOlayers | Evidence |
|---|---|---|---|
| MapTiler Cloud (partner) | "Stylized Mapcomp Imagery (the imagery styles with the MapTiler icon)", "Find Features by clicking the preview map", "Mapcomp labels in different languages", "Watermask Mapcomps from the toolbar", "Feature import for the current view to the browser", "Drawing 3D buildings from the toolbar"; MapTiler Search (geocoding, worldviews); MapTiler Satellite (V2); 3D terrain | Integrated in 1.1.9 (Nov 27 2020) | [S] HELP "MapTiler Data"; CL 1.1.9, 1.5.1, 1.7.0, 1.13.1, 1.18.0; MapTiler guide |
| OSM Nominatim | `geolayers3.geocode()` and the search bar ("OSM search") | Since 1.0.x | [S] HELP geocode(); CL 1.0.6, 1.9.0 |
| Overpass API | `addToBrowser({type:"overpass"})`, `draw()`, default scripts; auto server choice | Since 1.0.x; auto-selection 1.5.6 | [S] HELP; EX; CL 1.5.6 |
| Natural Earth | NE Imagery, NE Water Mask, NE Mapcomp Labels, NE Admin Boundaries Imagery, NE Search API, feature downloads (Land/Water, States/Provinces lines, Lakes/Rivers), "Inner Boundaries" | 1.1.9, 1.2.0, 1.2.2, 1.6.6 data update, 1.7.2 | [S] CL |
| Bing Maps | Bing imagery (Aerial plus Dark, Light, Gray) with language support | 1.1.4 | [S] CL 1.1.4; forum snippet "Bing Aerial" |
| CARTO (CartoDB) | CartoDB imagery style; API key field | key field since 1.19.0 (Sep 4 2026) | [S] CL 1.19.0; forum snippet |
| Stamen | Stamen imagery style | Removed 1.6.4 (Oct 9 2023), migrate to Stadia via a custom URL | [S] CL 1.6.4; Stadia GEOlayers tutorial |
| Any XYZ/TMS raster tile server | "Universal Raster Style" / custom Mapcomp Imagery; date variables; TMS | 1.0.9 TMS; 1.4.0 date vars and Map Bounds; 1.6.6 "Default Tiles in Universal Raster Style" | [S] CL; Stadia tutorial |
| Local tile directory | "Pick local tile directories in Imagery setup"; min/max zoom from `metadata.json` | 1.1.8, 1.5.2 | [S] CL |
| Routing (car/bike/pedestrian) | Routes with optional Maneuver Points | 1.0.4 bugfix, 1.4.0 pref | [S] CL; provider [?] |
| DEM / elevation tiles | 3D Landscapes (Mir, Plexus, FreeForm Pro, Helium), Sample Project Elevation | 1.1.2 sample, 1.2.5 DEM bugfix, 1.5.0 Helium | [S] CL; provider [I] |
| GitHub (public repos) | `githubBrowser()`, GitHub Search API | 1.2.0, 1.2.2 | [S] HELP, CL, EX |
| ArcGIS open data (example only) | COVID sample scripts via `fetchJson` | example scripts | [S] EX |

---

## 1. MapTiler Cloud

### 1.1 What GEOlayers says [S]
- HELP "MapTiler Data": "Some functions of GEOlayers are only possible because we partnered with MapTiler as a data provider." Six features are listed (see table above). "After the free trial to check it out you'll need a Flex plan on MapTiler Cloud to enable these features. You can of course use GEOlayers without it."
- CL 1.1.9: "MapTiler Cloud integration".
- CL 1.4.0: "MapTiler Lite Imagery free for non-commercial projects". **Lite** is a GEOlayers imagery style name.
- CL 1.7.0: "Disputed Borders can be toggled on/off on MapTiler Imagery Styles Basic, Advanced and Lite". So the MapTiler-backed GEOlayers styles are **Basic**, **Advanced** (CL 1.0.4 "New Mapcomp Imagery Style 'Advanced'") and **Lite**.
- CL 1.1.8: "Water visibility separate for oceans, lakes and rivers in Basic and Advanced Imagery". CL 1.1.6: "Advanced Imagery Style extra color for rail". CL 1.2.6 bugfix: "Advanced Style shows black polygons if street density set to 5". So Advanced has a **street density** control with values up to at least 5.
- CL 1.5.1: "MapTiler Satellite Imagery V2". CL 1.2.5: "Bugfix: MapTiler Satellite imagery blurry".
- CL 1.5.3, 1.5.5, 1.13.1, 1.18.0, 1.18.1: MapTiler Search. "loads full geometry from their internal dataset to ensure basemap consistency" (1.13.1). "supports worldviews (set in Preferences)" (1.18.0). "Bugfix: MapTiler Search downloads wrong countries if a specific worldview is set" (1.18.1).
- CL 1.16.0: "Improved Auth Error handling". CL 1.8.0: "User authentication error during startup".
- MapTiler guide https://docs.maptiler.com/guides/maps-apis/maps-platform/how-to-use-maptiler-maps-in-geolayers/
  - Licensing: "Your MapTiler Flex or Custom plan covers" the digital tiles used during editing. A separate Video Material License Agreement is needed for TV/movie productions and for YouTube/Vimeo channels with more than 100,000 subscribers.
  - Activation: create a dedicated new API key for GEOlayers. On first use, "click the missing maps, marked with a red exclamation mark" and paste the key.
  - Replace the key: "Open the preferences menu in GEOlayers, scroll all the way down" and enter it in the field.
  - "Import custom styles generated in MapTiler".
- https://www.maptiler.com/cloud/geolayers: Flex $30/month includes a video material license for individual creatives up to 100,000 channel subscribers. Features listed: satellite imagery, customizable basemaps, 3D buildings, watermasks, labels in different languages, Mapcomp Imagery Styles, custom maps. Setup says "The default key must be replaced" (so GEOlayers ships a default/trial key). Attribution is required unless a custom license exempts it.
- https://docs.maptiler.com/guides/maps-apis/maps-platform/maptiler-cloud-maps-for-adobe-after-effects-via-geolayers-3-plugin/: features "Flat maps, plus 3D buildings and 3D terrain", watermasks, multi-language labels.
- aescripts forum snippets (search engine only, undated): GEOlayers shipped with "6 months" of a **GEOlayers Mapdata** subscription. After that "styleable maps will not work". Mapdata also gave "a data license for your commercial projects". CL 1.0.4: "Click preview map to find features (GEOlayers Mapdata subscription)". CL 1.0.7: "En/disable GEOlayers Mapdata (green icon in prefs)". [I] Mapdata was the pre-MapTiler (2019–2020) subscription; MapTiler Flex replaced it in 1.1.9.

### 1.2 MapTiler API endpoints [S]
Base `https://api.maptiler.com` (an EU endpoint also exists). Auth is `?key=KEY` on every request.

- **Maps (styles)** https://docs.maptiler.com/cloud/api/maps/
  - `GET /maps/{mapId}/style.json` (marked deprecated on that page, still widely used)
  - Raster: `GET /maps/{mapId}/{tileSize}/{z}/{x}/{y}{scale}.{format}`, tileSize 256 (or omitted = 512), scale `@2x`, format png/jpg/webp
  - `/maps/{mapId}/{tileSize}/tiles.json`, `/maps/{mapId}/WMTSCapabilities.xml`, OGC `/tiles`
  - Example `https://api.maptiler.com/maps/streets-v4/256/1/1/1@2x.png?key=KEY`
  - Current map ids (2026, maptiler.com/maps): `streets-v4`, `base-v4`, `outdoor-v4`, `hybrid-v4`. Older ids `streets-v2`, `basic-v2`, `bright-v2`, `satellite`, `hybrid`, `dataviz`, `backdrop`, `toner-v2`, `winter-v2`, `topo-v2`, `ocean`, `landscape`, `aquarelle` [I, from memory; not re-verified].
- **Tiles (tilesets)** https://docs.maptiler.com/cloud/api/tiles/
  - `GET /tiles/{tilesId}/{z}/{x}/{y}`, `/tiles/{tilesId}/tiles.json`, WMTS, OGC
  - 204 = empty tile, 400 = out of bounds, 403 = key missing/invalid/restricted. [I] The 403 is what GEOlayers' "Auth Error handling" deals with.
  - Formats: pbf vector, png/jpg/webp raster, quantized-mesh terrain.
- **Satellite**: `satellite-v2`, min z0, max z22, JPG. A blend of satellite (2021–2022) and aerial imagery (up to 2026). https://docs.maptiler.com/schema-raster/satellite/ [S via search snippet]. A newer `satellite-v4` appears in the API examples.
  - Template: `https://api.maptiler.com/tiles/satellite-v2/{z}/{x}/{y}.jpg?key=KEY` [I; extension per schema]
- **Terrain-RGB**: `terrain-rgb-v2`, z0–14, WebP. 30 m global, 5 m in some areas, vertical resolution 6 m. https://docs.maptiler.com/schema-raster/terrain-rgb/ [S]
  - Template `https://api.maptiler.com/tiles/terrain-rgb-v2/{z}/{x}/{y}.webp?key=KEY` [I]
  - Decoding (Mapbox convention): `h = -10000 + ((R*65536 + G*256 + B) * 0.1)` metres [I; MapTiler's pages do not print the formula, but it is the format MapLibre's "mapbox" DEM encoding expects]
  - Also `terrain-quantized-mesh-v2` (Cesium), `ocean-rgb` (bathymetry) [S name via search; details I]
- **Vector planet (OpenMapTiles schema)**: "MapTiler Planet v3", `https://api.maptiler.com/tiles/v3/{z}/{x}/{y}.pbf?key=KEY` https://docs.maptiler.com/schema/planet/ [S]
  - Layers: aerodrome_label, aeroway, boundary, building, globallandcover, housenumber, landcover, landuse, mountain_peak, park, place, poi, transportation, transportation_name, water, water_name, waterway
  - `building`: `render_height`, `render_min_height`, `colour`, `hide_3d`. **[I] This is the most plausible source of "Drawing 3D buildings from the toolbar" (CL 1.0.5 "3D building extrusion independent from zoom when drawn from toolbar").**
  - `boundary`: `disputed`, `disputed_name`, `claimed_by`, `adm0_l`, `adm0_r`. [I] This is how the "Disputed Borders on/off" toggle (CL 1.7.0) would work.
  - Names: `name`, `name:xx` (82 values incl. script variants), `name_latin`, `name_int`. [I] This powers "Mapcomp labels in different languages".
  - [I] `water` with class ocean/lake/river powers "Water visibility separate for oceans, lakes and rivers". `transportation` with class rail powers "extra color for rail". "Current View Transport Features have separate ramps" (CL 1.3.0) likely means `transportation` features imported for the current view.
- **Countries** https://docs.maptiler.com/schema/countries/ [S]
  - Layer `administrative`: fields gid, level (0 = country … 4 = minor civil division), iso_a2, code, name, name:{code}, wikidata, area, continent, level_0..level_3. z0–11 (overzoom to 22).
  - Layer `postal` (z6–11).
  - [I] Powers "Adding Admin Bounds for higher zooms to the browser" (CL 1.17.0) and "MapTiler Search loads full geometry from their internal dataset" (1.13.1).
- **Geocoding** https://docs.maptiler.com/cloud/api/geocoding/ [S]
  - Forward `GET /geocoding/{query}.json`, reverse `GET /geocoding/{lon},{lat}.json`, by id `GET /geocoding/{id}.json` (id like `country.26561650`, returns full geometry), batch up to 50 queries separated by `;`
  - Params: `bbox`, `proximity` (lon,lat or `ip`), `language` (≤20 ISO 639-1), `country`, `limit` (1–10; default 5 forward, 1 reverse), `types`, `excludeTypes`, `fuzzyMatch` (default true), `autocomplete` (default true), `worldview` (default `default`)
  - Worldviews: `default`, `auto`, `ae`, `ch`, `jp`, `kr`, `us`. With `default`, disputed areas have no country info and countries with disputed borders return without full geometry. **[I] This explains GEOlayers bugfix 1.18.1 (wrong countries with a worldview set) and 1.5.3/1.13.1 ("only finds Point Features"): the search returns points, so GEOlayers then fetches full geometry by id or from the countries dataset.**
  - Response: GeoJSON FeatureCollection; features have id, text, place_name, place_type, relevance, center, bbox, geometry, context, properties.
- **Languages**: MapTiler SDK special modes AUTO, LATIN, NON_LATIN, LOCAL, STYLE, STYLE_LOCK, VISITOR, VISITOR_ENGLISH https://docs.maptiler.com/sdk-js/api/languages/ [S]. GEOlayers language work from CL: Welsh (1.6.1), Irish and Scottish Gaelic (1.5.0), Traditional Chinese fix (1.12.2), RTL fixes (1.12.2, 1.13.1), NE language support (1.1.3), "Preview map displayed in default language set in prefs" (1.1.8).

### 1.3 MapTiler pricing/limits (2026) [S] https://www.maptiler.com/cloud/pricing/
- Free $0: 5k sessions/month, 1k search sessions, 2k 3D sessions, 100k API requests. For "testing, PoC, prototyping, personal, or non-commercial use". MapTiler logo on the map. Pauses when exhausted. No card needed. 1 API key.
- Flex $30/month: 25k sessions, 3k search sessions, 10k 3D sessions, 500k API requests, 1,000 keys. Overage $0.15 per 1k API requests.
- Hobby build: the **Free plan is usable** for personal non-commercial work [S]. Raster/vector tile fetches count as API requests [I]. Keep the logo/attribution "© MapTiler © OpenStreetMap contributors" [I standard text].

---

## 2. OpenStreetMap Nominatim

### 2.1 GEOlayers usage [S]
- HELP `geolayers3.geocode(searchString, callback[, options])`: "This function uses OpenStreetMap Nominatim search engine. You shouldn't bulk geocode placenames. The geolayers scripting api will queue multiple requests at the same time according to the Nominatim usage policy." The `language` option is ISO 639 and defaults to "the selected Mapcomp's setting". The callback gets "an array of geojson features".
- HELP `addToBrowser({type:"geocode", query})`.
- CL 1.0.6: "Nominatim responses without geometry fall back to point".
- CL 1.0.2: "Additional online search service used as fallback if default search fails; can be set as default in prefs". [?] The fallback service is not named. [I] Candidates are Photon (komoot) or a second Nominatim instance. After 1.1.9 MapTiler Search is the obvious alternative.
- CL 1.9.0: "OSM search results do not appear for some users". [I] "OSM search" is the Nominatim-backed search in the UI.
- CL 1.2.3: "Adding a Current View Feature fails if view can't be geocoded". [I] Reverse geocoding of the view centre is used to name current-view features.

### 2.2 API [S] https://nominatim.org/release-docs/latest/api/Search/
- `https://nominatim.openstreetmap.org/search?q=...&format=geojson&polygon_geojson=1&accept-language=xx&limit=N`
- Formats: xml, json, jsonv2 (default), geojson, geocodejson. `limit` default 10, max 40.
- `polygon_geojson=1` (only one polygon_* at a time); `polygon_threshold` simplification in degrees.
- `accept-language`, `countrycodes`, `layer` (address, poi, railway, natural, manmade), `featureType` (country, state, city, settlement), `viewbox` + `bounded`, `addressdetails`, `extratags`, `namedetails`, `dedupe`, `email`.
- Other endpoints: `/reverse`, `/lookup?osm_ids=R123,W456,N789` [I standard], `/details`.

### 2.3 Policy [S] https://operations.osmfoundation.org/policies/nominatim/
- At most 1 request/second for the whole application (all users combined).
- A valid identifying User-Agent or Referer is required; default library UAs are not acceptable.
- Cache results. No bulk geocoding (single thread; ≤4 req/min for long or periodic jobs). **Client-side autocomplete is forbidden.** No systematic grids or POI dumps.
- Attribution: ODbL, "© OpenStreetMap contributors".
- [I] Our build: a serial queue of 1 req/s plus a cache, matching what GEOlayers' help says. No search-as-you-type against Nominatim; search on Enter only.

---

## 3. Overpass API

### 3.1 GEOlayers usage [S]
- HELP: `addToBrowser`/`draw` with `{type:"overpass", query:"Overpass query"}`.
- EX scripts (vendor's public repos) use **Overpass Turbo shortcuts inside queries**: `{{bbox}}` (= current Mapcomp/preview view) and `{{geocodeArea:Country}}`. Examples: Buildings_OSM, Charging_Stations_OSM, Hospitals_OSM, Railroads_OSM, Search_Amenity_in_bbox, Autobahn_* . All use `[out:json][timeout:N];(...);out body;>;out skel qt;`. **So GEOlayers expands the `{{bbox}}` and `{{geocodeArea:...}}` macros itself before sending.** [S that they are used; I that GEOlayers implements the expansion, because the raw Overpass API does not understand them.]
- CL 1.5.6: "Overpass service chosen automatically by response time and number of available slots".
- CL 1.0.6: "large OSM downloads freeze panel" (fixed).
- CL 1.17.0: "Getting Features by OSM Id is more stable". HELP `{type:"osm", id:"node1234|way4321|rel1243"}`. [I] Implemented as an Overpass query `rel(1243);out body;>;out skel qt;` or via the OSM API `https://api.openstreetmap.org/api/0.6/relation/1243/full`, then osmtogeojson-style conversion.
- Default scripts/browser feature downloads like "Buildings", "Railroads" use Overpass [S EX].

### 3.2 Endpoints [S] https://wiki.openstreetmap.org/wiki/Overpass_API
- `https://overpass-api.de/api/interpreter` (main, FOSSGIS): about 10,000 queries/day and 1 GB/day; ≤10 min; send a UA/Referer; back off 30 s after 429/406.
- `https://maps.mail.ru/osm/tools/overpass/api/interpreter` (VK Maps)
- `https://overpass.private.coffee/api/interpreter` (formerly overpass.kumi.systems; no rate limit)
- `https://overpass.osm.ch/api/interpreter` (Switzerland only)
- `https://ovp.dresel.systems/api/interpreter` (Europe) and `/api-na/interpreter` (North America); 4 parallel per IP
- Paid/keyed: Geofabrik, Tracestrack, Overspan, NextGIS, Mapsource, Overpass.ch (10k/month free)
- Mirrors `z.overpass-api.de`, `lz4.overpass-api.de` [I, historically used; the wiki now lists them only as monitoring links]
- **Slot status** [S] https://dev.overpass-api.de/overpass-doc/en/preface/commons.html: `GET /api/status` returns plain text. Line 1 `Connected as: <id>`, line 3 `Rate limit: <slots>`, then "N slots available now." or "Slot available after: <ISO time>, in N seconds." Requests wait ≤15 s for a slot, otherwise 429. Default `[timeout:180]`, `[maxsize:536870912]` (512 MiB). Resource refusals return 504.
- **[I] Mirror selection algorithm to replicate CL 1.5.6:** probe each mirror's `/api/status` in parallel. Score each by round-trip time and free slots (skip any with 0 slots). Send to the best one and fall back to the next on 429/504/timeouts.

### 3.3 Overpass Turbo shortcut semantics [S] https://wiki.openstreetmap.org/wiki/Overpass_turbo/Extended_Overpass_Turbo_Queries
- `{{bbox}}` → `south,west,north,east` of the current view
- `{{center}}` → `lat,lon`
- `{{geocodeArea:name}}` → `area(id:NNN)` of the first Nominatim result. Area id = 3600000000 + relation id, or 2400000000 + way id [I; standard Overpass convention]
- `{{geocodeId:name}}`, `{{geocodeBbox:name}}`, `{{geocodeCoords:name}}`, `{{date:N unit}}`, custom `{{x=value}}`
- [?] Which of these GEOlayers supports beyond bbox and geocodeArea is unknown. Implement all.

---

## 4. Natural Earth

### 4.1 GEOlayers usage [S CL]
- 1.1.9: "Natural Earth Imagery; Natural Earth Water Mask; Natural Earth Mapcomp Labels; Natural Earth Physical Areas Land & Water in Feature download"
- 1.2.0: "Natural Earth Admin Boundaries Imagery"; "Natural Earth States and Provinces Lines dataset in Feature download"; "Inner Boundaries Feature for each country in 'Find Features at Coordinate'"; "Improved Natural Earth data search by name"
- 1.2.2: "Natural Earth Search API" (a scripting/search source)
- 1.1.3: "Language support for Natural Earth data"
- 1.0.6: "Global dataset Lakes / Rivers downloadable to the browser" [I: NE lakes + rivers_lake_centerlines]
- 1.5.4: "Bugfix: Natural Earth States and Provinces display only Line Features"
- 1.6.6: "Natural Earth Data update" (Nov 2023; [I] to NE v5.1.x)
- 1.7.2: "Natural Earth Inner Boundaries for some countries are messed up"
- [I] Natural Earth is the no-subscription fallback for world-scale imagery (rendered vector, not raster), labels and water masks, so GEOlayers works without MapTiler.

### 4.2 Data [S]
- Public domain. Optional credit: "Made with Natural Earth." https://www.naturalearthdata.com/about/terms-of-use/
- Scales 1:10m, 1:50m, 1:110m; folders `10m_cultural`, `10m_physical`, … and a `geojson/` folder in https://github.com/nvkelso/natural-earth-vector
- Countries v5.1.1, shows de-facto boundaries by default, with optional point-of-view (POV) variants for dozens of countries. Download e.g. `https://www.naturalearthdata.com/http//www.naturalearthdata.com/download/10m/cultural/ne_10m_admin_0_countries.zip` [S]. CDN mirror `https://naciscdn.org/naturalearth/10m/cultural/ne_10m_admin_0_countries.zip` [I]. GeoJSON at `https://raw.githubusercontent.com/nvkelso/natural-earth-vector/master/geojson/ne_10m_admin_0_countries.geojson` [I].
- Datasets matching GEOlayers features [I]: `ne_10m_admin_0_countries`, `ne_10m_admin_0_boundary_lines_land`, `ne_10m_admin_1_states_provinces_lines`, `ne_10m_admin_1_states_provinces`, `ne_10m_populated_places` (labels), `ne_10m_land`, `ne_10m_ocean`, `ne_10m_lakes`, `ne_10m_rivers_lake_centerlines`, `ne_10m_geography_regions_*`. The `name_xx` columns (name_en, name_de, name_fr, name_zh, name_zht, …) give multilingual labels; Wikidata-derived names are CC0.

---

## 5. Bing Maps

### 5.1 GEOlayers usage [S]
- CL 1.1.4: "Bing Imagery Dark, Light, Gray; Bing imagery language support".
- Forum snippets (search engine): "consider using bing if you want satellite imagery"; "some non-advanced Imagery such as 'CartoDB' or 'Bing Aerial', they are not part of Maptile's data" (so Bing/CartoDB are not metered by MapTiler).
- cgpress article (GEOlayers 1/2 era): "connects to HTTP tile servers… Bing Maps and MapQuest" https://cgpress.org/archives/geolayers-for-after-effects.html
- [I] The GEOlayers Bing styles are therefore Bing Aerial (maybe also Aerial with Labels / Road) plus Dark, Light, Gray, which map 1:1 to imagerySets CanvasDark, CanvasLight, CanvasGray. Language = `culture`/`mkt`.

### 5.2 API [S] https://learn.microsoft.com/en-us/bingmaps/rest-services/imagery/get-imagery-metadata
- `https://dev.virtualearth.net/REST/v1/Imagery/Metadata/{imagerySet}?key=KEY&uriScheme=https[&c={culture}][&include=ImageryProviders]`
- imagerySet: Aerial, AerialWithLabels (deprecated), AerialWithLabelsOnDemand, Birdseye*, CanvasDark, CanvasLight, CanvasGray, OrdnanceSurvey, Road (deprecated), RoadOnDemand, Streetside. `mapLayer`: Basemap, Background, Foreground, TrafficFlow, Buildings.
- Response `imageUrl` e.g. `http://ecn.{subdomain}.tiles.virtualearth.net/tiles/r{quadkey}.jpeg?g=129&mkt={culture}&shading=hill&stl=H`, `imageUrlSubdomains` [t0..t3], 256 px, zoomMin/zoomMax (1–21).
- **{quadkey}** addressing: Bing uses quadkeys, not z/x/y. [I] The `{q}` placeholder named in the task would be GEOlayers' generic quadkey placeholder for raster URLs.
- Quadkey algorithm [I, standard]: for i = z..1, digit = ((x>>(i-1))&1) + 2*((y>>(i-1))&1).
- **Status**: "Get Imagery Metadata API is deprecated and has been retired for all free (Basic) account customers. Enterprise … until June 30th, 2028" [S]. Basic accounts ended June 30 2025 [S via greymatter.com/m365 snippets]. Replacement: Azure Maps Get Map Tile `https://atlas.microsoft.com/map/tile?api-version=2022-08-01&tilesetId=microsoft.imagery|microsoft.base.road|microsoft.base.darkgrey…&zoom=&x=&y=&tileSize=256|512&language=xx&subscription-key=KEY`. No aerial+labels tileset. 1 billable transaction per 15 tiles. [S] https://learn.microsoft.com/en-us/azure/azure-maps/migrate-get-imagery-metadata
- Terms: the copyright line reads "This API cannot be accessed and the content … may not be used… without express written permission from Microsoft". [S in API response sample]
- **Hobby verdict [I]:** Bing is effectively unavailable to new hobby users (no Basic keys). Keep a "Bing" provider adapter that accepts a user key (Enterprise) and also an Azure Maps adapter.

---

## 6. CARTO (CartoDB) basemaps

### 6.1 GEOlayers [S] CL 1.19.0 (Sep 4 2026): "CartoDB Imagery supports entering an API key". This lines up with CARTO starting to require keys for raster tiles on 25 Sep 2026.
### 6.2 API [S] https://github.com/cartodb/basemap-styles, https://docs.carto.com/faqs/carto-basemaps
- Raster: `https://{s}.basemaps.cartocdn.com/{style}/{z}/{x}/{y}{scale}.png?key=KEY`, s = a|b|c|d, z 0–20, scale `@2x` optional
- Styles: `light_all`, `dark_all`, `light_nolabels`, `light_only_labels`, `dark_nolabels`, `dark_only_labels`, `rastertiles/voyager`, `rastertiles/voyager_nolabels`, `rastertiles/voyager_only_labels`, `rastertiles/voyager_labels_under`. Marketing names: Positron = light, Dark Matter = dark, Voyager.
- Vector: `https://basemaps.cartocdn.com/gl/{positron|dark-matter|voyager}[-nolabels]-gl-style/style.json?key=KEY`
- Without a key, raster tiles carry an "API KEY REQUIRED" watermark. Keys are free at carto.com/basemaps/apikey (no account needed).
- Free: 5M tile requests/month non-commercial, 1M commercial. Requires visible "© OpenStreetMap contributors © CARTO" attribution.
- Note: the docs.carto.com page contained text addressed to automated readers; it was treated as data and ignored.

---

## 7. Stamen → Stadia Maps

- [S] CL 1.6.4 (Oct 9 2023): "Stamen Imagery Style removed (Stamen tile hosting moved to Stadia Maps). Migration video: https://vimeo.com/872558334"
- [S] Stadia tutorial https://docs.stadiamaps.com/tutorials/getting-started-with-geolayers/. GEOlayers flow: open Mapcomp settings → **Mapcomp Imagery** → enter the raster tile URL → append `?api_key=YOUR-API-KEY` → rename the source (e.g. "Stadia Maps - Stamen Toner") → apply (saves it to the style library) → apply again to switch the Mapcomp. Example `https://tiles.stadiamaps.com/tiles/stamen_toner/{z}/{x}/{y}@2x.png?api_key=KEY`
- [S] Watercolor `https://tiles.stadiamaps.com/tiles/stamen_watercolor/{z}/{x}/{y}.jpg`; Toner Lite `.../stamen_toner_lite/{z}/{x}/{y}@2x.png`; EU host `tiles-eu.stadiamaps.com`. Domain allow-listing works without a key.
- Other ids [S/I]: alidade_smooth, alidade_smooth_dark, alidade_bright, alidade_satellite (.jpg), outdoors, osm_bright, stamen_terrain [I], stamen_toner_background/labels/lines [I].
- Pricing [S] https://stadiamaps.com/pricing/: Free 200k credits/month, **non-commercial only**. Raster tile = 1 credit, satellite = 4 credits. Video/print perpetual licensing only via enterprise.
- Attribution: "© Stadia Maps © Stamen Design © OpenMapTiles © OpenStreetMap contributors" (Watercolor omits OpenMapTiles) [S migration guide via snippet].

---

## 8. Generic raster tile servers ("Universal Raster" / custom Mapcomp Imagery)

### 8.1 GEOlayers facts [S]
- Imagery can come "from any common image-based tileserver" (Gumroad listing snippet). Stadia tutorial shows a URL with `{z}/{x}/{y}` entered into Mapcomp Imagery.
- CL 1.4.0: "{year}, {month}, {date}, {dateISO} variable support in tile URIs". [I] Used for daily imagery such as NASA GIBS (`.../{dateISO}/...`). Whether "date" means day-of-month or a full date is [?]. [I] dateISO = `YYYY-MM-DD`.
- CL 1.4.0: "Map Bounds supported for Raster Styles". [I] A bbox outside of which no tiles are requested.
- CL 1.0.9: "Tilemerger supports TMS servers". [I] A TMS flag flips y: `y_tms = 2^z - 1 - y`. "Tilemerger" is GEOlayers' internal tile-stitching component (also CL 1.0.4 "tilemerger invalid urls at longitude < -180", so it wraps x).
- CL 1.6.6: "Default Tiles in Universal Raster Style not shown in some cases". [I] "Universal Raster" is a built-in style with a default tile URL that the user can change.
- CL 1.2.2: "Multiple Custom Imagery Styles can be im-/exported in the preferences". CL 1.4.2: "Styles based on a url source can be updated with one click".
- CL 1.1.2 bugfixes: "overzoomed raster imagery placed with lower-zoom tiles instead of overzoomed textures", "raster styles above max zoom render wrong", "imagery texture filtering blurry". So raster styles have a **max zoom**, and above it tiles are **overzoomed** (upscaled).
- CL 1.4.0: "Rejected server requests can lead to black holes in tiles". Forum snippet: missing tiles during finalization → "GEOlayers will use a lower zoom level to fix this gap". [I] Fallback to the parent tile, cropped and upscaled.
- CL 1.2.4: "Mapcomps with lower zooms than imagery minzoom → extremely long loading". So styles have a **min zoom** too.
- CL 1.15.1: "Default Imagery Quality Setting in Preferences". Finalize option `previewQuality` (HELP). [I] Quality changes the tile zoom offset/resolution.
- Placeholders named in the task but unconfirmed publicly: `{s}` (subdomain rotation) and `{q}` (quadkey) [?/I]. Common conventions to support: `{z} {x} {y} {-y} {s} {q}/{quadkey} {r}/@2x {year} {month} {date} {dateISO}`, plus `{switch:a,b,c}` (JOSM) [I].

### 8.2 OSM standard tiles [S] https://operations.osmfoundation.org/policies/tiles/
- `https://tile.openstreetmap.org/{z}/{x}/{y}.png`, HTTPS only. Identifying UA required; honour cache headers or cache ≥7 days. **Bulk download/prefetch and offline use are prohibited.** Attribution required. Video/print not addressed.
- [I] GEOlayers' finalize downloads many tiles at high zoom for every frame, which is close to bulk downloading. Do not use tile.openstreetmap.org as a default.

---

## 9. Local tile directories

- [S] CL 1.1.8 "Pick local tile directories in Imagery setup". CL 1.5.2 "Local Tile Directory min and max zoom are read from 'metadata.json'".
- [S] Forum snippet: make a local tile directory from a GeoTIFF "with an application like QGIS or MapTiler Desktop, then create a new map imagery style from this local tileset". aescripts tutorial title: "Using georeferenced imagery and local tile directories in GEOlayers 3".
- [S] MapTiler Engine/Desktop writes `{z}/{x}/{y}.png` folders plus `metadata.json` describing the dataset (https://docs.maptiler.com/engine/search snippet).
- [I] `metadata.json` (MapTiler Engine / mb-util style) keys: `name`, `format` (png/jpg/webp), `minzoom`, `maxzoom` (often strings), `bounds` "w,s,e,n", `center`, `scheme` (xyz/tms), `profile` (mercator), `attribution`. Our reader should parse min/maxzoom as numbers, use bounds as Map Bounds, and handle `scheme: tms` (gdal2tiles writes TMS y by default and `tilemapresource.xml` instead).
- [I] QGIS "Generate XYZ tiles (Directory)" writes no metadata.json, so default to 0–22 or scan folder names.

---

## 10. Routing (car / bike / pedestrian with maneuver points)

### 10.1 GEOlayers facts [S]
- CL 1.0.4: "Bugfixes: car/bicycle/pedestrian routes".
- CL 1.4.0: "Car, Bike and Pedestrian Routes imported without Maneuver Points by default (changeable in prefs)".
- CL 1.1.2: "connect features crossing date border". CL 1.7.1: "Feature Mesh … one-to-all lines instead of all-to-all lines". So the browser has **connect/mesh** tools that build lines between features. [I] Car/Bike/Pedestrian routes are probably modes of "connect features", next to straight lines and great-circle lines.
- Stadia tutorial and aescripts marketing: "animate driving routes". CL 1.2.4 ".tcx file import". A 2020 aescripts blog post covers routes from GPX.
- **Provider: [?] not publicly documented.** No search hit links GEOlayers to OSRM, ORS, GraphHopper, Valhalla or MapTiler (MapTiler has no routing API).

### 10.2 Most plausible provider [I]
- **FOSSGIS OSRM** (https://routing.openstreetmap.de, the router used on openstreetmap.org) offers exactly three worldwide profiles, car, bike and foot, which matches "Car, Bike and Pedestrian". It is free with no key, returns `steps` with `maneuver` objects, and is the default on osm.org. Policy [S] https://routing.openstreetmap.de/about.html: ≤1 req/s, valid UA/referrer, required attribution plus a "fix the map" link, no heavy use.
  - Endpoint pattern [I]: `https://routing.openstreetmap.de/routed-{car|bike|foot}/route/v1/driving/{lon1},{lat1};{lon2},{lat2}?overview=full&geometries=geojson&steps=true`
- Alternatives: the OSRM demo `router.project-osrm.org` (car only, so it does not fit all three modes); Mapzen Valhalla (shut down 2018; GEOlayers 3 launched 2019); openrouteservice (needs a key); GraphHopper (needs a key, free plan non-commercial).
- OSRM API [S] https://project-osrm.org/docs/v5.24.0/api/: `/route/v1/{profile}/{coords}?alternatives&steps=true&geometries=geojson&overview=full&annotations`. StepManeuver: `location [lon,lat]`, `bearing_before`, `bearing_after`, `type` (turn, new name, depart, arrive, merge, on ramp, off ramp, fork, end of road, continue, roundabout, rotary, roundabout turn, notification, exit roundabout, exit rotary), `modifier` (uturn, sharp right, right, slight right, straight, slight left, left, sharp left), `exit`. RouteStep: name, ref, distance, duration, mode, …
- [I] "Maneuver Points" = Point features at each `step.maneuver.location`, with properties type/modifier/name/distance/duration, imported next to the LineString route.

### 10.3 Free alternatives
- FOSSGIS OSRM (above), FOSSGIS Valhalla `https://valhalla1.openstreetmap.de/route` [I] (costing auto/bicycle/pedestrian, returns `maneuvers` with `instruction`, encoded polyline6 `shape`).
- openrouteservice `https://api.openrouteservice.org/v2/directions/{driving-car|cycling-regular|foot-walking}/geojson`, key required, free tier about 2,000 directions/day and 40/min [I from memory; plans page did not render]. Steps include `type`/`instruction`/`way_points`.
- GraphHopper free plan: 500 credits/day, non-commercial [S] https://www.graphhopper.com/pricing/
- Self-host: OSRM docker with Geofabrik extracts [I].

---

## 11. Elevation for 3D Landscapes

### 11.1 GEOlayers facts [S]
- 3D Landscape renderers: Trapcode **Mir** (cgpress, GEOlayers 1/2 era), **Plexus**, **FreeForm Pro** (Mettle tutorial: "Create the 3D landscape, Keyframe…, Finalize the GEOlayers 3 landscape, … Adjust the landscape elevation"), **Helium** (CL 1.5.0; aescripts tutorial: "Create 3D Landscape", raise the elevation height, works with AE cameras/shadows).
- CL 1.5.2: "Project Color Space set to 'None' on 3D Landscape creation due to the 'Calculations' effect used on the heightmap". **So the heightmap is processed inside AE with the Calculations effect.** [I] GEOlayers likely builds a grayscale heightmap comp from RGB-encoded DEM tiles, or combines channels from a DEM Mapcomp, using AE's Calculations effect.
- CL 1.2.5: "DEM tile requests to servers without valid response stall finalization". So DEM is fetched as **tiles** from (possibly configurable) servers.
- CL 1.1.8: "3D Landscape heightmaps/labels offset in AE 17.5". CL 1.0.1: "Can't set work area duration on 3D Landscape creation".
- CL 1.1.2: "Sample Project Elevation".
- MapTiler/GEOlayers pages advertise "3D terrain" [S].
- Forum snippet (aescripts comment 6827): a user asked "How do I obtain elevation tiles now that Mapzen is defunct". [I] Earlier GEOlayers versions used Mapzen terrain tiles, which have lived on AWS since 2017/2018 (`elevation-tiles-prod`). The current DEM source is probably MapTiler terrain-rgb (with the MapTiler key) and/or the AWS Terrarium tiles. [?] Not confirmed.

### 11.2 Sources
- **AWS Terrain Tiles (Mapzen/Tilezen)** [S] https://registry.opendata.aws/terrain-tiles/: bucket `elevation-tiles-prod` (us-east-1), EU replica `elevation-tiles-prod-eu`. Free, no key.
  - `https://s3.amazonaws.com/elevation-tiles-prod/terrarium/{z}/{x}/{y}.png` [S simonw/til]; also `normal/`, `geotiff/` (512 px), `skadi/{N|S}yy/{N|S}yy{E|W}xxx.hgt.gz` [I]. Max z15 [I].
  - Terrarium decode [S] https://github.com/tilezen/joerd/blob/master/docs/formats.md: `h = (R*256 + G + B/256) - 32768`.
  - Attribution [S] joerd attribution.md: credit the sources (USGS 3DEP/SRTM/GMTED2010 "data courtesy of the U.S. Geological Survey", ETOPO1 NOAA, EU-DEM Copernicus, Geoscience Australia CC BY 4.0, CDEM OGL-Canada, LINZ, Kartverket, Environment Agency UK, INEGI, ArcticDEM…).
- **MapTiler terrain-rgb-v2** (see §1.2): z0–14, WebP, Mapbox-RGB encoding [I], needs a key (Free plan OK for hobby use).
- Others [I]: Copernicus GLO-30 COGs on AWS (`copernicus-dem-30m` bucket), OpenTopography API (key), Mapbox terrain-rgb (key).

---

## 12. Other services seen in GEOlayers material
- **GitHub**: `geolayers3.githubBrowser()` (public repos, flatten folders, filter). [I] Uses the GitHub REST API `api.github.com/repos/{user}/{repo}/git/trees/{branch}?recursive=1` and `raw.githubusercontent.com`. CL 1.2.2 fix: "githubBrowser() repos without 'master'", so it reads the default branch. Unauthenticated GitHub API limit is 60 req/h [I].
- **ArcGIS Hub/FeatureServer GeoJSON** (COVID samples) via `fetchJson` [S EX].
- **historical-basemaps** repo (aourednik) via githubBrowser [S EX].

---

## 13. Free/open alternatives for a hobby build (summary)

| Need | Alternative | Endpoint | Key | Limits / terms | Attribution |
|---|---|---|---|---|---|
| Vector styled basemap (replaces MapTiler Basic/Advanced/Lite) | **OpenFreeMap** [S openfreemap.org] | `https://tiles.openfreemap.org/styles/{liberty|bright|positron|dark|fiord}` [I URL] | none | no view/request limits; offline ≤3,000 tiles/50 MB per download, ≤4 parallel, unique UA; self-host weekly planet (MBTiles/PMTiles/Btrfs) | "OpenFreeMap © OpenMapTiles Data from OpenStreetMap" (also in video/print) |
| Vector tiles, self-hosted | **Protomaps PMTiles** [S docs.protomaps.com] | daily planet ~120 GB, z0–15; `pmtiles extract` regional cutouts | none | ODbL produced work | © OpenStreetMap |
| Vector tiles (OpenMapTiles schema with building render_height, name:xx) | OpenFreeMap / MapTiler Free | — | — | — | — |
| Satellite | MapTiler Free satellite-v2; **EOX Sentinel-2 cloudless** WMTS (`tiles.maps.eox.at`, non-commercial, "Sentinel-2 cloudless - https://s2maps.eu by EOX IT Services GmbH (Contains modified Copernicus Sentinel data YYYY)") [S snippets]; Esri World Imagery `https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}` (**y before x**; Esri staff: needs an ArcGIS license, not for commercial use, not for offline export) [S snippets]; NASA GIBS (daily, `{dateISO}`) [I] | | | | |
| Raster basemaps | CARTO (free key, 5M/month non-commercial); Stadia (200k credits, non-commercial); OSM standard (no bulk/offline) | | | | |
| Geocoding | Nominatim (1 req/s, no autocomplete); MapTiler Free geocoding (1k search sessions/month); Photon `https://photon.komoot.io/api?q=` [I] | | | | |
| Country/admin polygons | Natural Earth (public domain, incl. POV worldviews); OSM admin via Overpass/Nominatim polygon_geojson; geoBoundaries [I] | | | | |
| POIs/buildings/roads | Overpass (public mirrors) | | | | |
| Routing | FOSSGIS OSRM car/bike/foot; FOSSGIS Valhalla; ORS free key; self-hosted OSRM | | | | |
| DEM | AWS Terrarium (no key); MapTiler terrain-rgb-v2 (Free key); Copernicus GLO-30 | | | | |
| Bing replacement | Azure Maps Render Get Map Tile (needs an Azure account) | | | | |

---

## 14. Implementation notes for the clone [I]
1. A provider abstraction with: id, type (`xyz-raster`, `vector-style`, `bing-metadata`, `local-dir`, `dem-terrarium`, `dem-mapbox-rgb`), url template, subdomains, tms flag, min/max zoom, bounds, tile size (256/512), retina, attribution string, api key slot, language/worldview capability flags.
2. URL template engine: `{z} {x} {y} {-y} {s} {q|quadkey} {r} {year} {month} {date} {dateISO} {key}`. `{date}` semantics need verifying [?].
3. Request scheduler per host: concurrency caps, 1 req/s for Nominatim/OSRM, back-off on 429/406/504, identifying UA. Disk tile cache keyed by URL (GEOlayers has "purgeImageryCache" and a hidden "Feature Cache Paths" pref [S]).
4. Missing-tile fallback: walk up to the parent tile and crop/upscale (forum statement plus the CL 1.4.0 "black holes" fix).
5. Overpass: macro expansion (`{{bbox}}` from the Mapcomp view in s,w,n,e; `{{geocodeArea:}}` via Nominatim → area id) → mirror selection via `/api/status` → osmtogeojson conversion.
6. Attribution: GEOlayers has a "Provider Attribution" text element (CL 1.6.1 "Provider Attribution aligned wrong if Paragraph set to Right-To-Left") [S]. The clone must render the provider's attribution string into the comp.
7. Preferences tied to providers [S]: MapTiler API key (bottom of prefs); worldview (1.18.0); default language (1.1.8); Maneuver Points on/off (1.4.0); fallback search service as default (1.0.2); GEOlayers Mapdata enable (1.0.7, legacy); import/export custom imagery styles (1.2.2); default imagery quality (1.15.1); preload features for search on startup (1.2.2); default features loaded on start, http/https allowed (1.1.7, 1.2.2); feature preview bounding boxes (1.2.2); hidden feature cache paths (1.0.7); CartoDB API key (1.19.0, exact location [?]).
