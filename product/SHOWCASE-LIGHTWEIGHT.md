# SHOWCASE: useful features without a heavy installation

Reviewed 26 October 2026 using the current [SHOWCASE index](https://github.com/Gathin23/cleanyourtv/blob/main/SHOWCASE.md)
and the primary guides/source linked below. This is selective triage, not an audit of every project.
The index itself says its projects are independently maintained and not reviewed by upstream.

Keep one native launcher, one app model and the existing two Android runtime dependencies. Port small
components with licence/source review; avoid nesting whole launchers or adding an engine for a layout.
Measure the release APK after every integration. Optional images/video remain external to the APK;
their storage and decoder cost must be reported separately from installation size.

| Candidate | User benefit | Likely cost / boundary | Recommendation |
|---|---|---|---|
| [Header Grid](https://github.com/OrenDdp/cleanyourtv-header-grid/blob/703609369f2e5955e282b6cf687691e82801b7bf/FORK.md): double focus frame | Selection stays legible on both bright and dark app banners | Small native drawing change; no intrinsic need for networking, media or a service | Frame source/licence reviewed and adapted in alpha 3; actual banner contrast remains to be tested |
| Header Grid: exact gaps and whole-row navigation | Easier reading at a distance; predictable D-pad movement | Native geometry/settings, without a separate launcher | Alpha 3 adds independent extra gutters; whole-row navigation remains deferred |
| [HDMI input visibility](https://github.com/midhatn/cleanyourtv/tree/feature/hdmi-input-visibility) | Hide disconnected ports from the Inputs menu | Source filters the existing TvInputManager state through `HdmiVisibility`; firmware can misreport state | Adapted in alpha 3 with Show all fallback; CEC/unknown rows retained; firmware acceptance pending |
| [Remote Picker](https://github.com/lionlev21/cleanyourtv/blob/tv-button-picker/REMOTE_PICKER.md) | Choose what the TV button opens directly on the TV | Guide reuses existing key helpers; no new service/network permission, but accessibility is still needed for cross-app key handling | Restricted TV-key picker adapted in alpha 3; no auto-grants/helper activation; physical acceptance pending |
| [XMB Home's simple Now Playing](https://github.com/amachador/xmb-home/blob/main/README.md) | Small indication that audio is still playing after returning Home | Guide explicitly has no notification access: no song titles, and app association is a guess based on the last opened app | Consider a generic audio-active indicator; do not label guessed app attribution as fact |
| [Bluetooth pill](https://github.com/nikhiltalreja/cleanyourtv/blob/bluetooth-pill-key-remaps/README.md) | Quicker earbud pairing | Full scan/pair implementation requires BLUETOOTH_CONNECT / SCAN grants on applicable Android versions | Prefer a shortcut to the TV's own Bluetooth settings first; full scanner is optional later |

The cost column is an engineering assessment from source/guide behavior, **not measured per-feature
kilobytes or runtime benchmarks**. Licence and component-source audit remain required for candidates not
already adapted. Platform permissions and firmware behavior need separate Android/webOS designs.

## Features integrated in this batch

- Bottom carousel: built using the existing native RecyclerView/adapter and preview/Undo path. No new engine.
- Offline theme packs: bounded visual-only ZIP import, inspired by [DipsySu](https://github.com/DipsySu/cleanyourtv/tree/1c05b6b50cd8e5a1f9cff9f8914b1439f02a66f4).
- Local silent video: a reduced adaptation of [csermet's player](https://github.com/csermet/cleanyourtv/tree/18b03b2145c51c35fb255b1c6863ed609746ece6), with lifecycle guards and a still fallback.
- LG recovery patch: separate source layer, absent from the Android APK; no helper installation.

See [appearance format/limits](ANDROID-APPEARANCE.md), [licences](THIRD-PARTY.md) and
[measured validation](VALIDATION.md). Local profiles were integrated in the previous batch.

## Keep out of the base launcher

Weather/alerts can remain optional module-card apps rather than giving Home Internet access. AI setup
suggestions can run on a desktop/phone and produce a reviewable config with Undo; a model SDK, paid
service or always-listening assistant is unnecessary for app launching. Rich recommendations, poster
feeds, voice agents, notification metadata and full animated engines need a clear provider/permission
and runtime budget first. Do not merge proprietary Projectivy code or assume a SHOWCASE listing grants
rights to redistribute its assets.

Alpha 3 implements **focus contrast + extra spacing**, followed by the optional input filter and TV-button
picker. These source integrations remain off by default and need native/physical acceptance; no TV was
changed. See [the usability contract](ANDROID-USABILITY.md). Whole-row navigation, the audio indicator
and Bluetooth changes remain candidates rather than bundled features.

## Follow-up review — 27 October 2026

The current upstream SHOWCASE lists 137 projects. That does **not** mean every useful feature has been
integrated or that every repository has had a source/security/licence audit. The following are still
worth evaluating. This section is a shortlist, not authorization to install them or change a TV.

| Priority | Candidate | Useful adaptation for this fork | Keep it light |
|---|---|---|---|
| 1 | [CleanYourTV Panel](https://github.com/Pagamase/cleanyourtv-panel/blob/main/README.md) | Arrange apps and edit names from a computer, with a preview and per-device backup | The panel documents Python-standard-library-only operation and no extra TV app. Its existing theme push clears some manual launcher choices, so a preservation-aware apply/Undo path must be designed before reuse. |
| 2 | [Bluetooth pill](https://github.com/nikhiltalreja/cleanyourtv/tree/bluetooth-pill-key-remaps) | A quick route to the TV's existing Bluetooth screen | Start with a settings shortcut. Pairing scans, battery reporting and their permissions need separate review. |
| 3 | [XMB Home](https://github.com/amachador/xmb-home/blob/main/README.md) | A small indication that audio is still active | A generic indicator is useful without song metadata or notification access. Never infer a track title or source app from the last app launched. |
| 4 | [Cosy Home](https://github.com/oliverjwroberts/cosy-home-tv) | Subtle pixel shifting or dimming of static launcher elements when idle | Optional, bounded and separate from the TV's own panel-care settings. Hardware acceptance is required; this cannot guarantee prevention of burn-in. |
| Optional | [CleanYourTV UI](https://github.com/matheus-caldeira/cleanyourtv-ui/blob/main/README.md) | Movie search and a title page with supported service links | Its TMDB/Wikidata catalogue is not AI and is substantially broader than an app launcher. Keep any adaptation in an optional companion; account for API terms, keys, image-cache limits and incomplete title links. Do not promise universal watch progress across streaming apps. |

### AI and voice

The most useful proposed AI feature is a **plain-language setup assistant on the computer or phone**:
for example, “make a kids profile with these four apps, larger labels and this wallpaper.” It would
produce a schema-validated proposal, show the exact changes and target device/profile, and require an
explicit Apply with backup and Undo. The TV needs only the resulting configuration, not a model SDK.
This is a proposal, **not implemented AI functionality**. Provider, data sent and any cost still need
to be selected and disclosed; no paid service is being invoked by this review.

[Chill TV](https://github.com/AlimangoStudio/chill-tv/blob/main/README.md) documents named voice commands
and a computer remote. Its Windows remote uses Windows' offline English speech recognizer; the Mac
uses Apple's recognizer, on-device where supported, and only commands go to the TV. These documented
fixed commands should not be presented as proof of a general-purpose LLM assistant. A reusable version
would be opt-in, show when the microphone is listening and retain ordinary remote navigation.

Smart movie suggestions such as “a short anime available on my services” are a possible later companion
feature, after reliable catalogue/availability data exists. Generated answers must not invent streaming
availability or imply they can inspect Netflix/STB EMU profiles. No AI, catalogue or remote-server
dependency has been added to the base launcher in this review.

The recent weather-header and appearance changes on the owner's devices are isolated prototypes.
Their public configuration/provider integration remains work in progress; they are not additional
finished SHOWCASE imports. The LG helper also still needs full packaging and actual LG-device testing.
