# Unified Home development

This is the product integration branch of [DVOpenLabs/cleanyourtv](https://github.com/DVOpenLabs/cleanyourtv),
derived from [Gathin23/cleanyourtv](https://github.com/Gathin23/cleanyourtv). It is an independent development
build, not an official upstream release or an LG or Apple product.

The goal is one maintained repository with Android and LG webOS implementations, consistent settings,
local profiles and media formats. Platform-specific Home handling and builds remain separate. The current
Android launcher lives in `launcher/`. The first webOS recovery patch/source layer lives in
`platforms/webos/`; **the complete webOS launcher/helper is not integrated or deployable yet**.

## What exists now

- Native Android style preview, timed Revert and scoped Undo, inherited from the tested Sony prototype.
- A shared profile format, desktop exporter and Android native profile picker with offline library import.
- Bottom carousel with native focus/preview/Undo; bounded offline visual themes; optional silent local video.
- A pinned, reproducible LG helper recovery patch with portable C and controller-protocol checks.
- An integration roadmap and source/licence review register. Listed projects are candidates, not bundled apps.

This branch has not been installed on a TV. The existing Sony style-test installation is unchanged.
The Android additions are compiled and locally tested, not device accepted. The webOS runtime/profile
adapter and hardware acceptance remain pending. See [Android profiles](ANDROID-PROFILES.md),
[appearance integration](ANDROID-APPEARANCE.md) and [lightweight SHOWCASE candidates](SHOWCASE-LIGHTWEIGHT.md).

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
That runtime integration still needs native UI and device testing; the webOS runtime adapter is pending.

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
