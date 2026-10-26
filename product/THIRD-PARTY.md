# Integrated component provenance

The fork retains the root MIT licence and existing font notices. Additional source adaptations:

| Component | Origin and reviewed revision | Reuse |
|---|---|---|
| `LiveVideo.java` | [csermet/cleanyourtv](https://github.com/csermet/cleanyourtv/tree/18b03b2145c51c35fb255b1c6863ed609746ece6), `18b03b2145c51c35fb255b1c6863ed609746ece6` | Local TextureView/MediaPlayer lifecycle and still-fallback design; reduced to silent local packs, with additional callback guards |
| `ThemeArchive.java` | [DipsySu/cleanyourtv](https://github.com/DipsySu/cleanyourtv/tree/1c05b6b50cd8e5a1f9cff9f8914b1439f02a66f4), `1c05b6b50cd8e5a1f9cff9f8914b1439f02a66f4` | Adapted strict JSON parser and manifest concept; visual-only format, bounded staging/import and library are product integration work |
| webOS recovery patch/source preparation | [Razdva122/clean-your-tv-lg-webos](https://github.com/Razdva122/clean-your-tv-lg-webos/tree/d5804f6a7ce5d4eb35c0093ca19ae011e5a6df7f), `d5804f6a7ce5d4eb35c0093ca19ae011e5a6df7f` | Small filter/relay exit-contract fix and tests; selected pinned source is extracted locally rather than bundling upstream executables |

Both Android source snapshots carry the Gathin MIT notice, reproduced in
[Android-integrations-MIT.txt](licenses/Android-integrations-MIT.txt). The webOS source carries Gathin
and OmkarK97 MIT notices, retained in [LICENSE.upstream](../platforms/webos/LICENSE.upstream).
Source preparation also copies its original `THIRD_PARTY_NOTICES.md`; further integration must retain
applicable transitive licences. No additional fonts, themed artwork, Android libraries, webOS IPKs,
desktop Qt packages or Node dependencies were imported by this batch.

The new native carousel does not copy an alternate launcher's renderer; it extends the existing native
adapter. SHOWCASE projects used only as design/research references are listed in the source register
and lightweight review, not represented as bundled components. Preserve this attribution and the licence
files when redistributing adapted source or builds.
