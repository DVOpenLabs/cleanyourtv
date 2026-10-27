# Android appearance integration — alpha 2

This is the historical alpha-2 source milestone. The later feature build and native Sony/Streamer
checks are described in [the current feature contract](ANDROID-COMMUNITY.md) and
[dated validation](VALIDATION.md); the original installation status below no longer describes the pilot.

The product build adds a bottom carousel, a restricted offline theme library and optional silent local
video. These are source integrations, compiled and locally tested; they have **not been installed or
accepted on the owner's TVs**. There are no new Android dependencies, permissions, network calls,
downloaders or background services. Media is imported separately and never bundled in the APK.

## Opt-in configuration

For a future isolated test build/theme, enable the following keys in its base `config.json`:

```json
{"home_style_picker": true, "theme_library": true, "live_wallpapers": true}
```

All three switches default to false. This document does not push configuration or install anything.
The `local_profiles` switch is independent. When profiles are enabled, theme selection and the live
toggle belong to the selected profile; remote controls, boot behavior and TV settings remain global.

## Bottom carousel

Launcher settings → Home styles → Bottom carousel uses the existing app adapter, focus frame, library
and reorder model. It keeps the selected cell centered, with end padding for the first/last cell; it
does not wrap infinitely or create duplicate app entries. The row remains near the bottom. The existing
20-second Keep/Revert preview, Undo and grid presets remain available. Reduced motion uses immediate
scrolling. Geometry is checked in both physical directions; real RTL/focus/reorder behavior is pending.

## Offline themes

Launcher settings → Offline themes lists Original theme, imported packs, Import and Remove. Import uses
Android's document picker and requires confirmation; importing never selects the pack. Some TV firmware
does not supply a document picker, which must be checked before deployment. Selection preserves manual
columns/opacity/style/wallpaper choices, favorites, order and remote mappings. Existing manual choices
can override pack defaults. Original theme returns to the existing external-files theme and its media.

A pack is a ZIP with these entries at its root:

| Entry | Contract |
|---|---|
| `manifest.json` | Exactly `schema: 1`, `id`, `name`, `version` |
| `config.json` | Flat visual settings below; `{}` is valid |
| `wallpaper.jpg` | Required, valid decodable still fallback |
| `preview.jpg` | Optional still; reserved for future preview UI |
| `video.mp4` | Optional local clip, always muted by this launcher |
| `wallpapers/*.jpg`, `.jpeg`, `.png`, `.webp` | Optional still collection |

Example manifest: `{"schema":1,"id":"night-sea","name":"Night sea","version":1}`.
IDs use 1–48 lowercase letters/digits/dashes, beginning with a letter or digit. Names are nonblank,
at most 60 characters and contain no control characters. Version is a positive integer.

Accepted booleans: `wallpaper`, `dock_row`, `dock_title`, `always_show_names`, `reduced_motion`,
`ambient_glow`, `app_glow`, `focus_overshoot`, `dock_glass`, `single_row`, `dock_band_all`.
Other accepted values: `home_style` (`minimal`, `cinematic`, `tv_grid`, `easy_view`, `carousel`),
`background_color` (`#RRGGBB`), `columns` (4–7), `rows_below_dock` (0–3), `dock_opacity` (30–100),
`wallpaper_dim` (0–90), `wallpaper_rotation_minutes` (0–1440). Style defaults require the base style
picker switch to be enabled. Unknown settings are rejected, including app lists, key mappings,
permissions, helper flags and boot controls.

Alpha 3 also accepts `glass_style` (`glass`, `rim`, `halo`, `double`) and `tile_gap` (0–32 extra dp).
See [usability controls](ANDROID-USABILITY.md); these additions are visual-only and cannot enable
the remote/input controls or write their global choices.

This is a deliberately smaller format inspired by DipsySu's pack implementation, **not full compatibility
with its richer packs**. Fonts, icons, banners, headers, scripts, remote URLs and bundled character
artwork are excluded. Keep personal media outside the public repository and distribute only media you
may redistribute.

Limits: 64 ZIP entries; 64 MiB archive and extracted total; 64 KiB per JSON; 8 MiB per image; 48 MiB
video; valid image dimensions within the existing 8.4-megapixel bound; eight installed packs and
128 MiB total library storage. Duplicate paths (case-insensitive), traversal, unsupported paths,
nonempty directory records and malformed/duplicate-key JSON are rejected. A single worker extracts
to unpublished staging, validates and renames to an immutable pack directory. Failed imports leave
published packs alone. Interrupted staging is reclaimed on the next import. Used packs cannot be
removed while selected by any saved profile. Unused packs can be removed explicitly.

## Silent live video

After selecting a pack containing `video.mp4`, enable Silent live wallpaper in settings. It is off
initially. The player uses Android MediaPlayer/TextureView, not a bundled engine, requests no audio
focus and always sets both audio channels to zero. Prefer a short 1080p H.264 MP4 with its audio track
removed; codec compatibility and seamless looping depend on the actual decoder/media.

Video is admitted only with at least **2 GiB reported RAM**, low-RAM mode off, reduced motion off and
wallpaper enabled. Reported RAM may be lower than a TV's advertised capacity; the still fallback is
intentional on those devices. This is a conservative admission rule, not a measured performance claim.
The decoder is released on Home pause, hidden-memory cleanup, surface loss and destruction. Stale
callbacks cannot restart a hidden player. Failed playback reveals the still and latches video off for
that activity instance instead of repeatedly retrying. A new Home instance can try again.

## Acceptance remaining

Native document-provider availability and dialog focus; carousel edges, all apps, library/reorder, RTL
and scaling; selection across process death/restart; actual mute/decoder release; corrupt/unsupported
media; standby/wake; and real Netflix/STB EMU playback after leaving Home. Do not replace device
acceptance with injected keys or app-launch checks. No TV installation or setting changes are authorized
by this source work.
