# Projectivy review and small native improvements

Reviewed 2026-10-27. This is an independent implementation, not a port of Projectivy code or assets.

## What was actually inspected

REA 6.8.0 with the checksum-verified, portable `jadx-headless-mcp` 0.7.1 provider inspected the official Projectivy 4.72 APK (version code 97, SHA-256 `a312935837b7cf323880455c322f4c98060e3e37e8c6187963fb2c4764534aae`). The Android MCP connection was repaired by restarting the client, but its provider environment was absent. Analysis therefore ran through REA's CLI with process-local Java/provider settings; no global client configuration changed.

Package evidence `ev_2056a050716c56df47627c9ad940594f7bafeb8261dd989a59660d00e27aabc4` identifies 7,395 classes and 1,171 decoded resources. Class inventories and selected methods were then inspected, rather than treating class names as proof of behavior:

- `CategoryPreferencesFragment.b0`: static branches for app-title visibility, multiple lines and a sliding single row. Evidence `ev_6edd13c3a8621e3efab177c6351c9d0598b93c34c9f7e1b412c26bc6fe33e662`.
- `CustomListPreference.o` and its shared preference helper: native preference binding and shared icon styling. Evidence `ev_892efdac1db89ff7de519d24e83809c79a537dd829351e63241624b2daada508`, `ev_ca6cdbc9b067d9f428f0f699817ddd9f43a4ef32339a7766def9da893189720a`.
- Settings activity/base-class inventories, initialization and incoming category-method references were retained privately. One constructor required smali fallback; it was not represented as recovered Java.

Separately, SDK resource inspection found appearance, font, background video, preview and restore-backup declarations. Resource strings establish available labels, not successful execution. The [official feature guide](https://github.com/spocky/miproja1) and [4.72 release notes](https://github.com/spocky/miproja1/releases/tag/4.72) provide published context.

These are static observations. The APK was not installed or executed for this review. Reflection, actual remote interactions, premium availability and every Projectivy settings screen remain outside the verified coverage. Raw decompiled bodies and original assets are not included in this repository.

## Selection

| Pattern | Decision for this fork |
| --- | --- |
| App-title visibility | Add **App names**: Follow theme, Show all, Selected app only, Hide names. Reserve the existing label space, so icons do not jump or resize. Accessibility names remain on app cards. |
| Readable appearance controls | Extend **Home text** with Original theme, Automatic contrast, Light text and Dark text. Automatic contrast follows the configured theme background, not changing video frames. This manual ink override is our own extension. |
| Guided native settings | Keep the existing menu/detail structure; show focused-control help and the current choice in the context pane. Remember each section's focused row while the settings activity is open, including normal saved-instance recreation. This interaction is our design, informed by the native preference approach. |
| Single/sliding rows and local backgrounds | Existing carousel, three positions, silent local video and safe position previews already cover the useful lightweight portion. Preserve them. |
| Backup/restore | Existing profile-scoped saved setups already cover manual restore. Automatic history needs bounded storage and restore safeguards before implementation. |
| Program/channel previews, online wallpaper providers | Defer: extra content integrations, permissions, caches and playback compete with the small core's purpose. |
| Broad accessibility remapping, system cleanup and device overrides | Do not import. Preserve the existing narrow Home/input helpers and TV settings. |

Both new appearance choices are profile-scoped and included in saved setups. Choosing Follow theme removes the app-name override. Choosing Original theme restores the original text renderer. Upgrades with no new choices keep their prior rendering. Canceling a choice or browsing a section writes nothing.

## Size and validation

The 589,824-byte APK gate is unchanged. The existing SDK resource optimization also deduplicates identical resource values. Resource IDs, named view IDs, runtime banner names, fonts, languages and density variants remain available; no sparse lookup encoding is enabled. Resource paths and extensions are preserved: an attempted path-shortening optimization failed native Android 12 color-state-list inflation and was removed before final acceptance.

Unit coverage checks ink overrides, original/selected/hidden name opacity, invalid saved values and profile isolation. Device validation must additionally check all label modes, solid text colors, focus-help updates, section re-entry, Cancel, all eight selection outlines, live wallpaper and preservation of owner layouts. Unit tests cannot establish sofa readability or real remote-button behavior.
