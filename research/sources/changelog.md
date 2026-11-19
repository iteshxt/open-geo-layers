# GEOlayers 3 — full public version history
Source: https://aescripts.com/geolayers/ ("Version History" → Show All), captured 2026-11-18.

## 1.20.0 — Oct 24, 2026
- Dataviz - Lines Imagery Style
- Scripting API geolayers3.snapLayerToMapcomp(...)
- Scripting API geolayers3.addLabel(...) supports null as labelTemplateComp argument, to add a null layer
- (Prerelease page, build 984): Globe and Equal Earth Map Projection via Plugin

## 1.19.0 — Sep 4, 2026
- CartoDB Imagery supports entering an API key
- Scripting API geolayers3.addLabel(...) supports coordinate array to animate along paths

## 1.18.1 — Aug 11, 2026
- Bugfix: MapTiler Search downloads wrong countries if a specific worldview is set

## 1.18.0 — Aug 10, 2026
- After Effects v27 support
- MapTiler Search supports worldviews (set in Preferences)
- Bugfix: internal expression error for certain rotations on snapped layers that rotate with the map

## 1.17.0 — Jun 5, 2026
- Getting Features by OSM Id is more stable
- Adding Admin Bounds for higher zooms to the browser is more stable

## 1.16.0 — Mar 30, 2026
- Improved Auth Error handling

## 1.15.2 — Feb 5, 2026
- Bugfix: Shape layer world copies all placed at same position if drawing preference "lock transform properties" is enabled

## 1.15.1 — Nov 9, 2025
- Default Imagery Quality Setting in Preferences
- License Framework Upgrade

## 1.14.1 — Jun 12, 2025
- Bugfix: AVLayer assets in label templates not swapped if no text layer in the composition
- Bugfix: Error in host method "initEngine" due to other scripts interfering
- Bugfix: Importing files with "#" in path fail with descriptive error

## 1.14.0 — Apr 28, 2025
- Distance Circle default Script
- Troubleshooting for corrupted projects due to After Effects "Recovering" Mapcomps

## 1.13.1 — Mar 30, 2025
- MapTiler Search loads full geometry from their internal dataset to ensure basemap consistency
- Bugfix: MapTiler Search returns only point features in certain countries
- Bugfix: RTL languages displayed in reverse in Mapcomp labels

## 1.12.2 — Jan 20, 2025
- Create 3D Scene & Camera Script
- Bugfix: RTL languages displayed reverse in Label Templates
- Bugfix: Traditional Chinese language setting not applied correctly

## 1.11.1 — Oct 24, 2024
- "coordinate_elevation" property is added on import of 3D Line Features
- About & Licensing Panel update
- Bugfix: AE crashes on Label swap

## 1.10.0 — Aug 25, 2024 — After Effects 25 support
## 1.9.0 — Aug 1, 2024 — Bugfix: OSM search results do not appear for some users
## 1.8.0 — Jul 5, 2024 — Bugfix: User authentication error during startup
## 1.7.2 — Apr 18, 2024 — Bugfix: Natural Earth Inner Boundaries for some countries are messed up

## 1.7.1 — Jan 5, 2024
- Creating a Feature Mesh in the Browser with one Feature and a Collection selected creates one-to-all lines instead of all-to-all lines

## 1.7.0 — Dec 5, 2023
- Disputed Borders can be toggled on/off on MapTiler Imagery Styles Basic, Advanced and Lite

## 1.6.6 — Nov 28, 2023
- Natural Earth Data update
- Bugfix: Default Tiles in Universal Raster Style not shown in some cases

## 1.6.4 — Oct 9, 2023
- Stamen Imagery Style removed (Stamen tile hosting moved to Stadia Maps). Migration video: https://vimeo.com/872558334

## 1.6.3 — Aug 30, 2023 — Bugfix: Downloading a Search Result errors in some cases
## 1.6.2 — Aug 18, 2023 — Bugfix: Some Features can not be subtracted or merged

## 1.6.1 — Jun 18, 2023
- Welsh language support
- Bugfix: Provider Attribution aligned wrong if Paragraph set to Right-To-Left
- aesp upgrade

## 1.5.7 — Apr 16, 2023 — Bugfix: Preview Map is blank
## 1.5.6 — Mar 24, 2023 — Overpass service chosen automatically by response time and number of available slots
## 1.5.5 — Mar 2, 2023 — Bugfix: MapTiler search results no longer displayed

