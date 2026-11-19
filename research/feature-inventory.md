# GEOlayers 3 — Master Feature Inventory (clean-room, one-to-one parity checklist)

Synthesized 2026-11-19 from 6 research tracks; **revised 2026-11-19 by the final editor** after three critic passes (missing items added, over-claimed tags downgraded; see "Revision log" and "Critic items rejected" at the bottom). **Updated 2026-11-19 from the EBE transcript** ("Every Button Explained"; 14 items added, 444 items in total; see the Revision log). Clean-room: built only from public docs, changelog, the vendor's public example scripts, tutorial metadata (titles/descriptions/chapters/thumbnails), third-party docs and forum snippets. The GEOlayers product (ZXP, installer, `scriptingApiExamples.zip`) was **not** downloaded or inspected.

Parity rule: the recreation is "complete" when every item below is implemented with the same behavior, options, defaults and modifier-key behavior. Items marked **[INFERRED]** or with "unknown" must be confirmed by the user during hands-on testing in After Effects (Win/Mac) before they are frozen.

## Legend

**Tags**
- **[SOURCED]**: stated in a cited source.
- **[INFERRED]**: synthesizer or researcher reasoning. Must be verified.
- **[SOURCED; partly INFERRED]**: the core fact is sourced; the parts written as "(I: …)" are inferred.
- **unknown**: the feature is known to exist, but its UI name or behavior is not documented anywhere reachable. Do not invent it.
- **weak**: the source is only a search-engine snippet of a page that returned HTTP 403 or 404. Every AESF-only fact carries "weak".
- **[GL2-sourced]**: the source describes GEOlayers 2 (pre-2019); the GL3 behavior is INFERRED from it.

**Source abbreviations** (paths relative to `/home/iteshxt/Documents/1.Projects/adobe-extension/research/`)

| Abbr | Source |
|---|---|
| HELP Lnn | `sources/help-readme.md` (official public help + Scripting API), line numbers |
| CL x.y.z | `sources/changelog.md` (aescripts version history 0.9.0 → 1.20.0) |
| EX:Name | `sources/geodata-repo__Name.jsx.txt` / `sources/development-repo__Name.jsx.txt` (vendor public example scripts) |
| STADIA | https://docs.stadiamaps.com/tutorials/getting-started-with-geolayers/ and its 6 screenshots: geolayers-main.png, geolayers-mapcomp-settings.png, geolayers-mapcomp-add-new.png, geolayers-mapcomp-url.png, geolayers-mapcomp-api-key.png, geolayers-mapcomp-apply.png. These are the best exact-UI source. The page says the screenshots come from the vendor's Stamen→Stadia video VM:872558334 (2023-10-09, v1.6.4 era), not from the Route Loop tutorial. Date matters for C-01, C-02, C-09. |
| VM:id | https://vimeo.com/id (oEmbed/API v2 title, description, HD thumbnail) |
| YT:id | https://www.youtube.com/watch?v=id (description/chapters) |
| MC | https://boonelovesvideo.teachable.com/p/geolayers-3-masterclass (curriculum titles) |
| WAR | https://boonelovesvideo.teachable.com/p/geolayers-3-war (curriculum titles) |
| AES | https://aescripts.com/geolayers/ (storefront, read once via reader proxy) |
| AESF | aescripts forum search-engine snippets (**weak**). Thread ids are given where known. **The forum has been removed: every aescripts forum URL returns 404 as of 2026-11-19**, so these citations are snippet-only and cannot be re-verified. "discussion/2017" etc. are thread ids, not years. |
| GHHELP | https://github.com/GEOlayers/Help/pull/2 (open, unmerged, third-party transcription of the help.html bundled with 1.19/1.20) and issue #1. Head README: https://raw.githubusercontent.com/volition74/Help/11a8d7cfe3435a48a6de0ffb206dda6c834337ed/README.md (re-read by the editor via WebFetch 2026-11-19; signatures below confirmed). Public documentation, not product code. |
| GEODATA | https://github.com/GEOlayers/geodata (tree lists 9 scripts; 7 are mirrored in `sources/`; the two RKI COVID19 scripts are cited by raw URL only, not mirrored) |
| INDIETIPS | https://indietips.com/?p=68726 (unreachable; search snippet only, weak) |
| AESLEARN | https://aescripts.com/learn/post/create-3d-terrains-in-after-effects-using-geolayers-and-helium (chapter titles) |
| MTD | https://docs.maptiler.com/guides/maps-apis/maps-platform/how-to-use-maptiler-maps-in-geolayers/ |
| MTG | https://www.maptiler.com/cloud/geolayers/ |
| MT3 | https://docs.maptiler.com/guides/maps-apis/maps-platform/maptiler-cloud-maps-for-adobe-after-effects-via-geolayers-3-plugin/ |
| ACOM:id | community.adobe.com thread (full URL in the notes) |
| METTLE | https://www.mettle.com/how-to-create-a-3d-sliced-landscape-geolayers-3-freeform-pro/ |
| EBE | `sources/transcript-every-button-explained.md`: auto-caption transcript of Boone Loves Video, "GEOlayers 3 for Beginners: Every Button Explained" (YT:DX_qi37vy0Y, 24:49, 2026-06-29), pasted by the user 2026-11-19. A third-party narrator walks the panel top to bottom. The transcript has no screenshots: button **functions and narrated order** are SOURCED, but "over here" positions, icon shapes and tooltip text are not, unless quoted. Caption errors: "pin tool" in the arrow section and in the Create-a-Shape-Layer-to-Draw-On section ("grab the pin tool") = pen tool, "nominatum" = Nominatim, "map Tyler" = MapTiler. (I) The narrator apparently has no MapTiler premium plan ("if you actually have a premium … plan"), so premium-only buttons are probably not shown. |
| n/docs, n/videos, n/community, n/providers, n/arch, n/webapp | `notes/docs-deep-read.md`, `notes/videos.md`, `notes/community.md`, `notes/providers.md`, `notes/ae-architecture.md`, `notes/webapp-and-adjacent.md` |

**ID prefixes**

| Prefix | Area |
|---|---|
| UI | Panel/UI & Mapcomp List |
| MCS | Mapcomp creation & settings |
| VIEW | View animation |
| IMG | Imagery styles & map styling |
| FIN | Finalize & tile rendering/caching |
| MLB | Mapcomp labels (automatic) |
| SRC | Search |
| FB | Feature Browser |
| DRW | Drawing |
| SLS | Shape Layer Styles |
| LBL | Label Templates |
| PIN | Pinning/snapping |
| RTE | Routes & arrows |
| DAT | Data import & DataViz |
| 3D | 3D |
| TB | Toolbar default scripts |
| PRJ | Projects/samples/templates |
| PRF | Preferences |
| API | Scripting API |
| LIC | Licensing/support/misc |

---

## 1. Panel / UI & Mapcomp List (UI)

- [ ] **UI-01 · Main dockable panel "GEOlayers 3"**
  - Behavior: a CEP HTML panel opened from **Window > Extensions > GEOlayers 3**. It is dockable and has a flyout (≡) menu and a context menu.
  - Requirement: the panel must be open for any Scripting API call.
  - EBE: the topmost element is the "GEOlayers 3 panel menu" (the flyout). It holds the general panel actions plus Support, Projects, Preferences and Help (UI-19).
  - Src: ACOM:13712683; HELP L23, L30; STADIA geolayers-main.png; n/arch §0; EBE
  - [SOURCED]

- [ ] **UI-02 · Panel layout (top → bottom)**
  1. Header row: ☰ Mapcomp-list button, the current Mapcomp name (centred; it is a **button**, UI-25), and a **"✓ Finalize"** button (top-right, outlined; "next to" the name button per EBE).
  2. Toolbar icon row.
  3. **"Search..."** field with a clear ×.
  4. Preview map.
  5. Preview footer (zoom/link controls).
  6. Feature Browser header.
  7. Feature tree.
  8. Feature action bar: buttons that appear below the tree when features are selected (FB-46; EBE).
  - A third-party tour names these regions: "Map Comp Bar, Toolbar, Search Bar, Map Preview, Feature Browser, Layer Styles, Feature Collections".
  - EBE walks the panel in the same order: panel menu (UI-19) → Mapcomp List opener → name button → Finalize → toolbar (UI-17) → search bar → preview (top controls UI-11, footer UI-12) → Browser "+" and header (FB-02) → tree → feature action bar.
  - Src: STADIA geolayers-main.png; YT:rJ9asneXYrE; EBE
  - [SOURCED]

- [ ] **UI-03 · Mapcomp bar / Mapcomp List opener**
  - Behavior: clicking the top-left control opens the Mapcomp List.
  - HELP describes this control as "the little arrow top left". The 2023 screenshot shows a ☰ list icon instead (see Contradictions C-01).
  - EBE: "this little icon here", just under the panel menu, opens "your map comp area". The narrator calls it the "map comps settings menu". EBE does not say whether it is an arrow or ☰.
  - Src: HELP L13, L25; STADIA; EBE
  - [SOURCED]

- [ ] **UI-04 · Mapcomp List**
  - Behavior: lists every Mapcomp in the open project, one row each. Clicking a row selects that Mapcomp, closes the List and returns to the main panel; the header shows its name (EBE). The List can also be left with the header name button (UI-25; EBE) or "Back" (STADIA).
  - Each row has:
    - a thumbnail
    - the name
    - a blue imagery icon: an **imagery on/off toggle** (UI-27; EBE)
    - an **"A"** icon (Mapcomp-labels toggle, UI-08; EBE)
    - a **"Preview"** pill
    - a lock icon: locks the Mapcomp (UI-09; EBE)
  - Row actions Settings / Duplicate / Remove / Finalize: UI-05.
  - EBE: the List also offers per-Mapcomp **"Prefer for label templates"** and **"Prefer for shape layer styles"** settings, which read "Prefer for drawing features" once Draw on Preferred Mapcomp is on (UI-26, DRW-28). Their exact form and position are unknown. (I, low confidence: the pill seen as "Preview" in STADIA could be one of these "Prefer…" controls; verify.)
  - Footer: an unknown icon at bottom-left and a **"Back"** button at bottom-right.
  - Src: HELP L25-27; STADIA geolayers-mapcomp-settings.png; EBE
  - [SOURCED; imagery toggle, lock and "Prefer for…" SOURCED (EBE); their exact UI form unknown]

- [ ] **UI-05 · Mapcomp List hover buttons**
  - Behavior: hovering a row reveals these buttons:
    - **Settings** (sliders icon, tooltip "Settings"); opens MCS-09
    - **Duplicate** (two-sheets icon); MCS-22
    - **Delete** (trash; "remove" in EBE); UI-07
    - **✓** = **Finalize this Mapcomp** (FIN-01). EBE: "You can duplicate the map comp. You can remove it or you can finalize it."
  - EBE narrates them in this order: Settings, Duplicate, Remove, Finalize, then the lock (UI-09). EBE does not say they appear only on hover.
  - HELP's list of these buttons is truncated ("the following buttons appear:"). EBE names no further row buttons, so (I) the set is these four plus the row toggles and lock.
  - Src: STADIA; HELP L27; EBE
  - [SOURCED (✓ = Finalize: EBE)]

- [ ] **UI-06 · "+ Create new Mapcomp" row**
  - Behavior: a row in the Mapcomp List that starts the two-step create dialog (MCS-03/04).
  - HELP calls the same action "+ Add Mapcomp" (C-01). EBE: "you can create a new map composition" from the List.
  - Src: STADIA; HELP L13; EBE
  - [SOURCED]

- [ ] **UI-07 · Remove Mapcomp**
  - Behavior: a confirm dialog appears before removing (1.2.6).
  - If other Mapcomps are linked, the dialog offers to remove all linked ones too (1.5.0).
  - Src: CL 1.2.6, 1.5.0
  - [SOURCED]

- [ ] **UI-08 · "A" icon = Mapcomp Labels on/off per Mapcomp**
  - Behavior: toggles the automatic Mapcomp labels (MLB) for that Mapcomp.
  - EBE: "right now, we have imagery, but we don't have any labels. So, if you click on here, it will bring up the labels."
  - Src: AESF (≈comment/24493, weak); STADIA row icon; EBE
  - [SOURCED (EBE); icon observed (STADIA)]

- [ ] **UI-09 · "Preview" pill and lock icon in list rows**
  - **Lock**: EBE: "You can also lock it right over here", i.e. the lock locks the Mapcomp. What locking does is not explained. (I: it toggles the AE lock of the Mapcomp layer in the containing comp.)
  - **"Preview" pill**: not mentioned by EBE; meaning unknown. (I: it marks unfinalized/preview-quality imagery, or it is one of the "Prefer for…" controls, UI-26.)
  - Src: STADIA; EBE
  - [SOURCED existence; lock = "lock the Mapcomp" SOURCED (EBE), its effect INFERRED; pill meaning unknown]

- [ ] **UI-10 · Collapse linked Mapcomps**
  - Behavior: linked/controller Mapcomps are shown grouped and can be collapsed in the list.
  - Src: CL 1.3.0
  - [SOURCED]

- [ ] **UI-11 · Interactive preview map**
  - Behavior:
    - Pan and zoom to navigate (YT:5B8p13M0AAI "How to Navigate the Map").
    - Change pitch and bearing (YT:kzUK7jjBIlg chapter "Changing the pitch and the bearing"; (I) that this is done in the preview map).
    - Must never come up blank (CL 1.5.7 bugfix "Preview Map is blank").
    - Shows the selected Mapcomp's view as a blue trapezoid with diagonals (the pitched/rotated frustum).
    - Corner icons: crosshair at top-left; camera icon (blue) and image icon at top-right.
    - Third-party labels for these controls: "Show Mapcomp view button", "Change control map imagery", "Zoom level limitation".
  - Top controls per EBE:
    - top-left: **"Find features at coordinate"** (FB-11)
    - **Mapcomp View** toggle: shows or hides the blue overlay of the selected Mapcomp's frame. EBE: "It will still uh keep everything locked up" (only this quote is sourced; I: link unaffected, i.e. turning the overlay off leaves the link state unchanged).
    - **switch the preview imagery**, e.g. to show the actual final map
  - Src: STADIA; YT:kzUK7jjBIlg; CL 1.5.7; EBE
  - [SOURCED; functions of the three top controls SOURCED (EBE); Find features at coordinate at the top left SOURCED (EBE, approximate position); crosshair icon = Find features at coordinate INFERRED (position match to STADIA); link-unaffected reading of the overlay toggle INFERRED; camera/image icon ↔ function INFERRED from STADIA icon order; pitch/bearing "in the preview" INFERRED]

- [ ] **UI-12 · Preview footer**
  - Controls, left to right (STADIA icons; EBE functions in the same narrated order):
    - a diamond/compass icon = the **Keyframe** button. EBE: it "will add key frames to whatever … map comp you have selected": Latitude, Longitude, Zoom, Bearing and Pitch at the current time. If the Mapcomp is linked to a controller, the keys go on the controller (MCS-21; VIEW-02).
    - a **chain-link icon** (blue = linked) = **"Link Mapcomp View"** (UI-13; EBE)
    - zoom-out (−) and zoom-in (+): big zoom steps (EBE)
    - a horizontal zoom slider: click and drag for fine zoom (EBE)
    - a north/compass icon = **reset rotation**: "put it flat, it gives you north up" (EBE; I: bearing 0 and pitch 0)
  - Src: STADIA; EBE
  - [SOURCED; functions SOURCED (EBE); diamond = Keyframe matched to the icon by narrated order, INFERRED]

- [ ] **UI-13 · Mapcomp sync / "Link and Unlink control map with preview map"**
  - Behavior: the footer chain-link toggle, named **"Link Mapcomp View"** in EBE.
    - Linked: the preview follows the selected Mapcomp's view at the current time, and moving the preview moves the Mapcomp view (EBE: "as I move the map comp view around, it moves this"). Once the view is keyframed (Keyframe button, UI-12), moving the preview at another time adds keys for the changed parameters (VIEW-02; EBE).
    - Unlinked: the preview can be explored freely without changing the Mapcomp view (EBE).
    - Re-linking snaps the preview back to the Mapcomp's view from its existing keyframes (EBE).
  - (I) With link on and no keys yet, moving the preview probably sets the values statically; EBE shows only the keyed case.
  - A 1.4.0 bugfix concerns Mapcomps with min/max zoom while sync is off.
  - Src: CL 1.4.0; YT:kzUK7jjBIlg; EBE
  - [SOURCED; write-back SOURCED (EBE); unkeyed case INFERRED]

- [ ] **UI-14 · Preview map language**
  - Behavior: the preview is displayed in the default language set in prefs (PRF-02).
  - Src: CL 1.1.8
  - [SOURCED]

- [ ] **UI-15 · Preview map click actions**
  - Clicking the preview:
    - finds features at that point (MapTiler data; FB-10)
    - selects matching features in the Browser (1.1.0); this includes a line clicked between two vertices (1.1.5)
  - EBE shows a separate **"Find features at coordinate"** button (FB-11) that lists the features at the current view location; EBE does not show clicking on the map.
  - Src: CL 1.0.4, 1.1.0, 1.1.5; HELP L391; EBE
  - [SOURCED]

- [ ] **UI-16 · Preview overlays**
  - Behavior: the preview shows the outline of the hovered search result or collection, labeled with name and feature count, e.g. "Deutschland (3)".
  - Optional bounding-box display (pref, PRF-05).
  - EBE: hovering a search result previews it on the map; selected Browser features are previewed while "Preview selection on map" is on (FB-45).
  - Src: VM:517178231 thumbnail; CL 1.2.2; EBE
  - [SOURCED]

- [ ] **UI-17 · Toolbar row layout**
  - Observed icons (STADIA, 2023):
    - Left group (7 icons): pin, pill/toggle, sliders, cube, mountain, bar chart, document.
    - Centre: the **Label Template selector** dropdown (UI-18) and a ⇄ swap button.
    - Right group: curved arrow, framed pen, colour swatch (current Shape Layer Style), ⇄ swap.
  - Narrated buttons, in order (EBE; a user without a MapTiler premium plan; positions approximate):
    1. **Pin Selected Layers to Map** (PIN-01). EBE: "this little pin selected layers to map button".
    2. **Create new Controller Mapcomp** (MCS-28)
    3. **Create 3D Landscape** (3D-05)
    4. **Draw Diagram** (Property Diagrams, DAT-14)
    5. **Scripts** list (TB-01)
    6. Label area in the middle: the template dropdown (UI-18), a **manual-label** button (icon unknown) that shows a crosshair while active (LBL-05), and **Swap Label Template** (LBL-12)
    7. **Draw Arrow** (RTE-06)
    8. **Create a Shape Layer to Draw On** (DRW-22)
    9. **current Shape Layer Style** button; opens the Shape Layer Style panel (SLS-01)
    10. **Swap Shape Layer Style** (SLS-11)
  - Icon ↔ button mapping: INFERRED, matched by narrated order to the STADIA (2023) icons. EBE describes no icon shapes, two STADIA left icons are not narrated, and the 2023 and 2026 versions may differ, so only the narrated functions and their order are SOURCED.
    - INFERRED (order + semantic match): pin = Pin; mountain = Create 3D Landscape; bar chart = Draw Diagram; document = Scripts; curved arrow = Draw Arrow; framed pen = Create a Shape Layer to Draw On; swatch = Shape Layer Style; centre ⇄ = Swap Label Template; right ⇄ = Swap Shape Layer Style (EBE narrates the swap while the Shape Layer Style panel is open and never places it in the toolbar; SLS-11).
    - INFERRED: Create Controller Mapcomp is one of pill/sliders (I: the icon right after pin).
    - Not narrated: two of pill/sliders/cube. EBE says a MapTiler premium plan shows "a couple of additional buttons" (UI-28), so (I) these two are premium-only: cube = 3D buildings (3D-03), the other = Watermask Mapcomp (IMG-17).
    - The manual-label button is not among the STADIA icons (C-21). (I: it may be hidden without a selection, or added after 2023.)
  - Exact tooltip text is still unknown.
  - C-09 is resolved: pin is the first toolbar button.
  - Src: STADIA; VM:517178231; AESF; EBE
  - [SOURCED layout (STADIA) and narrated button functions/order (EBE); every icon ↔ function mapping INFERRED (order match to STADIA); premium icons INFERRED; tooltips unknown]

- [ ] **UI-18 · Label Template selector dropdown (toolbar centre)**
  - Behavior: shows the current template, e.g. "Null Layer ▾" or "10 Region 2 ▾".
  - "Null Layer" is one of its entries (observed). (I: choosing it adds a null instead of a label, by analogy with the 1.20.0 API addLabel(null); the UI behaviour itself is not documented.)
  - EBE: "these are your existing label templates". From this control you can:
    - filter templates by keyword (LBL-27)
    - swap the template of an already placed label (LBL-12)
    - add a new template (LBL-15)
    - import other existing templates (LBL-16)
    - download templates "from uh the default geolar projects", i.e. the default GEOlayers projects (LBL-17)
  - EBE does not say whether these are dropdown entries or separate buttons. (I: entries inside the dropdown.)
  - The selected template is the one used when you label a map feature (LBL-05; EBE).
  - Next to it: a manual-label button (icon unknown; it shows a crosshair while active, LBL-05) and the **"swap label template"** button (LBL-12; EBE; I: the STADIA ⇄ icon).
  - Src: STADIA; VM:517178231; CL 1.20.0; EBE
  - [SOURCED dropdown + "Null Layer" entry + actions + ⇄ = swap (EBE); Null-Layer behaviour INFERRED; filter/add/import/download placement INFERRED]

