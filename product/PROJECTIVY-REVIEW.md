# Projectivy review and native feature integration

Reviewed 2026-10-27. This is an independent implementation, not a port of Projectivy code or assets.

## What was actually inspected

REA 6.8.0 with the checksum-verified, portable `jadx-headless-mcp` 0.7.1 provider inspected the official Projectivy 4.72 APK (version code 97, SHA-256 `a312935837b7cf323880455c322f4c98060e3e37e8c6187963fb2c4764534aae`). The Android MCP connection was repaired by restarting the client, but its provider environment was absent. Analysis therefore ran through REA's CLI with process-local Java/provider settings; no global client configuration changed.

Package evidence `ev_2056a050716c56df47627c9ad940594f7bafeb8261dd989a59660d00e27aabc4` identifies 7,395 classes and 1,171 decoded resources. Class inventories and selected methods were then inspected, rather than treating class names as proof of behavior:

- `CategoryPreferencesFragment.b0`: static branches for app-title visibility, multiple lines and a sliding single row. Evidence `ev_6edd13c3a8621e3efab177c6351c9d0598b93c34c9f7e1b412c26bc6fe33e662`.
- `CustomListPreference.o` and its shared preference helper: native preference binding and shared icon styling. Evidence `ev_892efdac1db89ff7de519d24e83809c79a537dd829351e63241624b2daada508`, `ev_ca6cdbc9b067d9f428f0f699817ddd9f43a4ef32339a7766def9da893189720a`.
- Settings activity/base-class inventories, initialization and incoming category-method references were retained privately. One constructor required smali fallback; it was not represented as recovered Java.
- Settings base `vu.L` wires the decorated preference view and nested Back dispatcher (evidence `ev_7d7b81926beca9a56877a2086f1f1707d089ba4e6ed8a66f052aea37feeeb45d`). `vu.Z` selects a keyed preference screen or the root (evidence `ev_05afc295a834aa41f1dc24025b5eb4fcdff3a09a774d07022107fa2f4de34763`); malformed decompiler locals limit exact control-flow confidence. Twenty-one complete Evidence results are retained privately.

Separately, SDK resource inspection found appearance, font, background video, preview and restore-backup declarations. Resource strings establish available labels, not successful execution. The [official feature guide](https://github.com/spocky/miproja1) and [4.72 release notes](https://github.com/spocky/miproja1/releases/tag/4.72) provide published context.

These are static observations. The APK was not installed or executed for this review. Reflection, actual remote interactions, premium availability and every Projectivy settings screen remain outside the verified coverage. Raw decompiled bodies and original assets are not included in this repository.

## Selection

| Pattern | Decision for this fork |
| --- | --- |
| App-title visibility | Add **App names**: Follow theme, Show all, Selected app only, Hide names. Reserve the existing label space, so icons do not jump or resize. Accessibility names remain on app cards. |
| Readable appearance controls | Extend **Home text** with Original theme, Automatic contrast, Light text and Dark text. Automatic contrast follows the configured theme background, not changing video frames. This manual ink override is our own extension. |
| Guided native settings | Full community build adds seven short root sections and nested Appearance pages, current values, focused-row help, predictable Back and focus memory. Existing save/preview handlers remain behind the presentation. |
| Categories | Independent opt-in named rows with bounded membership, row/app ordering, per-row focus and a preview before enabling. Original favorites/grid remain saved. |
| Font and app presentation | Separate clock/header/name size previews and explicit Apply/Cancel; on-TV rename/reset plus local icon, bundled banner and image choices. |
| Visual wallpaper selection | Native local picture/theme thumbnails and full preview before Apply, with bounded background decoding and no automatic video playback. |
| Single/sliding rows and local backgrounds | Existing carousel, three positions, silent local video and safe position previews already cover the useful lightweight portion. Preserve them. |
| Backup/restore | Keep named setups; add five automatic recent-change points per profile, bounded history/files, validation and a rollback journal. Media bytes are not backed up. Native ordinary restore tested; power-loss/storage-failure acceptance remains pending. |
| Program/channel previews | Defer until content-provider availability, permission and exact resume intent can be verified. A package-only recent list does not provide viewing progress. Size alone is no longer a reason to reject a useful feature. |
| Online wallpaper/AI providers | Separate opt-in design needed for provider, credentials, data consent and cost. No network provider or paid generation is bundled. |
| Broad accessibility remapping, system cleanup and device overrides | Do not import. Preserve the existing narrow Home/input helpers and TV settings. |

Both new appearance choices are profile-scoped and included in saved setups. Choosing Follow theme removes the app-name override. Choosing Original theme restores the original text renderer. Upgrades with no new choices keep their prior rendering. Canceling a choice or browsing a section writes nothing.

## Build scope and validation

The owner approved a larger full product for useful features. Build it with `-PcommunityFeatures=true`; it uses the same package/data identity. The reference build and 589,824-byte gate/CI remain unchanged. No runtime dependency or permission was added. The existing SDK resource optimization deduplicates identical resource values. Resource IDs, named view IDs, runtime banner names, fonts, languages and density variants remain available. Resource paths and extensions are preserved: an earlier path-shortening attempt failed native Android 12 color-state-list inflation and was removed before acceptance.

The [feature contract](ANDROID-COMMUNITY.md) describes scope, bounds and category/grid differences. The [dated validation record](VALIDATION.md) distinguishes automated checks, native pilot evidence and remaining physical-remote, playback and broader-device acceptance. Static inspection is not a claim that all Projectivy screens or premium features were executed or recreated.
