# Cinematic idle, readable theme text and saved Homes

The later [full native build](ANDROID-COMMUNITY.md) adds nested settings, optional category rows,
text-size previews, native app customization, visual wallpaper browsing and automatic recovery.
The earlier controls below remain available; see [validation](VALIDATION.md) for dated acceptance.

These features extend this fork's native Android Home. No runtime dependency, permission, network client or service is added. They do not change Android users or streaming accounts.

## Home layout

**Cinematic idle** offers Off, 30, 60 or 120 seconds, plus an immediate preview. While Home is visible and idle, apps, focus decoration and controls fade away; the clock, date, weather companion and silent wallpaper remain. The first navigation/OK/Back press only wakes the controls and consumes its matching key-up/repeats. Volume, Home and Input remain available. Leaving Home stops the timer and restores the views. Open menus, reordering, previews and touch exploration defer the timer. This is a Home presentation mode, not a replacement for the system screensaver.

**Theme-aware text** uses one solid light or dark ink palette from the active theme's background colour. Greeting/date/weather text uses full opacity; app names have no glyph stroke or label plate. Light ink has a small dark shadow. A still/poster-sampled header shade can assist where readable pixels are available. Hardware-backed posters skip pixel access safely. The existing decoded still image is sampled into a 32 by 18 colour grid; no screen capture or video-frame analysis runs. Text colour remains stable during a video loop. Mixed artwork still needs visual acceptance; this is not a guarantee of contrast against every frame.

**Refined focus motion** removes overshoot from the existing focus transition, respecting reduced-motion configuration. The focus decoration now refits when a row changes position even if the tile dimensions remain identical. All three app positions retain the same app order and native navigation.

These options are off on an unconfigured upgrade. They are profile-scoped and included in saved setups. Existing fonts, icon artwork, row size and selected wallpaper are retained.

## Profiles created on the TV

Open **Launcher settings → Apps & profiles → Launcher profiles → Create profile from current Home**. Enter a name using the native keyboard. Creation copies the current Home's app layout and appearance, without switching away from it. Switch through the profile menu or Quick actions; **Current layout** retains the owner's original Home.

Up to 16 imported and locally created profiles can coexist. Locally created entries can be renamed or deleted through **Manage created profiles**; deleting the active entry returns to Current layout. Imported libraries remain separate and cannot silently overwrite local IDs. A new profile does not inherit consent to remember recently opened apps or another profile's recent history. Profiles do not provide app locks or parental controls.

## Save my setup

Open **Advanced → Saved Home setups** to save up to five named restore points in the current profile. Restore, rename and delete are native D-pad dialogs; destructive confirmations start on Cancel. Names are 1–40 characters with case-insensitive duplicate checks.

A restore point contains an allowlist of launcher appearance/app choices. Restore restamps app-order priority and invalidates stale editor/style Undo, so an old desktop Undo cannot overwrite the restored Home. Recent-app consent/history, active profile, TV picture/sound/network settings, Home routing and accessibility selection are excluded. Media/theme references are saved without copying videos. Keep those local files available; unavailable offline packs are rejected before restore.

## Inputs

**Remotes & inputs → Inputs shortcut** controls a global header shortcut without changing a TV input setting. On an integrated TV it opens the existing real HDMI/input list. An external streamer with no input providers shows guidance and an ordinary system-settings shortcut rather than fictitious HDMI ports. Google documents configuring the Streamer remote's star button for TV input switching in its [remote guide](https://support.google.com/chromecast/answer/15323195?hl=en-CA). Direct control of an attached LG's ports is not established by this Android shortcut.

## Keeping the package small

The unchanged APK ceiling is 589,824 bytes. This branch runs the SDK's existing `aapt2 optimize --collapse-resource-names` against the shrunk release resource archive before ordinary packaging/signing. It retains all view-ID names and runtime `banner_*` drawable names; font files and locale/density variants are retained. No signing or SDK floor change is involved. See [AAPT2](https://developer.android.com/tools/aapt2).

The task integration targets this repository's pinned AGP 8.5.2 / build-tools 34.0.0 paths. It fails if its expected resource archive or symbols are missing; revalidate it when upgrading AGP. Preserve both R8 mapping and resource-ID symbols privately alongside each tested artifact. The dated [validation record](VALIDATION.md) distinguishes local checks, native trial evidence and remaining owner acceptance.

## Settings presentation

The alpha-10 native settings menu keeps a compact category sidebar and a wider controls panel. Rows
align icons, names, current values and chevrons; the footer explains the selected control. Outfit uses
explicit regular/semibold weights with the existing locale fallback. A strong border identifies focus,
separately from the active category or saved option. Position, row size and spacing offer local drafts,
and text sizes keeps its preview/Apply contract. Local alias search navigates without executing an
action. See the [current settings contract](ANDROID-COMMUNITY.md#settings-that-work-with-a-remote).
Settings uses opaque neutral surfaces so its readability is independent of the wallpaper. There is no
new UI framework, runtime dependency, network access, background task or stored search history.

This is an independent implementation informed by [Projectivy’s feature overview](https://github.com/spocky/miproja1), its [developer-published screenshots](https://play.google.com/store/apps/details?id=com.spocky.projengmenu), and a [published settings capture](https://quantrimang.com/dung-projectivy-launcher-tren-google-tv-210954). No Projectivy code or artwork is bundled. This fork does not claim its full feature set.

The subsequent [REA APK review](PROJECTIVY-REVIEW.md) records the actual static inspection and its limits. Home appearance now offers **Home text** (original, automatic, light or dark ink) and **App names** (follow theme, all, selected only or hidden). These choices belong to the active profile and saved setups. The context pane explains focused controls, and section re-entry returns to the last focused row during that settings session. Existing choices are preserved on upgrade.
