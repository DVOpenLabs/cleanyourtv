# DV Freedom Launcher

**Your TV. Your way.**

DV Freedom Launcher is developed on the product integration branch of [DVOpenLabs/cleanyourtv](https://github.com/DVOpenLabs/cleanyourtv),
derived from [Gathin23/cleanyourtv](https://github.com/Gathin23/cleanyourtv). It is an independent development
build, not an official upstream release or an LG or Apple product.

The full Android build uses the DV Freedom Launcher name in the system app list, settings and About.
About → Credits and licence retains the upstream MIT notice. Branding changes keep the existing
package/signing/data identity, so an update preserves profiles, wallpapers and Home/helper selections.
The GitHub fork name and the reference build remain unchanged; the original licence/attribution is retained.

The goal is one maintained repository with Android and LG webOS implementations, consistent settings,
local profiles and media formats. Platform-specific Home handling and builds remain separate. The current
Android launcher lives in `launcher/`. The first webOS recovery patch/source layer lives in
`platforms/webos/`; **the complete webOS launcher/helper is not integrated or deployable yet**.

## What exists now

- The optional [full native feature build](ANDROID-COMMUNITY.md): nested settings, category rows, text-size
  previews, on-TV app artwork/name editing, visual wallpaper browsing and bounded automatic recovery.
- Native Android style preview, timed Revert and scoped Undo, inherited from the tested Sony prototype.
- A shared profile format, desktop exporter and Android native profile picker with offline library import.
- Bottom carousel with native focus/preview/Undo; bounded offline visual themes; optional silent local video.
- Opt-in dual-contrast focus, per-profile extra spacing, global HDMI visibility and a remote TV-button picker.
- An optional [Google TV Streamer Home-only redirect](STREAMER-HOME.md), inactive until explicitly selected, replacing a separate custom Home watcher.
- Alpha 5 [Quick actions, offline app search, a native Bluetooth shortcut and optional audio status](ANDROID-COMPANION.md).
- Categorized native settings with current values, predictable Back, and a confirmation before restoring defaults.
- [Cinematic idle, adaptive theme text, on-TV profile creation and named Home restore points](ANDROID-POLISH.md).
- A local desktop app editor with exact-draft preview, backups, conflict checks and guarded Undo; no TV web server or model dependency.
- A pinned, reproducible LG helper recovery patch with portable C and controller-protocol checks.
- An integration roadmap and source/licence review register. Listed projects are candidates, not bundled apps.

Selected Android features are being tested in separate Sony and Streamer builds. The Streamer now uses
this fork's Home-only helper instead of its previous custom Home watcher; its Pokémon media and app
layout were preserved. The full standard product APK and every feature combination are not device
accepted. The owner-authorized Sony pilot is now selected as daily Home; the original launcher remains installed as a recovery route. The webOS runtime/profile
adapter and hardware acceptance remain pending. See [Android profiles](ANDROID-PROFILES.md),
[appearance integration](ANDROID-APPEARANCE.md) and [lightweight SHOWCASE candidates](SHOWCASE-LIGHTWEIGHT.md).
Alpha 3 usability controls and their remaining native checks are described in [Android usability](ANDROID-USABILITY.md).
The [dated SHOWCASE review](SHOWCASE-REVIEW-2026-10.md) explains the alpha-5 selection and what remains
deferred. The [LG next-phase plan](LG-NEXT.md) compares a manual Developer Mode web app with a separately
gated Home helper. The owner's OLED B3 is a model-family lead, not established webOS compatibility.

## Shared profiles: first working slice

Profiles describe local launcher preferences, not Google, Netflix or other streaming accounts. Each profile
has its own ordered favorites, hidden apps and column count. The app catalog maps a logical name to the
actual installed app ID on each platform; a missing mapping is an error, not an invitation to guess.
The public example uses placeholder app IDs. Replace them using an inventory of the actual TV.

```sh
python tools/product_profiles.py product/profiles.example.json --profile adult --platform android --output product/build/android.json
python tools/product_profiles.py product/profiles.example.json --profile child --platform webos --output product/build/webos.json
python -m unittest discover -s product/tests -v
```

Use `--base <existing-config.json>` to preserve other configuration fields. Output is JSON: the Android
theme uses it as `config.json`; the webOS adapter uses the values for `window.CONFIG`. This tool **does not**
connect to a TV, install an app, change Home, alter accounts or push a theme. It refuses to overwrite an
existing output file. Private configuration belongs in `product/private/`, which is ignored by Git.

The exporter remains a desktop migration tool. The Android product source now also imports the shared
library and separates each selected profile's native saved appearance, order and library shortcuts.
Native pilot testing is recorded in [validation](VALIDATION.md); the webOS runtime adapter is pending.

## Product rules

- One active custom Home and one reviewed update/signing path per device; no chain of competing launchers.
- Android and webOS share settings semantics where supported, not Android APKs or hardware assumptions.
- Profiles preserve unrelated TV settings and never imply switching a streaming account.
- Wallpaper animation is silent, optional, has a still fallback and stops when Home is hidden. Decoder,
  playback and standby behavior must be measured on each device before release.
- Import selected components with source revisions, attribution and required licence notices. Do not copy
  another project's installer/debloat behavior or unlicensed artwork just to gain a visual feature.
- Weather and future AI providers are explicit opt-ins. No credentials or personal media in public source.
  No paid API/generation calls without the owner's authorization.
- Small fixes can go upstream. Restyles, video and new network add-ons belong in this independent product;
  a SHOWCASE submission can describe the tested result. Do not keep resubmitting features the owner declined.
- Test preview builds before handover; retain a recovery route. Device acceptance includes real remote
  buttons and playback. Do not turn off this owner's ADB during ongoing work without their instruction.

See [the validation record](VALIDATION.md), [roadmap](ROADMAP.md), [LG helper review](LG-HELPER-REVIEW.md)
and [source register](sources.json). The root README below this branch's
notice continues to describe upstream; it is not a claim that these product plans have shipped.
