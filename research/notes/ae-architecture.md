# AE technical platform notes: rebuilding a GEOlayers-3-style map extension (as of 2026-11-18)

Legend: **[S]** = SOURCED (URL or local file given). **[I]** = INFERRED (my reasoning, must be verified in AE).
Local sources: `research/sources/help-readme.md` (HELP), `research/sources/changelog.md` (CL), `research/sources/*-repo__*.jsx.txt` (EX).
Nothing in here comes from inspecting the GEOlayers product itself.

---

## 0. TL;DR architecture decision

- Build as a **CEP 12 HTML panel (Chromium 99 / Node 17.7.1) + ExtendScript host layer**. This is what GEOlayers itself is ("GEOlayers, a CEP Extension for After Effects", the developer on the Adobe forum) [S: https://forums.creativeclouddeveloper.com/t/migrating-cep-extension-to-uxp-seems-impossible/1623].
- UXP for AE only reached **public beta in After Effects (Beta) 27.1 (build 27.1.0.045, UXP 9.4)** in Nov 2026; the AE UXP API members are all "Min Version 27.0" [S: https://community.adobe.com/product-updates (via search summary), https://github.com/AdobeDocs/uxp-hub/pull/46, https://developer.adobe.com/after-effects/uxp/after-effects-api/property]. UXP has no WebGL / no full browser, so MapLibre/Mapbox GL can't run in it (Adobe staff, 2020: "UXP is not a browser… Mapbox GL JS uses WebGL which UXP doesn't currently support") [S: forum thread above]. UXP has no Node [S: https://aescripts-or-author-wiki.gitbook.io/author-wiki/technical-guidelines/uxp-plugins].
- CEP timeline for AE: no new CEP submissions to Adobe Marketplace **and** CEP disabled by default **Dec 2028**; CEP removed from AE **Dec 2029**. ExtendScripts "not affected by this transition" [S: https://blog.developer.adobe.com/en/publish/2026/09/investing-in-the-future-of-creative-cloud-extensibility-uxp-comes-to-our-flagship-applications]. A hobby build installed via PlayerDebugMode works until then. "Disabled by default" probably means users can turn it back on [I].
- Keep a **host abstraction layer** (`HostAdapter` interface: createComp, addLayer, setExpression, …) so a later UXP port (`require("aftereffects")`) only swaps the adapter. The map renderer (WebGL) would then have to move to a helper (a local browser/Electron window over websocket), as Adobe staff suggested in 2020 [S: forum thread] [I for design].

---

## 1. CEP vs UXP status for After Effects

### 1.1 UXP in AE
- Adobe dev blog (Sep 2026) timeline table: AE "Public beta UXP plugins by November 2026"; Media Encoder public beta live; Premiere GA UXP + Hybrid; InDesign UXP since 2023; Illustrator beta by Spring 2027; Photoshop since 2020. Each app gets at least 2 years after its UXP public beta before CEP is removed [S: blog URL above].
- Shipping: UDT (UXP Developer Tool) v2.4.0 "Supports After Effects Beta v27.1+"; UXP 9.4.0 ↔ AE (Beta) 27.1.0 version-matrix row added on 2026-11-18 [S: https://github.com/AdobeDocs/uxp-hub/pull/46].
- Docs: https://developer.adobe.com/after-effects/uxp/ and the API reference https://developer.adobe.com/after-effects/uxp/after-effects-api/ [S]. 43 classes: Application, Project, Preferences, Settings, ItemCollection, FolderItem, FileSource, FootageItem, ImportOptions, PlaceholderSource, SolidSource, Fonts, Font, CompItem, Layer, LayerCollection, AVLayer, CameraLayer, LightLayer, ParametricMeshLayer, ShapeLayer, TextLayer, ThreeDModelLayer, Property, PropertyGroup, MaskPropertyGroup, KeyframeEase, MarkerValue, Shape, TextDocument, CharacterRange, ParagraphRange, ComposedLineRange, RenderQueue, RenderQueueItem, RQItemCollection, OMCollection, OutputModule, Viewer, View, ViewOptions, GuideOptions, DeferredCall [S].
- Usage: `const app = require("aftereffects"); app.beginUndoGroup("…"); …; app.endUndoGroup();` [S].
- `Property` in UXP AE: RW `expression`, `expressionEnabled`, `dimensionsSeparated`, `enabled`, `name`, `selected`; methods `addKey`, `setValue`, `setValueAtTime`, `setValuesAtTimes`, `setValueAtKey`, `valueAtTime`, `keyTime`, `setInterpolationTypeAtKey`, `setTemporalEaseAtKey`, `setSpatialTangentsAtKey`, `setPropertyParameters` (dropdowns), etc. — all Min Version 27.0 [S: …/after-effects-api/property]. `PropertyGroup.addProperty(matchName)` adds effects to "ADBE Effect Parade" by match or display name [S: …/after-effects-api/propertygroup]. Pseudo effects / expression controls not mentioned [S].
- UXP vs CEP comparison: UXP = direct API calls, no Node, sandboxed FS with user consent, limited CSS subset, `.ccx` unsigned; CEP = ExtendScript bridge, Node available, unrestricted FS, full browser CSS, `.zxp` signed, heavy (one Chromium per panel) [S: aescripts author wiki URL above].
- Community status (Jan–Apr 2026): "AE doesn't have UXP yet, CEP still is the way" [S: https://forums.creativeclouddeveloper.com/t/after-effects-2025-cep-removal-and-uxp-panel-support-issue/11655]; pushREC KB note dated 2026-04-19 said the same [S: https://github.com/pushREC/after-effects-sdk-kb/blob/main/scripting/UXP-STATUS-NOTE.md]. Both are out of date now.
- Bolt UXP 1.3 said it was "ready for Media Encoder and After Effects as soon as Adobe makes the UXP APIs public" [S: https://blog.developer.adobe.com/en/publish/2026/08/building-plugins-faster-with-bolt-uxp-and-bolt-express (search summary)].
- **Conclusion:** UXP AE = beta-only (27.1 Beta app), no WebGL/webview → cannot host the map renderer. Do not target it now [I].

### 1.2 CEP 12 facts
- CEP 12.0 engine: **CEF 3 branch 99 / Chromium 99**, **Node.js 17.7.1**, Node-WebKit 0.62.1, V8 9.9.115 [S: https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_12.x/Documentation/CEP%2012%20HTML%20Extension%20Cookbook.md]. CEP 12.0.0.14 upgrade note also lists Mac OpenGL→Metal migration [S: https://community.adobe.com/questions-707/photoshop-cep-upgrade-to-cep-12-0-0-14-1217270 (search summary)].
- Cookbook host table, CEP 12 column: After Effects 25.0 (AEFT); Premiere 25.0 (PPRO); Photoshop 25.12; Illustrator 29.5.1; InDesign 20.4; Audition 25.0 [S: cookbook].
- Field report: AE 25.6.5, 26.2.1, 26.3.0 all report PlugPlug 12.0.1.2 (CEP 12); debug key CSXS.12 [S: https://community.adobe.com/bug-reports-528/in-recent-versions-of-after-effects-i-m-unable-to-install-or-sign-new-zxp-files-1632227]. That same post (Jul 2026, macOS 26.5.2) reports newly installed extensions failing with `Unsupported Manifest version ''` before "Parsing manifest". Workaround: none posted. Watch out for this when testing [S].
- AE 27 (GEOlayers added "After Effects v27 support" on 2026-08-10 [S: CL 1.18.0]) — CEP version in AE 27: unknown; assume CEP 12, set PlayerDebugMode for CSXS.12 **and** CSXS.13 to be safe [I; the samabr85 blog also sets both: https://samabr85.github.io/blog/cep-after-effects/].
- Chromium 99 supports WebGL2, OffscreenCanvas (incl. WebGL in workers), Web Workers, `structuredClone`, ES2021 → MapLibre GL JS v5 should load. **Must be verified in AE** [I]. Field reports: Three.js WebGL works in AE CEP panels (with occasional crashes when updating textures; null WebGL context on some dual-GPU machines) [S: https://discourse.threejs.org/t/full-orbit-control-within-after-effects-extension-preview-panel/78966, https://community.adobe.com/questions-712/webgl-context-null-in-adobe-extension-with-dual-graphic-cards-1061448, https://community.adobe.com/questions-628/issues-when-using-webgl-in-an-extension-686022]. GEOlayers itself evidently uses web workers for tile rendering: "Bugfix: web-worker tile-render memory leaks" [S: CL 1.1.8], and the developer named "missing webgl and webworkers" as the reason UXP was impossible [S: search summary of the forum thread].

### 1.3 manifest.xml (CSXS/manifest.xml)
- HTML extensions need ExtensionManifest Version ≥ 5.0 [S: cookbook]. Host code for AE: `AEFT` [S: cookbook, search summary].
- Recommended skeleton [I, assembled from cookbook + samabr85 blog]:
```xml
<ExtensionManifest Version="7.0" ExtensionBundleId="com.hobby.maplayers" ExtensionBundleVersion="0.1.0">
  <ExtensionList><Extension Id="com.hobby.maplayers.main" Version="0.1.0"/>
                 <Extension Id="com.hobby.maplayers.worker" Version="0.1.0"/></ExtensionList>
  <ExecutionEnvironment>
    <HostList><Host Name="AEFT" Version="[25.0,99.9]"/></HostList>
    <LocaleList><Locale Code="All"/></LocaleList>
    <RequiredRuntimeList><RequiredRuntime Name="CSXS" Version="11.0"/></RequiredRuntimeList>
  </ExecutionEnvironment>
  <DispatchInfoList>
    <Extension Id="com.hobby.maplayers.main">
      <DispatchInfo>
        <Resources>
          <MainPath>./index.html</MainPath>
          <ScriptPath>./jsx/host.jsx</ScriptPath>
          <CEFCommandLine>
            <Parameter>--enable-nodejs</Parameter>
            <Parameter>--mixed-context</Parameter>
            <Parameter>--allow-file-access-from-files</Parameter>
            <Parameter>--allow-file-access</Parameter>
            <Parameter>--disable-site-isolation-trials</Parameter>
          </CEFCommandLine>
        </Resources>
        <Lifecycle><AutoVisible>true</AutoVisible></Lifecycle>
        <UI><Type>Panel</Type><Menu>MapLayers</Menu>
          <Geometry><Size><Width>420</Width><Height>720</Height></Size>
                    <MinSize><Width>300</Width><Height>300</Height></MinSize></Geometry>
          <Icons>…</Icons></UI>
      </DispatchInfo>
    </Extension>
  </DispatchInfoList>
</ExtensionManifest>
```
- CEF flags: `--enable-nodejs`, `--mixed-context` (CEP 7+), `--disable-site-isolation-trials`, `--disable-features=SameSiteByDefaultCookies,CookiesWithoutSameSiteMustBeSecure,NetworkService`, `--enable-media-stream` appear in the CEP 12 cookbook; CEP filters out `--remote-debugging-port` and `--ignore-certificate-errors` [S: cookbook]. `--allow-file-access`, `--allow-file-access-from-files` used by the samabr85 blog [S].
- Node contexts: separate context (default with `--enable-nodejs`) → `cep_node` shared across frames, Node globals only in main frame; mixed context → Node globals in every frame, `cep_node` per frame. `require` paths must be absolute. Libraries that detect `module`/`exports` (UMD) may load in the wrong mode — workaround needed (e.g. temporarily hide `module`) [S: cookbook]. For bundled (Vite/webpack) code, use `window.cep_node.require` or `window.require` for Node built-ins [I; Bolt CEP does `src/js/lib/node.ts` [S: https://github.com/hyperbrew/bolt-cep]].
- Extension types: `Panel` (dockable, reopens at startup), `ModalDialog`, `Modeless`, `Custom` (invisible, CEP 5+; `AutoVisible=false`, no `<Menu>`, `<StartOn><Event>…`) [S: cookbook]. Use `Modeless`/`ModalDialog` windows for dialogs like GEOlayers' "modalEditObject"/"modalActions" [S: HELP] or render modals inside the main panel [I]. Persistence events exist only for PS/ID/IC (`com.adobe.PhotoshopPersistent` etc.); AE: none documented [S: cookbook].
- Extension folder: `#` not allowed in path [S: cookbook] (cf. GEOlayers "Importing files with '#' in path fail" [S: CL 1.14.1]).
- Folders: user — Win `C:\Users\<USER>\AppData\Roaming\Adobe\CEP\extensions`, mac `~/Library/Application Support/Adobe/CEP/extensions`; system — Win `C:\Program Files (x86)\Common Files\Adobe\CEP\extensions` and `C:\Program Files\Common Files\Adobe\CEP\extensions`, mac `/Library/Application Support/Adobe/CEP/extensions` [S: cookbook]. (A 2026 blog claims only `…\Adobe After Effects 2026\Support Files\<folder>` worked on its Windows box — unverified, contradicts cookbook [S: samabr85 blog].)

### 1.4 Debug mode, .debug, signing
- Unsigned extensions: `PlayerDebugMode` = string "1".
  - Windows: `HKEY_CURRENT_USER\Software\Adobe\CSXS.12` → String `PlayerDebugMode` = `1`.
  - macOS: `defaults write com.adobe.CSXS.12 PlayerDebugMode 1`; then `killall cfprefsd` or log out (plist caching) [S: cookbook].
- Remote debugging: `.debug` file in extension root:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<ExtensionList>
  <Extension Id="com.hobby.maplayers.main">
    <HostList><Host Name="AEFT" Port="8088"/></HostList>
  </Extension>
</ExtensionList>
```
  Ports 1024–65534; open `http://localhost:8088/` in Chrome (or `chrome://inspect`) [S: cookbook].
- Signing: ZXPSignCmd from Adobe-CEP/CEP-Resources (`ZXPSignCMD/` folder; macOS & Windows binaries) [S: cookbook mentions tool; platforms I]. Commands:
  - `ZXPSignCmd -selfSignedCert <countryCode> <stateOrProvince> <org> <commonName> <password> <out.p12> [-validityDays N]`
  - `ZXPSignCmd -sign <srcDir> <out.zxp> <cert.p12> <password> -tsa <timestampURL>`
  - `ZXPSignCmd -verify <file.zxp> [-certinfo]` [S for selfSignedCert syntax: https://community.adobe.com/questions-628/zxpsigncmd-signing-extension-with-a-self-signed-certificate-on-mac-and-windows-685119 (search summary); -sign/-verify I].
  - TSA breakage Apr 2025, fixed in ZXPSignCmd 4.1.3 for all platforms / `vite-cep-plugin@1.2.9` [S: https://community.adobe.com/bug-reports-728/zxpsigncmd-sign-process-is-broken-segmentation-fault-1331508, https://github.com/hyperbrew/bolt-cep].
  - No native Linux ZXPSignCmd; a Docker image `lunalabsltd/zxpsigncmd` wraps v4.1.103 (old) [S: https://hub.docker.com/r/lunalabsltd/zxpsigncmd/tags]. **For a personal hobby build, skip signing: copy the folder + PlayerDebugMode** [I].
- Install signed ZXP: aescripts "ZXP/UXP Installer", or Adobe's `UnifiedPluginInstallerAgent --install x.zxp` (mac/win) [I].

### 1.5 Recommended toolchain
- **Bolt CEP** (hyperbrew): Vite + TS + Sass; ExtendScript authored in TS/ES6 → single ES3 file; JSON2 included; ponyfills `Object.freeze`, `Array.isArray` (+ custom via `jsxPonyfill()` in `vite.es.config.ts`); `// @include` bundling; JSXBIN via `jsxbin` pkg; typed bridge `evalTS("fn", args)`, `listenTS()` / `dispatchTS()` (event types in `shared/universals.ts`); `yarn dev` HMR with panel viewable at `http://localhost:3000/panel/`; `yarn zxp`, `yarn zip`; `cep.config.ts` (panels: `mainPath`, `scriptPath`, `panelDisplayName`, `type`, `startOnEvents`; `parameters` = CEF flags; `installModules`; `copyAssets`; `zxp.tsa`). Supports CC 2024+; Win + Mac (Linux not mentioned) [S: https://github.com/hyperbrew/bolt-cep].
- Types: `types-for-adobe` (docsforadobe/Types-for-Adobe; npm 7.2.x) for AE ExtendScript typings [S: https://github.com/aenhancers/Types-for-Adobe, https://www.npmjs.com/package/types-for-adobe].

---

## 2. Panel ⇄ host bridge (how a `geolayers3`-style ExtendScript API can be async)

Sourced observations:
- "In general, the GEOlayers panel needs to be open when you want to call any function of the API." [S: HELP]
- API callbacks are async `(error, data)`; `fetchText/fetchJson` take a **fetch() init object** ("Please search the fetch API docs for the init object") [S: HELP] → network is done in the Chromium panel, not ExtendScript.
- `geocode` queues Nominatim requests per usage policy [S: HELP]; `watch`/`watchCsv` poll with `interval` default 10000 ms, and calls are deferred "if the scripting api is busy finalizing" [S: HELP].
- User scripts call `geolayers3.addToBrowser(...)` with no `#include` and use `Array.prototype.forEach` [S: EX geodata-repo__COVID-19_Casedata_World.jsx.txt] → a global object exists in AE's shared main ExtendScript engine, and ES5 array methods are polyfilled (ExtendScript is ES3) [I].
- "Close Undo Groups on each host call manually"; "Hidden Pref to purge Undo Cache on each host call (prevent AE undo-grouping bugs)"; "Error in host method 'initEngine' due to other scripts interfering" [S: CL 1.5.3, 1.14.1] → there is a host-side init function that installs the engine into the shared global namespace, and other scripts' globals can collide [I].

Design [I]:
1. Panel loads → `csInterface.evalScript('$.evalFile("<ext>/jsx/host.jsxbin")')` then `MapLayersHost.initEngine()`, which defines global `geolayers3`-like object + ES5 polyfills (Array.forEach/map/filter/indexOf, Object.keys, JSON).
2. Sync API methods (getMapcomps, getMapcompAnchor, duplicate, setViewAtTime…) run purely in ExtendScript.
3. Async methods: ExtendScript stores callback in `callbacks[id]`, then dispatches to the panel:
   ```js
   var xLib = new ExternalObject("lib:PlugPlugExternalObject");
   var ev = new CSXSEvent(); ev.type = "com.hobby.maplayers.request"; ev.data = JSON.stringify({id:id, op:"fetchJson", args:[url, init]}); ev.dispatch();
   ```
   Panel: `csInterface.addEventListener("com.hobby.maplayers.request", handler)` → does work → `csInterface.evalScript('geolayers3._resolve(' + id + ',' + JSON.stringify(JSON.stringify(result)) + ')')`.
   PlugPlug/CSXSEvent pattern [S: cookbook; AE usage reported working on forums: https://community.adobe.com/questions-628/communicate-between-jsx-and-js-685529]. Not officially documented for AE in cookbook (which lists PS/AI/PPro/Audition) → test [S/I].
4. Large payloads (GeoJSON, rendered frame lists): write a temp JSON file with Node `fs` and pass only the path; ExtendScript reads via `File.open('r'); file.encoding='UTF-8'; read()` [I]. `evalScript` arguments need double JSON stringification [S: samabr85 blog].
5. Blocking calls (`alert`, `renderQueue.render()`) suspend the event chain → events dispatched before may not fire [S: https://community.adobe.com/questions-729/event-chaining-1424988 (search summary)].
6. AE has **no project-opened / selection-changed events** for CEP; options: poll `app.project.file` path/name at modest interval, or refresh on panel focus [S: https://community.adobe.com/questions-529/how-to-receive-an-event-when-opening-a-project-in-aftereffects-53920 (search summary)]. Needed for: Mapcomp list refresh, "autorun.jsx can run automatically on opening a project", "GeoJSONs next to the project load automatically when opened via the projects panel" [S: CL 1.0.9] → GEOlayers likely triggers autorun when the project is opened *through its own Projects panel* (sourced wording) [I].
7. `app.scheduleTask(code, delayMs, repeat)` / `app.cancelTask(id)` can drive host-side intervals (watch, deferred work) [I].
8. CSInterface utilities: `getSystemPath(SystemPath.EXTENSION|USER_DATA|HOST_APPLICATION)`, `openURLInDefaultBrowser`, `setPanelFlyoutMenu(xml)` + `com.adobe.csxs.events.flyoutMenuClicked`, `setContextMenuByJSON`, `registerKeyEventsInterest`, `getHostEnvironment()` (appSkinInfo for theme), `com.adobe.csxs.events.ThemeColorChanged`, `requestOpenExtension(id)`, `closeExtension()` [S partial: cookbook lists evalScript, addEventListener, dispatchEvent, setPanelFlyoutMenu, setContextMenuByJSON, registerKeyEventsInterest, getScaleFactor, resizeContent; the rest I]. GEOlayers uses a flyout/context menu ("Find the projects panel in the context- or flyout menu of the GEOlayers panel") [S: HELP].
9. Modifier-key behaviours (CTRL/CMD-click import from URL, SHIFT-click remove all features, SHIFT-click export, ALT/OPT-click scripts shows description, CTRL/CMD-click reveal file, SHIFT/ALT-select label template) [S: CL 1.4.2, 1.3.0, 1.2.0, 1.0.6] are plain DOM `MouseEvent.ctrlKey/metaKey/shiftKey/altKey` in the panel; "reveal in AE project" = ExtendScript `item.selected = true` / `app.executeCommand(app.findMenuCommandId("Reveal in Project"))` [I]; reveal file = Node `child_process` `open -R` (mac) / `explorer /select,` (win), or ExtendScript `Folder.execute()` [I].

Persistence of settings:
- Panel prefs → JSON file in a user folder (GEOlayers has a "GEOlayers User Folder", `geolayers3.userFolder()`/`userFile(name)` [S: HELP]); path via Node `os.homedir()` / `csInterface.getSystemPath(SystemPath.USER_DATA)` [I].
- Host-side: `app.settings.saveSetting(section, key, value)`, `getSetting`, `haveSetting` [I].
- Per-project data saved in .aep ("Drawing options … saved in the .aep" [S: CL 1.1.8]; "Mapcomp filters for Label Templates and Shape Layer styles not loaded from project on init" [S: CL 1.2.6]) → options: `app.project.xmpPacket` (read/write RDF XML; use `new ExternalObject("lib:AdobeXMPScript")`, `new XMPMeta(app.project.xmpPacket)`, `setProperty(ns, prop, value)`, `XMPMeta.registerNamespace(uri, prefix)`, `app.project.xmpPacket = meta.serialize()`) [S: https://ae-scripting.docsforadobe.dev/general/project/ (example uses xmp:Label)]; or `Item.comment` / `Layer.comment`; or JSON in a hidden "data" comp's layer marker comment / text layer source text [I].

---

## 3. ExtendScript host side — concrete APIs

(References: AE Scripting Guide https://ae-scripting.docsforadobe.dev/ [S where cited]; signatures below are standard ExtendScript API [I unless page cited].)

### 3.1 Project/comps/folders
- `app.project.items.addComp(name, width, height, pixelAspect, duration, frameRate)` → CompItem. Max comp size 30000×30000 [I].
- `app.project.items.addFolder(name)`; `item.parentFolder = folder` (GEOlayers "sort everything into its project structure" [S: HELP importProject]).
- `app.project.importFile(new ImportOptions(file))` → FootageItem; `importAs = ImportAsType.PROJECT` to import an .aep/.aet (for `importProject()` / Label Template projects [S: HELP]); `ImportAsType.COMP|COMP_CROPPED_LAYERS|FOOTAGE|PROJECT`; `canImportAs(type)` [S: https://ae-scripting.docsforadobe.dev/other/importoptions/].
- Image sequence import: `io.sequence = true; io.forceAlphabetical = false;` (undocumented `rangeStart`/`rangeEnd`) [S: same page]. Then `footage.mainSource.conformFrameRate = comp.frameRate` [I]. Replace existing: `footage.replaceWithSequence(file, forceAlphabetical)`, `footage.replace(file)`; refresh: `footage.mainSource.reload()` [I].
- `app.project.importFileWithDialog()`; `app.project.save([file])`; `app.project.consolidateFootage()`; `app.project.autoFixExpressions(old, new)` (useful after renaming Mapcomps — GEOlayers duplicate/import "update expressions accordingly" [S: HELP]); `app.project.expressionEngine` = `"extendscript"` | `"javascript-1.0"` (AE 16.0+); `app.project.layerByID(id)` (22.0+); `app.project.bitsPerChannel`, `workingSpace` [S: https://ae-scripting.docsforadobe.dev/general/project/].
- Layer IDs `Layer.id` (22.0+) → stable references for pinned layers [S: https://ae-scripting.docsforadobe.dev/introduction/changelog/].
- Duplicate: `comp.duplicate()` (then rename & fix expressions) [I].
- Precompose: `comp.layers.precompose(indices, name, moveAllAttributes)` (GEOlayers default script `precompose_feature_shape_layer.jsx` [S: CL 1.4.0]) [I for API].
- Comp settings: `comp.renderer` (`"ADBE Advanced 3d"` Classic 3D, `"ADBE Ernst"` CINEMA 4D, `"ADBE Calder"` Advanced 3D — verify via `comp.renderers`) [I]; GEOlayers sets CINEMA 4D renderer for extruded features ("autoChangeRenderer" option; "Renderer not set to CINEMA 4D when drawing Extruded Features") [S: HELP, CL 1.5.2]. Motion blur: `comp.motionBlur`, `comp.shutterAngle`, `comp.shutterPhase`, `layer.motionBlur` (GEOlayers "Mapcomp Settings for Motion Blur" [S: CL 1.0.7]).
- Work area: `comp.workAreaStart`, `comp.workAreaDuration` (finalize `onlyWorkArea` [S: HELP]; "Can't set work area duration" bug [S: CL 1.0.1]).

### 3.2 Layers
- `comp.layers.addNull([duration])`, `addSolid(color, name, w, h, pixelAspect[, duration])`, `addText(sourceText|TextDocument)`, `addBoxText([w,h])`, `addShape()`, `addCamera(name, centerPoint)`, `addLight(name, centerPoint)`, `add(item[, duration])`, `addVerticalText` (24.2) [I/S changelog].
- `layer.threeDLayer = true`, `layer.parent = anchorLayer`, `layer.setParentWithJump(newParent)`, `layer.moveBefore/After/ToBeginning`, `layer.locked`, `layer.shy`, `layer.guideLayer`, `layer.enabled`, `layer.comment`, `layer.label` (0–16), `layer.blendingMode = BlendingMode.MULTIPLY…` (Shape Layer Styles support blending modes [S: CL 1.1.2]), `layer.startTime`, `layer.inPoint/outPoint` (time offsets for drawing/labels [S: CL 1.2.3]), `layer.collapseTransformation` [I].
- 3D/material: `layer.property("ADBE Material Options Group").property("ADBE Casts Shadows")`, `"ADBE Accepts Shadows"` (draw options castsShadows/acceptsShadows [S: HELP]); extrusion depth on shape/text layers in C4D/Advanced 3D: `"ADBE Extrsn Options Group"` → `"ADBE Extrsn Depth"` [I – verify with a property-tree dump].
- Track mattes: `AVLayer.setTrackMatte(matteLayer, TrackMatteType.ALPHA)` (23.0+) [S: changelog page] — needed for water masks / "Preserve Transparency" [I].
- Separate dimensions: `position.dimensionsSeparated = true/false`; GEOlayers bug with pref "Default Position Properties to Separate Dimensions" [S: CL 1.5.0] → always check/force [I].
- Transform locks: GEOlayers "Shape Layer Transform Properties can be locked" [S: CL 1.2.6] → implemented with expressions on transform props or `layer.locked` [I].

### 3.3 Effects / Expression Controls / pseudo effects
- Add: `layer.property("ADBE Effect Parade").addProperty("ADBE Slider Control")`; set `.name = "Latitude"`; value `.property(1).setValue(48.137)`.
- Match names (effect): `ADBE Slider Control`, `ADBE Point Control`, `ADBE Point3D Control`, `ADBE Checkbox Control`, `ADBE Color Control`, `ADBE Angle Control`, `ADBE Layer Control`, `ADBE Dropdown Control` [S for Slider/Point/Checkbox/Layer: https://after-effects-scripting-guide.readthedocs.io/matchnames/effects/firstparty.html (search summary); rest I]. Inner property match names follow `"<effect>-0001"` (e.g. `ADBE Slider Control-0001`) [I – verify].
- Dropdown: `prop.setPropertyParameters(["A","B"])` (17.0.1+), `Property.propertyParameters`, `Property.valueText` (26.0) [S: changelog page]. After setting items the effect's match name changes to a `Pseudo/@@…` style name — community reports the dropdown match name "randomly changing" [S title only: https://community.adobe.com/questions-529/match-name-for-dropdown-menu-control-effect-randomly-changing-26647] → reference by index/name, not match name [I].
- Expression access in AE: `effect("Latitude")("Slider")` or `effect("Latitude")(1)` (index access is language-independent) [I].
- **Custom grouped control panels (pseudo effects)**: a single effect named e.g. "Map View" with params Latitude/Longitude/Zoom/Bearing/Pitch requires a **pseudo effect**. Without editing `PresetEffects.xml`, ship a `.ffx` animation preset containing a pseudo effect whose matchName begins with `Pseudo/`, and apply via `layer.applyPreset(File)` [S: https://community.adobe.com/questions-529/noob-friendly-guide-how-to-create-pseudo-effect-ffx-presets-that-can-be-used-on-any-system-59460 (search summary)]. `applyPreset` acts on the **currently selected layers** (select only the target first; if none selected AE creates a new solid) [S: https://creativecow.net/forums/thread/create-grouped-effects-in-effects-controls-panel-via-script/ (search summary) + Layer.applyPreset docs I]. Building .ffx bytes programmatically is unofficial / undocumented [S: https://scrapbox.io/Tsut-ps/… (search summary)]. **The .ffx must be authored once inside AE** (on a Mac/Win machine) — can't be produced on Linux reliably [I]. Fallback: separate expression-control effects named "Latitude", "Longitude", "Zoom", "Bearing", "Pitch" — the GEOlayers HELP calls them "the Latitude, Longitude, Zoom, Bearing and Pitch controls" [S: HELP removeMapcompControlKeys] — which form they use is UNKNOWN.
- GEOlayers label templates: "default to Scale or Rotate with the Map via effect controls on their 'Label Anchor Point' layer" [S: CL 1.0.4]; pinned layers: "separate Latitude, Longitude and Height properties" + "Position Progress" slider [S: CL 1.1.0, 1.1.2].

### 3.4 Keyframes / markers / expressions
- `prop.setValueAtTime(t, v)`, `prop.setValuesAtTimes(times[], values[])` (fast bulk), `prop.addKey(t)`, `prop.removeKey(i)`, `prop.numKeys`, `prop.nearestKeyIndex(t)`, `prop.setInterpolationTypeAtKey(i, KeyframeInterpolationType.BEZIER|LINEAR|HOLD)`, `prop.setTemporalEaseAtKey(i, [new KeyframeEase(speed, influence)])`, `prop.setSpatialTangentsAtKey`, `prop.setTemporalAutoBezierAtKey`, `prop.keyLabel/setLabelAtKey` (22.6) [I/S changelog].
  - `setViewAtTime(comp, view, forceKeyframe, time)`: if prop has keys or forceKeyframe → `setValueAtTime`, else `setValue` [S semantic: HELP; API I]. `removeMapcompControlKeys` → loop `removeKey(numKeys…1)` [I].
  - "If GEOlayers creates an animation it shows a prompt to adjust its duration" [S: CL 1.2.3]; "Keyframes created by a data-driven property can have non-linear timing" [S: CL 1.2.4].
- Expressions: `prop.expression = "…"`, `prop.expressionEnabled`, `prop.expressionError` [I]. Engine: JavaScript engine (ECMAScript 2018, AE 16.0+) vs Legacy ExtendScript (ES3); old projects default to Legacy; JS engine is strict on `if/else` (always use full `if(){…}else{…}` and end with a value); JS engine up to 5× faster [S: https://helpx.adobe.com/after-effects/using/legacy-and-extend-script-engine.html]. GEOlayers had "Dashed lines cause expression errors with legacy expression engine" [S: CL 1.2.0] → **generate expressions that run under both engines** (no `let/const`, no arrow functions, no template literals, `thisLayer` instead of `this`, no snake_case aliases) [I].
- Markers: `new MarkerValue(comment[, chapter, url, frameTarget, cuePointName, params])`, `mv.duration`, `mv.label`, `mv.setParameters(obj)`/`getParameters()` (hidden key-value storage — preferred for machine data since comment/chapter/url/frameTarget show on the timeline [S: samabr85 blog]); `layer.property("ADBE Marker").setValueAtTime(t, mv)`; comp markers `comp.markerProperty` [I]. GEOlayers: "Label layers update their markers from the source comp (responsive timing)" [S: CL 1.0.4].

### 3.5 Shape layers from vertices
- Structure: `shapeLayer.property("ADBE Root Vectors Group").addProperty("ADBE Vector Group")` → `.property("ADBE Vectors Group").addProperty("ADBE Vector Shape - Group")` → `.property("ADBE Vector Shape").setValue(shape)`; `addProperty("ADBE Vector Graphic - Stroke")`, `"ADBE Vector Graphic - Fill"`, `"ADBE Vector Graphic - G-Fill"`, `"ADBE Vector Filter - Trim"` (`ADBE Vector Trim Start/End/Offset/Type`), `"ADBE Vector Filter - Repeater"`, `"ADBE Vector Filter - Merge"`, `"ADBE Vector Filter - Offset"`; dashes: `"ADBE Vector Stroke Dashes"` → `addProperty("ADBE Vector Stroke Dash 1")`, `"ADBE Vector Stroke Gap 1"`, `"ADBE Vector Stroke Offset"`; `ADBE Vector Stroke Line Cap` (1 butt, 2 round, 3 projecting), `ADBE Vector Stroke Line Join` [S match names: https://ae-scripting.docsforadobe.dev/matchnames/layer/shapelayer/; enum values I].
- `var s = new Shape(); s.vertices = [[x,y],…]; s.inTangents = [[0,0],…]; s.outTangents = […]; s.closed = true;` (tangents relative to vertex; arrays same length) [S: https://ae-scripting.docsforadobe.dev/other/shape/].
- Polygons with holes: multiple paths in one group + fill rule (`ADBE Vector Fill Rule` 1=Non-Zero, 2=Even-Odd) or Merge Paths [I]. GEOlayers: "Multi Polygon shapes render faster (different shape layer content structure)" [S: CL 1.2.5].
- Dashes with zoom: GEOlayers "Dash Zoom Threshold", "Dash Speed", round caps on dashes, "dashThreshold" draw option default 1 [S: CL 1.1.7, 1.2.0, 1.2.5, HELP] → dash/gap/offset expressions scaled by `Math.pow(2, zoom - zoomAtDraw)` and offset += speed*time [I].
- Auto Stroke Width (stroke width compensates zoom scale) [S: CL 1.1.7, HELP autoStrokeWidth] → stroke width expression `w / scaleFactor` [I].
- Trim Paths for "addTrimPaths" + `duration` default 10 (line drawing animation) [S: HELP] → keyframe `ADBE Vector Trim End` 0→100 over duration [I].
- Large coordinate magnitudes: draw geometry in **local pixel space at a reference zoom** (relative to feature centroid or Mapcomp center), not absolute world pixels, to avoid float issues and AE's position range limits [I]. Date-line: draw world copies ±worldSize ("Features drawn multiple times if they cross the date line", "dateBorderCopy" option, "Shape layer world copies" [S: CL 1.2.0, HELP, CL 1.15.2]).
- Bulk shapes: building thousands of paths through ExtendScript is slow → build per-layer, wrap whole job in one undo group, set `app.beginSuppressDialogs()` and possibly disable comp viewer updates; consider `setValuesAtTimes` [I].

### 3.6 Text layers
- `var tl = comp.layers.addText("Berlin"); var td = tl.property("ADBE Text Properties").property("ADBE Text Document").value; td.font = "Arial-BoldMT"; td.fontSize = 48; td.fillColor = [1,1,1]; td.applyStroke = true; td.strokeColor; td.strokeWidth; td.justification = ParagraphJustification.CENTER_JUSTIFY; td.tracking; td.leading; td.allCaps; td.direction (24.0+, for RTL); tl.property("ADBE Text Properties").property("ADBE Text Document").setValue(td);` [I; RTL-related TextDocument.direction listed in 24.0 changelog S]. GEOlayers had multiple RTL bugfixes [S: CL 1.6.1, 1.12.2, 1.13.1].
- Fonts: `app.fonts` (24.0+), `getFontsByPostScriptName` [S changelog page].
- Label Templates = comps containing text layers + AVLayer assets that get swapped [S: CL 1.14.1] → `sourceText` expression or per-instance text set via `Essential Properties` / `layer.essentialProperty` / duplicating template comps [I].

### 3.7 Render queue / undo / misc
- `var rq = app.project.renderQueue.items.add(comp); rq.outputModule(1).applyTemplate("Lossless"); rq.outputModule(1).file = new File(path); rq.timeSpanStart/timeSpanDuration; app.project.renderQueue.render()` (blocking) or `renderQueue.queueInAME(startRender)`; `rq.render = false`; `rq.status` (RQItemStatus) [I]. GEOlayers "Directly queue Renders after finalization" [S: CL 1.2.6].
- Undo: `app.beginUndoGroup("Create Mapcomp")` / `app.endUndoGroup()` — always close in `finally` (GEOlayers closes undo groups on every host call [S: CL 1.5.3]); `app.purge(PurgeTarget.UNDO_CACHES)` for the hidden "purge undo cache" pref [I API; feature S]. `app.purge(PurgeTarget.ALL_CACHES)` includes disk cache from 24.3 [S: changelog page].
- Dialog suppression: `app.beginSuppressDialogs()` / `app.endSuppressDialogs(false)` [I].
- File I/O in ExtendScript requires pref *Scripting & Expressions → Allow Scripts to Write Files and Access Network* (`app.preferences.getPrefAsLong("Main Pref Section","Pref_SCRIPTING_FILE_NETWORK_SECURITY")`) [I]. Prefer Node fs in the panel [I].
- `app.version`, `app.buildNumber`, `app.isRenderEngine`, `$.os`, `app.executeCommand(id)`, `app.findMenuCommandId(name)` [I].
- Log files ("Create log files" [S: CL 1.1.6]) → Node fs append in user folder [I].

---

## 4. Rendering map imagery to files from a CEP panel

### 4.1 What GEOlayers does (sourced clues only)
- Finalize options `onlyCurrentFrame`, `previewQuality`, `onlyWorkArea`, `purgeImageryCache` [S: HELP] → imagery is rendered per frame / per time range, cached on disk, and a preview-quality mode exists. "Default Imagery Quality Setting in Preferences" [S: CL 1.15.1].
- "Tilemerger supports TMS servers", "tilemerger invalid urls at longitude < -180" [S: CL 1.0.9, 1.0.4] → raster tiles are fetched (XYZ / TMS y-flip) and merged.
- "Overzoomed raster imagery placed with lower-zoom tiles instead of overzoomed textures"; "imagery texture filtering blurry" [S: CL 1.1.2] → imagery textures are placed/scaled in AE (layers) and/or WebGL textures.
- "web-worker tile-render memory leaks" [S: CL 1.1.8] → tile rendering in Web Workers.
- "DEM tile requests … stall finalization" [S: CL 1.2.5]; "Local Tile Directory min and max zoom are read from metadata.json" [S: CL 1.5.2]; "{year},{month},{date},{dateISO} variable support in tile URIs" [S: CL 1.4.0]; "Map Bounds supported for Raster Styles" [S: CL 1.4.0].
- Exact pipeline (frame sequence vs tile layers) is UNKNOWN.

### 4.2 Recommended implementation [I unless noted]
Two compatible strategies (choose per style):
- **A. Per-frame rendered sequence (vector styles, MapLibre):** an offscreen `<div>` sized to the Mapcomp (CSS px = comp px / pixelRatio) with `new maplibregl.Map({container, style, interactive:false, fadeDuration:0, canvasContextAttributes:{preserveDrawingBuffer:true, antialias:true}, pixelRatio:1, maxCanvasSize:[16384,16384], center, zoom, bearing, pitch})`. For each frame: read Lat/Lon/Zoom/Bearing/Pitch (sampled by ExtendScript via `prop.valueAtTime(t, false)`, batched to a JSON file), `map.jumpTo({center, zoom, bearing, pitch})`, await `map.once('idle')` (or poll `map.loaded() && map.areTilesLoaded()`), then `map.getCanvas().toBlob('image/png')` → `Buffer.from(await blob.arrayBuffer())` → `fs.writeFileSync(path/frame_00001.png)`. Then ExtendScript imports with `ImportOptions.sequence = true`.
  - MapLibre facts: `canvasContextAttributes` defaults `antialias:false, preserveDrawingBuffer:false, powerPreference:'high-performance'`, `contextType` 'webgl2'|'webgl'; `pixelRatio` default `devicePixelRatio`; `maxCanvasSize` default `[4096,4096]` (don't exceed `MAX_TEXTURE_SIZE`); `fadeDuration` default 300 ms (label fade → set 0 for deterministic frames); `transformRequest(url, type)` for API keys/headers; `localIdeographFontFamily`; `maxPitch` default 60 (≤180 accepted in v5); `renderWorldCopies` default true; `setPixelRatio()`; `'idle'` event [S: https://cdn.jsdelivr.net/npm/maplibre-gl@5.23.0/src/ui/map.ts]. `preserveDrawingBuffer:false` → blank `toDataURL` [S: search summary of SO thread].
  - Larger than GPU limits (e.g. 8K/16K): render in **tiles** — offset the center by tile-sized pixel shifts (`map.project`/`unproject`) or use `map.transform` padding, then stitch with Node (e.g. `sharp` is native → avoid in CEP; use a pure-JS PNG encoder like `fast-png`/`UPNG.js` in a worker) or import tiles as separate layers [I].
  - Headless alternative outside AE (and on Linux for testing): `@maplibre/maplibre-gl-native` Node bindings: `new mbgl.Map({request, ratio})`, `map.load(style)`, `map.render({zoom, width, height, center, bearing, pitch}, cb)` → raw RGBA; binaries for Ubuntu 22.04, macOS 12, Windows amd64, Node 18/20/22 [S: https://cdn.jsdelivr.net/npm/@maplibre/maplibre-gl-native@6.4.1/README.md (search summary)]. Native ABI won't match CEP's Node 17.7.1 → run it as a **child process with a system Node** (`child_process.spawn`) if used [I]. `mbgl-renderer` wraps it with `ratio` [S: https://www.github.com/consbio/mbgl-renderer].
- **B. Raster tile mosaics (XYZ raster styles, satellite):** fetch `z/x/y` tiles (`fetch` in a Worker; respect provider rate limits), decode with `createImageBitmap`, composite onto `OffscreenCanvas` (2D) for the needed extent at integer zoom + overzoom, `convertToBlob({type:'image/png'|'image/jpeg', quality})`, write with fs. For animated zoom, render per frame or render a few zoom-level mosaics and crossfade/scale in AE with expressions (`scale = 100*2^(zoom - z_mosaic)`) [I]. TMS: `y_tms = 2^z - 1 - y` [I].
- Workers: `new Worker(url)` works in Chromium 99; Node isn't available inside workers in CEP → workers return ArrayBuffers (transferable) to the main frame which writes files [I]. Terminate/recycle workers and `ImageBitmap.close()` to avoid the kind of memory leaks GEOlayers fixed [I].
- Caching: disk cache directory keyed by provider/z/x/y (GEOlayers: "purgeImageryCache", "Configurable Feature Cache Paths (hidden pref)" [S: HELP, CL 1.0.7]) [I].
- DEM / 3D landscapes: Terrain-RGB decode `h = -10000 + (R*65536 + G*256 + B)*0.1`; Terrarium `h = R*256 + G + B/256 - 32768` [I – provider formulas]; output a 16-bit-ish heightmap (or split into two 8-bit channels recombined in AE — GEOlayers' heightmap used the "Calculations" effect [S: CL 1.5.2]) for Helium/Plexus/FreeForm Pro/Mir displacement [S plugin names: task context; I mechanism].
- Labels from vector tiles ("Mapcomp labels in different languages") → either baked into imagery or created as AE text layers from `map.queryRenderedFeatures` / tile data [I].

---

## 5. Expression maths: Web Mercator, pinned layers, anchor / "MapPivot"

### 5.1 Sourced constraints
- Mapcomp controls: Latitude, Longitude, Zoom, Bearing, Pitch (floats) [S: HELP setViewAtTime]. Anchor layer in containing comp "matches the Mapcomp's view animation and can be used to parent layers" [S: HELP]. A layer named **MapPivot** exists ("empty expressions on MapPivot layer") [S: CL 1.1.8]. `getLayersGeoPosition(layer)` returns [lon, lat] of a layer in a containing comp [S: HELP] → inverse projection needed. `snapLayerToMapcomp(...)` (1.20.0) [S: CL]. Pinned layers: separate Latitude, Longitude, Height; "(pinned)" suffix; rotate-with-map option; "Position Progress" slider for animated positions [S: CL 1.1.0, 1.4.0, 1.2.6, 1.1.2]. "Square/portrait maps can be pitched more" [S: CL 1.0.4]. "Strange keyframe interpolation on Zoom leads to expression errors" [S: CL 1.2.4]. "expression errors on large zoom" [S: CL 1.1.8].

### 5.2 Formulas (MapLibre-compatible) [S constants: https://cdn.jsdelivr.net/npm/maplibre-gl@5.23.0/src/geo/transform_helper.ts]
- `tileSize = 512` (MapLibre); `worldSize = tileSize * 2^zoom`; default `fov = 0.6435011087932844 rad` (36.87°); `cameraToCenterDistance = 0.5 / tan(fov/2) * height` = **1.5 × viewport height**; bearing wrapped to [-180,180]; pitch clamped [minPitch=0, maxPitch=60 default] [S].
- Raster XYZ convention uses 256-px tiles → zoom_raster = zoom_maplibre + 1 for the same scale [I]. Must pick one convention for the Zoom control (GEOlayers' convention UNKNOWN).
- Mercator (normalized 0..1):
  - `mx = (lon + 180) / 360`
  - `my = 0.5 - ln(tan(π/4 + φ/2)) / (2π)` = `0.5 - ln((1+sinφ)/(1-sinφ)) / (4π)`; clamp φ to ±85.051129° [I standard].
  - Inverse: `lon = mx*360 - 180`, `lat = (2*atan(exp(π*(1 - 2*my))) - π/2) * 180/π` → for `getLayersGeoPosition` [I].
- Screen position of a geo point in the Mapcomp's (unpitched) plane, comp centered at C=[W/2,H/2]:
  - `d = ([mx,my] - [mx0,my0]) * worldSize` (do the subtraction in normalized space **before** multiplying → keeps numbers small, avoids "large zoom" errors) [I]
  - wrap: `if (d.x > worldSize/2) d.x -= worldSize; if (d.x < -worldSize/2) d.x += worldSize` (antimeridian) [I]
  - rotate by −bearing: `p = C + [d.x*cosθ - d.y*sinθ, d.x*sinθ + d.y*cosθ]`, `θ = -bearing*π/180` [I – sign to verify against MapLibre]
- AE expression (works in both engines) for a pinned 2D layer's Position, in the **containing comp** [I]:
```js
var ctl = comp("Mapcomp 01").layer("Map Controls");   // name/location of controls UNKNOWN in GEOlayers
var lat0 = ctl.effect("Latitude")(1), lon0 = ctl.effect("Longitude")(1);
var z = ctl.effect("Zoom")(1), b = ctl.effect("Bearing")(1);
var lat = effect("Latitude")(1), lon = effect("Longitude")(1);
function mY(l){ l = Math.max(-85.051129, Math.min(85.051129, l)) * Math.PI/180;
  return 0.5 - Math.log(Math.tan(Math.PI/4 + l/2)) / (2*Math.PI); }
var ws = 512 * Math.pow(2, z);
var dx = ((lon - lon0) / 360), dy = (mY(lat) - mY(lat0));
if (dx > 0.5) { dx -= 1; } else if (dx < -0.5) { dx += 1; }
dx *= ws; dy *= ws;
var t = -b * Math.PI / 180;
var mc = thisComp.layer("Mapcomp 01");              // Mapcomp layer in containing comp
var c = mc.toComp(mc.anchorPoint);
[c[0] + dx*Math.cos(t) - dy*Math.sin(t), c[1] + dx*Math.sin(t) + dy*Math.cos(t)];
```
  Scale for "scale with map": `s = 100 * Math.pow(2, z - zPinned)`; Rotation for "rotate with map": `-b` (+ original rotation) [I].
- Performance: expressions referencing controls in another comp are re-evaluated per layer per frame; for many labels prefer **parenting to the anchor/pivot null** whose transform encodes the view (then each child only needs a static offset at a reference zoom) [I]:
  - Anchor null position = screen position of a fixed reference geo point (e.g. the Mapcomp center at creation, lat_ref/lon_ref); Scale = `100*2^(z - z_ref)`; Rotation (Z) = `-bearing`. Children placed at `(mercator(child) - mercator(ref)) * 512*2^z_ref` local px. The child's world position = anchor transform ∘ local offset → follows lat/lon/zoom/bearing exactly, because Mercator is affine in (x,y) for a given zoom/bearing [I]. Precision: choose z_ref near the animated zoom range; re-base when zoom changes by >~8 levels to keep scale values in a sane range [I].
  - Anchor null rotation/scale distort child layer size; for labels that must *not* scale/rotate, counter-transform in the child (`scale = 100/ (parentScale/100)` etc.) or use non-parented `toComp()` expressions — matches GEOlayers' "scale with map / rotate with map" label options [S options; I mechanism].
- **3D pitch** (MapLibre-equivalent camera) [I]:
  - In the containing comp, add a camera "Map Camera" (one-node), Zoom = `1.5 * thisComp.height` (equals MapLibre's cameraToCenterDistance, so a z=0 layer appears at 100%), Position = `[W/2, H/2, -1.5*H]`, Point of Interest = `[W/2, H/2, 0]`.
  - "MapPivot": a 3D null at comp center (z=0); `X Rotation = pitch` (sign to verify: MapLibre tilts the far/top edge away from the viewer) and `Z Rotation = -bearing`; the 3D Mapcomp layer and the anchor null are parented to the pivot, so pitch is a rotation of the map plane about the screen center — mathematically the same as MapLibre orbiting the camera around the center point with fixed distance [I].
  - Because pitch shows the far area, the imagery must cover more than the comp frame (top edge extends toward horizon) → imagery comp taller than the containing comp, and max pitch has to be limited for wide formats — consistent with GEOlayers "Square/portrait maps can be pitched more" [S fact, I explanation]. Alternatively render pitched frames directly from MapLibre (strategy A) and only use the 3D rig for pinned layers [I].
  - Pinned layer "Height" (meters) → z offset: `pixelsPerMeter = worldSize / (40075016.686 * cos(lat))`; z = `-height * pixelsPerMeter` (toward camera) [I; MapLibre uses the same pixels-per-meter at latitude idea].
- Zoom animation: interpolating Zoom linearly gives constant *perceived* zoom speed (exponential scale) — good. Linear Lat/Lon during big zoom changes looks wrong; use MapLibre `flyTo`'s van Wijk–Nuij path (`curve` ρ=1.42) when generating keyframes for "Animate View to Feature" (GEOlayers had keyframe easing bugfixes there [S: CL 1.2.5]) [I]. Clamp Zoom in expressions to avoid bezier overshoot below min zoom ("Strange keyframe interpolation on Zoom" [S: CL 1.2.4]).
- Expression engine compatibility: avoid `const/let`, arrows, template strings, `Array.prototype.includes`; use `thisComp`, `thisLayer`; all `if` with braces and `else` [S rules: helpx page; I list].

---

## 6. Testing on Linux without After Effects

AE does not run on Linux; there is no ExtendScript engine for Linux (ESTK/VS Code ExtendScript Debugger attach to a running host app on Win/Mac) [I]. Strategy:
1. **Panel UI in a normal browser**: Bolt CEP dev server serves the panel at `http://localhost:3000/panel/` [S: bolt-cep]. Guard host calls: `const inCEP = !!(window.__adobe_cep__ || window.cep_node)`; in browser, inject a **mock CSInterface** (`evalScript(code, cb)` routes to a JS mock host implemented in TS; `addEventListener/dispatchEvent` via an EventTarget; `getSystemPath` → fake dirs; Node modules unavailable → wrap `fs` behind an interface, use in-memory or IndexedDB impl in browser) [I]. Bolt advises loading Node modules inside functions so browser preview doesn't break [S: bolt-cep].
2. **MapLibre rendering on Linux**: runs in desktop Chrome/Chromium exactly as in CEP (Chromium 99 vs current Chrome — test with an old Chromium 99 build, e.g. Puppeteer/Playwright with a pinned Chromium ~r961656 / Chrome 99, to catch syntax/API gaps) [I]. Headless: Playwright Chromium with `--use-gl=swiftshader`/`--enable-unsafe-swiftshader` for WebGL in CI [I]. Snapshot tests of rendered PNGs (pixelmatch).
3. **ExtendScript host code**: write in TypeScript with `types-for-adobe` typings [S], compile to ES3 (Bolt's Vite ExtendScript build, or `tsc --target ES3` + Babel `@babel/preset-env` targets "ie 5"/es3 transforms) [I]; lint output with ESLint `ecmaVersion: 3` to catch ES5+ syntax [I]. Unit-test host modules in **Node** by running the ES3 bundle inside `vm.createContext({app: mockApp, File, Folder, $, CompItem, ShapeLayer, Shape, MarkerValue, ImportOptions, KeyframeInterpolationType, BlendingMode, …})` with hand-written mocks of the AE DOM (CompItem, LayerCollection, PropertyGroup.addProperty(matchName) registry, Property.setValueAtTime storing keys, Shape value objects). Validate match-name usage against a fixture of real property trees later dumped on a Win/Mac machine [I]. No maintained full AE DOM mock library was found [I – search found only typings].
4. **Expressions**: keep the projection math in one plain-ES3 JS module; unit-test it in Node against MapLibre's own `map.project()` (run MapLibre in Playwright, compare screen points for random lat/lon/zoom/bearing/pitch) [I]; generate expression strings from the same source (`fn.toString()` injection) and test them in Node `vm` with mocks for `comp()`, `thisComp`, `thisLayer`, `effect()`, `toComp()`, `time`, `value` [I].
5. **Bridge contract tests**: JSON schemas for every `evalTS` call / CSXSEvent payload; the same tests run against the mock host and (later) real AE [I].
6. **Final verification** must happen on Win/Mac AE (user will test later): enable PlayerDebugMode (CSXS.12/13), copy built folder to the CEP extensions dir, attach Chrome DevTools via `.debug` port, check `Window > Extensions > …` [S for debug steps: cookbook].

---

## 7. Open technical questions (UNKNOWN — verify in AE)
- Whether GEOlayers' map controls are a pseudo effect or separate expression controls, and on which layer they live.
- GEOlayers Zoom convention (256 vs 512 tile size) and bearing sign convention.
- Per-frame image sequence vs. tile-layer placement in Finalize.
- Whether `CSXSEvent` dispatch from ExtendScript is reliable in AE 25–27 (cookbook doesn't list AE).
- CEP version in AE 27.x; and the 2026 "Unsupported Manifest version ''" registration bug (AE 25.6.5/26.2.1/26.3 on macOS 26).
- WebGL2 / MapLibre v5 behaviour inside AE's CEF 99 (GPU blacklist on some machines; dual GPU null context).
- Exact AE match names for extrusion & material options; Advanced 3D renderer match name.
- Whether an AE effect plugin (C++ AE SDK) is behind "Globe and Equal Earth Map Projection via Plugin" (CL 1.20.0 prerelease) — building such a plugin needs the AE C++ SDK and Win/Mac toolchains; cannot be built/tested on Linux.