- [ ] **UI-19 · Flyout (≡) / context menu**
  - EBE calls it the "GEOlayers 3 panel menu", at the very top of the panel. Entries in order:
    - "general panel actions" (not listed; I: AE's own panel-menu items such as Close Panel / Undock)
    - **Support**: opens the Support dialog (LIC-19). The narrator says "contact support", so Support may be a submenu with a "Contact Support" entry.
    - **Projects** ("just under support"): the Projects dialog (PRJ-01)
    - **Preferences** ("just beneath that"): PRF-20
    - **Help** menu: learn about GEOlayers, links to the creator's tutorials, keyword search (LIC-14; YT:gfXl9-7oZjY "The Help Menu")
  - About & Licensing: the Support dialog shows the licence and version (LIC-02/03/19). (I: there may be no separate About entry.) Troubleshooting tools (CL 1.14.0): location unknown.
  - Src: HELP L23; YT:gfXl9-7oZjY; CL 1.3.0, 1.11.1, 1.14.0; EBE
  - [SOURCED (entries and order: EBE); "general panel actions" and troubleshooting location unknown]

- [ ] **UI-20 · Fullscreen sub-panels and animated UI**
  - Behavior: sub-views (Mapcomp List, filters, search results, settings) take over the whole panel and have a "Back" button. The UI is animated.
  - The Mapcomp filter has been shown as a fullscreen panel since 1.2.6.
  - EBE: the Mapcomp List is left by clicking a Mapcomp row or the header name button; the Find-features-at-coordinate view has a back control ("I'm going to click back"). The List's "Back" button is from STADIA, not EBE.
  - Src: CL 1.1.3, 1.2.6; STADIA; VM:517178231; EBE
  - [SOURCED]

- [ ] **UI-21 · Generic modal dialogs**
  - Built-in modal for editing a flat object (strings/numbers/booleans) and an action-button modal, each with title and message.
  - These are used by the API (API-30/31). (I: the same modals are reused for the missing-label-data prompt LBL-13 and the duration prompt VIEW-11.)
  - Src: HELP L350-364; CL 1.1.5, 1.2.3
  - [SOURCED; reuse INFERRED]

- [ ] **UI-22 · Quick Start Buttons**
  - Behavior: quick-start buttons in the panel (I: shown when the project has no Mapcomp). Their contents are unknown.
  - Src: CL 1.2.0
  - [SOURCED existence]

- [ ] **UI-23 · Known UI strings / errors to reproduce**
  - Strings:
    - "Data not licensed" (LIC-05; thumbnail) [SOURCED]
    - "N Tile(s) couldn't be rendered" (AESF, weak; local-tile-directory case)
    - "You're trying to draw N features with N …" (truncated; ACOM:11865951) [SOURCED]
    - "Get Dimensions Error" (YT:gfXl9-7oZjY chapter; probably an error string; weak)
    - "Invalid MapTiler API Key" (third-party video title YT:N0Lqfk0iJ_M; weak)
    - a descriptive error when a path contains "#" (CL 1.14.1; wording unknown) [SOURCED]
  - Troubleshooting **topics**, not UI strings: "Mapcomp Not Showing", "Mapcomp Not Linking" (YT:gfXl9-7oZjY chapters).
  - [SOURCED per line as tagged]

- [ ] **UI-24 · GitHub Browser panel**
  - A panel "in which you can browse GitHub Repos" (HELP). UI parts: title bar (`title`), user and repo fields (locked when `fixedRepo`), a **"Flatten folders"** checkbox, a filter bar (`filterText`), and an action button (default text **"Import to GEOlayers"**). Items are filtered by `actionFilter` (default: GeoJSON files).
  - It is also the UI behind the GitHub search source (SRC-07/SRC-12; YT:y34htfkMmxc "Connecting the Repository").
  - API: API-32.
  - Src: HELP L366-379; CL 1.2.0, 1.2.2; YT:y34htfkMmxc
  - [SOURCED; exact layout INFERRED]

- [ ] **UI-25 · Header Mapcomp-name button (select / open / return)**
  - The current Mapcomp's name in the header is a button (EBE: "This is actually a button where it gives you the name of your map comp"). Clicking it also closes the Mapcomp List.
  - Click cycle:
    - If the Mapcomp layer is not selected in the containing comp, click 1 selects it. EBE: useful for finding the Mapcomp layer in a stack of "hundreds of layers".
    - The next click opens the Mapcomp itself.
    - The next click returns to the containing comp.
    - If the layer is already selected, the first click opens the Mapcomp.
  - (I: with several containing comps, "the containing comp" is the one the Mapcomp was opened from, or the first one found.)
  - Src: EBE
  - [SOURCED (EBE); multi-containing-comp case INFERRED]

- [ ] **UI-26 · Preferred Mapcomp ("Prefer for …") in the Mapcomp List**
  - Per-Mapcomp settings in the Mapcomp List: **"Prefer for label templates"** and **"Prefer for shape layer styles"**. When the drawing option "Draw on preferred Mapcomp" (DRW-28) is on, the List shows **"Prefer for drawing features"** (EBE; the narration is ambiguous about which control this is).
  - The preferred Mapcomp is where drawn features (and, I, labels) go when that option is on.
  - Exact control form (toggle, pill, menu), placement and whether several Mapcomps can be preferred are unknown. (I: one preferred Mapcomp per kind. Possibly related to the template/style-to-Mapcomp assignment matrix, LBL-18 / SLS-12.)
  - Src: EBE
  - [SOURCED existence and labels (EBE); behaviour details INFERRED]

- [ ] **UI-27 · Imagery on/off per Mapcomp (List row toggle)**
  - The blue imagery icon in each Mapcomp List row toggles that Mapcomp's imagery on and off (EBE: "we have the imagery and the label buttons. So, you can toggle these on and off"). It is the imagery counterpart of the labels toggle (UI-08).
  - (I: off hides or skips the imagery layer(s) in the Mapcomp, e.g. for a labels-only or controller Mapcomp, MCS-21/23.)
  - Src: EBE; STADIA (icon)
  - [SOURCED (EBE); effect in AE INFERRED]

- [ ] **UI-28 · MapTiler-plan-dependent panel buttons**
  - EBE: "if you actually have a premium data subscription plan to Map Tyler, you're going to notice a couple of additional buttons and features within the panel." Without the plan these buttons are hidden.
  - Which buttons is not stated. (I: Watermask Mapcomp, IMG-17, and 3D buildings, 3D-03, the two STADIA toolbar icons EBE does not narrate; maybe also current-view import, FB-12.)
  - Src: EBE; HELP L385-398 (premium feature list)
  - [SOURCED existence (EBE); button list INFERRED]

## 2. Mapcomp creation & settings (MCS)

- [ ] **MCS-01 · Mapcomp concept**
  - An AE composition filled with map imagery and/or geographical labels. It is the basic building block of every project.
  - Src: HELP L10-11
  - [SOURCED]

- [ ] **MCS-02 · Entry points to create a Mapcomp**
  - "New project" in the Projects window (PRJ-01)
  - "+ Add Mapcomp" / "+ Create new Mapcomp" (UI-06)
  - Quick Start buttons (UI-22)
  - Separate creation actions that are sourced: **Watermask Mapcomp** from the toolbar (IMG-17; HELP L393), **"Create 3D Landscape"** (3D-05; AESLEARN chapter "Create 3D Landscape Mapcomp", INDIETIPS weak; toolbar button per EBE) and **Create new Controller Mapcomp** from the toolbar (MCS-28; EBE).
  - (I) "Texture Mapcomp" (IMG-26) and "Choropleth Mapcomp" (DAT-15) are ordinary Mapcomps created with the **Seamless Texture** or **Choropleth** imagery style, not separate entry points. The changelog calls Choropleth an imagery style (CL 1.1.8).
  - Src: HELP L13, L393; YT:v36dD4_lZ4U, YT:Vq6q-X3KVgo, YT:WUhJe_9DQ4Y; CL 1.1.8; EBE
  - [SOURCED for the first three bullets + Watermask + 3D Landscape + Controller; Texture/Choropleth as styles INFERRED]

- [ ] **MCS-03 · Create dialog, step 1 "Composition options and location"**
  - Fields: Name, pixel size (width × height), duration, framerate.
  - These are **prefilled from the currently selected composition** (I: otherwise from the default comp specs pref, PRF-01).
  - The user navigates the preview map to the location; the search bar can jump to a place.
  - Button: **Next**.
  - Src: HELP L17-18; CL 1.1.5
  - [SOURCED; fallback INFERRED]

- [ ] **MCS-04 · Create dialog, step 2 "Map Style"**
  - Choose an imagery preset (thumbnail strip below the preview map, with a "+" tile), a **color preset**, or **change individual colors**.
  - The preview updates live while styles are edited (1.2.1 fix).
  - Choosing a locked MapTiler style opens the "Data not licensed" dialog (LIC-05).
  - Footer buttons: **Cancel** / **Create**.
  - Src: HELP L19-20; CL 1.2.1; VM:479822829 thumbnail
  - [SOURCED]

- [ ] **MCS-05 · What creation produces in AE**
  - The Mapcomp is added as a layer in the currently selected comp, which becomes the **containing comp**.
  - The **anchor layer** sits directly above it.
  - Observed naming:
    - containing comp "containing <Mapcomp>"
    - project folder **"GEOlayers 3 Items"**
    - anchor "<Mapcomp> Anchor", parented to the Mapcomp layer
  - Observed containing-comp renderer: Classic 3D. (Classic 3D is the AE default; whether GEOlayers sets it is INFERRED/unknown.)
  - Src: HELP L21; STADIA geolayers-main.png
  - [SOURCED; rule "containing <name>" created when starting from scratch is INFERRED]

- [ ] **MCS-06 · Mapcomp anchor layer**
  - A helper layer in the containing comp that matches the Mapcomp's view animation (position/scale/rotation) so other layers can be parented to it.
  - Anchor layers can themselves be pinned (1.0.1).
  - Anchor source items have naming rules (1.0.1).
  - Users call it the "map anchor".
  - API: getMapcompAnchor().
  - (I: it is an expression-driven 3D null; there is no camera, i.e. "pseudo 3D with an anchor null".)
  - Src: HELP L21, L90-94; CL 1.0.1; ACOM:14019807, ACOM:14719327
  - [SOURCED; mechanism INFERRED]

- [ ] **MCS-07 · MapPivot layer**
  - An internal layer named "MapPivot" that carries expressions (1.1.8 bugfix "empty expressions on MapPivot layer").
  - (I: it is the pivot that applies pitch (X rotation) and bearing (Z rotation) about the comp centre.)
  - Src: CL 1.1.8; n/arch §5.2
  - [SOURCED name; role INFERRED]

- [ ] **MCS-08 · Automatic Mapcomp naming**
  - New Mapcomps are named "Mapcomp 01", "Mapcomp 02", … with automatic leading zeros (1.3.0).
  - Names are kept unique.
  - A pre-1.3.0 thumbnail shows "Mapcomp 1" (C-02).
  - Src: CL 1.3.0; VM:517178231
  - [SOURCED]

- [ ] **MCS-09 · Mapcomp Settings dialog**
  - Opened from the List hover "Settings" button. Title "Mapcomp Settings".
  - Fields: MCS-10 … MCS-19, plus a **link-view** control that links this Mapcomp's view to a controller (MCS-29; EBE).
  - Buttons: **Cancel** / **Apply**. Apply re-renders the Mapcomp.
  - Src: STADIA geolayers-mapcomp-settings.png / -add-new.png / -apply.png; EBE
  - [SOURCED]

- [ ] **MCS-10 · Settings: Name**
  - Text field; renames the Mapcomp (I: and updates expressions).
  - Src: STADIA
  - [SOURCED]

- [ ] **MCS-11 · Settings: Duration**
  - Example "30 seconds". Changing it also changes the durations of related (linked) Mapcomps.
  - Src: STADIA; CL 1.2.4
  - [SOURCED]

- [ ] **MCS-12 · Settings: Language**
  - Dropdown, e.g. "English - United States".
  - Is the default language for geocode() (HELP L149). (I: also drives the Mapcomp labels' language.)
  - Full list unknown (see MLB-03).
  - Src: STADIA; HELP L149
  - [SOURCED field + geocode default; label-language role INFERRED]

- [ ] **MCS-13 · Settings: Max Zoom / Min Zoom**
  - Sliders. Observed values: Max **16**, Min **0**.
  - These limit imagery zoom (I: the imagery tile zoom; beyond Max Zoom tiles are over-zoomed).
  - Tutorial topics "How to Override Zoom Level" and "Zoom level limitation".
  - Src: STADIA; CL 1.1.2, 1.4.0; YT:KM3lFJEa3JA
  - [SOURCED; exact semantics INFERRED]

- [ ] **MCS-14 · Settings: Attribution**
  - A position dropdown (observed **"bottom left"** in the settings screenshot), a checkbox (I: show/hide attribution), and an ⓘ icon. The ⓘ is named an **"attribution info"** button that links to each provider's terms of use (AESF discussion/2017, weak).
  - The rendered "Provider Attribution":
    - Vimeo thumbnails: italic text, bottom-left.
    - Official HELP image (MapTiler era): uppercase "© MAPTILER © OPENSTREETMAP CONTRIBUTORS" at the **bottom-right**.
    - Position and style therefore vary (C-19). (I: it is an AE text layer, suggested by the CL 1.6.1 bugfix "Provider Attribution aligned wrong if Paragraph set to Right-To-Left".)
  - Other position values are unknown (I: the four corners).
  - Src: STADIA; AESF (weak); CL 1.6.1; VM thumbnails 352731294 etc.; HELP README image user-images…/233547961-62f27e76….png; MC "Where to Find License Information"
  - [SOURCED field + observed looks; button name weak; text-layer form and options INFERRED]

- [ ] **MCS-15 · Settings: Imagery section**
  - A picture-icon header, a crossed-tools icon (unknown function), and a grid of imagery-style thumbnails.
  - Badges on thumbnails:
    - MapTiler icon/badge at top-right (subscription styles)
    - red "!" when a MapTiler key is missing
    - loading spinners
  - A **"+"** tile creates a new imagery style (opens IMG-18).
  - Src: STADIA; HELP L390; MTD; AESF
  - [SOURCED]

- [ ] **MCS-16 · Settings: Quality**
  - Slider, observed **80%**.
  - (I: sets imagery resolution / tile-zoom offset. Forum (weak): "finalize with a higher Quality … this loads other zoom levels".)
  - (I: the default value comes from the Default Imagery Quality pref, PRF-12.)
  - Src: STADIA; AESF (weak); CL 1.15.1
  - [SOURCED slider; semantics INFERRED; forum advice weak]

- [ ] **MCS-17 · Settings: Blend Duration**
  - Slider, observed **0.3 s**. (I: the cross-fade time between imagery of different zoom levels/tiles during animation.)
  - Src: STADIA
  - [SOURCED existence; meaning INFERRED]

- [ ] **MCS-18 · Settings: Imagery Motion Blur**
  - Toggle. Adds built-in motion blur to map imagery. GEOlayers has "built-in support for adding motion blur to map and label layers" (YT:XKd_cNsDF-g). (I: whether this single toggle also covers labels, or labels have a separate control, is unknown.) Detailed parameters are unknown.
  - Src: STADIA; CL 1.0.7; VM:520372999; YT:XKd_cNsDF-g
  - [SOURCED; toggle-covers-labels INFERRED]

- [ ] **MCS-19 · Mapcomp Label settings**
  - A Mapcomp-level label settings area containing a **button to create Label Templates** (1.1.2).
  - MC lesson "Mapcomp Label Templates".
  - (I: it converts or derives template labels from Mapcomp labels.)
  - Location in the UI is unknown.
  - Src: CL 1.1.2; MC
  - [SOURCED; behavior INFERRED]

- [ ] **MCS-20 · Pitch limit depends on aspect ratio**
  - Square and portrait Mapcomps can be pitched more than landscape ones. The formula is unknown.
  - Src: CL 1.0.4
  - [SOURCED]

- [ ] **MCS-21 · Linked / Controller Mapcomps**
  - One controller Mapcomp drives the view of others. Example: separate "Main Map" (imagery) / "Main Labels" / "Overview Map" Mapcomps, the "Multi-Mapcomp Workflow".
  - Linked Mapcomps:
    - share the view (I: through expressions)
    - share duration changes
    - collapse together in the List
    - can be removed together
  - User report (weak, unresolved): blend modes inside controller comps did not blend into the master map with Collapse Transformations on. (I: AE 3D render-group behaviour.)
  - EBE:
    - A **Controller Mapcomp** is "essentially just a map comp without imagery or labels", used to keyframe the view that other Mapcomps follow.
    - It is created with a toolbar button (MCS-28): a new composition named "controller" that also appears in the Mapcomp List.
    - A Mapcomp is linked to it from that Mapcomp's Settings dialog ("I can link the view right here to my controller", MCS-29). The linked Mapcomp then follows the controller's view.
    - The footer Keyframe button on a linked Mapcomp writes the keys to the controller (UI-12, VIEW-02).
    - Workflow: keyframe the controller, then select the linked Mapcomp as the target to draw features on.
  - How a link is broken is not shown. (I: set the link-view control back to none.)
  - Src: MC "Controller Mapcomps"; VM:357897696 "Link Mapcomps"; YT:9TYySccpjdA; YT:2Ga-6DD2epo; VM:466519449; CL 1.2.4, 1.3.0, 1.5.0; AESF comment/41477 (weak); EBE
  - [SOURCED; creation and link UI SOURCED (EBE); expression mechanism and unlinking INFERRED]

- [ ] **MCS-22 · Duplicate Mapcomp**
  - From the List (UI-05). The dialog has **at least two options**; a forum user picked the "second option", which is (I: duplicate Mapcomp only / duplicate with containing comp).
  - It also has a **Link** toggle that creates a linked duplicate. Example use: an imagery-only duplicate trimmed in time. (EBE links Mapcomps through Settings, MCS-29, and does not show the Duplicate dialog, so this toggle remains weak.)
  - Updates all related expressions.
  - API: duplicate(comp, {newName | newContainingCompName}).
  - Src: AESF comment/41477, /24493 (weak); HELP L51-57; MC "Duplicate Mapcomps"; YT:4kWcuolF16Q; EBE
  - [SOURCED API + existence; dialog options weak; option names INFERRED]

- [ ] **MCS-23 · Multi-Mapcomp layer split**
  - "Split your Map into different Layers like Labels and Imagery". Separate Mapcomps hold imagery vs labels and are linked.
  - Src: VM:357897696
  - [SOURCED]

- [ ] **MCS-24 · Expression-rig robustness**
  - The Mapcomp/anchor/labels/pinned-layer expressions must:
    - work in both expression engines (legacy ExtendScript and JavaScript)
    - work with the AE pref "Default Position Properties to Separate Dimensions"
    - not error at large zoom
    - not break with odd Zoom keyframe interpolation
  - Src: CL 1.1.8, 1.2.0, 1.2.4, 1.5.0
  - [SOURCED]

- [ ] **MCS-25 · Overlay layers are 3D layers on one Z plane**
  - Layers placed by GEOlayers are 3D. Equal Z values can z-fight. The user fix is a 1 px Z offset.
  - Src: ACOM:42201
  - [SOURCED]

- [ ] **MCS-26 · Corrupted-project troubleshooting**
  - A tool that repairs projects corrupted by AE "Recovering" Mapcomps (1.14.0).
  - Related field report: imagery is file-backed, so a disconnected cache drive gave "MapComp could no longer be found". (I: relink imagery on open.) Storage and moving projects: FIN-18.
  - Src: CL 1.14.0; ACOM:14880531
  - [SOURCED; relink INFERRED]

- [ ] **MCS-27 · Minimum and over-zoom safety**
  - Mapcomps zoomed below the imagery's minzoom must not load forever (1.2.4).
  - Previews with min zoom must not be empty or blocked (1.4.0).
  - Src: CL 1.2.4, 1.4.0
  - [SOURCED]

- [ ] **MCS-28 · "Create new Controller Mapcomp" (toolbar)**
  - A toolbar button (the second narrated, right after Pin; UI-17). It adds a new composition named **"controller"**, which appears in the Mapcomp List (EBE: "it's going to add a new composition here called controller").
  - The controller is a Mapcomp without imagery or labels (MCS-21).
  - Unknown: the size, duration and initial view of the new comp, whether it gets a containing comp, and the naming of further controllers. (I: specs and view copied from the selected Mapcomp; further ones named "controller 2"… by the MCS-08 uniqueness rule.)
  - Src: EBE
  - [SOURCED (EBE); comp specs and naming INFERRED]

- [ ] **MCS-29 · Settings: link view to a controller**
  - A control in the Mapcomp Settings dialog (MCS-09) that links this Mapcomp's view to a Controller Mapcomp. Once linked, the Mapcomp follows the controller's view (VIEW-14). EBE: "if I go to my world map comp settings, I can link the view right here to my controller."
  - Its label, its control type (I: a dropdown of Mapcomps plus "none") and how unlinking works are unknown.
  - Src: EBE
  - [SOURCED existence and effect (EBE); control form INFERRED]

## 3. View animation (VIEW)

- [ ] **VIEW-01 · View controls Latitude / Longitude / Zoom / Bearing / Pitch**
  - Five keyframeable float controls per Mapcomp, readable by user expressions (e.g. triggering animations from Zoom).
  - Which layer they live on, and whether they are separate Expression Controls or a pseudo effect, is unknown.
  - Src: HELP L59-68, L81-85; YT:-yfNEqew-a4
  - [SOURCED; location unknown]

- [ ] **VIEW-02 · Animate by navigating the preview**
  - Getting-started flow: "Animate your map" by keyframing the view.
  - EBE:
    - The footer **Keyframe** button (UI-12) adds keys for all five controls (Latitude, Longitude, Zoom, Bearing, Pitch) of the selected Mapcomp at the current time. For a linked Mapcomp the keys go on its controller (MCS-21).
    - After that, with "Link Mapcomp View" on (UI-13), moving the playhead and then the preview adds keys for the parameters that changed.
  - Manual keyframing in AE + Graph Editor fine-tuning is also used (MC).
  - Src: VM:352731294; MC "Animate the Mapcomp View", "Fine Tune Motion with the Graph Editor"; EBE
  - [SOURCED (write-back: EBE)]

- [ ] **VIEW-03 · Set view at time (API)**
  - setViewAtTime(view {latitude, longitude, zoom, bearing, pitch}, forceKeyframe, time).
  - (I) If the property is not yet keyframed and forceKeyframe is false, the value is set statically.
  - (I) If the property already has keys, a key is added at `time`.
  - HELP only says forceKeyframe keys a control "even if it is not yet keyframed".
  - `time` is in containing-comp time and defaults to the current time.
  - (I) The footer Keyframe button (UI-12, VIEW-02) is the UI equivalent of setViewAtTime with forceKeyframe = true at the current time.
  - Src: HELP L59-68
  - [SOURCED]

- [ ] **VIEW-04 · Fit view to bbox**
  - fitViewAtTime(bbox [lonMin, latMin, lonMax, latMax] WGS84).
  - UI (EBE): the first quick action on a search result ("quickly fit the map view") and the first feature action, **"Fit feature to view"** (FB-46). The latter opens a menu: its plain "fit view to feature" entry fits without animating; entries with a keyframe symbol animate (VIEW-05…08).
  - (I) The dashed-frame icon is this action (icon meaning from a thumbnail).
  - Src: HELP L70-79; VM:517178231; EBE
  - [SOURCED API and UI actions (EBE); icon and API equivalence INFERRED]

- [ ] **VIEW-05 · Animate View to Feature**
  - One click keyframes the view from the current state to fit the selected feature, with proper keyframe easing (1.2.5 fix).
  - Location (EBE): an entry with a keyframe symbol in the "Fit feature to view" menu of the feature action bar (FB-46). EBE does not read out the entry names (Q-51).
  - Src: CL 0.9.0, 1.2.5; YT:g4zTDhlMYzg; YT:3OU3eQsudeM; EBE
  - [SOURCED; menu location SOURCED (EBE); entry name INFERRED]

- [ ] **VIEW-06 · Animate View Along Features**
  - Animates the view along one or **multiple selected** features (lines, and sequences of features).
  - The view follows a **smoothed** version of the feature path "so it doesn't wiggle too much" (AESF, weak; page 404). Whether a smoothing parameter is exposed is unknown.
  - Imported GPX routes can "adapt the view according to that route with one click" (AESF, weak), i.e. this action works on imported tracks (DAT-05).
  - (I) It is one of the keyframe-symbol entries in the "Fit feature to view" menu (FB-46; EBE shows the menu but does not name its entries).
  - Src: CL 1.0.9, 1.1.0; YT:g4zTDhlMYzg; AESF (weak); EBE
  - [SOURCED; smoothing + GPX one-click weak]

- [ ] **VIEW-07 · Animate View Between Features**
  - Automatically animates the view from one feature to the next.
  - (I) A keyframe-symbol entry in the "Fit feature to view" menu (FB-46; EBE).
  - Src: VM:359745657 ("automatically animate mapcomp views along and between features"); YT:g4zTDhlMYzg; EBE
  - [SOURCED]

- [ ] **VIEW-08 · One-click flyover animations**
  - "One-click flyover animations" together with Mapcomp View features.
  - "Never-Ending Flyover / Revolution / Ping Pong" are tutorial moves; (I: these are AE techniques such as loopOut, not separate buttons).
  - EBE calls the keyframe-symbol entries of the "Fit feature to view" menu "automation techniques" that "automate an animation" (FB-46). (I: the one-click flyovers live there.)
  - Src: VM:528729028; YT:fZ17qbEFvko; YT:g4zTDhlMYzg; EBE
  - [SOURCED; button names unknown]

- [ ] **VIEW-09 · Mapcomp View features**
  - "Create a Mapcomp View" stores a Mapcomp's view as a feature. You can manually animate to Mapcomp View features.
  - UI (EBE): the Browser "+" menu entry **"Mapcomp view"** (FB-02). It asks for a name (example "world view"), confirms with **Apply**, and adds a new map feature to the Browser. The feature stores the whole view, including pitch and bearing; several can be saved ("world view 1", "world view 2").
  - (I: FB-43 "Add Current View Feature" may be this same action under an older name.)
  - The "Draw Mapcomp View" script (TB-12) draws one Mapcomp's view rectangle on another Mapcomp.
  - Src: VM:528729028; YT:TXjU-WVPwa4; YT:g4zTDhlMYzg; CL 1.1.6; EBE
  - [SOURCED; creation UI SOURCED (EBE); geometry/property model INFERRED]

- [ ] **VIEW-10 · Animate View Along a Custom Feature Path**
  - Make a feature from a drawn layer (FB-30), then animate the view along it.
  - EBE workflow: Create a Shape Layer to Draw On (DRW-22), draw a path (stroke on), then "+" > Feature from layer.
  - Src: YT:g4zTDhlMYzg; YT:M_iUcgEILw8; EBE
  - [SOURCED]

- [ ] **VIEW-11 · Duration prompt**
  - Whenever GEOlayers creates an animation, it prompts the user to adjust its duration.
  - Src: CL 1.2.3
  - [SOURCED]

- [ ] **VIEW-12 · Remove view keyframes**
  - Removes all keys from the 5 controls (API removeMapcompControlKeys).
  - (I: the UI equivalent is unknown.)
  - Src: HELP L81-85
  - [SOURCED]

- [ ] **VIEW-13 · Zoom interpolation safety**
  - Odd Zoom keyframe interpolation (bezier overshoot) must not cause expression errors.
  - (I: clamp Zoom; use a flyTo-like path for big moves.)
  - Src: CL 1.2.4; n/arch §5.2
  - [SOURCED; fix INFERRED]

- [ ] **VIEW-14 · Linked view propagation**
  - Linked/controller Mapcomps follow the controller's view (MCS-21).
  - EBE: the link is set in the linked Mapcomp's Settings (MCS-29); zooming the controller makes the linked Mapcomp follow.
  - Src: MCS-21 sources; EBE
  - [SOURCED]

- [ ] **VIEW-15 · Imagery motion blur during view moves**
  - See MCS-18.
  - [SOURCED]

## 4. Imagery styles & map styling (IMG)

- [ ] **IMG-01 · Imagery style library**
  - Imagery styles (vector-rendered or raster) are saved into a **style library**, then applied per Mapcomp (MCS-15).
  - Multiple custom styles can be **imported/exported in Preferences** (file format unknown).
  - Src: STADIA; CL 1.2.2
  - [SOURCED]

- [ ] **IMG-02 · Locked-style indicators**
  - Styles that need a MapTiler subscription show a **badge at top-right** of the thumbnail (the "MapTiler icon").
  - Unlicensed or missing-key styles show a **red exclamation mark**. Clicking one prompts for the API key.
  - The badge originally marked GEOlayers Mapdata styles (2019–20).
  - Src: HELP L390; AESF discussion/2017 (thread id; Mapdata era 2019–20, weak: "little Badge top right"); MTD
  - [SOURCED; badge wording weak]

- [ ] **IMG-03 · Basic (MapTiler imagery style)**
  - Stylized map. (I: vector-rendered.)
  - Separate water visibility toggles for **oceans, lakes, rivers** (1.1.8).
  - **Disputed borders** on/off (1.7.0).
  - Needs a MapTiler key (Flex plan per vendor).
  - (I: OpenMapTiles schema.)
  - Src: CL 1.1.8, 1.7.0; HELP L390
  - [SOURCED]

- [ ] **IMG-04 · Advanced (MapTiler imagery style)**
  - Added 1.0.4 under GEOlayers Mapdata; MapTiler-backed since 1.1.9. CL 1.7.0 calls Basic, Advanced and Lite "MapTiler Imagery Styles". (I: vector-rendered.)
  - Water toggles for oceans/lakes/rivers.
  - Extra **rail** color (1.1.6).
  - **Street density** setting (value 5 is valid; range unknown).
  - Disputed borders toggle.
  - Src: CL 1.0.4, 1.1.6, 1.1.8, 1.2.6, 1.7.0
  - [SOURCED]

- [ ] **IMG-05 · Lite (MapTiler)**
  - Free for non-commercial projects (1.4.0). Disputed borders toggle.
  - Src: CL 1.4.0, 1.7.0
  - [SOURCED]

- [ ] **IMG-06 · MapTiler Satellite (V2)**
  - Satellite imagery style. V2 since 1.5.1; a blur fix shipped in 1.2.5.
  - Src: CL 1.2.5, 1.5.1
  - [SOURCED]

- [ ] **IMG-07 · Import from MapTiler Cloud**
  - Import and update maps from cloud.maptiler.com: the user's custom maps plus the standard maps (Basic, Streets, Outdoor, Satellite Hybrid, Toner, Topo, Voyager…, plus country maps).
  - Src: VM:654002705; MTD
  - [SOURCED]

- [ ] **IMG-08 · Mapstyling inside AE**
  - Applies to MapTiler vector styles:
    - adjust colors
    - **use Adobe Swatch files (.ase)**
    - change **fonts** and **language**
    - adjust line widths
    - add **hillshading**
    - show/hide **groups of geographic features**
  - Requires a MapTiler Cloud plan.
  - The exact editor layout and group names are unknown.
  - Src: VM:357897696; AES; editingcorp/ifmlearn (n/community §9)
  - [SOURCED; UI unknown]

- [ ] **IMG-09 · Generate a style from a composition's colors or from an image**
  - Product copy. The UI location is unknown.
  - Src: AES; editingcorp
  - [SOURCED; UI unknown]

- [ ] **IMG-10 · Color presets and individual colors**
  - Available in the create dialog (MCS-04) and after creation.
  - Src: HELP L20
  - [SOURCED]

- [ ] **IMG-11 · Disputed borders toggle**
  - On Basic, Advanced and Lite. Related "Disputed Area Dataset" (FB-14).
  - Src: CL 1.7.0; MC
  - [SOURCED]

- [ ] **IMG-12 · Bing imagery**
  - Aerial, plus **Dark, Light, Gray** variants (1.1.4), with language support.
  - Not metered by MapTiler.
  - Exact UI names (e.g. "Bing Aerial") and "not metered by MapTiler" come from a forum post (weak). Road/AerialWithLabels variants are unknown.
  - Src: CL 1.1.4; AESF (weak)
  - [SOURCED variants; names + metering weak]

- [ ] **IMG-13 · CartoDB imagery**
  - CARTO basemap imagery. An **API key** can be entered (1.19.0).
  - Which CARTO styles are exposed (Positron, Dark Matter, Voyager…) and where the key is entered are unknown.
  - Src: CL 1.19.0; AESF
  - [SOURCED]

- [ ] **IMG-14 · Stamen style (REMOVED 1.6.4)**
  - Removed after Stamen hosting moved to Stadia. Migration: add a Stadia URL as custom imagery (IMG-18).
  - Parity: do not ship Stamen as built-in. Document the migration.
  - Src: CL 1.6.4; VM:872558334; STADIA
  - [SOURCED]

- [ ] **IMG-15 · Natural Earth Imagery**
  - Imagery rendered from public-domain Natural Earth data. It is the no-subscription world-scale basemap.
  - Data was updated in 1.6.6.
  - Src: CL 1.1.9, 1.6.6; YT:KM3lFJEa3JA
  - [SOURCED]

- [ ] **IMG-16 · Natural Earth Admin Boundaries Imagery**
  - Added 1.2.0. (I: the 1.7.2 fix "Natural Earth Inner Boundaries for some countries are messed up" may also affect this imagery; its wording matches FB-11, where it is now cited.)
  - Src: CL 1.2.0; (CL 1.7.2 link INFERRED)
  - [SOURCED existence; 1.7.2 link INFERRED]

- [ ] **IMG-17 · Watermask Mapcomps**
  - A land/water mask Mapcomp, used for detailed coastlines and mattes.
  - Variants:
    - **MapTiler Watermask from the toolbar** (premium)
    - **Natural Earth Water Mask** imagery (free)
  - Tutorial: "How to Make a Watermask Mapcomp".
  - EBE (no MapTiler premium plan) narrates no Watermask button. (I: the toolbar button is shown only with a premium plan, UI-28.)
  - Src: HELP L393; CL 1.1.9; YT:LEaMUpjupG0; YT:KM3lFJEa3JA; EBE
  - [SOURCED]

- [ ] **IMG-18 · "Mapcomp Imagery" editor (custom raster style)**
  - Title "Mapcomp Imagery". Header icons: **save, import, export**. Live preview with zoom slider and compass.
  - Fields:
  - Values below are **observed in the new-source screenshot** of a video whose steps include editing the URI (I: likely, not proven, factory defaults; C-12 stays open):
    - **Name**: "Universal Raster"
    - **Raster Source** group:
      - **URI**: `https://a.tile.openstreetmap.org/{z}/{x}/{y}.png`
      - a **"…"** picker (I: local tile directory, IMG-23)
      - a **scheme dropdown "xyz"** (I: also "tms")
    - **Min Zoom** 0, **Max Zoom** 16
    - **Tile Size** 512 px
    - **Bounds**: -180°, -85.051129°, 180°, 85.051129°
    - Adjustment sliders: **Hue** 0, **Opacity** 1, **Black** 0, and more below the fold (unknown)
  - Buttons: Cancel / Apply. Apply saves to the library; Apply again in Mapcomp Settings assigns the style and re-renders.
  - Src: STADIA geolayers-mapcomp-url.png, -api-key.png, -apply.png
  - [SOURCED fields + observed values; "default" status INFERRED]

- [ ] **IMG-19 · Universal Raster Style ("Default Tiles")**
  - A built-in raster profile with default tiles (1.6.6 fix). It is covered in the tutorial "The Free Basemap in GEOlayers" alongside Natural Earth, the Watermask Mapcomp and OSM. (I: whether "the free basemap" means this profile, Natural Earth, or the set as a whole is unknown.)
  - Src: CL 1.6.6; YT:KM3lFJEa3JA
  - [SOURCED existence; "the free basemap profile" INFERRED]

- [ ] **IMG-20 · Tile URI template variables**
  - `{z}`, `{x}`, `{y}`; date variables **`{year}`, `{month}`, `{date}`, `{dateISO}`** (1.4.0).
  - Literal `@2x` and query-string keys (`?api_key=`, `?access_token=`) pass through unchanged.
  - The only GL3-confirmed coordinate syntax is lowercase `{z}/{x}/{y}` (STADIA).
  - GL2 legacy note (unverified in GL3): a 2017 forum post maps GL2's `{{ZoomLevel}}`, `{{pX}}`, `{{pY}}` to `{z}`, `{X}`, `{Y}`. Whether GL3 accepts uppercase `{X}/{Y}` is unknown; accepting them is optional (I).
  - `{s}`, `{q}`/quadkey and `{-y}` support is unknown. (I: implement them.)
  - What `{date}` means (day of month vs full date) is unknown.
  - Src: CL 1.4.0; STADIA; AESF p10 [GL2-sourced, weak]
  - [SOURCED; extras and GL2 mapping INFERRED]

- [ ] **IMG-21 · XYZ / TMS schemes**
  - The tilemerger supports TMS (y flipped).
  - Src: CL 1.0.9; STADIA "xyz ▾"
  - [SOURCED]

- [ ] **IMG-22 · Map Bounds, Min/Max zoom per raster style, overzoom**
  - Map Bounds limit tile requests to an extent (1.4.0).
  - Above the style's max zoom, overzoomed textures are used, not lower-zoom tiles (1.1.2).
  - Src: CL 1.1.2, 1.4.0
  - [SOURCED]

- [ ] **IMG-23 · Local tile directories**
  - Pick a local `{z}/{x}/{y}` folder in the imagery setup (1.1.8). Min/max zoom are read from **metadata.json** (1.5.2).
  - Typical source: GeoTIFF (e.g. NASA Visible Earth) tiled with MapTiler Desktop/Engine or QGIS.
  - Src: CL 1.1.8, 1.5.2; VM:519954560; YT:gxtlqJodDkI; AESF
  - [SOURCED]

- [ ] **IMG-24 · Georeferenced imagery workflow**
  - Reproject a GeoTIFF into tiles and use them as Mapcomp imagery.
  - Src: VM:519954560; YT:gxtlqJodDkI
  - [SOURCED]

- [ ] **IMG-25 · Mapbox Studio styles**
  - Used through a raster tile URL with a token in the custom raster style.
  - (I: e.g. `api.mapbox.com/styles/v1/{user}/{style}/tiles/256/{z}/{x}/{y}@2x?access_token=`.)
  - Src: VM:367072536; YT:ns-pyGCcJt4; MC "How to Work With Mapbox"
  - [SOURCED; URL INFERRED]

- [ ] **IMG-26 · "Seamless Texture" image profile / Texture Mapcomp**
  - A built-in image profile that builds a tileset from the user's texture.
  - Flow: Create new Texture Mapcomp → choose Seamless Texture profile → Finalize → set blend mode.
  - Src: YT:v36dD4_lZ4U
  - [SOURCED]

- [ ] **IMG-27 · URL-sourced style update**
  - "Styles based on a url source can be updated with one click" (1.4.2). It is unclear whether this means imagery styles, shape layer styles or both (C-07).
  - Src: CL 1.4.2
  - [SOURCED]

- [ ] **IMG-28 · Import GEOlayers 2 server profiles as imagery styles**
  - Src: AES; editingcorp
  - [SOURCED]

- [ ] **IMG-29 · Imagery thumbnail grid (observed contents)**
  - About 30 tiles: transparent/checkerboard, satellite, Natural Earth, dark and light basemaps, black/white water masks, dotted (dataviz), purple, "!" error tiles.
  - The full name list is unknown.
  - Src: STADIA
  - [SOURCED]

- [ ] **IMG-30 · Dataviz imagery styles**
  - Choropleth, Circles, Heatmap (?), Lines. See DAT-15…18.
  - [SOURCED]

- [ ] **IMG-31 · Elevation / DEM tiles**
  - Fetched for 3D landscapes (3D-08).
  - Src: CL 1.2.5
  - [SOURCED]

- [ ] **IMG-32 · Map projection**
  - Web Mercator only. Globe and Equal Earth arrive "via Plugin" in the 1.20 prerelease (3D-10).
  - Src: AESF comment/9997; CL 1.20.0
  - [SOURCED]

- [ ] **IMG-33 · WMS imagery (decision needed)**
  - GEOlayers 2 could connect to Web Map Services (Vimeo 241651263 "GEOlayers 2 Tutorial - 09.3 WMS"). v3 imports GL2 server profiles as imagery styles (IMG-28). Whether v3 supports WMS, or what happens when a WMS GL2 profile is imported, is unknown.
  - Src: vimeo.com/api/v2/geolayers/videos.json?page=3 (VM:241651263); AES
  - [SOURCED for GL2; GL3 support unknown]

## 5. Finalize & tile rendering / caching (FIN)

- [ ] **FIN-01 · Finalize**
  - Renders the final, full-quality map imagery into the Mapcomp(s).
  - (I) Before finalizing, the Mapcomp shows preview-quality imagery. (ACOM:15438647 only says things looked fine until Finalize; the "Preview" pill's meaning is unknown, UI-09.)
  - Triggered by the header **"✓ Finalize"** button (current Mapcomp; next to the name button), the List ✓ row button (EBE: "you can finalize it"), or the API. Modifier behaviour: FIN-02.
  - Src: STADIA; HELP L42-49; ACOM:15438647; EBE
  - [SOURCED; preview-before-finalize INFERRED]

- [ ] **FIN-02 · Finalize options**
  - `onlyCurrentFrame` = "Finalize One Frame" tip
  - `previewQuality`
  - `onlyWorkArea`
  - `purgeImageryCache`
  - All are booleans; defaults unknown (I: false).
  - UI (EBE), header Finalize button:
    - plain click (no modifier): finalizes "your entire composition" / "the entire timeline"
    - **CTRL/CMD-click**: finalizes "just the … existing frame" (= `onlyCurrentFrame`)
  - EBE shows only these two cases; how `previewQuality`, `onlyWorkArea` and `purgeImageryCache` are exposed is not shown (modifier keys, menu or Preferences all possible). (I: further modifier keys such as SHIFT/ALT, following the panel's modifier pattern in FB-03/04/05 and TB-01.)
  - Src: HELP L47; YT:0SoBCtbF3B0; EBE
  - [SOURCED; onlyCurrentFrame = CTRL/CMD-click SOURCED (EBE); other options' UI unknown]

- [ ] **FIN-03 · "3 Ways to Finalize Imagery"**
  - Course lesson; content unknown. EBE shows two ways: plain click = whole composition, CTRL/CMD-click = current frame (FIN-02). (I: the third is work area only, or finalizing from the List row.)
  - GEOlayers 2 had "Finalize All". The v3 API finalizes all Mapcomps when `comps` is undefined.
  - Src: MC; METTLE GL2; HELP L45; EBE
  - [SOURCED existence; two ways SOURCED (EBE); third INFERRED]

- [ ] **FIN-04 · Work area matters**
  - In GL2, finalize covered only the work area. In GL3, work-area-only is the `onlyWorkArea` option (HELP L47).
  - EBE: plain click finalizes the whole comp (narrator wording "your entire composition" / "the entire timeline"; work area not tested or mentioned). (I, weak) The GL3 plain-click default is therefore probably **not** work-area-only; confirm in AE with a short work area. The UI form of `onlyWorkArea` is unknown (FIN-02).
  - GL2 folklore troubleshooting tip: black or missing areas → check the work area.
  - Src: aescripts FAQ snippet (entry still headed GEOlayers 2); AESF discussion/950 p10 [GL2]; HELP L47; EBE
  - [GL2-sourced; GL3 plain click = whole comp: weak (EBE narrator wording, work area not tested), INFERRED not work-area-only; work-area UI unknown]

- [ ] **FIN-05 · Quality → tile zoom**
  - The Quality setting (MCS-16) and Default Imagery Quality (PRF-12) change which tile zoom levels are loaded.
  - Src: AESF; CL 1.15.1
  - [SOURCED weak; mechanism INFERRED]

- [ ] **FIN-06 · Queue renders after finalization**
  - Option to add comps to the AE Render Queue directly after finalizing.
  - Src: CL 1.2.6
  - [SOURCED]

- [ ] **FIN-07 · Preview settings for real-time work**
  - Preview quality can be lowered to work in real time.
  - Src: YT:0SoBCtbF3B0
  - [SOURCED; UI unknown]

- [ ] **FIN-08 · Imagery cache and purge**
  - An imagery cache exists and is purgeable via the `purgeImageryCache` finalize option [SOURCED HELP L47]. (I: it is on disk.)
  - GL2/weak note: users purged "tiles, the GEOlayers cache, and the AE cache" (AESF discussion/950 p10, [GL2]).
  - Src: HELP L47; AESF p10 (GL2, weak)
  - [SOURCED option; rest INFERRED/GL2]

- [ ] **FIN-09 · Tile rendering in Web Workers**
  - Memory leaks in this rendering were fixed in 1.1.8.
  - Src: CL 1.1.8
  - [SOURCED]

- [ ] **FIN-10 · Tilemerger**
  - Merges raster tiles. Supports XYZ and TMS, and wraps tile x for longitudes < -180.
  - Src: CL 1.0.4, 1.0.9
  - [SOURCED]

- [ ] **FIN-11 · Missing / rejected tiles**
  - Rejected requests must not leave black holes (1.4.0) [SOURCED].
  - (I) Missing tiles fall back to a lower zoom (parent tile cropped and upscaled). A forum snippet says "GEOlayers will use a lower zoom level to fix this gap" (weak), but CL 1.1.2 says over-zoomed imagery must use over-zoomed textures rather than lower-zoom tiles, which works against a general fallback.
  - Error string (weak, forum, local-tile-directory case): "N Tile(s) couldn't be rendered".
  - Src: CL 1.4.0, 1.1.2; AESF (weak)
  - [SOURCED no-black-holes; fallback INFERRED; string weak]

- [ ] **FIN-12 · Texture filtering and overzoom quality**
  - Imagery must not be blurry: texture filtering fix; overzoomed textures (1.1.2); Satellite blur fix (1.2.5).
  - Src: CL 1.1.2, 1.2.5
  - [SOURCED]

- [ ] **FIN-13 · DEM / server robustness**
  - DEM tile requests with no valid response must not stall finalization. General stall fixes in 1.0.1.
  - Src: CL 1.0.1, 1.2.5
  - [SOURCED]

- [ ] **FIN-14 · Imagery stored as PNG files imported as footage**
  - AE 25/26 PNGIO errors appear when GEOlayers tiles load. (I: write PNGs atomically and validate them; store them on a stable path.)
  - Src: ACOM bug-report 1552832; ACOM:14880531
  - [INFERRED]

- [ ] **FIN-15 · Async finalize with callback**
  - Errors and finalized comps are returned to the API callback.
  - Src: HELP L49
  - [SOURCED]

- [ ] **FIN-16 · Known failure modes to avoid**
  - Map portions disappearing on finalize.
  - "Big empty square" artifacts.
  - Src: ACOM:15438647; AESF comment/39509
  - [SOURCED]

- [ ] **FIN-17 · Faster finalization than v2**
  - Marketing claim. GEOlayers handles all imagery rendering while the user keyframes.
  - Src: editingcorp; AES
  - [SOURCED]

- [ ] **FIN-18 · Project portability: imagery storage, moving and collecting projects**
  - Imagery is file-backed: a disconnected cache drive gave "MapComp could no longer be found" (ACOM:14880531).
  - Exporting/moving a GEOlayers project correctly is a known user topic (Razker "How To Export GeoLayer 3 Project Correctly", YT:21RUQrIzYyU).
  - Where finalized imagery is stored, and whether AE Collect Files / a cache-drive choice is required, is unknown. (I: finalized imagery is footage the user can collect; the cache path is a preference.)
  - Src: ACOM:14880531; YT:21RUQrIzYyU; n/community §2.4; n/videos §5
  - [SOURCED problem; storage rules unknown]

## 6. Mapcomp labels — automatic (MLB)

- [ ] **MLB-01 · Mapcomp labels**
  - Small, text-only geographic labels that are placed, **blended and animated automatically** for the Mapcomp's view (AESF, weak).
  - MapTiler-sourced Mapcomp labels need licensed MapTiler data (HELP L392). Natural Earth labels do not (CL 1.1.9). The forum line "not placed if no data is licensed" is pre-1.1.9 (weak, probably obsolete).
  - Default look (official HELP image, MapTiler labels): UPPERCASE, letter-spaced, dark grey text with a light halo; place dots; label size by place rank.
  - Toggled per Mapcomp with the "A" icon (UI-08; EBE confirms the List labels toggle). "Turn Off Labels" tip.
  - Src: AESF (weak, pre-1.1.9); YT:0SoBCtbF3B0; HELP L392 + README image user-images…/233547961-62f27e76….png; CL 1.1.9; EBE
  - [SOURCED weak (behaviour); look SOURCED from one image]

- [ ] **MLB-02 · Label data sources**
  - MapTiler (multilingual; partner feature) and **Natural Earth Mapcomp Labels** (1.1.9, free).
  - Src: HELP L392; CL 1.1.9
  - [SOURCED]

- [ ] **MLB-03 · Languages**
  - Per-Mapcomp Language setting (MCS-12).
  - Explicitly supported: Welsh (1.6.1), Irish Gaelic & Scottish Gaelic (1.5.0), Traditional Chinese (1.12.2 "language setting" fix; scope unspecified), RTL scripts in Mapcomp labels (1.13.1; the 1.12.2 RTL fix is for Label Templates, LBL-23). Natural Earth language support (1.1.3).
  - Full list unknown. (I: MapTiler `name:xx`, 82 values; NE `name_xx` columns.)
  - Src: CL; n/providers §1.2
  - [SOURCED; list INFERRED]

- [ ] **MLB-04 · Place dots**
  - Mapcomp labels can display dots (1.5.1 fix).
  - Src: CL 1.5.1
  - [SOURCED]

- [ ] **MLB-05 · RTL rendering**
  - RTL labels must not be reversed.
  - Src: CL 1.13.1
  - [SOURCED]

- [ ] **MLB-06 · Label renaming applies to Mapcomp labels**
  - Dictionaries (PRF-09).
  - Src: CL 1.2.1, 1.2.5
  - [SOURCED]

- [ ] **MLB-07 · Fonts and language for map labels via Mapstyling**
  - See IMG-08.
  - Src: VM:357897696
  - [SOURCED]

- [ ] **MLB-08 · Labels-only Mapcomp**
  - E.g. "Main Labels" linked to "Main Map" (MCS-23).
  - [SOURCED]

- [ ] **MLB-09 · Label motion blur**
  - Built-in motion blur exists for label layers (YT:XKd_cNsDF-g). (I: whether the Imagery Motion Blur toggle controls it, or a separate control does, is unknown.)
  - Src: YT:XKd_cNsDF-g
  - [SOURCED existence; control INFERRED]

- [ ] **MLB-10 · Representation in AE**
  - Unknown: baked into imagery, or separate AE text/precomp layers. "Placed, blended and animated" suggests layers.
  - [INFERRED]

- [ ] **MLB-11 · Mapcomp Label Templates**
  - See MCS-19 / LBL-14.
  - [SOURCED]

## 7a. Search (SRC)

- [ ] **SRC-01 · Search bar**
  - A "Search..." field with clear ×. Used in the main panel and the create dialog.
  - (I: search runs on Enter, not as-you-type, per the Nominatim policy.)
  - It sits below the toolbar (UI-02). EBE: typing a country name lists results; "you have to be very specific. If you misspell it, it won't show you what you want to see" (narrator remark, not demonstrated; weak; SRC-14).
  - Src: STADIA; HELP L18; EBE
  - [SOURCED; trigger INFERRED]

- [ ] **SRC-02 · Results list**
  - Results are grouped under **source headers** (e.g. "Natural Earth").
  - Each row shows:
    - a geometry icon: ● point, > line, ■ polygon, 📁 collection
    - the name
    - a type subtitle, e.g. "Sovereign Country", "Inner Boundaries", "States & Provinces", "Hamlet: Houston County, Texas, …"
  - (I: results may carry a type/category label such as "Administrative"; a forum user (weak) reports such results disappeared in newer versions.)
  - EBE: the source headers are light grey. Natural Earth comes first, then Nominatim (OpenStreetMap) below it. A country query returns both the country polygon and an "Inner Boundaries" result.
  - "Back" button.
  - Src: VM:517178231; AESF (weak); EBE
  - [SOURCED observed subtitles; category label INFERRED]

- [ ] **SRC-03 · Result row hover actions**
  - **fit/zoom-to** (dashed frame)
  - **label** (tag) → LBL-05
  - **draw** (pen) → DRW-01
  - **add to browser** (⤓ download)
  - EBE confirms the four quick actions in this order: fit the map view, add a label, draw, add to browser.
  - Src: VM:517178231; AES ("labelled, drawn, downloaded"); EBE
  - [SOURCED]

- [ ] **SRC-04 · Feature preview**
  - The hovered result's outline is shown on the preview map with name and count. Bounding boxes can be shown (pref).
  - EBE: "As you hover the mouse over, it shows you … your results."
  - Src: CL 1.2.2; VM:517178231; EBE
  - [SOURCED]

- [ ] **SRC-05 · OSM / Nominatim search**
  - Requests are queued per the usage policy.
  - Results without geometry fall back to points (1.0.6).
  - Language defaults to the Mapcomp's language.
  - EBE: Nominatim results appear as their own group under the Natural Earth group (SRC-02).
  - Src: HELP L143-151; CL 1.0.6, 1.9.0; EBE
  - [SOURCED]

- [ ] **SRC-06 · MapTiler Search**
  - Geocoding through MapTiler.
  - Loads **full geometry** from MapTiler's dataset for basemap consistency (1.13.1).
  - Supports **worldviews** set in Preferences (1.18.0).
  - Bugfixes: point-only results (1.5.3/1.13.1), wrong countries with a worldview (1.18.1), results not displayed (1.5.5).
  - Src: CL 1.5.3, 1.5.5, 1.13.1, 1.18.0, 1.18.1
  - [SOURCED]

- [ ] **SRC-07 · Multiple search sources**
  - Since 1.2.2. Sources:
    - Natural Earth Search API
    - Browser Search API
    - Browser Downloads Search API
    - File Search API (http URLs; zipped shapefiles since 1.5.0)
    - Folder Search API
    - GitHub Search API (public repos)
  - Plus OSM and MapTiler.
  - A source set up as a File without a URL must not open file pop-ups (1.5.4).
  - Where sources are configured is unknown (I: Preferences).
  - Src: CL 1.2.2, 1.5.0, 1.5.4
  - [SOURCED; config UI unknown]

- [ ] **SRC-08 · Fallback online search service**
  - Used when the default search fails; can be set as default in prefs. The service is unnamed.
  - Src: CL 1.0.2
  - [SOURCED]

- [ ] **SRC-09 · Preload features for search on startup**
  - Src: CL 1.2.2
  - [SOURCED]

- [ ] **SRC-10 · Download a search result into the Browser**
  - EBE: the "add to browser" quick action puts the result in the Feature Browser below the preview, "so then I can use it again later".
  - Src: CL 1.6.3; VM:517178231; EBE
  - [SOURCED]

- [ ] **SRC-11 · Export search results for GIS software**
  - Src: AES
  - [SOURCED; UI unknown]

- [ ] **SRC-12 · Historical basemaps via the GitHub source**
  - "Connecting the Repository" to aourednik/historical-basemaps, then searching it.
  - Src: VM:517178231; YT:y34htfkMmxc; EX:Historical_Basemaps_Browser
  - [SOURCED]

- [ ] **SRC-13 · Coordinates input**
  - "Paste Coordinates in GEOlayers 3" when georeferencing photos.
  - (I: the search bar or a coordinate field accepts "lat, lon".)
  - Src: YT:tZ7SVVmgwyE; ifmlearn "manual coordinate input"
  - [SOURCED; UI INFERRED]

- [ ] **SRC-14 · Natural Earth search by name (matching quality)**
  - "Improved Natural Earth data search by name" (1.2.0), separate from the later NE Search API source (1.2.2, SRC-07). The matching algorithm is unknown. (I: case/diacritic-insensitive matching over NE name and name_xx columns.)
  - EBE (weak, narrator remark, no misspelled query is demonstrated): "If you misspell it, it won't show you what you want to see." (I: no typo-tolerant (fuzzy) matching; parity default, to confirm.)
  - Src: CL 1.2.0; EBE
  - [SOURCED existence; no fuzzy matching weak (EBE remark, not demonstrated); algorithm INFERRED]

## 7b. Feature Browser (FB)

- [ ] **FB-01 · Feature Browser**
  - A tree of features and **Feature Collections** (nested).
  - Default top-level collections: **"☆ Favorite Features"** and **"📄 Features"**.
  - EBE: "And then, of course, you can create a feature collection, which is just a way to hold all of your data." (I: it is created from a "+" menu entry **"Feature collection"** (FB-02) and starts empty; EBE states neither.) Selected features can be added to a collection from the feature action bar (FB-46).
  - Src: STADIA; HELP L172-226; EBE
  - [SOURCED]

- [ ] **FB-02 · Browser header**
  - **"+"**, labelled **"Add features to browser"** (EBE). It opens a dropdown menu. Entries in EBE order (the first two stated as such, the rest in narration order):
    1. **Download features** (FB-14)
    2. **Import file** (FB-03)
    3. **Mapcomp view** (VIEW-09)
    4. **Feature from layer** (FB-30)
    5. (I) **Feature collection**: a new collection (FB-01). EBE says only "you can create a feature collection", right after Feature from layer; that it is a "+" entry and that it starts empty are INFERRED. Order of entries 3–5 to confirm.
  - funnel + **"Filter..."** field (FB-06; EBE)
  - geometry-type toggles **● points, > lines, ■ polygons**: filter the tree by geometry type (EBE: "if you just want to look at lines, you can filter out polygons and points")
  - a dashed-square icon (unknown)
  - a stacked-shapes icon (unknown)
  - EBE names a **"Preview selection on map"** toggle (FB-45). (I: it is one of the two unknown icons, probably the dashed square.)
  - Src: STADIA; EBE
  - [SOURCED; "+" menu entries 1–4 and toggles SOURCED (EBE); "Feature collection" as entry 5 INFERRED; order of 3–5 narration order; icon ↔ "Preview selection" INFERRED]

- [ ] **FB-03 · Import File**
  - Import a geospatial file into the Browser.
  - **CTRL/CMD-click** opens import from a remote URL; "almost every file" type works.
  - It is the second entry of the "+" menu (FB-02). Files can also be **dragged and dropped onto the Feature Browser** (EBE: "you can simply drag and drop your files directly over the feature browser").
  - Src: CL 1.2.2, 1.4.2; EBE
  - [SOURCED]

- [ ] **FB-04 · Remove features**
  - Removes the selected features.
  - **SHIFT-click** clears the whole Browser (favorites are kept, per clearBrowser semantics).
  - EBE: a feature-action-bar button, narrated last ("And then of course you can remove features as well"; FB-46). Its position is not stated.
  - Src: CL 1.3.0; HELP L208-209; EBE
  - [SOURCED; favorites kept on SHIFT INFERRED from API]

- [ ] **FB-05 · Export features**
  - Exports the selected features or collection as **GeoJSON** (EBE: "I could export this as a gojson").
  - **SHIFT-click** exports all features except favorites to **`features.geojson` beside the project file**.
  - "How to Export to a Geojson". It is a feature-action-bar button (FB-46; EBE).
  - Src: CL 1.3.0; YT:Vq6q-X3KVgo; EBE
  - [SOURCED]

- [ ] **FB-06 · Filter**
  - Fast text filter that shows only the needed collections. (EBE's "another little keyword search here" comes during the Feature Properties discussion and is probably the property filter in FB-33, not this field; attribution to FB-06 uncertain.)
  - "Filter in the Browser via Data" (I: filter expressions on properties).
  - Src: CL 1.0.9; YT:2Ga-6DD2epo; EBE
  - [SOURCED; data-filter syntax unknown]

- [ ] **FB-07 · Collection naming and sorting**
  - Sort features by Name (1.2.2). EBE: the sort is in the collection's **Feature Properties** view (FB-33). Rename a Feature Collection.
  - The FeatureCollection property `GEOlayersDisplayName` sets its display name. EBE: Feature Properties lets you choose which property is shown as the features' names.
  - Src: CL 1.2.2; YT:JcCTGlszHcw; YT:2Ga-6DD2epo; EX:COVID-19_*; EBE
  - [SOURCED]

- [ ] **FB-08 · Favorites**
  - Features can be favorited. Favorites survive clearBrowser and SHIFT-export exclusions.
  - Src: HELP L209; CL 1.3.0
  - [SOURCED]

- [ ] **FB-09 · Selection**
  - Multi-select in the tree: **CTRL-click** adds a feature to the selection (EBE; I: CMD on macOS). Click on the preview map selects in the tree.
  - Selecting features shows the feature action bar (FB-46). Double-clicking a feature previews and zooms the preview map to it (FB-45; EBE).
  - Src: CL 1.1.0, 1.1.5; HELP L220-226; EBE
  - [SOURCED]

- [ ] **FB-10 · Find Features by clicking the preview map**
  - Uses MapTiler data (formerly the "GEOlayers Mapdata" subscription).
  - Src: HELP L391; CL 1.0.4
  - [SOURCED]

- [ ] **FB-11 · Find Features at Coordinate**
  - Returns the features at a point, including an **Inner Boundaries** feature for each country.
  - UI (EBE): a button "right here at the top left" of the preview area (approximate position; I: the STADIA crosshair icon, UI-11, by position match). It shows the coordinate of the current view location and lists all the data features there, from large to small. Example over Kansas: United States of America → Inner Boundaries → the states collection → Kansas → a point for the coordinate itself. A back control returns to the main view. EBE: "a great way to quickly … sample and pull data from where your current view is located."
  - Inner Boundaries must be correct for every country (CL 1.7.2 bugfix "Natural Earth Inner Boundaries for some countries are messed up").
  - (I: the listed results offer the usual per-result actions; EBE does not show them.)
  - Src: CL 1.2.0, 1.7.2; YT:aELgIVzP_HE; EBE
  - [SOURCED]

- [ ] **FB-12 · Current View features import**
  - Imports features for the current view (MapTiler; HELP L394 "Feature import for the current view").
  - **Transport** features get separate ramps (1.3.0).
  - Src: HELP L394; CL 1.3.0
  - [SOURCED]

- [ ] **FB-43 · "Add Current View Feature"**
  - CL 1.2.3 bugfix: "Adding a Current View Feature fails if view can't be geocoded". Possibly a single feature representing the view (close to "Create a Mapcomp View", VIEW-09), not a data import. (I: a view-extent polygon named by reverse geocoding; must still work when reverse geocoding fails.)
  - (I) EBE's "+" > "Mapcomp view" entry asks for a name instead of reverse geocoding, so it may be this feature in a later form (VIEW-09).
  - Src: CL 1.2.3
  - [SOURCED existence; nature INFERRED]

- [ ] **FB-13 · Admin Bounds to browser (higher zooms)**
  - Src: CL 1.17.0
  - [SOURCED]

- [ ] **FB-14 · Downloadable datasets**
  - Global **Lakes / Rivers** (1.0.6)
  - **Natural Earth Physical Areas Land & Water** (1.1.9)
  - **NE States and Provinces** lines + polygons (1.2.0, 1.5.4)
  - **Disputed Area Dataset** (MC)
  - EBE: all countries of the world in one step (it arrives as a Feature Collection); Natural Earth **Populated Places** (cities as points).
  - UI (EBE): the first entry of the "+" menu, **"Download features"** (FB-02), "access to all these data sets".
  - Lesson: "Download Datasets"
  - Src: CL; MC; EBE
  - [SOURCED]

- [ ] **FB-15 · Get features by OSM id**
  - Accepts node/way/rel ids. Stability improved in 1.17.0.
  - Src: HELP L190-193; CL 1.1.7, 1.17.0
  - [SOURCED]

- [ ] **FB-16 · Overpass queries into the Browser**
  - With `{{bbox}}` = current view; `{{geocodeArea:…}}` support is uncertain (C-05).
  - Server auto-selection (1.5.6).
  - Src: EX:*; CL 1.5.6
  - [SOURCED]

- [ ] **FB-17 · Default Features on startup**
  - Loaded on extension start (pref); http/https sources allowed.
  - Src: CL 1.1.7, 1.2.2
  - [SOURCED]

- [ ] **FB-18 · Auto-load GeoJSONs next to the project**
  - When the project is opened via the Projects panel.
  - Src: CL 1.0.9
  - [SOURCED]

- [ ] **FB-19 · Update URL-sourced collections with one click**
  - The mechanism is unknown. (I: the original import URL is stored separately.) The FeatureCollection `source` property cannot be the update URL: in EX:COVID-19_Casedata_USA/World the data is fetched from opendata.arcgis.com/…geojson while `source` is a hub.arcgis.com landing page (HTML).
  - Src: CL 1.4.2; EX:COVID-19_*
  - [SOURCED existence; mechanism unknown]

- [ ] **FB-20 · Merge Features**
  - Geometry union. Non-blocking and cancelable.
  - EBE: a feature-action-bar button shown when two polygons are selected (FB-46).
  - Src: CL 1.2.1, 1.6.2; YT:2Ga-6DD2epo; EBE
  - [SOURCED]

- [ ] **FB-21 · Subtract Features**
  - Geometry difference, e.g. keep only the land portion. Non-blocking and cancelable.
  - EBE: shown with Merge when two polygons are selected; it subtracts "the lower features" (I: the later-listed or lower-in-tree selected features are subtracted from the first). Workflow: draw a custom polygon (DRW-22 + FB-30), subtract it from a country, or subtract a country from the ocean polygon, for an exact coastline.
  - Src: CL 1.2.1, 1.6.2; VM:373569122; YT:PPFpNW8dYFc; EBE
  - [SOURCED]

- [ ] **FB-22 · Merge Features in Collections by name**
  - Src: CL 1.1.8
  - [SOURCED]

- [ ] **FB-23 · Split a feature into individual polygons**
  - Multipolygon → separate features.
  - Src: YT:2Ga-6DD2epo; YT:PPFpNW8dYFc
  - [SOURCED]

- [ ] **FB-24 · Create Lines from Polygons**
  - Non-blocking and cancelable.
  - EBE: a feature-action-bar button that turns a selected polygon (e.g. Australia) into its outline.
  - Src: CL 1.2.1; EBE
  - [SOURCED]

- [ ] **FB-25 · Line Mesh from polygons with common borders**
  - Shared borders become lines. (I: without duplicates.)
  - Src: CL 1.0.7
  - [SOURCED]

- [ ] **FB-26 · Feature Mesh**
  - Connection lines between selected features, all-to-all.
  - With **one feature + one collection** selected it produces one-to-all lines.
  - Src: CL 1.7.1
  - [SOURCED]

- [ ] **FB-27 · Connect features**
  - Lines between features (route creation); works across the date border.
  - UI (EBE): select two or more features (CTRL-click), then **"Connect features"** in the action bar. It opens a menu of route types, including **"plane route"** (RTE-10). Some types fail for some pairs (I: road routes across seas). EBE: "And it shows you how it is pulling this data." (I: the menu shows which data source each route type uses; it could also be a progress/status display. Provider not named.) Full route-type list and sources: Q-50.
  - UI: the route-type menu/dialog stays open after a route is created and is closed manually (EBE: "it will create a new map feature actually. So, let me close this."; I: a sub-view with Back/close, UI-20).
  - Result: a new **map feature** in the Browser (not a layer), named like "plane route, Afghanistan to Albania" (exact punctuation unknown). Its Feature Properties include the **distance** (FB-41) and the label position.
  - See RTE-02.
  - Src: CL 1.1.2; VM:359745657; EBE
  - [SOURCED]

- [ ] **FB-28 · Simplify Feature Topologies**
  - Topology-preserving simplification of a collection (shared borders stay shared). Non-blocking and cancelable.
  - Src: CL 1.2.0, 1.2.1
  - [SOURCED]

- [ ] **FB-29 · Points-inside-Polygons tools**
  - "Isolate Points with/inside Polygons", e.g. keep the populated places inside a country.
  - UI (EBE): select a point collection, CTRL-click a polygon, then **"Points inside polygons"** in the action bar. It offers "a couple of different options". The option **"points inside of polygons"** creates a **new Feature Collection** holding the polygon used and the points inside it (filter/isolate).
  - The other options are not named. (I: count per polygon, join polygon properties to points; "Convert polygons to points", FB-47, may be one of them.) Open question: Q-53.
  - Src: CL 1.0.9; YT:2Ga-6DD2epo; YT:D_X3xN5Uc_Y; EBE
  - [SOURCED; isolate op SOURCED (EBE); other ops unknown]

- [ ] **FB-30 · Import features from Shape Layers**
  - Creates polygon/line/point features from AE shape layers ("Create Custom Map Feature from a Layer", "Create Feature from Layer", "Make the Arrow a Map Feature", digitizing).
  - UI (EBE): the "+" menu entry **"Feature from layer"** (FB-02). An arrow layer (RTE-06) becomes two features: the **arrowhead shape** and the **arrow line**. A path drawn on a "Create a Shape Layer to Draw On" layer (DRW-22) becomes a line feature to animate along (VIEW-10).
  - EBE: conversion uses the path geometry regardless of fill/stroke (the drawn path "doesn't even have to be a stroke"; the narrator turns fill off and stroke on only to see it), so a fill-only path still converts.
  - Src: CL 1.3.0; YT:17tHaEJuZNA; YT:rmITTGwVENo; WAR; EBE
  - [SOURCED]

- [ ] **FB-31 · Custom features from georeferenced layers**
  - Trace a georeferenced image, then create a feature from it ("Georeference Existing Maps", "Parent to Map and Create Map Feature").
  - Src: MC; YT:EtsCl787YR8
  - [SOURCED; tool UI unknown]

- [ ] **FB-32 · Add Null to Feature**
  - Adds a null layer positioned at the feature (e.g. "Add Null Object to Launch Site").
  - Src: CL 1.1.8; YT:sxh0aGwXfQI
  - [SOURCED]

- [ ] **FB-33 · Feature Properties panel**
  - Shows a feature's properties and **collective properties** for multiple features (0.9.0).
  - http links open in the browser (1.0.9).
  - Shows the **duration of timed data** (1.1.4).
  - The **label position can be dragged and centered** (1.1.3).
  - EBE:
    - opened with the "Feature properties" button of the action bar (FB-46)
    - **collection** view: the **minimum and maximum** of each property, a **Sort by Name** control (FB-07), and per-property settings for how the property is used, e.g. to drive timing (DAT-12) or to supply the features' display names
    - **single feature** view: a different layout with that feature's properties
    - route features (FB-27) show the distance and where the label will be shown
    - (I, probable) a keyword filter for the property list: "Now you have another little keyword search here if you want to filter this down by uh what you have in here. If you have a lot of different feature properties, you can filter it down." (Earlier attributed to FB-06.)
  - Src: CL; EBE
  - [SOURCED]

- [ ] **FB-34 · Add / edit feature properties**
  - "Add Map Data Column", "How to Add Data to a Feature", "How to Add Feature Properties".
  - Src: YT:4kWcuolF16Q; YT:2Ga-6DD2epo; MC; YT:Duglh9VTLes
  - [SOURCED; UI unknown]

- [ ] **FB-35 · Collection properties CSV/TSV export → spreadsheet → import**
  - Round-trip a collection's properties through a spreadsheet (data join).
  - Src: CL 1.1.5; VM:433581148
  - [SOURCED]

- [ ] **FB-36 · One-click data-driven style from a collection's properties panel**
  - See DAT-07.
  - Src: CL 1.0.9
  - [SOURCED]

- [ ] **FB-37 · Automatic label point**
  - Always centered inside the largest polygon (I: pole of inaccessibility).
  - (I) "Convert polygons to points" (FB-47) probably places its points here.
  - Src: CL 1.2.0
  - [SOURCED]

- [ ] **FB-38 · Robust import**
  - Must handle:
    - empty coordinate arrays
    - multipoint
    - Nominatim responses without geometry
    - large OSM downloads (no freeze)
    - labeling empty collections (no freeze)
  - Src: CL 1.0.6, 1.2.2, 1.4.0, 1.5.3
  - [SOURCED]

- [ ] **FB-39 · `coordinate_elevation` property**
  - Added on import of 3D line features (stores Z).
  - Src: CL 1.11.1
  - [SOURCED]

- [ ] **FB-40 · Feature cache**
  - Downloaded features are cached; cache paths are configurable (hidden pref).
  - Src: CL 1.0.7
  - [SOURCED]

- [ ] **FB-41 · Measure distance and area**
  - A course lesson. Route layer names include the distance ("(93.224 mi)").
  - EBE: a route feature made with Connect features (FB-27) has a **distance** property shown in Feature Properties. Units are not stated.
  - (I: area appears in Feature Properties for polygons.) Units preference unknown.
  - Src: WAR; STADIA; EBE
  - [SOURCED existence + route distance property (EBE); area UI INFERRED]

- [ ] **FB-42 · Feature Browser and search write into the same tree**
  - Search downloads go to the Browser (SRC-10). "Browser Downloads" is also a search source.
  - Src: CL 1.2.2
  - [SOURCED]

- [ ] **FB-44 · Overpass OSM-JSON → GeoJSON conversion**
  - (I, requirement) Overpass results must be converted to GeoJSON features: ways become lines or polygons; multipolygon and route relations are assembled. Vendor examples all end with `out body;>;out skel qt;` and query relations/nwr, e.g. EX:Railroads_OSM `relation["route"="railway"]({{bbox}})`, EX:Autobahn_BAB `nwr[...]`.
  - Src: EX:Railroads_OSM, EX:Autobahn_BAB, EX:Buildings_OSM
  - [SOURCED usage; conversion rules INFERRED]

- [ ] **FB-45 · "Preview selection on map" toggle**
  - A Browser toggle, **on** in the video, that previews the selected features on the preview map. Double-clicking a feature (e.g. Australia, or a feature made from an arrow) zooms the preview to it (EBE: "So if I double click on this now I can perfectly zoom it up").
  - Turning it off stops the selection preview (EBE). (I: it is the dashed-square or stacked-shapes header icon, FB-02; whether double-click zoom still works when off is unknown.)
  - Src: EBE
  - [SOURCED (EBE); icon and off-state details INFERRED]

- [ ] **FB-46 · Feature action bar**
  - When features are selected, a row of action buttons appears below the Browser. The set "will be different depending on the geometry type and which features you select" (EBE).
  - Buttons narrated for a single country, in order:
    1. **Fit feature to view**: a menu of fit and animate options (VIEW-04…08)
    2. **pin** (I: pin the selected layer to the feature, PIN-06)
    3. **label** with the current template (LBL-05)
    4. **draw** (DRW-01)
    5. **add to a feature collection** (FB-01)
    6. **feature properties** (FB-33)
  - Shown for other selections (EBE):
    - Connect features (FB-27), with 2+ features
    - Create lines from polygons (FB-24)
    - Merge / Subtract features (FB-20/21), with two polygons
    - Points inside polygons (FB-29), with a point collection + a polygon
    - Convert polygons to points (FB-47)
    - Draw property charts (DAT-14)
    - Export features (FB-05)
    - Remove features (FB-04)
  - Layout (EBE, approximate): a first group (fit, pin, label, draw, add to collection, properties, connect), then a geometry group introduced with "Down here is where you can mess with the geometry" (lines from polygons, merge/subtract, points inside polygons, convert to points), then "Down here we have" charts/export/remove. Whether these are rows or groups is unknown.
  - Icons, tooltips and the exact show/hide rule per geometry type are unknown. (I: polygon-only tools are hidden for points and lines; two-feature tools need 2+ features.) Open questions: Q-51 (Fit-feature-to-view menu entries), Q-52 (icons and visibility rules).
  - Src: EBE
  - [SOURCED (EBE); grouping approximate; icons and visibility rules INFERRED]

- [ ] **FB-47 · Convert polygons to points**
  - An action that makes a point at the centre of each selected polygon, e.g. one point per country for data visualisation (EBE: "a great way to get a point … out of the center of this polygon").
  - EBE: "if I select this and I click on this, I can do convert polygons to points". It is unclear whether this is its own button or an option in the Points-inside-polygons menu (FB-29); Q-53.
  - (I: the point is the automatic label point, FB-37, and keeps the polygon's properties; the output is a new collection.)
  - Src: EBE
  - [SOURCED (EBE); placement, point position and output form INFERRED]

## 8. Drawing features to shape layers (DRW)

- [ ] **DRW-01 · Draw features as shape layers**
  - Draws the selected Browser features, or a search result (pen icon), as AE shape layers using the current Shape Layer Style.
  - By default the layers go into the **containing comp**, parented to "<Mapcomp> Anchor".
  - Each layer is named after its feature (★ shape-layer icon, e.g. "Lyon"). Blend mode comes from the style, e.g. Multiply.
  - EBE: drawing a feature (search-result or action-bar draw button) uses the colour of the currently selected Shape Layer Style.
  - Src: HELP L228-298; STADIA; VM:358244424; MC "How Features are Drawn"; EBE
  - [SOURCED]

- [ ] **DRW-02 · Drawing options UI, persisted in the .aep**
  - Drawing options such as simplify geometry, Auto Stroke Width and individual layers are saved in the project (1.1.8).
  - The UI label "Simplify Geometry (Current Zoom)" is seen in the wild.
  - Location (EBE): at the bottom of the **Shape Layer Style panel** (SLS-01), below the style settings: "these preferences … tell After Effects how to draw out these shape layers". The earlier guess (a popover next to the draw button) is wrong.
  - User-visible options named by EBE: Auto Stroke Width (DRW-05); geometry control (simplify, DRW-06/07); split map features into individual layers (DRW-10); draw inside Mapcomps (DRW-04); draw on a preferred Mapcomp (DRW-28). Whether the other API draw options (DRW-08/09/11–19) are in this list is unknown.
  - Lessons: "Control the Geometry", "Draw Features in the Mapcomp".
  - Src: CL 1.1.8; ACOM:11865951; MC; EBE
  - [SOURCED; location SOURCED (EBE); full option list unknown]

- [ ] **DRW-03 · shapeLayerStyle**
  - Accepts a style index, a name, or a style object. In the UI it is the current style swatch: a toolbar button that shows the current Shape Layer Style and opens the Shape Layer Style panel (EBE; SLS-01).
  - Src: HELP L235; EBE
  - [SOURCED]

- [ ] **DRW-04 · drawInsideMapcomp** (default **false**)
  - (I) false draws into the containing comp; true draws inside the Mapcomp. Consistent with the STADIA timeline (drawn layers in the containing comp) and the MC lesson title "Draw Features in the Mapcomp".
  - User-visible in the drawing options (EBE: "You can draw them inside of your map comps and or you can even specify a specific map comp"; the specific Mapcomp is DRW-28).
  - Src: HELP L236; MC; STADIA; EBE
  - [SOURCED name/default; semantics INFERRED]

- [ ] **DRW-05 · autoStrokeWidth / "Auto Stroke Width"** (default **false**)
  - Stroke width stays visually constant while zooming.
  - User-visible in the drawing options (EBE: "if you want to have auto stroke width").
  - Src: HELP L237; CL 1.1.7, 1.1.8; EBE
  - [SOURCED name; behavior INFERRED]

- [ ] **DRW-06 · simplify** (default **true**)
  - Simplifies geometry for the current zoom. The UI label "Simplify Geometry (Current Zoom)" is seen in the wild (ACOM); (I) it maps to this option (C-14).
  - Explained in the "Geometry Simplification" tutorial. Simplification is tip 1 for detailed borders.
  - EBE: the drawing options let you "control the geometry" (no option names read out).
  - Src: HELP L238; ACOM:11865951; VM:507479009; YT:LEaMUpjupG0; EBE
  - [SOURCED option + default; UI label SOURCED; option↔label mapping INFERRED]

- [ ] **DRW-07 · simplyfyByZoomRange (sic)** (default **false**)
  - "Simplify Geometry by max Zoom" (1.2.0): simplify for the maximum zoom reached in the animation. (I: rather than the current zoom.)
  - Src: HELP L239; CL 1.2.0
  - [SOURCED; mapping INFERRED]

- [ ] **DRW-08 · simplificationZoomOffset** (default **0**)
  - (I: zoom levels added to the simplification zoom; a higher value keeps more detail.)
  - Src: HELP L240
  - [SOURCED name; behavior INFERRED]

- [ ] **DRW-09 · useTopoSimplificationIfPossible** (default **false**)
  - (I) Accurate topology simplification for polygons with shared borders (1.2.0 "Accurate topology geometry simplification"; linked by name only).
  - Src: HELP L241; CL 1.2.0
  - [SOURCED name/default; changelog link INFERRED]

- [ ] **DRW-10 · individualLayers** (default **false**)
  - One layer per feature. Otherwise features share a layer as content groups.
  - User-visible in the drawing options (EBE: "You can split … map features into individual layers").
  - Src: HELP L242; CL 1.1.4; EBE
  - [SOURCED; grouping INFERRED]

- [ ] **DRW-11 · autoChangeRenderer** (default **false**)
  - Switches the comp renderer to CINEMA 4D when drawing extruded features (1.5.2 fix).
  - Src: HELP L243; CL 1.5.2
  - [SOURCED; partly INFERRED]

- [ ] **DRW-12 · addTrimPaths** (default **false**)
  - Adds Trim Paths for draw-on animation, e.g. route drawing.
  - Src: HELP L244; VM:359745657 "draw animated Shape Layers"
  - [SOURCED name; I: Trim End keyed 0→100]

- [ ] **DRW-13 · duration** (default **10**)
  - Duration of the created animation. Units unknown (I: seconds).
  - Src: HELP L245
  - [SOURCED; units INFERRED]

- [ ] **DRW-14 · dateBorderCopy** (default **false**)
  - (I) Features that cross the date line are drawn multiple times (world copies).
  - Conflict (C-20): CL 1.2.0 "Features drawn multiple times if they cross the date line" reads as automatic, yet the API default is false. Either the UI default differs from the API default (cf. drawing options saved per .aep, CL 1.1.8) or dateBorderCopy is something else. Verify in AE.
  - Copies must keep correct positions when lockLayerTransforms is on (1.15.2 "Shape layer world copies").
  - Src: HELP L246; CL 1.2.0, 1.15.2
  - [SOURCED name/default; mapping INFERRED]

- [ ] **DRW-15 · dashThreshold / "Dash Zoom Threshold"** (default **1**)
  - (I) Makes dashed lines with Auto Stroke Width look right during zooms (CL 1.1.7 "Dash Zoom Threshold"; linked by name only).
  - Src: HELP L247; CL 1.1.7
  - [SOURCED name/default; changelog link INFERRED; formula unknown]

- [ ] **DRW-16 · absoluteZScale** (default **false**)
  - Extrusion height independent of zoom. Toolbar 3D buildings use this (1.0.5).
  - Src: HELP L248; CL 1.0.5
  - [SOURCED; mapping INFERRED]

- [ ] **DRW-17 · castsShadows** (default **false**)
  - (I) Sets the AE material option Casts Shadows. Lesson "How to Use Shadows" (does not name the options).
  - Src: HELP L249; MC
  - [SOURCED name/default; behaviour INFERRED]

- [ ] **DRW-18 · acceptsShadows** (default **false**)
  - (I) Sets the AE material option Accepts Shadows.
  - Src: HELP L250
  - [SOURCED name/default; behaviour INFERRED]

- [ ] **DRW-19 · lockLayerTransforms** (default **false**)
  - (I) Locks the transform properties of drawn shape layers; linked by name to the 1.2.6 Preference "Shape Layer Transform Properties can be locked" (PRF-08).
  - Src: HELP L251; CL 1.2.6, 1.15.2
  - [SOURCED name/default; changelog link INFERRED]

- [ ] **DRW-20 · Draw with time offsets**
  - Features can be drawn with time offsets, optionally data-driven by a property, e.g. earthquakes appearing at their timestamps.
  - Src: CL 1.2.3; VM:533574071
  - [SOURCED]

- [ ] **DRW-21 · Large-draw warning**
  - Drawing many features (observed 3484 points) shows a warning: "You're trying to draw N features with N …" (text truncated).
  - The threshold is unknown.
  - Src: ACOM:11865951
  - [SOURCED]

- [ ] **DRW-22 · "Create a Shape Layer to Draw On"**
  - Creates a georeferenced empty shape layer that the user draws on with AE pen tools. It can then become a map feature (FB-30).
  - EBE: a toolbar button ("This button right here allows you to create a shape layer to draw on"; the eighth narrated, after Draw Arrow). The new shape layer is already **parented to the map** and **set to 3D**, so shapes drawn into it stick to the map. A plain AE shape is not attached and must be parented or pinned (PIN-07).
  - 1.1.4 fixed "shape layers created to draw on at large scales". Lesson "How to Draw on Shape Layers".
  - (I: the toolbar framed-pen icon, matched by narrated order.) Parent target (anchor or Mapcomp layer) and layer name are unknown.
  - Src: YT:2Ga-6DD2epo; CL 1.1.4; MC; EBE
  - [SOURCED (toolbar action, parenting, 3D: EBE); icon INFERRED]

- [ ] **DRW-23 · Multi-polygon content structure**
  - Multipolygons use a faster shape-layer content structure (1.2.5). Holes are rendered correctly.
  - Src: CL 1.2.5
  - [SOURCED; structure INFERRED]

- [ ] **DRW-24 · Point features with the CINEMA 4D renderer**
  - Points must render with the C4D renderer.
  - Src: CL 1.1.2
  - [SOURCED]

- [ ] **DRW-25 · Lines from CSV cross the date border**
  - Src: CL 1.2.5
  - [SOURCED]

- [ ] **DRW-26 · Rasterize vector shapes workflow**
  - Simplify geometry → precompose (precompose_feature_shape_layer.jsx) → auto-trace alpha.
  - Choropleth imagery is the raster alternative.
  - Src: YT:JTQ4aniKPek; WAR
  - [SOURCED]

- [ ] **DRW-27 · Draw onto a 3D Landscape**
  - Draw features and add a displaced label on a 3D landscape.
  - Src: VM:361319119
  - [SOURCED]

- [ ] **DRW-28 · "Draw on preferred Mapcomp"**
  - A drawing option (DRW-02, in the Shape Layer Style panel) that draws features onto a **preferred Mapcomp** instead of the selected one. EBE: "it says draw on preferred map comp to select that preferred map comp. You need to go back to your map comp menu here."
  - The preferred Mapcomp is chosen in the Mapcomp List (UI-26); with this option on, the List shows "Prefer for drawing features".
  - There is no matching API draw option (HELP L233-251). (I: UI-only; it combines with drawInsideMapcomp, DRW-04, to choose the target comp.)
  - Src: EBE
  - [SOURCED (EBE); interplay with DRW-04 INFERRED]

## 9. Shape Layer Styles (SLS)

- [ ] **SLS-01 · Shape Layer Style library**
  - Reusable styles shown as thumbnails.
  - Thumbnails show stroke width relative to the min/max of the displayed set.
  - The current style appears as a toolbar button (colour swatch) that shows "your existing shape layer style" and opens the **Shape Layer Style panel** (EBE).
  - Panel layout top → bottom (EBE narration order; only items 1 and 4 are placed with confidence):
    1. the default/preset styles ("up here"); click one to make it current (SOURCED)
    2. **Add style** (SLS-08) and style **import / export** (SLS-19). (I) Placement INFERRED: EBE says "If you click on add style, it's going to bring up this" and then describes creating, importing and exporting, so import/export may be entries of what Add style opens rather than separate panel buttons.
    3. the style settings ("style preferences"), with **data-driven options** that open data-driven range settings (SLS-15, DAT-07). (I) Whether these sit in the main panel or inside the Add-style editor is INFERRED.
    4. the **drawing options** ("down below this"; DRW-02) (SOURCED)
  - Swapping (SLS-11): EBE narrates "click on this little swap shape layer style" while the panel is open and adds "You can also swap them out right here". (I: a toolbar ⇄ next to the swatch, UI-17; toolbar placement not stated.)
  - Src: CL 1.0.3; STADIA; EBE
  - [SOURCED: swatch opens the panel, presets at the top, drawing options at the bottom (EBE); placement of import/export and style settings (panel or Add-style editor) INFERRED; exact controls unknown]

- [ ] **SLS-02 · Fill**
  - Fill color and opacity. GEOlayers 2 styles controlled color, stroke, fill, opacity and extrusion.
  - Src: nofilmschool GL2
  - [INFERRED for v3 field set]

- [ ] **SLS-03 · Stroke**
  - Stroke width and color, plus **stroke outlines** (an outline around the stroke).
  - **Line join defaults to round.**
  - Src: CL 1.0.7
  - [SOURCED]

- [ ] **SLS-04 · Dashes**
  - Dashed strokes, which can have **round caps** (1.2.5).
  - **Dash Speed** animates the dashes (1.2.0).
  - Dashes work under the legacy expression engine (1.2.0).
  - Src: CL 1.2.0, 1.2.5
  - [SOURCED]

- [ ] **SLS-05 · Layer blending modes**
  - Part of the style, and applied on swap too.
  - Src: CL 1.1.2, 1.1.4
  - [SOURCED]

- [ ] **SLS-06 · Preserve Transparency**
  - Style option (the AE layer switch).
  - Src: CL 1.2.0
  - [SOURCED]

- [ ] **SLS-07 · Extrusion**
  - Styles can extrude (3D). Swapping to or from extrusion adapts the anchor point.
  - Src: CL 1.0.7
  - [SOURCED]

- [ ] **SLS-08 · Create custom styles / "Layer Style Presets"**
  - Lessons "Create Custom Layer Styles" and "Create a Layer Style Preset".
  - EBE: an **"Add style"** button in the Shape Layer Style panel. "If you click on add style, it's going to bring up this. You can create an entirely new style." Add style opens a menu/dialog ("bring up this") that offers creating a new style; whether import/export (SLS-19) are entries of it or separate panel buttons is unknown (EBE ambiguous). EBE also mentions "all of your style preferences" for customising a style, including data-driven options (SLS-15); whether they are in the panel or in the Add-style editor is unknown.
  - Editor fields are unknown beyond SLS-02…07.
  - Src: MC; YT:2Ga-6DD2epo; YT:Duglh9VTLes; VM:358244424; EBE
  - [SOURCED; Add style SOURCED (EBE); what it opens (menu/dialog/editor) unknown; editor fields unknown]

- [ ] **SLS-09 · Pick style from a layer or shape content group**
  - Src: CL 1.0.9
  - [SOURCED]

- [ ] **SLS-10 · Create Shape Layer Styles from an image**
  - Src: YT:aELgIVzP_HE
  - [SOURCED; mechanism unknown]

- [ ] **SLS-11 · Swap Shape Layer Style**
  - Applies the current style to selected, already-drawn layers. Blend modes and the extrusion anchor update too.
  - EBE: select the drawn layer, pick another style in the panel, then click the **"swap shape layer style"** button. EBE also says you can swap "right here" in the panel, so (I) a swap control may exist in both places.
  - (I: the toolbar ⇄ next to the swatch, matched by narrated order. EBE narrates the swap while the panel is open and never places the button in the toolbar.)
  - Src: CL 1.0.1, 1.0.7, 1.1.4; STADIA; EBE
  - [SOURCED feature + button name (EBE); toolbar icon INFERRED]

- [ ] **SLS-12 · Filter styles**
  - Filter by name (1.0.4).
  - **Right-click the filter bar** to filter by Mapcomp (1.1.6), shown fullscreen (1.2.6). The filter is saved in the project.
  - Shares the same matrix UI as LBL-18.
  - Src: CL 1.0.4, 1.1.6, 1.2.6
  - [SOURCED]

- [ ] **SLS-13 · Randomize color**
  - Exists (CL 1.0.4 bugfix line "randomize color", no context). Location unknown (I: Shape Layer Style or color presets).
  - Src: CL 1.0.4
  - [SOURCED existence; location INFERRED]

- [ ] **SLS-14 · URL-sourced styles update with one click**
  - Src: CL 1.4.2
  - [SOURCED; C-07]

- [ ] **SLS-15 · Data-driven styles**
  - See DAT-07…12.
  - EBE: the Shape Layer Style panel has **data-driven options** below the presets; clicking them sets up **data-driven ranges** ("pretty insanely deep"; not shown).
  - Src: DAT-07 sources; EBE
  - [SOURCED]

- [ ] **SLS-16 · Line patterns**
  - Patterns that highlight countries, e.g. repeating-line borders, via add_line_pattern(s).jsx (TB-03).
  - Src: VM:434592647; YT:FD6LcXbFes8
  - [SOURCED]

- [ ] **SLS-17 · Import GEOlayers 2 shape layer styles**
  - Src: AES
  - [SOURCED]

- [ ] **SLS-18 · Style rules by feature type (GL2 "Styles & Rules")**
  - GEOlayers 2 could apply styles automatically by feature type. Whether v3 keeps this is unknown.
  - Src: nofilmschool; GL2 titles
  - [INFERRED]

- [ ] **SLS-19 · Import / export Shape Layer Styles**
  - Shape Layer Styles can be **imported** (e.g. a personal library of styles) and **exported** (e.g. to send custom styles to another animator), from the Shape Layer Style panel (EBE). Whether import/export are entries of the Add-style menu/dialog (SLS-08) or separate panel buttons is unknown (EBE ambiguous).
  - Separate from the imagery-style import/export in Preferences (PRF-10).
  - File format, whether single or multiple styles are exported, and duplicate-name handling are unknown. (I: one JSON file holding a list of styles.)
  - Src: EBE
  - [SOURCED (EBE); file format INFERRED]

## 10. Label Templates & template labels (LBL)

- [ ] **LBL-01 · Template labels**
  - **Any AE composition can be a label template.** It has text fields (e.g. "name") filled from feature properties.
  - A label is a precomp layer named **`Label '<name>'`** in the containing comp.
  - Src: AES; HELP L99-117; STADIA
  - [SOURCED]

- [ ] **LBL-02 · Default template set**
  - Names:
    - 01 Place Top, 02 Place Bottom, 03 Place Left, 04 Place Right, 05 Region
    - 06 Place Top 2, 07 Place Bottom 2, 08 Place Left 2, 09 Place Right 2, 10 Region 2
    - 11 Locator, 11 Locator 2
    - "Capital Citi…" (cut off), and more
  - Observed in the VM:466519449 thumbnail (a project with Mapcomps "Main Map", "Main Labels", "Overview Map"). (I: these are the default set; "Capital Citi…" and the duplicate "11" suggest some may be custom.) They form two design families plus Locator and Icon templates. The name "Default Label Templates Project" is a forum snippet (weak).
  - GL2 had a "Box Label Templates" family (VM:230256972 "Box Labels in Hong Kong"); whether v3 defaults descend from it is unknown.
  - Src: VM:466519449 thumbnail; CL 1.0.6; AESF (weak); VM:230256972
  - [SOURCED as observed; "default" status INFERRED; project name weak; list incomplete]

- [ ] **LBL-03 · Template kinds and pointers**
  - Place (four positions), Region, **Locator**, **Icon** (1.0.6).
  - Default templates have **pointers that follow the label anchor point** (1.4.1).
  - Src: CL 1.0.6, 1.4.1
  - [SOURCED]

- [ ] **LBL-04 · Template selector and "Null Layer" entry**
  - See UI-18. API: addLabel(null) adds a null layer.
  - Src: CL 1.20.0
  - [SOURCED]

- [ ] **LBL-05 · Add a label**
  - One-click label from:
    - a search result (tag icon)
    - selected Browser features (feature action bar, FB-46; EBE)
    - manual placement ("Add a Label Manually", "Manually Place Label Templates")
  - The label uses the template selected in the dropdown (UI-18; EBE).
  - Manual placement UI (EBE): a manual-label button (icon unknown) in the toolbar label area that "will give you a little crosshair". Type the label text (e.g. "Atlantic Ocean"), position the crosshair ("before you hit add label, you just put it over here"), then click **"Add label"**. The label is placed "right over your crosshair" and is pinned (LBL-26). (I) Where the crosshair appears (preview map or AE viewer) and how it is positioned (moving the map or dragging the crosshair) are INFERRED.
  - (I: the text field and "Add label" button replace or overlay part of the panel while the tool is active.)
  - "Label Locations Quickly".
  - Src: VM:517178231; YT:5B8p13M0AAI; YT:0SoBCtbF3B0; WAR; EBE
  - [SOURCED; manual tool SOURCED (EBE); its icon unknown; crosshair location, positioning and layout INFERRED]

- [ ] **LBL-06 · "Label Anchor Point" layer**
  - A layer (null) inside the template whose **effect controls** make the label default to **Scale with Map** and/or **Rotate with Map**.
  - EBE: placed labels carry the same pin effect controls as pinned layers (PIN-14), e.g. Scale with Map.
  - Src: CL 1.0.4; YT:y05ttPEuQow; EBE
  - [SOURCED]

- [ ] **LBL-07 · LabelColors comp**
  - Each template has a LabelColors comp. **SHIFT-select** a template to reveal it.
  - (I: used for light/dark mode rigs, linking it to the YT:y05ttPEuQow chapter "Light/Dark Mode Rig".)
  - Src: CL 1.0.6; YT:y05ttPEuQow
  - [SOURCED; light/dark link INFERRED]

- [ ] **LBL-08 · ALT-select a template**
  - Reveals the template comp in the AE Project panel.
  - Src: CL 0.9.0, 1.0.6
  - [SOURCED]

- [ ] **LBL-09 · Responsive timing via markers**
  - Label layers copy and update their **markers** from the template comp (CL 1.0.4 "responsive timing"). (I: these are intro/outro "Responsive Design – Time" markers.)
  - Src: CL 1.0.4; YT:y05ttPEuQow
  - [SOURCED; marker-type link INFERRED]

- [ ] **LBL-10 · Animated label text**
  - Label text can change over time from timed data.
  - Src: CL 1.0.8
  - [SOURCED]

- [ ] **LBL-11 · Label time offsets**
  - Labels can be added with time offsets, optionally data-driven.
  - Src: CL 1.2.3
  - [SOURCED]

- [ ] **LBL-12 · Swap Label Template**
  - Swaps the template of the selected labels **regardless of the selected Mapcomp**.
  - EBE: select a label layer, choose another template in the dropdown (e.g. a "place left" template), then click the **"swap label template"** button next to the dropdown. EBE also lists "swap them if you've already drawn one out" among the dropdown's actions.
  - Swaps AVLayer assets even when the comp has no text layer. Must not crash AE.
  - Src: CL 1.2.3, 1.11.1, 1.14.1; YT:17tHaEJuZNA "Change the Label"; EBE
  - [SOURCED (toolbar button: EBE)]

- [ ] **LBL-13 · Missing-data prompt**
  - If a feature can't provide all of a template's data fields, GEOlayers asks for the missing data.
  - Src: CL 1.1.5
  - [SOURCED]

- [ ] **LBL-14 · "Create Label Templates" button**
  - Located in the Mapcomp Label settings (MCS-19).
  - Src: CL 1.1.2
  - [SOURCED]

- [ ] **LBL-15 · Authoring a new template**
  - "How to Add a New Label Template"; "Create Labels for Multiple Positions"; Essential Graphics use.
  - EBE: "You can add a new template here", from the template dropdown area (UI-18).
  - The exact registration mechanism (folder? naming?) is unknown. (I: "add a new template" registers the selected comp.)
  - Src: YT:y05ttPEuQow; YT:OTzPcJTSlpU; YT:D32kKyXBCyo; EBE
  - [SOURCED; entry point SOURCED (EBE); mechanism unknown]

- [ ] **LBL-16 · Label Template Projects**
  - A Custom Label Template Project can be loaded (1.2.2).
  - importProject(file, isLabelTemplateProject=true).
  - "Create a Label Template Project".
  - EBE: "you can import other existing templates", from the template dropdown area (UI-18).
  - Src: CL 1.2.2; HELP L36-40; YT:y05ttPEuQow; EBE
  - [SOURCED]

- [ ] **LBL-17 · Download online Label Templates**
  - "Download Label Templates" tip; GEOlayers 2 had "Online Label Templates".
  - EBE: from the label area you can "download templates from uh the default geolar projects" (caption for "the default GEOlayers projects"; whether these are dedicated template projects or the default/sample projects is not stated).
  - Where these projects are hosted and how the list is fetched are unknown.
  - Src: YT:0SoBCtbF3B0; n/videos §1; EBE
  - [SOURCED; entry point in the label area SOURCED (EBE), exact placement INFERRED; source = "the default GEOlayers projects" (EBE quote); hosting unknown]

- [ ] **LBL-18 · Assign Label Templates (and Shape Layer Styles) to Mapcomps**
  - A full-panel matrix of checkboxes: rows = templates, columns = Mapcomps, with **"Select all"** / **"Select none"** (VM:466519449).
  - Right-clicking the filter bars filters by Mapcomp (CL 1.1.6). (I: that right-click is how this matrix opens; the video does not show it.) Persisted in the .aep.
  - Described as a "well hidden feature for power users and template creators".
  - (I) Possibly related to the per-Mapcomp "Prefer for label templates / shape layer styles" settings in the Mapcomp List (UI-26; EBE).
  - Src: VM:466519449; YT:krDpSgT2qEg; CL 1.1.6, 1.2.6; EBE
  - [SOURCED]

- [ ] **LBL-19 · Unique label layer names**
  - Labels of the same place get unique names so expressions stay correct.
  - Src: CL 1.5.4
  - [SOURCED]

- [ ] **LBL-20 · Label renaming applies to template labels**
  - Src: CL 1.2.1
  - [SOURCED]

- [ ] **LBL-21 · Placement control**
  - "Fine Tune Label Placement", "Control Labels Precisely", "Control Label Position".
  - Drag/center the label point in Feature Properties (FB-33).
  - Labels follow the position on trimmed linestrings (1.1.4).
  - Src: MC; YT:0SoBCtbF3B0; YT:17tHaEJuZNA; CL 1.1.3, 1.1.4
  - [SOURCED]

- [ ] **LBL-22 · Scaling labels**
  - Src: YT:0SoBCtbF3B0 "Scaling Labels"
  - [SOURCED; UI unknown]

- [ ] **LBL-23 · RTL and Traditional Chinese in templates**
  - Src: CL 1.12.2
  - [SOURCED]

- [ ] **LBL-24 · Labels animated along a path**
  - addLabel accepts a coordinate array (1.19.0) as a separate optional 4th argument: `[lon,lat]` or an array of `[lon,lat]`; options.duration (seconds) sets the path animation length (API-12).
  - Src: CL 1.19.0; GHHELP
  - [SOURCED]

- [ ] **LBL-25 · Import GEOlayers 2 label templates**
  - Src: AES
  - [SOURCED]

- [ ] **LBL-26 · Pinned labels**
  - Labels can be pinned, get the "(pinned)" suffix, and can be unpinned.
  - EBE: manually placed labels (LBL-05) are pinned automatically ("all these labels are pinned") and show the same pin effect controls (PIN-14), e.g. Scale with Map.
  - Src: CL 1.1.3, 1.4.0; EBE
  - [SOURCED]

- [ ] **LBL-27 · Filter Label Templates by name**
  - A name filter bar for the Label Template list (parallel to SLS-12). Separate from the by-Mapcomp matrix (LBL-18).
  - EBE: the template dropdown "can filter them by keyword".
  - Src: CL 1.0.4 "Filter Label Templates and Shape Layer Styles by name"; EBE
  - [SOURCED]

## 11. Pinning / snapping layers (PIN)

- [ ] **PIN-01 · Pin Selected Layers to Map**
  - Toolbar pin button. Pins any selected layer (including text layers and anchor layers) to its geo location.
  - EBE: the first toolbar button, "pin selected layers to map". Pinning **adds a set of effects** to the layer (PIN-14), and the layer then follows map moves.
  - **Clicking the pin button again unpins** and removes all those effects (EBE).
  - EBE: a layer with its own animation (a bouncing icon) keeps animating after pinning while it follows the map ("it's going to move and bounce and look great"). Pinning must not overwrite the layer's existing keyframes or expressions.
  - Not parity: EBE demos with a "Bouncing Map Icons" preset from AE's Effects & Presets ("Elements" category). Nothing says it ships with GEOlayers (I: third-party or course preset), so it is not added to the inventory.
  - Src: CL 1.0.1, 1.0.4, 1.1.3; YT:Dm_1hGYNExQ; AESF; EBE
  - [SOURCED]

- [ ] **PIN-02 · "(pinned)" suffix**
  - Appended to the names of pinned layers and labels.
  - Src: CL 1.4.0
  - [SOURCED]

- [ ] **PIN-03 · Latitude / Longitude / Height properties**
  - Pinned layers get separate Lat, Lon and Height properties (1.1.0; this reduced interpolation inaccuracy).
  - EBE: Height lifts the layer off the map ("bump it up … the height here and like pull it off of the map"). (I: these are among the pin effect controls, PIN-14.)
  - Src: CL 1.1.0; EBE
  - [SOURCED]

- [ ] **PIN-04 · "Position Progress" slider/effect**
  - Drives a pinned layer's animated position along a path/feature, e.g. an arrowhead or vehicle.
  - Src: CL 1.1.2; YT:17tHaEJuZNA
  - [SOURCED]

- [ ] **PIN-05 · Rotate with map**
  - Option for pinned/snapped layers. Respects existing layer rotation (1.2.6 / 1.18.0 fixes).
  - The snap API has scaleWithMap and rotateWithMap (both default true) (API-16).
  - UI (EBE): the options are **effect controls on the pinned layer** (PIN-14), not a panel popover: Scale with Map, pixel offset, Height, bearing, 3D, Rotate with Map. They can be switched after pinning. Rotate along Path and path duration are not mentioned.
  - EBE turns Scale with Map on after pinning, which suggests it is off by default for a pinned layer, unlike the API default (C-22; weak).
  - Src: CL 1.2.6, 1.18.0; GHHELP; EBE
  - [SOURCED; UI = effect controls SOURCED (EBE); UI defaults unknown]

- [ ] **PIN-06 · Pin to a Feature**
  - Lesson "Pin Layers to a Map or Feature". (I: pins to the feature's label point, or along the feature via Position Progress.)
  - EBE: the feature action bar (FB-46) has a pin action for a selected feature ("now I can … pin this layer"). (I: it pins the selected AE layer to that feature.)
  - Src: MC; EBE
  - [SOURCED; behavior INFERRED]

- [ ] **PIN-07 · Pin vs Parent**
  - Pinning makes the layer zoom and rotate with the map in the 3D environment.
  - Parenting to the map/anchor keeps the layer flat. Both workflows are supported.
  - EBE: an ordinary AE shape is not attached to the map: "I've got to parent it or I've got to pin it". Arrows (RTE-06) and draw-on layers (DRW-22) are parented automatically.
  - Src: YT:Dm_1hGYNExQ; MC "Parent Layers to a Map"; EBE
  - [SOURCED]

- [ ] **PIN-08 · Snap layer to Mapcomp ("snapped layers")**
  - API snapLayerToMapcomp(comp, layer, coordinates, [options]) (1.20.0; API-16). Snapped layers follow the viewport's transformations, with optional scale-with-map, rotate-with-map, rotate-along-path, bearing offset, 3D-layer toggle, and an optional path animation when `coordinates` is a path.
  - Snapped layers can rotate with the map (1.18.0 bugfix mentions them).
  - The UI difference between "pin" and "snap" is unknown. EBE shows only the pin button, whose effect controls (PIN-14) mirror the snap options, so (I) "snap" is the API name for the same rig.
  - Src: CL 1.18.0, 1.20.0; GHHELP; EBE
  - [SOURCED API; pin-vs-snap UI difference unknown]

- [ ] **PIN-09 · Get a layer's geo position**
  - getLayersGeoPosition → [lon, lat].
  - Src: HELP L119-124
  - [SOURCED]

- [ ] **PIN-10 · Animate layers along features**
  - Accurate at street scale (1.1.0). "Animate Layer Along Feature Path".
  - Src: CL 1.1.0; YT:17tHaEJuZNA
  - [SOURCED]

- [ ] **PIN-11 · Georeference images and photos**
  - Steps: move the image's anchor point to a known location, navigate the map there, adjust opacity/scale/rotation, then "Parent to Map and Create Map Feature".
  - For photos, paste coordinates (SRC-13) and georeference a null. (I: bulk mode uses ExifTool EXIF→CSV.)
  - Src: YT:EtsCl787YR8; YT:tZ7SVVmgwyE; YT:kA-w8W4eqCM; WAR
  - [SOURCED; tooling INFERRED]

- [ ] **PIN-12 · Anchor-based parenting for footage and particles**
  - Put footage (e.g. clouds) in the containing comp and parent it to the Anchor.
  - Src: AESF dev reply (weak)
  - [SOURCED weak]

- [ ] **PIN-13 · Backward compatibility of older pinned layers**
  - Pinned layers created by older versions must keep working (CL 1.1.4 bugfix "pinned layers from older versions"). The pinned rig changed to separate Lat/Lon/Height in 1.1.0 (PIN-03). (I: the rig carries a version marker and is migrated or tolerated on load.)
  - Src: CL 1.1.0, 1.1.4
  - [SOURCED requirement; mechanism INFERRED]

- [ ] **PIN-14 · Pin effect controls**
  - Pinning adds "all of these effects" to the layer's Effect Controls (EBE). Controls named:
    - **Scale with Map** (on/off)
    - a **pixel offset** (screen-space offset from the pinned point)
    - **Height** (lifts the layer off the map; PIN-03, 3D-14)
    - **bearing** (I: a bearing offset, cf. API `bearingOffset`)
    - **3D** (makes it a 3D layer)
    - **Rotate with Map** (on/off)
  - (I, from EBE phrasing "you can even set it to 3D and make it rotate with the map") Rotate with Map may require, or be grouped with, the 3D switch; confirm hands-on (Q-33).
  - The same controls appear on pinned labels (LBL-26).
  - Unpinning removes them all (PIN-01).
  - Exact effect names, grouping (one pseudo effect or several), defaults and Lat/Lon placement are unknown. (I: Scale with Map off by default for pinned layers, C-22.) The snap API has no pixel-offset option (API-16).
  - Src: EBE
  - [SOURCED control list (EBE); names, grouping and defaults INFERRED]

## 12. Routes & arrows (RTE)

- [ ] **RTE-01 · Car / Bike / Pedestrian routes**
  - Route features between locations for three travel modes.
  - Imported **without Maneuver Points by default** (pref PRF-07).
  - Provider unknown (I: FOSSGIS OSRM).
  - EBE: route types are chosen in the Connect features menu (FB-27). EBE: "And it shows you how it is pulling this data." (I: the menu shows the data source behind each type; could also be a status display. Not named in the transcript.) Full list: Q-50.
  - Src: CL 1.0.4, 1.4.0; AES "Animate driving routes"; EBE
  - [SOURCED; provider INFERRED]

- [ ] **RTE-02 · Create a route by connecting features**
  - The resulting shape layer is named **"<A> - <B> (<distance> mi)"**.
  - EBE: Connect features first creates a route **feature** in the Browser named like "plane route, Afghanistan to Albania", with a distance property (FB-27, FB-41). The "<A> - <B> (… mi)" name was observed on a drawn layer (STADIA). (I: drawing that feature produces the named layer.)
  - Distance units: miles were observed; whether this is a preference is unknown.
  - Src: VM:359745657; STADIA; EBE
  - [SOURCED]

- [ ] **RTE-03 · Connections across the date border**
  - Src: CL 1.1.2
  - [SOURCED]

- [ ] **RTE-04 · Animated route drawing**
  - Trim Paths + duration (DRW-12/13). "Quickly Create and Animate a Route".
  - EBE: arrows from the Draw Arrow tool are created already keyframed (RTE-06).
  - Src: YT:0SoBCtbF3B0; MC "Animate Paths and Routes"; EBE
  - [SOURCED]

- [ ] **RTE-05 · Arrowhead/vehicle along route**
  - Animate a layer as an arrowhead along the feature path, using Position Progress (PIN-04).
  - Src: VM:359745657
  - [SOURCED]

- [ ] **RTE-06 · Arrow tool ("Create Arrows from the toolbar")**
  - "Draw Arrow Tool" creates animated arrows in a few clicks.
  - EBE ("draw arrow" toolbar button, narrated after the label area, UI-17):
    - Clicking it activates AE's **pen tool**; the path you draw becomes an arrow (the caption's "pin tool" means pen tool).
    - The result is a new **arrow shape layer** that is **already animated** (keyframes visible with U).
    - It carries "a ton of different effects" for adjusting it (not named).
    - Its colour comes from the current **Shape Layer Style** (SLS-01).
    - It is **automatically parented to the map**, so it stays in place when pitch or bearing change (PIN-07).
    - The arrow path stays editable afterwards with the pen tool (EBE: "you can also hit the G key and uh you know move this if you want"; G = AE pen tool). (I) The arrow keeps following the edited path, i.e. the animation is driven by the path, not baked.
  - "Feature from layer" on an arrow gives two features: arrowhead shape and arrow line (FB-30).
  - Follow-up steps from tutorials: Retime; Reposition; **Make the Arrow a Map Feature**; **Animate Along Arrow Path**; animate zoom/pitch/bearing; add shadow; 3D look; add a stroke layer style.
  - Tool parameters (head shape, width, curvature), effect names and the animation length are unknown.
  - Src: CL 1.1.7; VM:475011262; YT:rmITTGwVENo; YT:fUD6QdB3ipc; YT:oilXRAW0_CM; EBE
  - [SOURCED (workflow, pre-animation, style colour, parenting: EBE); params and effect names unknown]

- [ ] **RTE-07 · shapes_to_arrows.jsx**
  - Converts shapes to arrows. See TB-07.
  - Src: CL 1.1.7
  - [SOURCED]

- [ ] **RTE-08 · Branching arrows / "3 Ways to Add Arrows"**
  - Src: YT:oilXRAW0_CM; WAR
  - [SOURCED; content unknown]

- [ ] **RTE-09 · Tracks from GPX / KML / TCX as routes**
  - See DAT-05.
  - [SOURCED]

- [ ] **RTE-10 · Plane / straight-line route modes (GEOlayers 2)**
  - GEOlayers 2 had car, bicycle, plane, foot and straight-line modes.
  - **Plane routes exist in GL3** (EBE): "Connect features" offers "plane route" (FB-27), e.g. "plane route, Afghanistan to Albania". (I: a great-circle or arced line.)
  - Whether a separate straight-line mode exists in v3 is unknown.
  - Src: nofilmschool; EBE
  - [SOURCED plane (EBE); geometry and straight-line mode INFERRED]

- [ ] **RTE-11 · Symbols on a path**
  - Src: WAR; YT:mapfwee2cL4
  - [SOURCED; tooling unknown]

## 13. Data import & DataViz (DAT)

- [ ] **DAT-01 · Supported file formats**
  - Sourced in the changelog/help:
    - GeoJSON / JSON
    - GeoJSON **file sequences** (1.1.8)
    - CSV (',' and **';'** 1.2.2)
    - TSV (Browser import, 1.1.5); `.txt` is sourced only as a `watchCsv` input (HELP L165). (I: Browser `.txt` import.)
    - **TCX** (1.2.4)
    - zipped shapefile (search source, 1.5.0)
    - .aep/.aet projects
  - Seen in tutorials: **KML** (Google My Maps, NASA eclipse) and **GPX** (gpx.studio, GPS watches). QGIS and Felt exports arrive as GeoJSON.
  - "Almost every file" can be imported from a URL.
  - TopoJSON and plain shapefile import are unknown.
  - Src: CL; HELP L165; YT:Q-Z2qs21tx8; YT:38ynpb1N4PE; YT:1ayTEAQfbiE; YT:pAsBSu5Vw8I
  - [SOURCED; Browser .txt import INFERRED]

- [ ] **DAT-02 · GeoJSON file sequence import**
  - (I: one file per time step, which yields timed data.)
  - Src: CL 1.1.8
  - [SOURCED; semantics INFERRED]

- [ ] **DAT-03 · CSV / TSV import**
  - Points from lat/lon columns (I: column auto-detection).
  - Property names are trimmed (1.0.3).
  - Line features from CSV can cross the date border (1.2.5).
  - Src: CL 1.0.3, 1.2.2, 1.2.5; AES "import .csv/.tsv"
  - [SOURCED; column rules unknown]

- [ ] **DAT-04 · Remote URL import**
  - CTRL/CMD-click Import File (FB-03). API with a URL that includes the file extension.
  - Src: CL 1.4.2; HELP L178
  - [SOURCED]

- [ ] **DAT-05 · GPS tracks**
  - GPX/KML/TCX tracks become line features for routes. 3D lines get coordinate_elevation.
  - EBE: an imported GPX route's elevation can be charted with Draw Diagram (DAT-14).
  - Src: CL 1.2.4, 1.11.1; YT:-3TPi_9-DS4; EBE
  - [SOURCED]

- [ ] **DAT-06 · Zipped shapefiles**
  - Via the File Search API. Direct import is unknown.
  - Src: CL 1.5.0
  - [SOURCED]

- [ ] **DAT-07 · One-click data-driven styles**
  - Created from a Feature Collection's properties panel. A chosen property drives color, size or other style values.
  - "The Best Way to Create a Data-Driven Style".
  - EBE: the Shape Layer Style panel also has data-driven options that set up **data-driven ranges** (SLS-15). Details not shown.
  - Src: CL 1.0.9; YT:D_X3xN5Uc_Y; VM:358244424; EBE
  - [SOURCED]

- [ ] **DAT-08 · Gradient preview**
  - Data-driven color is previewed with a gradient in the UI.
  - Src: CL 1.2.1
  - [SOURCED]

- [ ] **DAT-09 · Interpolation curves**
  - **"Circle Area"** curve for points (1.0.7). Other curves are unknown (I: linear).
  - Src: CL 1.0.7
  - [SOURCED]

- [ ] **DAT-10 · Timed data and animated data-driven styles**
  - Properties with time series animate styles (1.0.8). Feature Properties shows the timed-data duration (1.1.4).
  - The property naming convention is unknown (I: `prop:<date>`, cf. `confirmed:latest` in EX).
  - Tutorial: racetrack telemetry synced to on-board footage.
  - Src: CL 1.0.8, 1.1.4; VM:373569335; VM:376376225; EX:COVID-19_*
  - [SOURCED; convention INFERRED]

- [ ] **DAT-11 · Non-linear keyframe timing**
  - Keyframes created by data-driven properties can have non-linear timing.
  - Src: CL 1.2.4
  - [SOURCED]

- [ ] **DAT-12 · Data-driven time offsets**
  - For drawing (DRW-20) and labels (LBL-11), e.g. earthquakes by date.
  - EBE: a collection's Feature Properties view lets you set how each property is treated, e.g. to drive the timing (FB-33).
  - The thumbnail shows a date-timeline overlay (I: user-built).
  - Src: CL 1.2.3; VM:533574071; EBE
  - [SOURCED]

- [ ] **DAT-13 · Data join via CSV/TSV**
  - Collection properties export/import (FB-35).
  - Src: CL 1.1.5
  - [SOURCED]

- [ ] **DAT-14 · Property Diagrams**
  - Diagrams of feature property values (1.2.6).
  - EBE: two entry points:
    - the toolbar **"Draw Diagram"** button (fourth narrated; I: the bar-chart icon)
    - **"Draw property charts"** in the feature action bar (FB-46)
  - Example: the elevation profile of a route from an imported **GPX** file (DAT-05; I: from `coordinate_elevation`, FB-39).
  - Chart types, options and output form are unknown. (I: charts drawn as AE shape layers.)
  - Src: CL 1.2.6; EBE
  - [SOURCED (entry points + GPX elevation example: EBE); output INFERRED]

- [ ] **DAT-15 · Choropleth Mapcomp (imagery dataviz)**
  - "Create a New Choropleth Mapcomp", then **connect a GeoJSON** (export the features first).
  - Polygons are rendered as **raster imagery**, not shape layers, to handle large datasets.
  - Src: CL 1.1.8; YT:Vq6q-X3KVgo; YT:JcCTGlszHcw; YT:aELgIVzP_HE; MC
  - [SOURCED]

- [ ] **DAT-16 · Circles imagery (dataviz)**
  - Proportional circles rendered as imagery.
  - Src: CL 1.2.2; AESF
  - [SOURCED]

- [ ] **DAT-17 · Heatmap imagery (dataviz)**
  - Named by the vendor in the forum; **not in the changelog** (C-06).
  - Src: AESF (≈comment/30836)
  - [SOURCED weak]

- [ ] **DAT-18 · Lines imagery (dataviz)**
  - Src: CL 1.20.0
  - [SOURCED]

- [ ] **DAT-19 · Live data**
  - watch / watchCsv poll files (default 10000 ms) and feed updates to scripts.
  - Src: HELP L153-170
  - [SOURCED]

- [ ] **DAT-20 · Scripted data join**
  - fetchJson → mutate properties → set `GEOlayersDisplayName` / `source` → addToBrowser.
  - Src: EX:COVID-19_*
  - [SOURCED]

- [ ] **DAT-21 · Filter by data**
  - See FB-06.
  - [SOURCED]

- [ ] **DAT-22 · Large-dataset guidance**
  - Thousands of features as layers hang AE. Use dataviz imagery styles instead.
  - Src: ACOM:11865951; AESF (weak)
  - [SOURCED (ACOM); advice weak]

## 14. 3D (3D)

- [ ] **3D-01 · Pitch and bearing ("Animate in 3D space")**
  - Keyframeable tilt and rotation of the map.
  - Src: HELP; AES
  - [SOURCED]

- [ ] **3D-02 · Pseudo-3D rig (no camera)**
  - Pitch and bearing are rendered into the imagery. Overlays follow through the expression-driven anchor/MapPivot.
  - Src: ACOM:14019807; CL 1.1.8
  - [SOURCED observation; mechanism INFERRED]

- [ ] **3D-03 · Draw 3D buildings (toolbar)**
  - Extruded buildings from MapTiler data. Extrusion is independent of zoom (1.0.5). Renderer becomes CINEMA 4D.
  - (I: OpenMapTiles `render_height`.)
  - Also available: the vendor Overpass "Buildings_OSM" script.
  - EBE (no MapTiler premium plan) narrates no 3D-buildings button. (I: the toolbar cube icon, shown only with a premium plan, UI-28.)
  - Src: HELP L395; CL 1.0.5, 1.5.2; EX:Buildings_OSM; EBE
  - [SOURCED]

- [ ] **3D-04 · Extruded features, shadows, lights**
  - Extrusion via styles (SLS-07); cast/accept shadows (DRW-17/18).
  - GEOlayers 2 added lights for depth.
  - Src: CL; MC "How to Use Shadows"; nofilmschool
  - [SOURCED]

- [ ] **3D-05 · 3D Landscape Mapcomp**
  - Created from real elevation data with a **"Create 3D Landscape"** button in the panel, usable before any Mapcomp exists. The result is a distinct project type with its own view navigation (INDIETIPS, weak; AESLEARN chapter "Create 3D Landscape Mapcomp"; YT:WUhJe_9DQ4Y).
  - [SOURCED (EBE)] The button is in the toolbar (third narrated; I: the mountain icon). Its dialog has:
    - a **Texture Size** choice
    - a **Height Map Size** choice
    - a list of third-party renderer plugins (3D-06), one of which is **required** ("you need one of these third-party plugins. So, any of these will work")
  - The single "resolution preset" ("Ultra HD 4K", INDIETIPS weak) is probably one of these two size choices. Their values are unknown. Nothing suggests a plugin-free option.
  - [SOURCED GL3] The heightmap uses AE's **"Calculations"** effect; this must not change the project color space (1.5.2). The work area duration is set on creation (1.0.1). Renderers: 3D-06. "Create landscape edge": 3D-09.
  - [INFERRED for GL3 (from GL2)] Structure: a 3D Landscape comp containing **texture comp(s)** and **height-map comp(s)** (resizable, e.g. 4500 px in Mettle's GL2 360° tutorial).
  - Src: CL 1.0.1, 1.5.2; METTLE; Mettle GL2 360° [GL2]; YT:WUhJe_9DQ4Y; AESLEARN; INDIETIPS (weak); EBE
  - [SOURCED (button + dialog fields: EBE); size values unknown; comp structure GL2-sourced/INFERRED]

- [ ] **3D-06 · 3D landscape renderer plugins**
  - **Helium (X)** (1.5.0): works with AE cameras/lights, shadows, DoF.
  - **Mettle FreeForm Pro**
  - **Rowbyte Plexus (3)**: Point/Line/Facet looks; EBE: "a little bit more of like a vector look"
  - **Trapcode Mir**
  - EBE: the Create 3D Landscape dialog lists these plugins (Plexus 3 and Helium named) and needs one of them.
  - Src: CL 1.5.0; AES; METTLE; editingcorp; EBE
  - [SOURCED]

- [ ] **3D-07 · Landscape controls**
  - Chapters "The Controls", "Increase the Elevation Height", "Fix the Background" (AESLEARN / YT:WUhJe_9DQ4Y); "Adjust landscape elevation" (METTLE); "Value Range" in the Height Map comp (GL2).
  - For Helium, the elevation slider sits in the Effect Controls of a pink layer named **"Helium"** (INDIETIPS, weak). (I: other plugins get a control layer named after the plugin.)
  - Other control names are unknown.
  - Src: YT:WUhJe_9DQ4Y; AESLEARN; METTLE; INDIETIPS (weak)
  - [SOURCED; "Helium" layer weak; other names unknown]

- [ ] **3D-08 · Elevation (DEM) source**
  - DEM tiles are fetched during finalize. The provider is unknown.
  - (I: MapTiler terrain-rgb-v2 and/or AWS Terrarium; GEOlayers 2 used Mapzen.)
  - Src: CL 1.2.5; MT3 "3D terrain"; AESF
  - [SOURCED existence; provider INFERRED]

- [ ] **3D-09 · Sliced landscape / landscape edge**
  - Steps: "Create landscape edge" (sidewalls), keyframe, finalize, draw features and a displaced label on it.
  - Src: VM:361319119; METTLE
  - [SOURCED]

- [ ] **3D-10 · Globe and Equal Earth projection "via Plugin"**
  - Prerelease page, build 984 (1.20.0). Mechanism unknown (I: a native AE effect plugin).
  - Before this, GEOlayers was Mercator only.
  - Behaviour reference only: the vendor's sibling product GEOlayers.app shipped a one-setting "Switch Your Map to a Globe Projection" in 2026 (YT:UqHnzr2NbzQ 2026-09-07, YT:lcyUlF3iids 2026-07-14, YT:zeZSD_BJ8S4). These are the only public demos of the intended flat-map → 3D-globe behaviour. (I: GL3's plugin behaves similarly; GEOlayers.app itself stays out of scope, LIC-17.)
  - Src: CL 1.20.0; AESF comment/9997 (weak); YouTube @GEOlayersAPP / @MarkusGEOlayers
  - [SOURCED existence; behaviour INFERRED]

- [ ] **3D-11 · Globe sample project**
  - Uses the free Video Copilot **Orb** plugin to make geo-accurate 3D globes.
  - Src: CL 1.0.4; VM:519605279; YT:m5bMw8Fq9q4
  - [SOURCED]

- [ ] **3D-12 · Create 3D Scene & Camera**
  - A default script (TB-13). Creates a real AE 3D camera setup that matches the map.
  - Supports multiple 3D scenes. The tutorial explains the 3D renderers.
  - EBE lists "the 3D scenes" among the default scripts in the toolbar Scripts list.
  - Src: CL 1.12.2; YT:NDqpEe3G2yc; EBE
  - [SOURCED]

- [ ] **3D-13 · 3D models in the map (compatibility, not a parity feature)**
  - Native AE 3D model layers (GLB), environment light and shadow catcher are After Effects features (AE 24+). (I) They work together with the GEOlayers 3D Scene & Camera script (3D-12; the tutorial chapter "GEOlayers' Native Script for 3D Workflows"). GLB import itself is **not** counted as a parity feature.
  - Src: YT:zS11R5Cdszc
  - [SOURCED tutorial; GEOlayers role INFERRED]

- [ ] **3D-14 · Pinned layer Height**
  - Height property (PIN-03), e.g. a rocket trajectory moved in 3D. EBE: a pin effect control that lifts the layer off the map (PIN-14).
  - Src: CL 1.1.0; YT:sxh0aGwXfQI; EBE
  - [SOURCED]

- [ ] **3D-15 · Renderer handling**
  - Classic 3D observed on Mapcomps (the AE default; whether GEOlayers sets it is unknown, MCS-05). CINEMA 4D for extrusions (auto). The Advanced 3D renderer is explained in tutorials.
  - Src: STADIA; CL 1.5.2; YT:NDqpEe3G2yc
  - [SOURCED]

- [ ] **3D-16 · Heightmap and labels aligned with the landscape texture**
  - 3D Landscape heightmaps and labels must line up with the texture, with no offset (CL 1.1.8 bugfix "3D Landscape heightmaps/labels offset in AE 17.5").
  - Src: CL 1.1.8
  - [SOURCED]

## 15. Toolbar default scripts (TB)

- [ ] **TB-01 · Run scripts from the toolbar**
  - A scripts menu lists .jsx scripts. EBE: a toolbar button (fifth narrated; I: the document icon) opens "a list of different actions": the helpful scripts "pre-installed … by default". You can also "write your own script and then run it right here".
  - Default scripts named by EBE: line pattern (TB-03), transitions in and out (TB-04), blinking transition (TB-08), distance circles (TB-11), 3D scenes (TB-13), basic staggers (TB-09).
  - **ALT/OPT-click** a script shows its description. **CTRL/CMD-click** reveals the file.
  - Src: CL 1.0.4, 1.2.0; EBE
  - [SOURCED; icon INFERRED]

- [ ] **TB-02 · Script location**
  - Default scripts ship with GEOlayers ("pre-installed … by default", EBE). EBE: "or you can write your own script and then run it right here" (I: user scripts appear in the same list). (I: user scripts live in the GEOlayers User Folder, API-38.) Description source unknown (I: header comment).
  - Src: CL; HELP L341; EBE
  - [default scripts SOURCED (EBE); user scripts in the same list INFERRED; folder INFERRED]

- [ ] **TB-03 · add_line_pattern.jsx / add_line_patterns.jsx**
  - Adds line patterns to selected layers. Must work when the layer already has effects.
  - The filename is spelled two ways (C-03). EBE lists "a line pattern" in the default scripts.
  - Src: CL 1.2.0, 1.4.0; VM:434592647; EBE
  - [SOURCED]

- [ ] **TB-04 · blend_in_and_out.jsx**
  - Fades/blends layers in and out. EBE lists "transitions in and out" in the default scripts.
  - Src: CL 1.2.0; EBE
  - [SOURCED; params unknown]

- [ ] **TB-05 · north_needle.jsx**
  - A north needle/compass (I: bound to bearing). (I: the tip "Turn Something into a Compass" (YT:2Ga-6DD2epo) uses this script to convert the selected layer into a compass.)
  - Src: CL 1.2.0; YT:2Ga-6DD2epo
  - [SOURCED existence; binding + tip mapping INFERRED]

- [ ] **TB-06 · scale_bar.jsx**
  - A map scale bar; observed label "90 m" (VM:468890858). (I: switches to km and updates with zoom.) "Find Pixel Width for 1k" (YT:h-mJiAIydXI).
  - Src: CL 1.1.7; VM:468890858; YT:h-mJiAIydXI
  - [SOURCED existence + "90 m"; km/zoom behaviour INFERRED]

- [ ] **TB-07 · shapes_to_arrows.jsx**
  - Converts selected shape paths into arrows.
  - Src: CL 1.1.7
  - [SOURCED; params unknown]

- [ ] **TB-08 · blink.jsx**
  - Blinking animation. EBE lists "a blinking transition" in the default scripts.
  - Src: CL 1.0.7; EBE
  - [SOURCED; params unknown]

- [ ] **TB-09 · stagger_layers.jsx**
  - Staggers layer timing. EBE lists "basic staggers" in the default scripts.
  - Src: CL 1.0.7; EBE
  - [SOURCED; params unknown]

- [ ] **TB-10 · precompose_feature_shape_layer.jsx**
  - Precomposes drawn feature shape layers, e.g. for rasterizing (DRW-26).
  - Src: CL 1.4.0; YT:JTQ4aniKPek
  - [SOURCED]

- [ ] **TB-11 · Distance Circle**
  - Draws a distance circle around a point/view center (I: geodesic). Has **Parameters** and a **vertex count**; circles can be duplicated. EBE lists "distance circles" in the default scripts.
  - Src: CL 1.14.0; YT:3ih7TuYiZow; EBE
  - [SOURCED; geodesic INFERRED; param names unknown]

- [ ] **TB-12 · Draw Mapcomp View**
  - Draws another Mapcomp's view extent as a rectangle feature on an overview/inset map. Renders at higher zooms (1.2.2 fix).
  - Locator/inset map workflow.
  - Src: CL 1.1.6, 1.2.2; VM:468873758; YT:qwKj1lOyNtU; MC
  - [SOURCED]

- [ ] **TB-13 · Create 3D Scene & Camera**
  - See 3D-12. EBE lists "the 3D scenes" in the default scripts.
  - Src: CL 1.12.2; EBE
  - [SOURCED]

- [ ] **TB-14 · autorun.jsx**
  - Runs automatically when a project is opened (I: via the Projects panel).
  - Src: CL 1.0.9
  - [SOURCED]

- [ ] **TB-15 · Built-in toolbar actions (non-script)**
  - SOURCED as toolbar actions:
    - Pin/Unpin selected layers (PIN-01; CL 1.1.3; EBE)
    - Create new Controller Mapcomp (MCS-28; EBE)
    - Create 3D Landscape (3D-05; EBE)
    - Draw Diagram / Property Diagrams (DAT-14; EBE)
    - Run scripts (TB-01; CL 1.0.4; EBE)
    - Label template dropdown, manual label, Swap Label Template (UI-18, LBL-05, LBL-12; EBE)
    - Draw Arrow (RTE-06; CL 1.1.7; EBE)
    - Create a Shape Layer to Draw On (DRW-22; EBE)
    - Shape Layer Style button + Swap Shape Layer Style (SLS-01, SLS-11; EBE)
    - Watermask Mapcomp (IMG-17; HELP L393) and 3D buildings (3D-03; HELP L395): not narrated by EBE; (I) shown only with a MapTiler premium plan (UI-28)
  - Order and icon mapping: UI-17.
  - Src: HELP L393-395; CL 1.0.4, 1.1.3, 1.1.7; STADIA; EBE
  - [SOURCED list; icon mapping partly INFERRED]

- [ ] **TB-16 · Vendor community script repos**
  - Public "geodata" and "development" repos with scripts like Buildings_OSM, Railroads_OSM, Hospitals_OSM, Charging_Stations_OSM, COVID-19 casedata (USA, World), Historical_Basemaps_Browser, Autobahn_*, Search_Amenity_in_bbox.
  - Two more in GEODATA, not mirrored locally: **"RKI COVID19 Bundeslaender Data import.jsx"** and **"RKI COVID19 Landkreise Data import.jsx"** (ArcGIS Hub GeoJSON URLs; namingProp `LAN_ew_GEN` / `GEN`; attribution "Bundesamt für Kartographie und Geodäsie / Robert Koch-Institut"). They call `addToBrowser(url, {namingProp, name})` (API-23 overload).
  - Parity test corpus: all 13 scripts must run unmodified against our API (with our namespace alias, API-01).
  - Src: EX:*; GEODATA (api.github.com/repos/GEOlayers/geodata/git/trees/main?recursive=1)
  - [SOURCED]

## 16. Projects, sample projects & templates (PRJ)

- [ ] **PRJ-01 · Projects panel**
  - Opened from the panel's context/flyout menu: the entry directly under Support (UI-19; EBE). Create a new project there, or start from a sample.
  - EBE calls it the "projects dialogue box". It lists:
    - **recent projects** at the top
    - **sample projects** below them (PRJ-02)
    - an **Import Project** action (PRJ-04)
  - Src: HELP L13, L23; EBE
  - [SOURCED]

- [ ] **PRJ-02 · Sample projects**
  - **Globe** (needs VC Orb)
  - **Coronavirus animated Timeline** (1.0.8)
  - **Elevation** (1.1.2)
  - **3D Landscape** (used in "Bikepark Tracks")
  - Showcase clips probably match bundled samples (INFERRED): Zugspitze, Galathea Depth, Singapore, Snow Globe, New York Comic, Madrid–Rome, Halftone, Flood Animation, Empire State Building, Berlin–NY.
  - GL2 sample projects "Folded Map" (unfolding paper map, VM:248306385) and "Elevation 4K" (Trapcode Mir, VM:243509982). (I: v3's "Elevation" sample may derive from "Elevation 4K".) Whether v3 ships descendants is unknown.
  - EBE: there are "a bunch" of samples in the Projects dialog. Some need third-party plugins; some are not kept up to date and are set up differently, so their controls differ. Usage directions are left inside the projects as **markers on layers**. No sample names are read out.
  - Src: CL; VM:519605279, VM:425849140, VM:404914688, VM:248306385, VM:243509982; Vimeo API v2; EBE
  - [SOURCED; showcase mapping and GL2 lineage INFERRED]

- [ ] **PRJ-03 · Download-with-browser button for samples**
  - Src: CL 1.0.3
  - [SOURCED]

- [ ] **PRJ-04 · Open/import projects through the panel**
  - Sorts items into the GEOlayers structure, uniquifies comp names and updates expressions ("How to Import a Project").
  - EBE: the Projects dialog has an **Import Project** action for bringing one GEOlayers project into another. Plain AE drag-and-drop of an .aep "duplicate[s] … geollayers items and it can mess up your project", so Import Project is the supported way.
  - Src: HELP L36-40; AESF; YT:gfXl9-7oZjY; EBE
  - [SOURCED]

- [ ] **PRJ-05 · Project-open hooks**
  - GeoJSONs beside the project auto-load (FB-18). autorun.jsx runs (TB-14).
  - Src: CL 1.0.9
  - [SOURCED]

- [ ] **PRJ-06 · Label Template projects**
  - See LBL-16 / LBL-17.
  - [SOURCED]

- [ ] **PRJ-07 · GEOlayers 2 asset import**
  - Server profiles, shape layer styles and label templates import. **GL1/GL2 projects are not supported.**
  - UI location unknown.
  - Src: AES
  - [SOURCED]

- [ ] **PRJ-08 · Meaning of "template" for parity**
  - Per the user's definition, "template" = whatever the original ships as reusable starting material:
    - Label Templates (+ Default Label Templates Project)
    - Shape Layer Styles / Layer Style Presets
    - Imagery styles (incl. Universal Raster, Seamless Texture)
    - sample projects
    - default scripts
  - All of these must be recreated.
  - [INFERRED]

- [ ] **PRJ-09 · Quick Start Buttons**
  - See UI-22.
  - [SOURCED]

- [ ] **PRJ-10 · Tutorial lessons that may hide features (check during hands-on)**
  - Masterclass lessons with no inventory ID: "Improve Playback Performance", "Animating Flags", "Stylize With Adjustment Layers", "How to Use Mattes", "Simple Transitions", "Animate Labels". (I: preview quality / sync / labels-off for performance → FIN-07, UI-13, UI-08; watermask as matte → IMG-17; label intro/outro markers → LBL-09.) None is known to be a separate GEOlayers feature.
  - Src: MC curriculum (boonelovesvideo.teachable.com/courses/1932860/lectures/44411996)
  - [SOURCED titles; feature mapping INFERRED]

- [ ] **PRJ-11 · "Revision Workflow" and "How to Export Your Map Animation"**
  - Chapters of YT:dBqZs9uT6Hw "Create Your First Map in GEOlayers Step-by-Step". Whether a GEOlayers feature is behind either is unknown. (I: revision = re-finalize after changes; export = AE render queue, cf. FIN-06.)
  - Src: YT:dBqZs9uT6Hw; n/videos §4
  - [SOURCED titles; content unknown]

## 17. Preferences (PRF)

- [ ] **PRF-01 · Default comp specs**
  - Size, duration and framerate for new comps. (I: in the "composition settings" group EBE mentions, PRF-20.)
  - Src: CL 1.1.5; EBE
  - [SOURCED]

- [ ] **PRF-02 · Default language**
  - The preview map is displayed in this language (CL 1.1.8). (I: also the initial Language of new Mapcomps.)
  - Src: CL 1.1.8
  - [SOURCED preview; new-Mapcomp use INFERRED]

- [ ] **PRF-03 · Default Features loaded on startup**
  - Accepts files and http/https URLs.
  - Src: CL 1.1.7, 1.2.2
  - [SOURCED]

- [ ] **PRF-04 · Preload features for search on startup**
  - Src: CL 1.2.2
  - [SOURCED]

- [ ] **PRF-05 · Feature preview shows bounding boxes**
  - Src: CL 1.2.2
  - [SOURCED]

- [ ] **PRF-06 · Fallback online search service as default**
  - Src: CL 1.0.2
  - [SOURCED]

- [ ] **PRF-07 · Routes: import Maneuver Points** (default **off**)
  - Src: CL 1.4.0
  - [SOURCED]

- [ ] **PRF-08 · Drawing: lock shape layer transform properties**
  - Src: CL 1.2.6, 1.15.2
  - [SOURCED]

- [ ] **PRF-09 · Label renaming dictionaries**
  - Can be loaded from files (format unknown). Applies to Mapcomp labels and template labels.
  - Src: CL 1.2.1, 1.2.5
  - [SOURCED]

- [ ] **PRF-10 · Custom imagery styles: import / export multiple**
  - Src: CL 1.2.2
  - [SOURCED]

- [ ] **PRF-11 · MapTiler API key field**
  - At the **bottom of Preferences**. Replaces the default/trial key. Shows an "Invalid MapTiler API Key" error.
  - Src: MTD; MTG; YT:N0Lqfk0iJ_M
  - [SOURCED]

- [ ] **PRF-12 · Default Imagery Quality**
  - Values unknown (I: a percentage like Quality 80%).
  - Src: CL 1.15.1
  - [SOURCED]

- [ ] **PRF-13 · MapTiler Search worldview**
  - (I: values default/auto/ae/ch/jp/kr/us, from the MapTiler API.)
  - Src: CL 1.18.0; n/providers §1.2
  - [SOURCED; values INFERRED]

- [ ] **PRF-14 · Enable/disable GEOlayers Mapdata (green icon)**
  - Legacy. (I: superseded by the MapTiler key.)
  - Src: CL 1.0.7
  - [SOURCED]

- [ ] **PRF-15 · CartoDB API key**
  - Location unknown (Preferences or the style dialog).
  - Src: CL 1.19.0
  - [SOURCED existence]

- [ ] **PRF-16 · Search sources configuration**
  - File/Folder/GitHub sources with URLs (I: in prefs, in the "feature search" settings EBE mentions, PRF-20).
  - Src: CL 1.2.2, 1.5.4; EBE
  - [INFERRED location]

- [ ] **PRF-17 · Hidden: Feature Cache Paths**
  - Src: CL 1.0.7
  - [SOURCED]

- [ ] **PRF-18 · Hidden: purge Undo Cache on each host call**
  - Src: CL 1.5.3
  - [SOURCED]

- [ ] **PRF-19 · Per-project settings stored in the .aep**
  - Drawing options; Mapcomp filters for styles/labels.
  - Src: CL 1.1.8, 1.2.6
  - [SOURCED]

- [ ] **PRF-20 · Other preferences**
  - Covered by "Project Setup & Preferences" (Thong) and GEOlayers 2's "09 Preferences".
  - EBE: Preferences is the third panel-menu entry, below Projects (UI-19). There are "a ton of different preferences" (not enumerated). Groups named:
    - feature search (I: PRF-03/04/06/13/16)
    - composition settings (I: PRF-01)
    - how animations are handled (I: e.g. the duration prompt, VIEW-11)
    - Mapcomp icons and settings, with visibility on/off toggles (PRF-21)
    - "how certain things operate"
  - The full list and layout are still unknown.
  - Src: YT:UxP_B6Lco00; EBE
  - [SOURCED existence and groups (EBE); contents unknown]

- [ ] **PRF-21 · Mapcomp icon visibility**
  - Preferences include "all of your map comp icons and settings": "You can turn the … visibility of these on and off" (EBE).
  - Which icons is not stated. (I: the per-row icons and toggles in the Mapcomp List, UI-04/08/09/26/27, or Mapcomp-related buttons in the panel.)
  - Src: EBE
  - [SOURCED existence (EBE); scope INFERRED]

## 18. Scripting API (API) — global `geolayers3` (ExtendScript)

Every function below must keep the same name, argument order, defaults, return value and async callback contract. HELP is the authority.

- [ ] **API-01 · Global object and runtime**
  - `geolayers3` is available in AE's ExtendScript engine. **The panel must be open.**
  - "Get in touch for automation needs" (vendor).
  - The vendor's scripting examples archive has moved to `geolayers3-v2-prod.web.app/scriptingApiExamples.zip` (GHHELP). **Do not download it** (hard rule); the bundled help may be hosted on the same host, which may also serve product code, so reading it needs a user decision (OPEN_QUESTIONS).
  - Src: HELP L29-30; GHHELP
  - [SOURCED]

- [ ] **API-02 · `comp` argument convention**
  - "A Mapcomp name, Mapcomp or comp that contains a Mapcomp".
  - Src: HELP passim
  - [SOURCED]

- [ ] **API-03 · Async convention**
  - Node-style `callback(error, data)`; `error.message` holds the text. Work after an async call must go inside the callback.
  - Src: HELP L49, L133; EX:COVID-19_*
  - [SOURCED]

- [ ] **API-04 · `importProject(file[, isLabelTemplateProject])`**
  - `file`: a File (.aep/.aet).
  - Sorts the imported items into the GEOlayers structure, uniquifies comp names and updates expressions.
  - `isLabelTemplateProject`: true when the project only contains label templates.
  - Returns: unknown.
  - UI: the Projects dialog's Import Project action (PRJ-04). EBE: plain drag-and-drop duplicates GEOlayers items instead.
  - Src: HELP L36-40; CL 1.1.6; EBE
  - [SOURCED]

- [ ] **API-05 · `finalize(comps, callback[, options])`**
  - `comps`: one, or an array of, names / Mapcomps / containing comps; **undefined = all Mapcomps**.
  - `callback(error, mapcomps)` is called after **all** finalizations finish.
  - `options` booleans: `onlyCurrentFrame`, `previewQuality`, `onlyWorkArea`, `purgeImageryCache`.
  - UI: CTRL/CMD-click on Finalize = `onlyCurrentFrame` (FIN-02; EBE).
  - Src: HELP L42-49; EBE
  - [SOURCED]

- [ ] **API-06 · `duplicate(comp[, options])`**
  - Duplicates a Mapcomp or containing comp and updates related expressions.
  - `options.newName` (Mapcomp) or `options.newContainingCompName` (containing comp).
  - Returns the duplicate, or undefined on failure.
  - Src: HELP L51-57
  - [SOURCED]

- [ ] **API-07 · `setViewAtTime(comp, view[, forceKeyframe[, time]])`**
  - `view` = {latitude, longitude, zoom, bearing, pitch} (floats).
  - `forceKeyframe` keys a control even if it is not yet keyframed.
  - `time` is containing-comp time; default is the current time.
  - Returns the applied view, or undefined.
  - Src: HELP L59-68
  - [SOURCED]

- [ ] **API-08 · `fitViewAtTime(comp, bbox[, forceKeyframe[, time]])`**
  - `bbox` = [lonMin, latMin, lonMax, latMax] (WGS84).
  - Returns the view, or undefined.
  - Src: HELP L70-79
  - [SOURCED]

- [ ] **API-09 · `removeMapcompControlKeys(comp)`**
  - Removes all keys from the Lat/Lon/Zoom/Bearing/Pitch controls.
  - Src: HELP L81-85
  - [SOURCED]

- [ ] **API-10 · `getMapcomps()`**
  - Returns all Mapcomps.
  - Src: HELP L87-88
  - [SOURCED]

- [ ] **API-11 · `getMapcompAnchor(mapcomp)`**
  - Returns the anchor layer in the containing comp.
  - Src: HELP L90-94
  - [SOURCED]

- [ ] **API-12 · `addLabel(comp, labelTemplateComp, labelData[, coordinates[, options]])`**
  - `labelTemplateComp`: a comp, a template name, or an integer (the template's position in the project). **`null` adds a null layer** (1.20.0).
  - `labelData` must contain `latitude`|`lat` and `longitude`|`lon` (numbers), plus the template's string fields (e.g. `name`).
  - `coordinates` (optional, 4th argument, 1.19.0): `[lon,lat]` or an array of `[lon,lat]` defining a path the label animates along.
  - `options.duration`: seconds of the path animation when `coordinates` is given (no default documented).
  - Returns the label layer, or undefined.
  - Src: HELP L99-107; CL 1.19.0, 1.20.0; GHHELP (third-party transcription of bundled help, verified 2026-11-19)
  - [SOURCED]

- [ ] **API-13 · `getLabelTemplateComps()`**
  - Returns the template comps, or [].
  - Src: HELP L96-97
  - [SOURCED]

- [ ] **API-14 · `updateLabelData(labelLayer, newLabelData)`**
  - Updates a template label's string data. Returns the layer, or undefined.
  - Src: HELP L109-117
  - [SOURCED]

- [ ] **API-15 · `getLayersGeoPosition(layer)`**
  - For a layer in a containing comp, returns [longitude, latitude], or undefined.
  - Src: HELP L119-124
  - [SOURCED]

- [ ] **API-16 · `snapLayerToMapcomp(comp, layer, coordinates[, options])`**
  - Attaches any layer to a Mapcomp so it follows the viewport's transformations, optionally moving along a path (1.20.0).
  - `coordinates`: `[lon,lat]` or an array of `[lon,lat]` (a path).
  - `options`: `scaleWithMap` (default true), `rotateWithMap` (true), `rotateAlongPath` (true), `bearingOffset` (degrees, 0), `threeDLayer` (true), `duration` (seconds; used when `coordinates` is a path; no default documented).
  - Returns the snapped layer, or undefined on failure.
  - The UI pin button exposes these as effect controls (PIN-14), plus a pixel offset that has no API option. The UI default of Scale with Map may differ from this API default (C-22).
  - Src: CL 1.20.0; GHHELP (unmerged third-party transcription, verified 2026-11-19); EBE
  - [SOURCED]

- [ ] **API-17 · `fetchText(url, callback[, initObject])`**
  - HTTP from the panel. `callback(error, text)`. `initObject` = a WHATWG fetch init.
  - Src: HELP L126-133
  - [SOURCED]

- [ ] **API-18 · `fetchJson(url, callback[, initObject])`**
  - Same as fetchText, but `callback(error, json)`.
  - Src: HELP L135-141
  - [SOURCED]

- [ ] **API-19 · `geocode(searchString, callback[, options])`**
  - Uses Nominatim, queued per its usage policy; no bulk geocoding.
  - `options.language` = ISO 639; default is the selected Mapcomp's language.
  - `callback(error, features[])` with GeoJSON features.
  - Src: HELP L143-151; CL 1.0.1
  - [SOURCED]

- [ ] **API-20 · `watch(fileOrFolder, callback[, options])`**
  - Callback: `cb(file)` for a file, or `cb(changedFiles, allFiles)` for a folder.
  - `options.interval` in ms, default **10000**.
  - HELP verbatim: "If a file or folder is allready being watched it will be canceled". (I) Single slot: calling watch again replaces the existing watch. Ambiguity: re-watching the same file may cancel it rather than restart it. `unwatch()` "Stops the current watch task" supports the single-slot reading.
  - If the API is busy (e.g. finalizing), the callback is deferred to the next interval.
  - Src: HELP L153-159
  - [SOURCED; replacement semantics INFERRED]

- [ ] **API-21 · `watchCsv(csvFile, callback[, options])`**
  - Accepts .csv/.tsv/.txt. `cb(changedRows, allRows)`.
  - `options.interval` 10000; `options.delimiter` default ",".
  - Same deferral as watch; (I) same single-slot replacement.
  - Src: HELP L161-167
  - [SOURCED; replacement semantics INFERRED]

- [ ] **API-22 · `unwatch()`**
  - Stops the current watch.
  - Src: HELP L169-170
  - [SOURCED]

- [ ] **API-23 · `addToBrowser(addObj[, callback[, options]])`**
  - `callback(error, data)`.
  - `options`:
    - `returnData` (default false; slow for large data)
    - `pointGeometries` (return point representations, faster)
    - `normalizeToFeatureArray` (flatten nested collections)
    - `name` (name of the root feature/collection)
    - `namingProp` (property used to name all features)
  - **Overload (compatibility requirement):** vendor scripts call `addToBrowser(url, {namingProp, name})` with the options object in the callback slot (GEODATA RKI COVID19 scripts). If the 2nd argument is a non-function object, treat it as `options` with no callback. (I: apply the same overload to draw/drawBrowserSelection/getBrowserSelection for safety.)
  - Src: HELP L172-206; CL 1.0.4, 1.2.1; GEODATA RKI scripts
  - [SOURCED; overload on other functions INFERRED]

- [ ] **API-24 · `addObj` forms (shared by addToBrowser and draw)**
  - ExtendScript File
  - URL string (must include the file extension)
  - GeoJSON object
  - `{type:"geocode", query}`
  - `{type:"overpass", query}`
  - `{type:"osm", id:"node1234|way4321|rel1243"}`
  - `{type:"geojsonurl", url}`
  - Src: HELP L176-198; CL 1.0.9, 1.1.7
  - [SOURCED]

- [ ] **API-25 · `clearBrowser([callback])`**
  - Removes everything except favorites.
  - Src: HELP L208-212
  - [SOURCED]

- [ ] **API-26 · `removeBrowserSelection([callback])`**
  - Src: HELP L214-218; CL 1.2.0
  - [SOURCED]

- [ ] **API-27 · `getBrowserSelection([callback[, options]])`**
  - `options.fullGeometry` (default false → points only).
  - `data` is an array of features.
  - Src: HELP L220-226; CL 1.2.1
  - [SOURCED]

- [ ] **API-28 · `drawBrowserSelection(comp[, callback[, options]])`**
  - Draws the Browser selection. Draw options are DRW-03…19 with the same defaults.
  - Src: HELP L228-251; CL 1.2.0
  - [SOURCED]

- [ ] **API-29 · `draw(comp, addObj[, callback[, options]])`**
  - Draws directly from an addObj, with the same draw options.
  - Src: HELP L253-298; CL 1.2.0
  - [SOURCED]

- [ ] **API-30 · `modalEditObject(obj, callback[, options])`**
  - Edits a flat object of strings/numbers/booleans in the GEOlayers UI.
  - `callback(error, editedObj)` is called "after the user applied his edits or canceled", so it also fires on cancel. (I: `editedObj` is undefined on cancel, like modalActions.)
  - `options.title`, `options.message`.
  - Src: HELP L350-356; CL 1.1.6
  - [SOURCED; cancel payload INFERRED]

- [ ] **API-31 · `modalActions(actions, callback[, options])`**
  - "lets the user choose one of a certain number of actions" (GHHELP; the local HELP copy repeats modalEditObject's text, C-11). Shows a string[] as buttons.
  - `callback(error, index)`; index is **undefined on cancel**. `options.title`, `options.message`.
  - Src: HELP L358-364; CL 1.1.6
  - [SOURCED]

- [ ] **API-32 · `githubBrowser([options])`**
  - A GitHub repo browser panel. Options:
    - `title`
    - `defaultUser`, `defaultRepo`
    - `fixedRepo` (Boolean)
    - `flatten` (initial "Flatten folders" value)
    - `filterText`
    - `actionBtnText` (default **"Import to GEOlayers"**)
    - `actionFilter(item)` (default: GeoJSON files)
    - `action(item, url)` (default: import to the Feature Browser)
  - Must handle repos whose default branch is not "master".
  - Src: HELP L366-379; CL 1.2.0, 1.2.2; EX:Historical_Basemaps_Browser
  - [SOURCED]

- [ ] **API-33 · `filesInDir(folder[, filterRegEx[, includeSubDirectories]])`**
  - `folder`: ExtendScript Folder. `filterRegEx`: regular expression tested against the file **name**. `includeSubDirectories`: boolean; true adds files from sub-directories.
  - Returns File[] (the doc also says "Returns a string"; C-04).
  - Src: HELP L300-307
  - [SOURCED]

- [ ] **API-34 · `readFile(file[, encoding])`**
  - Accepts a File or a filename string; encoding default UTF8. Returns a string.
  - Src: HELP L309-315
  - [SOURCED]

- [ ] **API-35 · `readFileCsv(file[, encoding])`**
  - Reads .csv/.tsv. `file`: ExtendScript File object or filename string; `encoding` String, default UTF8. Returns rows, each an array of columns.
  - Src: HELP L317-323
  - [SOURCED]

- [ ] **API-36 · `readFileJson(file[, encoding])`**
  - `file`: File object or filename string; `encoding` default UTF8. Returns an object.
  - Src: HELP L325-331
  - [SOURCED]

- [ ] **API-37 · `writeFile(file, content[, encoding])`**
  - `file`: File object or filename string; `content`: String to write; `encoding` default UTF8.
  - Src: HELP L333-339
  - [SOURCED]

- [ ] **API-38 · `userFolder()`**
  - Returns the GEOlayers User Folder as a Folder.
  - Src: HELP L341-342
  - [SOURCED]

- [ ] **API-39 · `userFile(fileName)`**
  - Returns a File in the user folder ('basename.extension').
  - Src: HELP L344-348
  - [SOURCED]

- [ ] **API-40 · Overpass query macros**
  - `{{bbox}}` is replaced with the current view's bbox. (I: Overpass order south,west,north,east.)
  - `{{geocodeArea:Name}}` appears in vendor scripts, but another vendor script avoids it (C-05). (I: implement all Overpass-Turbo macros.)
  - Queries follow the `[out:json][timeout:N];(...);out body;>;out skel qt;` pattern.
  - Src: EX:Buildings_OSM, Autobahn_*, Search_Amenity_in_bbox
  - [SOURCED usage; expansion INFERRED]

- [ ] **API-41 · Reserved GeoJSON properties**
  - FeatureCollection `properties.GEOlayersDisplayName` sets the Browser display name.
  - `properties.source` (URL). (I: a human-readable link, probably shown as an http link in Feature Properties, CL 1.0.9.) It is **not** the update URL (FB-19).
  - Feature `name` is the default naming property.
  - `coordinate_elevation` (FB-39).
  - Src: EX:COVID-19_*; CL 1.11.1
  - [SOURCED; roles partly INFERRED]

- [ ] **API-42 · Engine bootstrap and ES5 polyfills**
  - Vendor scripts call `Array.prototype.forEach` in ES3 ExtendScript, so GEOlayers installs polyfills.
  - Host method "initEngine" must survive interference from other scripts.
  - Src: EX:COVID-19_World; CL 1.14.1
  - [SOURCED clue; INFERRED design]

- [ ] **API-43 · Doc quirks to keep as aliases**
  - `simplyfyByZoomRange` (typo; I: also accept `simplifyByZoomRange`)
  - `lat`/`lon` aliases in addLabel
  - Src: HELP L239, L105
  - [SOURCED; alias INFERRED]

- [ ] **API-44 · Busy queue**
  - Watch callbacks are deferred while finalizing or doing "any other async method" (HELP L158) [SOURCED].
  - Geocode requests are queued per the Nominatim policy (HELP L151) [SOURCED].
  - (I) All async operations are serialized.
  - Src: HELP L151, L158
  - [SOURCED; serialization INFERRED]

- [ ] **API-45 · Doc-text clarifications confirmed by the bundled help**
  - draw/drawBrowserSelection callback fires "after the data has been drawn"; modalActions "let[s] the user choose one of a certain number of actions". The local HELP copy had copy-paste text (C-11, now resolved).
  - Src: GHHELP
  - [SOURCED]

## 19. Licensing / support / misc (LIC)

- [ ] **LIC-01 · Commercial licensing**
  - The original uses the aescripts licensing framework ("aesp", License Framework upgrade 1.15.1).
  - **Hobby build: omit or stub this.** The user's build is personal and unshared.
  - Src: CL 1.6.1, 1.15.1
  - [SOURCED]

- [ ] **LIC-02 · About window**
  - Shows the build number and a prerelease indicator.
  - EBE: the licence-information section of the Support dialog (LIC-19) shows the installed GEOlayers version. (I: this may be the current form of the About window.)
  - Src: CL 1.3.0; EBE
  - [SOURCED]

- [ ] **LIC-03 · About & Licensing panel**
  - EBE: the Support dialog's licence section shows the licence information and version and has a **Deactivate** action; another section shows terms of use and third-party credits (LIC-19). (I: this is the About & Licensing panel of CL 1.11.1.)
  - Src: CL 1.11.1; EBE
  - [SOURCED]

- [ ] **LIC-04 · Auth error handling**
  - Startup authentication errors must be handled gracefully.
  - Src: CL 1.8.0, 1.16.0
  - [SOURCED]

- [ ] **LIC-05 · "Data not licensed" dialog (MapTiler gate)**
  - MapTiler logo.
  - Text (verbatim from the thumbnail transcription, n/videos §2b): "In order to use this Imagery you'll need to license data powered by MapTiler."
  - Steps, verbatim:
    1. `Click the "License Data" button below`
    2. `Check your browser and sign up for a FLEX plan`
    3. `Copy your MapTiler API Key and paste it right here:` followed by a field with placeholder `Paste your API Key here`
    4. `Hit Apply and restart GEOlayers`
  - Buttons: **Cancel** / **License Data**.
  - Src: VM:479822829 thumbnail
  - [SOURCED]

- [ ] **LIC-06 · MapTiler integration terms**
  - GEOlayers ships with a **default key** that must be replaced.
  - Trial: currently "two weeks"; formerly "6 months" of Mapdata (C-08).
  - After the trial, a Flex plan is needed for the partner features: stylized imagery, click-find, multilingual labels, watermask, current-view import, 3D buildings, 3D terrain.
  - GEOlayers works without it. EBE: with a premium MapTiler data plan, extra buttons and features appear in the panel (UI-28).
  - Hobby note: MapTiler's **Free plan** (non-commercial, logo required) fits this project.
  - MTG (fetched 2026-11-19): Flex is $30/month and includes a video material licence for individual creatives up to 100,000 channel subscribers; TV/film/commercial streaming need a custom licence; there is a logo-for-discount option; the default key must be replaced via the Keys page.
  - Src: HELP L385-398; MTG; MTD; AES; n/providers §1.3; EBE
  - [SOURCED]

- [ ] **LIC-07 · GEOlayers Mapdata (legacy subscription)**
  - The 2019–2020 data subscription bundled with purchase, replaced by MapTiler in 1.1.9.
  - Src: CL 1.0.4, 1.0.7, 1.1.9; AESF
  - [SOURCED weak; replacement INFERRED]

- [ ] **LIC-08 · Disclaimer and attribution**
  - Provider terms apply. Content must be attributed unless otherwise agreed with the provider. No liability for remote data. No ownership of acquired data.
  - EBE: the Support dialog shows the terms of use and lists "all of the other kind of applications that are used to make geolayers run" (third-party credits; LIC-19).
  - Src: HELP L400-407; EBE
  - [SOURCED]

- [ ] **LIC-09 · Log files**
  - GEOlayers can create log files.
  - Src: CL 1.1.6
  - [SOURCED]

- [ ] **LIC-10 · Undo hygiene**
  - Undo groups are closed manually on every host call.
  - Src: CL 1.5.3
  - [SOURCED]

- [ ] **LIC-11 · "#" in file paths**
  - Imports fail with a descriptive error.
  - Src: CL 1.14.1
  - [SOURCED]

- [ ] **LIC-12 · AE version support**
  - Support added for v22 (1.3.0), 25 (1.10.0) and **27** (1.18.0).
  - Storefront: "CC 2015 (13.6)+" vs a compatibility list of CC 2017–2026 (C-10).
  - Hobby target: AE 25–27, CEP 12.
  - Src: CL; AES; n/arch §1
  - [SOURCED]

- [ ] **LIC-13 · Install and update**
  - Installed via ZXP / the aescripts ZXP/UXP Installer, and updated through the aescripts manager app.
  - EBE: the Support dialog has **Check for updates**, which shows the latest available version number (LIC-19).
  - Hobby: an unsigned folder + PlayerDebugMode. (I: the update check compares against a version file the user hosts, or is stubbed.)
  - Src: x.com/GEOlayers/status/1892224844776779833; n/arch §1.4; EBE
  - [SOURCED]

- [ ] **LIC-14 · Help and troubleshooting**
  - Help menu topics: Contact Support; "Get Dimensions Error"; Mapcomp Not Showing; Mapcomp Not Linking; How to Import a Project; Fix Imagery Issues.
  - EBE: the Help menu (last panel-menu entry, UI-19) explains GEOlayers, links to the creator's tutorials and has a **keyword search**. Contact Support is a section of the Support dialog (LIC-19).
  - FAQ (weak; the FAQ entry is still headed GEOlayers 2): a blank panel on first run comes from antivirus/firewall blocking licensing.
  - Src: YT:gfXl9-7oZjY; YT:1z_f9kwvSCg; aescripts FAQ snippet (weak); EBE
  - [SOURCED topics; FAQ weak]

- [ ] **LIC-15 · Panel not opening on some Macs**
  - Known issue on macOS 13 / M1; no fix published.
  - Src: ACOM:13712683, ACOM:14198877
  - [SOURCED]

- [ ] **LIC-16 · Bundles with Helium X / FreeForm Pro / Plexus 3**
  - Storefront only; no functional impact. (EBE names Plexus 3 and Helium in the 3D Landscape dialog, 3D-06.)
  - Src: AES; EBE
  - [SOURCED]

- [ ] **LIC-17 · GEOlayers.app (sibling web product)**
  - **Out of scope** for parity. Its concepts (item timeline, voiceover, Fit Viewport, Follow Feature, Auto Bearing, project colors, flags tool) are not GEOlayers 3 features. Its globe demos are used only as a behaviour reference for 3D-10.
  - Src: https://www.geolayers.app; n/webapp §1
  - [SOURCED]

- [ ] **LIC-18 · MapTiler trial/default-key request limit**
  - Trial users can hit a request limit, after which MapTiler imagery stops rendering (forum snippet, weak). (I: show a clear error and fall back to free styles rather than black frames.)
  - Src: n/webapp §4 (AESF, weak)
  - [SOURCED weak]

- [ ] **LIC-19 · Support dialog (four sections)**
  - Opened from the panel menu's **Support** entry (UI-19); the narrator clicks "contact support". It has four sections (EBE):
    1. **Licence information**: the installed version and a **Deactivate** action (LIC-01/02/03)
    2. **Contact support**: send a request to support from the panel, with a **priority** choice
    3. **Check for updates**: shows the latest version number (LIC-13)
    4. **Terms of use** and the third-party applications/libraries the extension uses (LIC-08)
  - Field names, the support form's fields beyond priority, and how the request is sent are unknown.
  - Hobby build: keep the dialog and its four sections; licence = "personal build" (no activation, LIC-01); contact support = a local note/log export or a link the user configures (no vendor endpoint); update check = stub or user-hosted version file; credits = MapForge's own third-party licence list.
  - Src: EBE
  - [SOURCED sections (EBE); fields and transport unknown; hobby treatment INFERRED]

---

## Contradictions (flagged)

| ID | Contradiction | Sources | Resolution suggestion |
|---|---|---|---|
| C-01 | Mapcomp List opener and create label: HELP says "the little arrow top left" and "+ Add Mapcomp"; the 2023 screenshot shows a ☰ list icon and "+ Create new Mapcomp". | HELP L13/L25 vs STADIA | Both are circa 2023 (HELP image upload id ≈233547961 ≈ Apr 2023; STADIA screenshots from VM:872558334, Oct 2023), so this is **not** resolved by date. "+ Create new Mapcomp" is observed; "the little arrow" may be the same ☰ control. Follow the screenshot. EBE (2026) only says "this little icon" and "create a new map composition", so it does not settle the icon or the label. |
| C-02 | Default Mapcomp name "Mapcomp 1" (Feb 2021 thumbnail) vs "Mapcomp 01" (1.3.0, Aug 2021, leading zeros). | VM:517178231 vs CL 1.3.0 | Chronological. Use "Mapcomp 01". |
| C-03 | The line-pattern script is called `add_line_pattern.jsx` (1.2.0) and `add_line_patterns.jsx` (1.4.0). | CL | Exact filename unknown. Pick one and alias. |
| C-04 | `filesInDir` "Returns File objects…" vs "Returns a string." | HELP L301 vs L307 | Doc error. Return File[]. |
| C-05 | `{{geocodeArea:…}}`: used in EX:Autobahn_Input, but EX:Autobahn_BAB comments it out and uses `area[name=…]`. | EX | Support status unknown. Implement it anyway; it is harmless. |
| C-06 | Dataviz imagery styles: the vendor forum lists Choropleth, Circles, **Heatmap**; the changelog has Choropleth (1.1.8), Circles (1.2.2), Lines (1.20.0) and never Heatmap. | AESF vs CL | Treat Heatmap as existing but weakly sourced. |
| C-07 | "Styles based on a url source can be updated with one click" (1.4.2): unclear whether this means imagery styles, shape layer styles, or both. | CL 1.4.2 | Implement for both. |
| C-08 | MapTiler trial length: "two weeks" (current storefront) vs "6 months" (older Toolfarm/forum, Mapdata era). | AES vs AESF | Chronological. Irrelevant for a hobby build. |
| C-09 | Pin button position: "second button in the toolbar" (GL2-era forum) vs the first icon of the left group (GL3 screenshot). | AESF vs STADIA | **RESOLVED by EBE:** "Pin selected layers to map" is the first toolbar button narrated after the header. The forum post is GL2-era. |
| C-10 | Minimum AE version: "CC 2015 (13.6)+" text vs compatibility list CC 2017–2026. | AES; Toolfarm snippet | Irrelevant. Target AE 25+. |
| C-11 | Help text bugs: `watchCsv` says "Watches a file or folder" but takes a CSV file; `modalActions` reuses modalEditObject's description; draw/drawBrowserSelection callbacks say "after the data has been imported to the browser". | HELP L162, L359, L233, L280 | **Resolved (SOURCED, GHHELP):** the bundled help says the draw callbacks fire "after the data has been drawn" and modalActions "let[s] the user choose one of a certain number of actions". Follow the argument lists. |
| C-12 | Default Universal Raster URI is OSM's 256-px tile server, but Tile Size shows 512 px. | STADIA | (I: Tile Size may be the render size per tile, not the source size.) Verify in testing. |
| C-13 | Anchor name capitalization: "Route Loop Anchor" (screenshot) vs a user's "World map comp anchor". | STADIA vs ACOM:14021559 | Use "<Mapcomp> Anchor". |
| C-14 | Simplify option names: API `simplify` / `simplyfyByZoomRange`; UI "Simplify Geometry (Current Zoom)"; changelog "Simplify Geometry by max Zoom". | HELP; ACOM; CL 1.2.0 | Two UI checkboxes map to the two API options (INFERRED). |
| C-15 | "No camera / pseudo-3D" (user observation) vs "You can finally use 3D cameras in GEOlayers" (3D Scene script, 2025). | ACOM:14019807 vs YT:NDqpEe3G2yc | Both are true. The core rig has no camera; the 3D Scene & Camera script adds one. |
| C-16 | Where "Quality" lives: a Mapcomp Settings slider (80%), the `previewQuality` boolean finalize option, a Default Imagery Quality pref, and "finalize with a higher Quality" advice. | STADIA; HELP; CL 1.15.1; AESF | Model it as a per-Mapcomp Quality % (default from pref) plus a preview-quality finalize switch (INFERRED). |
| C-17 | FreeForm Pro / Plexus / Mir 3D landscapes are not in the local docs (only Helium, CL 1.5.0), but the storefront and Mettle confirm them. | n/docs vs AES/METTLE | Treat all four as SOURCED. |
| C-18 | The Stamen "Toner Background" style was free without Mapdata (forum 2019–20), but Stamen was removed in 1.6.4. | AESF vs CL 1.6.4 | Chronological. Do not ship Stamen. |
| C-19 | Provider attribution look: italic, bottom-left (Vimeo thumbnails; "bottom left" in the settings screenshot) vs uppercase "© MAPTILER © OPENSTREETMAP CONTRIBUTORS" bottom-right (official HELP image). | VM thumbnails; STADIA vs HELP image | Position is a setting, so both can be true. Style may differ by provider or version. Default to the settings screenshot ("bottom left"); verify the text style in AE. |
| C-20 | `dateBorderCopy` defaults to false (API) but CL 1.2.0 says "Features drawn multiple times if they cross the date line" as if automatic. | HELP L246 vs CL 1.2.0 | Unresolved (DRW-14). Possibly the UI default differs from the API default. Verify in AE. |
| C-21 | Toolbar contents: the 2023 screenshot shows 7 left icons and only selector + ⇄ in the centre; EBE (2026) narrates 5 left buttons and a manual-label button (icon unknown; it shows a crosshair while active) in the label area. | STADIA vs EBE | Partly explained: EBE says a MapTiler premium plan adds buttons (UI-28), so (I) the two un-narrated left icons are premium-only (3D buildings, Watermask). The manual-label button is unexplained (I: added after 2023, or hidden in the screenshot state). Build the EBE set; show the premium buttons only with a MapTiler premium plan (key with premium access); verify in AE. |
| C-22 | Pin "Scale with Map" default: the snap API defaults `scaleWithMap` (and `rotateWithMap`) to true, but EBE turns Scale with Map on after pinning, which implies it was off. | GHHELP (API-16) vs EBE | Weak: the narrator may have toggled it only to demonstrate. Keep the API default for `snapLayerToMapcomp`; make the pin button's UI default a constant to confirm in AE (PIN-14). |

## Unknowns (exist, but behavior/UI not documented)

U-01 to U-15 are ranked by impact on one-to-one parity.

1. **U-01 Toolbar icons**: the tooltip/function of each of the 7 left and 4 right toolbar icons (UI-17). **Mostly RESOLVED by EBE:** functions and narrated order of the 10 non-premium buttons are known (UI-17). Still open: exact tooltip text; the icon of each button (all icon ↔ function matches are INFERRED by narrated order against the 2023 STADIA icons); which of pill/sliders is Create Controller Mapcomp; which two icons are premium-only (I: cube = 3D buildings, other = Watermask); why the manual-label button is missing from the 2023 screenshot (C-21).
2. **U-02 View-control rig**: which layer holds Lat/Lon/Zoom/Bearing/Pitch; pseudo effect vs separate sliders; the effect names; the zoom convention (256 vs 512 px tiles); the bearing and pitch sign; the pitch-limit formula (VIEW-01, MCS-07, MCS-20).
3. **U-03 Finalize pipeline and UI**: per-frame image sequence vs tile layers; how options are exposed (checkboxes/modifiers); the "3 Ways to Finalize Imagery"; option defaults; the cache folder layout (FIN-01…08). **Narrowed by EBE:** plain click = whole composition (narrator wording; work area not tested, so "not work area" is INFERRED); CTRL/CMD-click = current frame only; the List row ✓ finalizes that Mapcomp. Still open: UI form of previewQuality / onlyWorkArea / purgeImageryCache, the output form, the third "way", cache layout.
4. **U-04 Imagery styles**: the full list of built-in style names, and each style's editable parameters (Basic/Advanced color groups, feature-group toggles, fonts, hillshading, street density range, the sliders below Hue/Opacity/Black, the crossed-tools icon) (IMG-03…29).
5. **U-05 Mapcomp labels engine**: placement/density rules, fonts, blending/animation, whether labels are separate layers or baked, and what "Mapcomp Label Templates" are (MLB-*, MCS-19).
6. **U-06 Shape Layer Style editor**: the field list and the names of the default style presets (SLS-02, SLS-08). **Narrowed by EBE:** presets at the top and drawing options at the bottom of the panel are known; Add style "brings up" something that offers creating a style, and import/export and the style settings with data-driven options may sit in it or in the panel (SLS-01/08/19, INFERRED placement). Still open: field list, preset names, what Add style opens.
7. **U-07 Data-driven styles UI**: which style properties can be driven, the ramps/curves other than "Circle Area", the timed-data property naming convention, and the Property Diagrams output (DAT-07…14). **Narrowed by EBE:** data-driven ranges are set up from the Shape Layer Style panel; collection Feature Properties show min/max per property and per-property treatment (timing, display name); Property Diagrams are drawn from the toolbar or the action bar (e.g. GPX elevation). Still open: drivable properties, curves, naming convention, diagram output.
8. **U-08 Mapcomp List details**: the full hover-button set, and the meaning of the "Preview" pill, the lock icon and the footer icon; the Duplicate dialog's option names (UI-05, UI-09, MCS-22). **Narrowed by EBE:** row actions are Settings, Duplicate, Remove, Finalize; the blue icon toggles imagery (UI-27); the "A" toggles labels; the lock locks the Mapcomp; rows also carry "Prefer for …" settings (UI-26). Still open: the "Preview" pill, the footer icon, what locking does, the Duplicate dialog.
9. **U-09 Linked/controller Mapcomp mechanics**: which properties are linked, how a link is created or broken, and what "Mapcomp sync" writes back (MCS-21, UI-13). **Mostly RESOLVED by EBE:** controllers come from a toolbar button (MCS-28); a link is made in the linked Mapcomp's Settings (MCS-29); the footer Keyframe button keys all five controls (on the controller when linked) and Link Mapcomp View then writes keys for changed parameters (UI-12/13, VIEW-02). Still open: how a link is broken, and the expression mechanism.
10. **U-10 Arrow tool and default scripts parameters**: Arrow tool, Distance Circle, Scale Bar, North Needle, Blend in/out, Blink, Stagger, Line Pattern, 3D Scene & Camera (RTE-06, TB-*). **Narrowed by EBE:** the arrow tool uses AE's pen tool, creates a pre-animated arrow shape layer with many effects, coloured by the current Shape Layer Style and parented to the map (RTE-06). Parameters and effect names are still open.
11. **U-11 3D Landscape setup**: the comps and layers created, per-plugin setup, control names, and the DEM provider/encoding (3D-05…08). **Narrowed by EBE:** the dialog has Texture Size, Height Map Size and a required third-party plugin choice (3D-05). The rest is still open.
12. **U-12 Globe / Equal Earth "via Plugin"**: what the plugin is, its parameters, and how it interacts with the Mapcomp controls (3D-10).
13. **U-13 Routes**: the routing provider and the UI for creating car/bike/pedestrian routes; whether plane/straight modes exist; the distance units preference (RTE-01/02/10). **Narrowed by EBE:** routes come from the Connect features menu (FB-27), which lists route types incl. "plane route" (plane exists in GL3); EBE: "it shows you how it is pulling this data" (I: each type's data source); the result is a route feature with a distance property. Still open: the provider, the other type names, a straight-line mode, distance units (Q-50).
14. **U-14 API gaps**: the return value of `importProject` (API-04); the `duration` defaults of snapLayerToMapcomp/addLabel paths. (snapLayerToMapcomp and addLabel signatures are now SOURCED via GHHELP.)
15. **U-15 Preferences**: the complete preference list and layout; where the CartoDB key and search-source settings live; the formats of the imagery-style export file and the label-renaming dictionary file (PRF-*). **Narrowed by EBE:** groups include feature search, composition settings, animation handling and Mapcomp icon visibility (PRF-20/21). The rest is still open.
16. U-16 Points-inside-Polygons operations; Feature Mesh/Connect UI; "Filter via Data" syntax; the dashed-square and stacked-shapes browser icons (FB-02/06/26/29). **Narrowed by EBE:** "points inside of polygons" isolates the points into a new collection with the polygon (FB-29); Connect features is an action-bar menu of route types (FB-27); one header icon is probably "Preview selection on map" (FB-45). Still open: other Points-inside-Polygons options (Q-53), Feature Mesh UI, data-filter syntax, the second icon.
17. U-17 Style generation from comp colors / image / .ase: where it lives and the algorithm (IMG-08/09, SLS-10).
18. U-18 Search sources configuration UI; the fallback search service identity; MapTiler worldview effect on imagery (SRC-07/08).
19. U-19 Label template registration mechanics (how a comp becomes a template); the source of online label templates; the full default template list beyond "Capital Citi…" (LBL-02/15/17). **Narrowed by EBE:** templates can be downloaded "from … the default GEOlayers projects" (EBE wording), and "add a new template" / "import" are reached from the label area (exact placement INFERRED; LBL-15/16/17). Registration mechanics and the full list are still open.
20. U-20 Attribution dropdown options and its checkbox; the Blend Duration semantics; the Quality semantics (MCS-14/16/17).
21. U-21 The large-draw warning threshold and full text (DRW-21).
22. U-22 GEOlayers 2 asset import location in the UI (PRJ-07).
23. U-23 Mapcomp View feature data model ("Create a Mapcomp View") (VIEW-09). **Mostly RESOLVED by EBE:** "+" > "Mapcomp view" asks for a name and stores the current view incl. pitch and bearing as a Browser feature. The exact geometry/properties are still open.
24. U-24 Measure distance/area UI (FB-41); "Georeference Existing Maps" tooling (FB-31). **Narrowed by EBE:** route features carry a distance property in Feature Properties (units not stated).
25. U-25 Draw-options UI location and which options are user-visible vs API-only (DRW-02). **RESOLVED by EBE (location):** the drawing options are at the bottom of the Shape Layer Style panel; user-visible ones include Auto Stroke Width, geometry/simplify, individual layers, draw inside Mapcomps and draw on preferred Mapcomp (DRW-28). Whether the remaining API options are visible is still open.
26. U-26 WMS support in GL3 (IMG-33).
27. U-27 Whether the masterclass lessons in PRJ-10 and the "Revision Workflow"/export chapters (PRJ-11) hide any GEOlayers feature.
28. U-28 Pin vs snap UI (PIN-08); the "Create 3D Landscape" dialog's full options (3D-05); imagery storage location and portability rules (FIN-18). **Narrowed by EBE:** there is one pin button whose options are effect controls on the layer (PIN-14); the 3D Landscape dialog has Texture Size, Height Map Size and plugin choice. Still open: effect names/defaults, size values, imagery storage.

## Provider table

| Provider | GEOlayers feature(s) | Status in original | Endpoint / format | Key | Hobby-use notes | Attribution | Tag / Src |
|---|---|---|---|---|---|---|---|
| **MapTiler Cloud** (partner) | Basic/Advanced/Lite styles, Satellite V2, Mapcomp labels (multilingual), click-to-find, current-view import, admin bounds, watermask, 3D buildings, 3D terrain, MapTiler Search (+worldview), import MapTiler Cloud maps | Since 1.1.9; Flex plan after trial; ships with a default key | `https://api.maptiler.com` with `?key=`; styles `/maps/{id}/style.json`; raster `/maps/{id}/{256?}/{z}/{x}/{y}{@2x}.{png,jpg,webp}`; tiles `/tiles/{v3,satellite-v2,terrain-rgb-v2,countries}/{z}/{x}/{y}`; geocoding `/geocoding/{q}.json` (worldview, language, by-id full geometry) | Yes | Free plan: personal/non-commercial, logo, 100k req/month, 1k search sessions. Trial/default key can hit a request limit (LIC-18, weak). Flex $30/mo incl. video licence ≤100k subscribers (MTG 2026-11-19) | "© MapTiler © OpenStreetMap contributors" | SOURCED (features); endpoints SOURCED from MapTiler docs; mapping of features→tilesets INFERRED — HELP L385-398; n/providers §1 |
| **OSM Nominatim** | Search bar (OSM search), `geocode()`, `{type:"geocode"}` | Since 1.0.x; queued per policy | `https://nominatim.openstreetmap.org/search?q=&format=geojson&polygon_geojson=1&accept-language=&limit=` | No | ≤1 req/s per app, identifying UA, no autocomplete or bulk, cache results | "© OpenStreetMap contributors" (ODbL) | "uses Nominatim" GEOlayers-SOURCED (HELP L151); endpoint + parameters INFERRED (provider-doc sourced, n/providers §2) |
| **Fallback search service** | Used when default search fails | 1.0.2 | unknown (I: Photon / MapTiler) | ? | — | — | SOURCED existence — CL 1.0.2 |
| **Overpass API** | `{type:"overpass"}`, OSM downloads, vendor scripts (buildings, rail, hospitals…) | Auto server selection by latency and free slots (1.5.6) | `/api/interpreter` on overpass-api.de, maps.mail.ru, overpass.private.coffee…; status `/api/status` | No | ~10k queries/day and 1 GB/day on main; back off on 429/504 | © OSM contributors | usage + auto server selection GEOlayers-SOURCED (EX; CL 1.5.6); server list + status endpoint INFERRED (provider-doc sourced, n/providers §3) |
| **OSM by id** | `{type:"osm", id}` | 1.1.7, stabilized 1.17.0 | (I: Overpass or OSM API `/api/0.6/{type}/{id}/full`) | No | — | © OSM | SOURCED feature; backend INFERRED |
| **Natural Earth** | NE Imagery, NE Water Mask, NE Mapcomp Labels, NE Admin Boundaries imagery, NE Search API, datasets (land/water, states/provinces, lakes/rivers), Inner Boundaries | 1.1.9+, data update 1.6.6 | (I: bundled data, from naturalearthdata.com / github nvkelso/natural-earth-vector) | No | Public domain, offline-capable | Optional "Made with Natural Earth" | SOURCED — CL; n/providers §4 |
| **Bing Maps** | Bing imagery Aerial + Dark/Light/Gray, language | 1.1.4 | `dev.virtualearth.net/REST/v1/Imagery/Metadata/{Aerial,CanvasDark,CanvasLight,CanvasGray}?key=&c=` → `{quadkey}`, subdomains t0–t3, 256 px | Yes | **Basic keys retired June 30 2025**; Enterprise only until 2028. Alternative: Azure Maps Get Map Tile (`microsoft.imagery`, `microsoft.base.darkgrey`…) | Microsoft/data-provider strings | SOURCED (GEOlayers + MS docs); imagerySet mapping INFERRED |
| **CARTO (CartoDB)** | CartoDB imagery + API key field | key since 1.19.0 | `https://{a-d}.basemaps.cartocdn.com/{light_all,dark_all,rastertiles/voyager,…}/{z}/{x}/{y}{@2x}.png?key=` | Yes (free, since 25 Sep 2026; watermark otherwise) | Free: 5M req/month non-commercial | "© OpenStreetMap contributors © CARTO" | SOURCED — CL 1.19.0; n/providers §6 |
| **Stamen → Stadia Maps** | Former Stamen style (removed 1.6.4); now via a custom URL | Removed | `https://tiles.stadiamaps.com/tiles/stamen_toner/{z}/{x}/{y}@2x.png?api_key=` (watercolor `.jpg`) | Yes | Free 200k credits, non-commercial | "© Stadia Maps © Stamen Design © OpenMapTiles © OSM contributors" | SOURCED — CL 1.6.4; STADIA |
| **Any XYZ/TMS raster server ("Universal Raster")** | Custom Mapcomp Imagery | 1.0.9 TMS; 1.4.0 date vars and bounds; 1.6.6 default tiles | Template `{z}/{x}/{y}` (+`{year}{month}{date}{dateISO}`, @2x, query keys); default `https://a.tile.openstreetmap.org/{z}/{x}/{y}.png` | Depends | **OSM standard tiles forbid bulk/offline**, so don't default to them for finalize. Use OpenFreeMap/Protomaps/CARTO/etc. | Per provider | SOURCED — STADIA; CL |
| **Mapbox** | Mapbox Studio styles via raster URL | Tutorial-supported | (I: `api.mapbox.com/styles/v1/{user}/{style}/tiles/256/{z}/{x}/{y}@2x?access_token=`) | Yes | Mapbox ToS | © Mapbox © OSM | SOURCED usage; URL INFERRED |
| **Local tile directory** | Imagery from disk | 1.1.8; metadata.json 1.5.2 | `{z}/{x}/{y}.{png,jpg}` + `metadata.json` (minzoom/maxzoom; I: bounds, format, scheme) | No | Generated with MapTiler Desktop/Engine, QGIS, gdal2tiles | Per data source | SOURCED; schema INFERRED |
| **NASA Visible Earth / GIBS** | Georeferenced imagery tutorial; date-variable URLs | Tutorial | GeoTIFF → tiles; (I: GIBS WMTS with `{dateISO}`) | No | Public domain-ish (NASA) | NASA | SOURCED (Visible Earth); GIBS INFERRED |
| **Routing provider** | Car/Bike/Pedestrian routes, Maneuver Points | 1.0.4/1.4.0 | **unknown** (I: FOSSGIS OSRM `routing.openstreetmap.de/routed-{car,bike,foot}/route/v1/driving/{lon,lat;lon,lat}?steps=true&geometries=geojson&overview=full`) | No (OSRM) | ≤1 req/s, attribution + "fix the map" link; alternatives Valhalla, ORS (key), GraphHopper (key) | © OSM / OSRM | INFERRED provider — n/providers §10 |
| **DEM / elevation** | 3D Landscapes heightmaps; Elevation sample | 1.2.5 DEM tiles; GL2 used Mapzen | **unknown** (I: MapTiler `terrain-rgb-v2` webp z0–14, h=-10000+(R·65536+G·256+B)·0.1; or AWS Terrarium `s3.amazonaws.com/elevation-tiles-prod/terrarium/{z}/{x}/{y}.png`, h=R·256+G+B/256−32768) | MapTiler: yes; AWS: no | AWS Terrarium is free | Terrarium source credits (USGS, NOAA, Copernicus…) | INFERRED — n/providers §11 |
| **GitHub** | `githubBrowser()`, GitHub Search API source, historical basemaps | 1.2.0/1.2.2 | (I: `api.github.com/repos/{u}/{r}/git/trees/{branch}?recursive=1` + raw.githubusercontent.com) | No | 60 req/h unauthenticated | Per repo licence | SOURCED feature; endpoints INFERRED |
| **ArcGIS Hub / FeatureServer** | Example COVID data join via fetchJson | Example scripts | `.../FeatureServer/2/query?outFields=*&where=1%3D1&f=geojson` | No | — | Data owner | SOURCED — EX |
| **USGS earthquake feed** | Data-driven time-offset tutorial | Tutorial | `earthquake.usgs.gov/earthquakes/feed/v1.0/geojson.php` | No | Public domain | USGS | SOURCED — VM:533574071 |
| **aourednik/historical-basemaps** | Historical boundaries via GitHub source | Tutorial/EX | GitHub repo GeoJSONs | No | Check repo licence | Repo author | SOURCED — EX; VM:517178231 |
| **Video Copilot Orb** (AE plugin) | Globe sample project | 1.0.4 | AE effect | — | Free plugin, Win/Mac only | — | SOURCED |
| **Helium X / FreeForm Pro / Plexus 3 / Trapcode Mir** (AE plugins) | 3D Landscape rendering | Helium 1.5.0; others since GL2/GL3 | AE effects | Paid licences | Win/Mac only; cannot test on Linux | — | SOURCED — CL 1.5.0; AES; METTLE |
| **WMS servers** | GL2 WMS connections (VM:241651263); GL2 profile import (IMG-28) | GL3 support unknown (IMG-33) | OGC WMS GetMap (I) | Depends | — | Per provider | SOURCED for GL2; GL3 INFERRED |
| **Free replacements (not in original)** | Vector basemap for Basic/Advanced/Lite parity without MapTiler: **OpenFreeMap** (no key; OpenMapTiles schema), **Protomaps PMTiles** (self-host); satellite: MapTiler Free, EOX Sentinel-2 cloudless (non-commercial) | n/a | see n/providers §13 | mostly no | Good fit for a personal hobby build | "OpenFreeMap © OpenMapTiles Data from OpenStreetMap" etc. | INFERRED recommendation |

## Revision log (final editor, 2026-11-19)

Items added (17): UI-24, IMG-33, FIN-18, SRC-14, FB-43 (split from FB-12), FB-44, LBL-27, PIN-13, 3D-16, PRJ-10, PRJ-11, API-45, LIC-18, plus contradictions C-19, C-20 and unknowns U-26…U-28.
Items corrected or downgraded: UI-08, UI-11, UI-18, UI-23, MCS-02, MCS-05, MCS-12, MCS-14, MCS-16, MCS-18, MCS-21, MCS-22, VIEW-03, VIEW-04, VIEW-06, IMG-02, IMG-03, IMG-04, IMG-12, IMG-16, IMG-18, IMG-19, IMG-20, FIN-01, FIN-04, FIN-08, FIN-11, MLB-01, MLB-03, MLB-09, SRC-02, SRC-06, FB-11, FB-12, FB-19, FB-25, DRW-04, DRW-06, DRW-09, DRW-14, DRW-15, DRW-17, DRW-18, DRW-19, DRW-22, SLS-01, SLS-11, SLS-13, LBL-02, LBL-07, LBL-09, LBL-12, LBL-18, LBL-24, PIN-05, PIN-08, PIN-11, PIN-12, DAT-01, DAT-22, 3D-05, 3D-07, 3D-10, 3D-13, TB-05, TB-06, TB-11, TB-15, TB-16, PRJ-02, PRF-02, API-01, API-12, API-16, API-20, API-21, API-23, API-30, API-31, API-33, API-35, API-36, API-37, API-41, API-44, LIC-05, LIC-06, LIC-14, LIC-17, C-01, C-11, U-14, legend (AESF, STADIA, GHHELP, GEODATA), provider table (MapTiler, Nominatim, Overpass, WMS rows).

### EBE pass (2026-11-19)

Source added: **EBE** (transcript of YT:DX_qi37vy0Y "GEOlayers 3 for Beginners: Every Button Explained"), deduplicated from two claim extractions.

Items added (14): UI-25 (header name button), UI-26 (Preferred Mapcomp "Prefer for …"), UI-27 (imagery toggle per Mapcomp), UI-28 (MapTiler-plan-dependent buttons), MCS-28 (Create Controller Mapcomp), MCS-29 (Settings: link view), FB-45 (Preview selection on map), FB-46 (feature action bar), FB-47 (Convert polygons to points), DRW-28 (Draw on preferred Mapcomp), SLS-19 (style import/export), PIN-14 (pin effect controls), PRF-21 (Mapcomp icon visibility), LIC-19 (Support dialog). Contradictions added: C-21, C-22.

Items corrected or upgraded: UI-01, UI-02, UI-03, UI-04, UI-05 (✓ = Finalize now SOURCED), UI-06, UI-08 (now SOURCED), UI-09, UI-11, UI-12, UI-13 (write-back now SOURCED), UI-15, UI-16, UI-17 (rewritten to the narrated button set and order), UI-18, UI-19, UI-20, MCS-02, MCS-09, MCS-21, MCS-22, VIEW-02 (write-back now SOURCED), VIEW-03, VIEW-04, VIEW-05, VIEW-06, VIEW-07, VIEW-08, VIEW-09, VIEW-10, VIEW-14, FIN-01, FIN-02, FIN-03, FIN-04 (GL3 default now SOURCED), MLB-01, SRC-01, SRC-02, SRC-03, SRC-04, SRC-05, SRC-10, SRC-14, FB-01, FB-02, FB-03, FB-04, FB-05 (GeoJSON now SOURCED), FB-06, FB-07, FB-09, FB-11, FB-14, FB-20, FB-21, FB-24, FB-27, FB-29, FB-30, FB-33, FB-37, FB-41, FB-43, DRW-01, DRW-02 (location corrected), DRW-03, DRW-04, DRW-05, DRW-06, DRW-10, DRW-22 (now SOURCED), SLS-01, SLS-08, SLS-11, SLS-15, LBL-05, LBL-06, LBL-12 (toolbar location now SOURCED), LBL-15, LBL-16, LBL-17, LBL-18, LBL-26, LBL-27, PIN-01, PIN-03, PIN-05 (popover guess corrected to effect controls), PIN-06, PIN-07, PIN-08, RTE-01, RTE-02, RTE-04, RTE-06, RTE-10 (plane routes now SOURCED), IMG-17, DAT-05, DAT-07, DAT-12, DAT-14, 3D-03, 3D-05 (dialog corrected to Texture Size + Height Map Size + required plugin), 3D-06, 3D-12, 3D-14, TB-01, TB-02, TB-03, TB-04, TB-08, TB-09, TB-11, TB-13, TB-15, PRJ-01, PRJ-02, PRJ-04, PRF-01, PRF-16, PRF-20, API-04, API-05, API-16, LIC-02, LIC-03, LIC-06, LIC-08, LIC-13, LIC-14, LIC-16, C-01 (note), C-09 (RESOLVED), unknowns U-01, U-03, U-06–U-11, U-13, U-15, U-16, U-19, U-23–U-25, U-28.

Not added: the "Bouncing Map Icons" preset used in the pin demo (an AE Effects & Presets item not stated to ship with GEOlayers; noted under PIN-01).

### EBE verification pass (2026-11-19)

A verifier re-read the EBE transcript against the EBE pass and raised 35 issues; all were applied (two with small adjustments, noted below). No item was added or removed (444 items).

Downgraded or re-tagged (over-reading of narration):
- UI-17: every icon ↔ function match (pin, mountain, bar chart, document, curved arrow, framed pen, swatch, both ⇄) is now INFERRED (narrated order matched to the 2023 STADIA icons); only the narrated functions and their order stay SOURCED. The manual-label button's icon is unknown (it *shows* a crosshair while active); same fix in UI-18, LBL-05, C-21, U-01.
- UI-11 / FB-11: Find features at coordinate "at the top left" is SOURCED; the crosshair icon match is INFERRED. The Mapcomp View toggle keeps only the EBE quote as sourced; "link unaffected" is (I).
- UI-20 / UI-04: removed "close/back button" from the EBE claim for leaving the Mapcomp List (Back stays, cited to STADIA).
- FIN-04 / U-03: "GL3 default = whole composition SOURCED" corrected to weak narrator wording (work area not tested); "not work-area-only" is INFERRED. (Supersedes "FIN-04 (GL3 default now SOURCED)" above.)
- SRC-01 / SRC-14: "no fuzzy matching" is a weak narrator remark, not demonstrated.
- FB-27 / RTE-01 / U-13: "shows each type's data source" is now (I), with the EBE line quoted; FB-27 also records that the route menu stays open and is closed manually.
- FB-06 → FB-33: the "another little keyword search" quote moved to FB-33 as a probable Feature Properties filter.
- FB-01 / FB-02: the "Feature collection" '+' entry and its empty state are INFERRED; order of entries 3–5 is narration order.
- FB-04 / RTE-06: exact positions ("last button", "first of the right group") rephrased as narration order.
- SLS-01 / SLS-08 / SLS-19 / U-06: Add style "brings up" a menu/dialog; whether import/export and the style settings sit in it or in the panel is INFERRED/unknown. Presets at top and drawing options at bottom stay SOURCED. SLS-11: toolbar placement INFERRED (EBE narrates the swap with the panel open).
- LBL-05: crosshair location and positioning INFERRED. LBL-17 / U-19: source quoted exactly ("the default GEOlayers projects"); placement in the label area INFERRED.
- TB-02: user scripts in the same list INFERRED.
- Source table EBE row: second "pin tool" caption error (draw-on section) added; the narrator's plan status is now "(I) apparently has no premium plan".
- Src lines: EBE appended to MCS-22, LIC-06, LIC-16, LBL-18.

Added detail (from EBE text not recorded before): PIN-01 (a pre-animated layer keeps its own animation after pinning), PIN-14 (Rotate with Map may depend on the 3D switch, I), RTE-06 (arrow path stays editable with the pen tool, G; I: arrow follows the edited path), FB-30 (fill-only paths convert too), FB-46 (approximate grouping: feature actions, geometry group, then charts/export/remove). New open questions Q-50–Q-55 are cross-referenced from FB-27/29/46/47, VIEW-05, U-13 and U-16.

Adjustments (not rejections): OPEN_QUESTIONS Q-01 uses the count regenerated from FEATURE_SPEC after this pass (163 PARTLY INFERRED + 6 INFERRED = 169), not the suggested 162, because this pass downgraded seven more spec items (FB-01, FB-27, SLS-01, SLS-11, LBL-05, VIEW-07, LIC-03). The spec RTE-01 Req says each route type *may* show its data source (I), instead of stating it, to stay consistent with the FB-27 fix.

Rejected: none.

## Critic items rejected

| Critic item | Decision | Reason |
|---|---|---|
| TB-16: "Add both RKI scripts to `sources/`" | **Partly rejected** | The facts are added (TB-16, API-23) and cited by URL, but the files are not downloaded into `sources/`: saving files is a download, which this editing step was not authorised to do. Mirror them later if the user approves (OPEN_QUESTIONS). |
| Duplicate findings ("Improved NE search by name" from two critics; "Revision Workflow" from two critics) | Merged, not rejected | One item each: SRC-14, PRJ-11. |
| "Preview-map-blank / MapTiler-results-not-displayed" as new items | Folded in, not new IDs | Cited in UI-11 and SRC-06; they are bugfix regressions of existing features. |
| GEOlayers.app globe videos as a GL3 feature | Accepted only as a behaviour reference | GEOlayers.app stays out of scope (LIC-17); the videos inform 3D-10 only. |

No critic finding was judged factually wrong after re-checking the cited changelog/help lines (and GHHELP via WebFetch).
