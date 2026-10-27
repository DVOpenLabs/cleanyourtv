# Integrated component provenance

The fork retains the root MIT licence and existing font notices. Additional source adaptations:

| Component | Origin and reviewed revision | Reuse |
|---|---|---|
| `LiveVideo.java` | [csermet/cleanyourtv](https://github.com/csermet/cleanyourtv/tree/18b03b2145c51c35fb255b1c6863ed609746ece6), `18b03b2145c51c35fb255b1c6863ed609746ece6` | Local TextureView/MediaPlayer lifecycle and still-fallback design; reduced to silent local packs, with additional callback guards |
| `ThemeArchive.java` | [DipsySu/cleanyourtv](https://github.com/DipsySu/cleanyourtv/tree/1c05b6b50cd8e5a1f9cff9f8914b1439f02a66f4), `1c05b6b50cd8e5a1f9cff9f8914b1439f02a66f4` | Adapted strict JSON parser and manifest concept; visual-only format, bounded staging/import and library are product integration work |
| `GlassView.java` double frame | [OrenDdp/cleanyourtv-header-grid](https://github.com/OrenDdp/cleanyourtv-header-grid/tree/703609369f2e5955e282b6cf687691e82801b7bf), `703609369f2e5955e282b6cf687691e82801b7bf` | Adapted black/white dual-frame drawing; no extra glow, fonts or layout renderer |
| `HdmiVisibility.java` | [midhatn/cleanyourtv](https://github.com/midhatn/cleanyourtv/tree/eeda31c7029737286e74016e7b24a5882ea4deef), `eeda31c7029737286e74016e7b24a5882ea4deef` | Bare-port state filter; product adds explicit settings and a one-visit Show all escape |
| `TvButtonSettings.java`, `TvButtonPress.java` | [lionlev21/cleanyourtv](https://github.com/lionlev21/cleanyourtv/tree/e412c5b2b4e0819598f5da96060c0d1bf8522526), `e412c5b2b4e0819598f5da96060c0d1bf8522526` | Adapted picker/press-gate concepts and app-query/sorting code; restricted native dialog, shared per-device deduplication, no new watchers/services or auto-grants |
| webOS recovery patch/source preparation | [Razdva122/clean-your-tv-lg-webos](https://github.com/Razdva122/clean-your-tv-lg-webos/tree/d5804f6a7ce5d4eb35c0093ca19ae011e5a6df7f), `d5804f6a7ce5d4eb35c0093ca19ae011e5a6df7f` | Small filter/relay exit-contract fix and tests; selected pinned source is extracted locally rather than bundling upstream executables |

The listed Android source snapshots carry the Gathin MIT notice, reproduced in
[Android-integrations-MIT.txt](licenses/Android-integrations-MIT.txt). The webOS source carries Gathin
and OmkarK97 MIT notices, retained in [LICENSE.upstream](../platforms/webos/LICENSE.upstream).
Source preparation also copies its original `THIRD_PARTY_NOTICES.md`; further integration must retain
applicable transitive licences. No additional fonts, themed artwork, Android libraries, webOS IPKs,
desktop Qt packages or Node dependencies were imported by this batch.

The new native carousel does not copy an alternate launcher's renderer; it extends the existing native
adapter. SHOWCASE projects used only as design/research references are listed in the source register
and lightweight review, not represented as bundled components. Preserve this attribution and the licence
files when redistributing adapted source or builds.

## Alpha 5 design references

The desktop editor/provider, local app search and audio/settings controllers are independent code.
They use [Panel](https://github.com/Pagamase/cleanyourtv-panel/tree/8c52c884314cd85fba21a47e60b6e055d7c4e689),
the [Bluetooth fork](https://github.com/nikhiltalreja/cleanyourtv/tree/e374b929576ea052011feaa5064194ad92ac2a07),
[XMB Home](https://github.com/amachador/xmb-home/tree/2acc01861e95004a9ca55de92c19c07daa5c362b) and
[Home Deck's Jump To guide](https://github.com/glidiandev/home-deck/tree/6ae44996b2970b1a49e49c0b1e505f17bb458bda)
as design references. Their MIT licences were reviewed. No panel installer/theme-push code, Bluetooth
scanner, NowPlaying metadata/browser code, WebView shell, fonts or fan artwork was copied from these
projects. The controls extend the native launcher and QuickSettingsActivity already in this fork.

Readable Home and the Chromecast glass library were reviewed for readability/library design, without
copying their restyles, fonts or source changes. The SamuelSHernandez webOS edition is a planning
candidate only: its root licence and guides were reviewed, while a full webOS/transitive licence audit
remains required before import. See [the source register](sources.json) and [LG plan](LG-NEXT.md).
