# SHOWCASE review: choose useful pieces, keep Home quiet

Reviewed 27 October 2026. The upstream [SHOWCASE.md](https://github.com/Gathin23/cleanyourtv/blob/main/SHOWCASE.md)
snapshot was blob `9bcf74c6d12f7765d612ad3e68051c4319cc1d65`, listing 137 projects. All project
descriptions were considered by feature family. Component-source review is selective, recorded below
and in [sources.json](sources.json); this is **not** a security, licence or hardware audit of all 137
repositories. Listing on SHOWCASE is not upstream endorsement.

The following priorities are our engineering judgment, not popularity scores. A feature earns its
place by removing remote presses or making Home clearer; it should not turn Home into another feed.

| Feature family / examples | Decision and reason | Installation/runtime boundary |
|---|---|---|
| Local profiles — LiquiTvOs | Existing independent profile integration; retain separate favorites, appearance and order | No Google/streaming-account access; native webOS adapter still needed |
| Clear focus and sofa readability — Header Grid, Readable Home | Keep the integrated dual outline/gaps; retain the owner's chosen typography and app sizes | No font pack, glow engine or wholesale restyle |
| Layouts — Home Styles, Zen list, Apple TV 8-Dock | Existing native grid/carousel and timed preview cover the immediate need | Aurora/Cinema/Orbit renderer ports deferred; do not embed competing launchers |
| Silent local backgrounds — csermet, local-video forks | Existing local loop/still fallback retained | External media storage and decoder cost are separate from APK size; release player behind streaming apps |
| Offline theme library — DipsySu | Existing restricted pack importer retained | No bundled fan artwork/fonts; preserve app order and manual choices |
| Computer app arrangement — CleanYourTV Panel/Studio | **Alpha 5:** independent app-only editor with exact preview, backup and guarded Undo | Python standard library on PC; no TV web server or theme-push reset |
| Quick controls — Home Deck Jump To, Control Center | **Alpha 5:** reuse native QuickSettingsActivity and add direct entry points | No overlays watching other apps; no automatic picture/sound changes |
| Installed-app search — library/remote projects | **Alpha 5:** independent offline name/package filter in existing library | No catalogue API, web search or index service; loaded on demand |
| Bluetooth pill | **Alpha 5:** native Settings shortcut with an honest fallback | Do not import scan/pair/battery code or extra grants |
| Audio status — XMB Home, Home Deck | **Alpha 5:** optional generic music-active label | No notification access, track metadata, guessed source app or background polling |
| HDMI/CEC — visibility fork, Quay, Home Deck | Keep integrated bare-port filter and Show all escape | Firmware-reported states only; no universal cable/signal claims or DIAL network control |
| Remote buttons — Remote Picker, Chromecast fork | Keep restricted native picker and per-device press gate | Physical buttons need testing; do not activate broad accessibility filters automatically |
| Weather — Weather Clock, provider-card forks, Ambient Theme | Retain optional add-on/provider direction; current dual-unit header is a device trial | Public header integration remains separate work; no launcher Internet permission |
| OLED idle care — Cosy Home, ExtremeSlimTV | Later: bounded launcher-only dim/shift with explicit opt-in | Never alter the TV's pixel cleaning, panel care, standby or power settings; cannot guarantee burn-in prevention |
| Movies/Explore — CleanYourTV UI, Forslag, kermes-tv | Later optional companion if title/service discovery is wanted | Provider terms, keys, cache/network budgets and reliable links first; no invented availability |
| Continue watching / app channels — PeachOS, TCL GTV, Vibe | Investigate actual provider data before selecting | Do not promise universal Netflix/STB progress or private-account access |
| Recent/frequent apps — Home Deck, Google TV-style forks | Later optional app history; keep pinned favorites stable now | Local launcher launches only; no UsageStats grant or silent reordering |
| Phone remotes and upload panels — Webmote, Casa, TV Remote | Desktop editor first; phone control remains separate | TV-side server, authentication and key/text access need their own review |
| Voice and AI — Chill TV, setup tools | Draft import implemented; AI setup proposals can be reviewed through it | Fixed voice commands are not an LLM; no microphone, model, paid API or cloud dependency added |
| RAM cleaners / task managers / forced stopping | Excluded from this batch | Do not disrupt streaming to make an unmeasured speed claim |
| Sensors, stock/prayer/sports/news widgets | Optional add-ons for people who request them | No permanent dashboard or extra polling on a simple app launcher |
| Root, Fire OS, older Android and Samsung-specific takeover | Not applicable to these two Android targets | Do not transfer firmware assumptions, installer/debloat scripts or Home hooks |
| LG webOS ports | Separate platform plan, after Android validation | Developer Mode web app first; helper recovery and model/runtime evidence are separate gates |

## Selected source evidence

- [Panel](https://github.com/Pagamase/cleanyourtv-panel/tree/8c52c884314cd85fba21a47e60b6e055d7c4e689):
  reviewed its panel implementation and MIT licence. Its theme-push route can reset manual choices;
  alpha 5 instead uses an independently implemented app-only provider transaction.
- [Bluetooth fork](https://github.com/nikhiltalreja/cleanyourtv/tree/e374b929576ea052011feaa5064194ad92ac2a07):
  reviewed the feature code and MIT licence; selected the native-settings idea, excluding inline
  scanner/paired-device operations and their permissions.
- [XMB Home](https://github.com/amachador/xmb-home/tree/2acc01861e95004a9ca55de92c19c07daa5c362b):
  reviewed its README, NowPlaying implementation and MIT licence. Its fuller source includes metadata/
  app-association paths; none were imported. Our indicator uses only the generic Android music signal.
- [Home Deck](https://github.com/glidiandev/home-deck/tree/6ae44996b2970b1a49e49c0b1e505f17bb458bda):
  README/licence design reference for Jump To; our implementation extends our pre-existing native
  panel. No WebView shell, tile art, fonts or notification listener copied.
- [Readable Home](https://github.com/itismowgli/cleanyourtv/tree/584f7bfa80ba190092723f4a3690da622dd8eb6f):
  detailed readability/performance guide reviewed as reported evidence. Its device measurements do
  not measure our fork; no font or restyle imported.
- [Chromecast glass fork](https://github.com/NewPlayer0213/cleanyourtv/tree/ea2df03e7f109d0310dbe2a21dd0bb4d12abe545):
  guide and AppLibraryDialog reviewed. Full banner-library/glass/key-service changes deferred;
  our small local search filter is independent code.

The alpha-5 package, tests and device limits are recorded in [VALIDATION.md](VALIDATION.md). Earlier
integrated video/parser/frame/input/remote components keep their pinned notices in
[THIRD-PARTY.md](THIRD-PARTY.md). Research references are not bundled integrations.

## Next choices

The owner's follow-up asked for settings that are easier to use like Projectivy. Alpha 5 therefore
also groups the existing settings into five native categories, displays current layout values,
supports the existing eight-column layout in its chooser and confirms before restoring defaults.
This is independent UI work, not a SHOWCASE code import or a port of Projectivy. Browsing categories
was verified not to change the native snapshot/revision on either test device.

Finish native/physical acceptance before adding another visual renderer. Then prioritize the supported
LG web-app shell and shared profile/theme adapter. A movie-discovery companion and a plain-language
setup assistant are useful candidates only after choosing data/provider/cost boundaries. A large
home-screen catalogue, GPU effect engine or always-listening assistant is not required for this product.