## 1.5.4 — Nov 24, 2022
- Bugfix: Labels of the same Place get the same layer name → incorrect expressions
- Bugfix: Natural Earth States and Provinces display only Line Features
- Bugfix: Setting up a File for the Search without a url opens file popups during each search

## 1.5.3 — Oct 19, 2022
- Bugfix: MapTiler Search only finds Point Features
- Bugfix: Features with empty coordinates array error out on import
- Close Undo Groups on each host call manually
- Hidden Pref to purge Undo Cache on each host call (prevent AE undo-grouping bugs)

## 1.5.2 — Aug 17, 2022
- Local Tile Directory min and max zoom are read from "metadata.json"
- Bugfix: Renderer not set to CINEMA 4D when drawing Extruded Features
- Bugfix: Project Color Space set to "None" on 3D Landscape creation due to the "Calculations" effect used on the heightmap

## 1.5.1 — Jun 14, 2022
- MapTiler Satellite Imagery V2
- Bugfix: Mapcomp Labels do not display dots anymore

## 1.5.0 — Apr 26, 2022
- Helium plugin support for 3D Landscapes
- Removing a Mapcomp with others linked to it gives the option to remove all linked ones too
- Language support for Irish (Gaelic) and Scottish Gaelic
- File Search API supports zipped shapefiles
- Bugfix: AE pref "Default Position Properties to Separate Dimensions" leads to errors

## 1.4.2 — Nov 12, 2021
- Almost every file can be imported from a remote url holding CTRL/CMD while clicking the import button
- Styles based on a url source can be updated with one click
- Feature Collections based on a url source can be updated with one click

## 1.4.1 — Oct 4, 2021 — Bugfix: Pointers of default label templates don't follow the label's anchor point

## 1.4.0 — Oct 1, 2021
- MapTiler Lite Imagery free for non-commercial projects
- Pinned Layers and Labels indicated by "(pinned)" suffix in layer name
- precompose_feature_shape_layer.jsx default script to precompose drawn features
- Car, Bike and Pedestrian Routes imported without Maneuver Points by default (changeable in prefs)
- Map Bounds supported for Raster Styles
- {year}, {month}, {date}, {dateISO} variable support in tile URIs
- Bugfix: Labeling empty Feature Collections freezes the panel
- Bugfix: Rejected server requests can lead to black holes in tiles
- Bugfix: Blocked preview map for Mapcomps with min/max zoom when mapcomp sync is off
- Bugfix: Empty previews for Mapcomps with min zoom
- Bugfix: add_line_patterns.jsx fails if layer already has effects

## 1.3.0 — Aug 2, 2021
- Import Polygon, Line & Point Features from Shape Layers
- Collapse linked Mapcomps in the Mapcomp List
- Mapcomp naming supports leading zeros automatically (Mapcomp 01, Mapcomp 02...)
- Click remove Features holding SHIFT to clear the whole browser
- Click export Features holding SHIFT exports all features except favorites to features.geojson beside the project
- Current View Transport Features have separate ramps
- AE v22 support
- About window shows Build Nr and prerelease indicator

## 1.2.6 — Jun 21, 2021
- Property Diagrams
- Directly queue Renders after finalization
- Confirm dialog before removing a Mapcomp
- Shape Layer Transform Properties can be locked (Preferences)
- Mapcomp Filter for Styles and Labels displayed as fullscreen panel
- Bugfix: Mapcomp filters for Label Templates and Shape Layer styles not loaded from project on init
- Bugfix: Advanced Style shows black polygons if street density set to 5
- Bugfix: Pinned Layer rotation wrong if rotate-with-map on and layer rotated other than 0,0,0

## 1.2.5 — May 21, 2021
- Dashed strokes can have round caps
- Multi Polygon shapes render faster (different shape layer content structure)
- Line Features from csv data can cross the date border
- Label renaming dictionaries can be loaded from files
- Bugfix: MapTiler Satellite imagery blurry
- Bugfix: Keyframe easing in 'Animate View to Feature' wrong in some cases
- Bugfix: DEM tile requests to servers without valid response stall finalization

## 1.2.4 — Apr 26, 2021
- Change Mapcomp duration via Mapcomp Settings (also changes related Mapcomps' durations)
- .tcx file import
- Keyframes created by a data-driven property can have non-linear timing
- Bugfix: Strange keyframe interpolation on Zoom leads to expression errors
- Bugfix: Mapcomps with lower zooms than imagery minzoom → extremely long loading

## 1.2.3 — Mar 26, 2021
- If GEOlayers creates an animation it shows a prompt to adjust its duration
- Features can be drawn with time offsets (also data-driven by properties)
- Labels can be added with time offsets (also data-driven by properties)
- Labels can be swapped no matter which Mapcomp is selected
- Bugfix: Adding a Current View Feature fails if view can't be geocoded

## 1.2.2 — Feb 12, 2021
- Multiple sources can be used for the Feature Search
- Natural Earth Search API; Browser Search API; Browser Downloads Search API
- File Search API (http urls supported); Folder Search API; GitHub Search API (public repos)
- Preload Features for Search on startup
- Improved Feature Preview in Search; Feature Preview can show bounding boxes (pref)
- CTRL/CMD click "Import File" in the browser to import from a remote url
- Circles Imagery for DataViz
- Features of Collections can be sorted by Name
- Default Features can be loaded by http/https
- Multiple Custom Imagery Styles can be im-/exported in the preferences
- CSV files separated by ";" supported
- Bugfix: Multipoint geometries; default-script-drawn Mapcomp Views not rendered at higher zooms; Custom Label Template Project not loaded; githubBrowser() repos without 'master'

## 1.2.1 — Jan 29, 2021
- Data-driven color previewed with a gradient in the UI
- Merge / Subtract Features, Create Lines from Polygons, Simplify Feature Topologies — non-blocking & cancelable
- geolayers3.addToBrowser() naming options; geolayers3.getBrowserSelection()
- Label Renaming (prefs) also works for Template Labels apart from Mapcomp Labels
- Bugfix: Preview map not updating on editing styles in create-mapcomp dialog

## 1.2.0 — Jan 7, 2021
- Quick Start Buttons
- Simplify Geometry by max Zoom
- Dash Speed in Shape Layer Styles
- Preserve Transparency in Shape Layer Styles
- Features drawn multiple times if they cross the date line
- Natural Earth Admin Boundaries Imagery
- Natural Earth States and Provinces Lines dataset in Feature download
- Inner Boundaries Feature for each country in 'Find Features at Coordinate'
- Improved Natural Earth data search by name
- Automatic label point always centered inside largest polygon
- Accurate topology geometry simplification (polygons with shared borders)
- ALT/OPT click scripts in toolbar shows description; CTRL/CMD click reveals file
- Default scripts: add_line_pattern.jsx, blend_in_and_out.jsx, north_needle.jsx
- geolayers3.githubBrowser(), draw(), drawBrowserSelection(), removeBrowserSelection()
- Bugfix: Dashed lines cause expression errors with legacy expression engine

## 1.1.9 — Nov 27, 2020
- MapTiler Cloud integration
- Natural Earth Imagery; Natural Earth Water Mask; Natural Earth Mapcomp Labels
- Natural Earth Physical Areas Land & Water in Feature download

## 1.1.8 — Nov 19, 2020
- Preview map displayed in default language set in prefs
- Drawing options (simplify geometry, Auto Stroke Width, individual layers…) saved in the .aep
- Water visibility separate for oceans, lakes and rivers in Basic and Advanced Imagery
- Choropleth Mapcomp Imagery style for large datasets
- Merge Features in Feature Collections by name
- Pick local tile directories in Imagery setup
- GeoJSON file sequence import
- Bugfixes: 3D Landscape heightmaps/labels offset in AE 17.5; Add Null to Feature; expression errors on large zoom; web-worker tile-render memory leaks; empty expressions on MapPivot layer

## 1.1.7 — Sep 28, 2020
- Create Arrows from the toolbar
- Dashed lines with Auto Stroke Width look better during zooms ("Dash Zoom Threshold")
- Default Features loaded on extension start (prefs)
- scale_bar.jsx default script; shapes_to_arrows.jsx default script
- geolayers3.addToBrowser() supports OSM id

## 1.1.6 — Jul 20, 2020
- Filter Label Templates and Shape Layer Styles by Mapcomp (right-click filter bars)
- 'Draw Mapcomp View' default script
- Advanced Imagery Style extra color for rail
- geolayers3.filesInDir(), importProject(), modalActions(), modalEditObject()
- Create log files

## 1.1.5 — Jun 28, 2020
- Default comp specs (size, duration, framerate) in preferences
- Feature properties for a whole Feature Collection export to / import from csv or tsv
- GEOlayers asks for missing data if a feature can't provide all Label Template data fields
- Bugfix: Click UI map to select line features between two vertices

## 1.1.4 — Jun 2, 2020
- Bing Imagery Dark, Light, Gray; Bing imagery language support
- Duration of timed data displayed in Feature Properties
- Bugfixes: drawing on individual layers; shape layers created to draw on at large scales; swapping shape layer styles doesn't apply blending modes; label position of trimmed linestrings; pinned layers from older versions

## 1.1.3 — May 19, 2020
- Feature's label position can be dragged and centered in the feature properties panel
- Unpin pinned layers/labels by clicking pin button in toolbar again
- Language support for Natural Earth data
- UI animations

## 1.1.2 — May 11, 2020
- Shape Layer Styles support layer blending modes
- Pinned layers' animated position controlled via "Position Progress" slider
- Max Zoom can be set in Mapcomp Settings
- Button in Mapcomp Label settings to create Label Templates
- Sample Project Elevation
- Bugfixes: point features not rendered with CINEMA 4D renderer; connect features crossing date border; imagery texture filtering blurry; overzoomed raster imagery placed with lower-zoom tiles instead of overzoomed textures; raster styles above max zoom render wrong; swap label template errors

## 1.1.1 — Apr 29, 2020 — Bugfixes: panel won't init; swap Label Template error

## 1.1.0 — Apr 26, 2020
- Click the preview map to select Features in the Browser
- Pinned layers have separate Latitude, Longitude and Height properties (reduced interpolation inaccuracy)
- Bugfixes: animate view along single feature; subtract features; layers animated along features at street scale not exact

## 1.0.9 — Apr 7, 2020
- Pick Shape Layer Styles from layers or shape content groups
- Points-inside-Polygons tools in Browser
- Browser filter much faster; shows only necessary collections on filtering
- Create data-driven styles with one click from a Feature Collection's properties panel
- Open http link properties in browser from Feature Properties panel
- Animate the view along multiple selected Features
- geolayers3.addToBrowser supports geojson urls
- autorun.jsx can run automatically on opening a project
- Tilemerger supports TMS servers
- GeoJSONs next to the project load automatically when opened via the projects panel

## 1.0.8 — Mar 28, 2020
- Coronavirus animated timeline sample project
- Animated Data Driven Styles
- Animated Label Text

## 1.0.7 — Mar 20, 2020
- Stroke outlines for Shape Layer Styles; stroke join defaults to round
- Mapcomp Settings for Motion Blur
- Default scripts blink.jsx, stagger_layers.jsx
- Create Line Mesh from Polygon Features with common borders
- Data-driven style interpolation curve 'Circle Area' for points
- Configurable Feature Cache Paths (hidden pref)
- En/disable GEOlayers Mapdata (green icon in prefs)
- Bugfix: swap shape layer style with extrusion doesn't adapt anchor point

## 1.0.6 — Feb 10, 2020
- Icon & Locator Label Templates
- SHIFT-select a Label Template reveals its LabelColors comp; ALT-select reveals it in AE project
- Global dataset Lakes / Rivers downloadable to the browser
- Bugfix: Nominatim responses without geometry fall back to point; large OSM downloads freeze panel

## 1.0.5 — Jan 27, 2020
- 3D building extrusion independent from zoom when drawn from toolbar

## 1.0.4 — Jan 11, 2020
- Globe sample project using Video Copilot Orb
- Click preview map to find features (GEOlayers Mapdata subscription)
- New Mapcomp Imagery Style "Advanced"
- Filter Label Templates and Shape Layer Styles by name
- Label layers update their markers from the source comp (responsive timing)
- Label Templates can default to Scale or Rotate with the Map via effect controls on their "Label Anchor Point" layer
- Run scripts from the toolbar
- Square/portrait maps can be pitched more
- geolayers3.addToBrowser(), clearBrowser()
- Bugfixes: car/bicycle/pedestrian routes; text layers can't be pinned; randomize color; tilemerger invalid urls at longitude < -180

## 1.0.3 — Nov 29, 2019
- Sample projects have a download-with-browser button
- Shape layer style thumbnails show stroke width relative to min/max of displayed set
- CSV import property names trimmed

## 1.0.2 — Nov 5, 2019
- Additional online search service used as fallback if default search fails; can be set as default in prefs

## 1.0.1 — Oct 30, 2019
- geolayers3.geocode(), watch(), watchCsv(), unwatch()
- Bugfixes: "Can't set work area duration" on 3D Landscape creation; Mapcomp Anchor layers can't be pinned; anchor source items naming; finalization stalls; swap Shape Layer Style "Unvalid Object"

## 1.0.0 — Oct 11, 2019 — initial release
## 0.9.3 — Oct 5, 2019 — prerelease
## 0.9.0 — Sep 27, 2019 — animate view to feature; reveal label template comp (ALT click); collective feature properties
