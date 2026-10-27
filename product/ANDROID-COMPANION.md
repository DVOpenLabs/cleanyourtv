# Everyday controls and local app editing

Community alpha 5 extends this fork's existing native launcher. It does not install another launcher,
web engine, notification listener, microphone service or TV-side HTTP server. No Android dependency or
uses-permission is added. Personal artwork remains outside the APK.

## Categorized Launcher settings

The settings page has a category rail and an independently scrolling control pane: **Home layout**,
**Apps & profiles**, **Wallpaper**, **Remote & extras** and **Advanced**. Up/Down selects a category;
Right or OK enters its controls. Back from the controls returns to the category; Back again closes
settings. The selected category survives activity recreation through instance state, without writing
preferences simply because a page was browsed. Unsupported profile/theme controls retain their
existing capability gates.

Layout captions show current values, including theme values, and apps-per-row now offers 4 through 8
so an existing eight-app layout has a matching selected control. Numeric rows retain Left/Right
movement. Restoring theme defaults requires a confirmation, focused on Cancel; it preserves app order
and media files. The dialog explains the startup-app reset in Current Home. Opening TV settings remains
an explicit action and never applies a TV setting automatically.

This is an independent native implementation using the public
[Projectivy feature overview](https://github.com/spocky/miproja1) and the owner's usability feedback as
references. No Projectivy application code, proprietary assets or additional preference framework was
imported. English-device navigation was tested; RTL, large-font and physical-remote acceptance are
separate checks, not inferred from the layout compiling.

## Quick actions and finding an app

Open **Launcher settings → Quick actions**, or hold OK on the header's Settings button and choose
**Quick actions**. The compact panel reuses the existing QuickSettingsActivity. It opens the TV's own
settings, display, network, Bluetooth/accessories and apps pages; it also offers **Find apps** and
Launcher settings. Opening a page does not apply a picture, sound, network or pairing setting.

**Find apps** opens the installed-app library without adding a new Home tile. Up from a card selects
the Find apps button. Search uses the native keyboard and matches every query word against the app's
installed name and package, ignoring case and accents. Clear search restores the full catalogue.
Nothing is sent to a search service or saved as search history. In this quick-action library, OK opens
an app; adding/removing a Home shortcut remains an explicit hold-menu action. The ordinary library
retains its existing OK/add/selection settings. Back closes the utility library and returns focus to
the header Settings button.

The Bluetooth shortcut tries exported, permission-accessible native Settings entry points. On the
tested Sony Android 12 build it opens Remotes & accessories directly. On the tested Google TV Streamer
Android 14 build those direct routes are unavailable; it opens TV Settings and explains where to go.
It does not scan, request Bluetooth grants, bypass startup verification, select a device or pair one.

## Recently opened apps, profiles and wallpaper dimming

**Launcher settings → Apps & profiles → Remember recently opened apps** is off by default. Enabling
it explicitly remembers up to six package IDs opened successfully through Home tiles or the app library.
**Quick actions → Recently opened apps** shows newest first, using the current profile's app names and
omitting hidden, disabled, removed or unavailable apps. Reopening an app moves it to the front without
changing favorites. It opens the app, not a promised episode/playback position. Dedicated remote app
buttons, other launchers and viewing history are not tracked. There is no UsageStats permission,
notification access, timestamp log, service or network request.

History uses a small separate preferences file per launcher profile, so recording a launch does not
invalidate an unrelated desktop app draft. **Clear recently opened apps** clears that profile's list;
switching the feature off clears it and removes the optional flag. Failed saves/clears repair the
in-memory preference state. This history is independent of the pre-existing cinematic shelf's
`LastOpened` timestamps; it neither imports nor clears those older shelf records.

When the existing `local_profiles` capability is enabled, holding OK on the header Settings button
offers **Switch launcher profile** directly. Quick actions has the same shortcut. It uses the existing
profile library and chooser; import remains in Launcher settings. It switches launcher preferences only.
Profile import/setup and Google/streaming accounts are unchanged. The two owner trial configurations
currently leave this capability off; they do not gain a profile row or a fabricated user automatically.

**Launcher settings → Wallpaper → Wallpaper dimming** offers Follow configuration, 0%, 15%, 30%, 45%
and 60%. The existing still/video renderer applies it only to the background. Icons, labels, app layout,
wallpaper files, the screensaver and the TV's actual brightness setting are preserved. The choice is
scoped per profile; Follow configuration removes it, and explicit theme-default reset clears it.

These are independent implementations inspired by Projectivy's wallpaper controls, Home Deck's recent
apps and LiquiTvOs profile access. Quick actions can scroll to keep added entries reachable on shorter
screens. No new dependencies, permissions or Home routing services are introduced.

## Optional audio indicator

**Launcher settings → Audio-playing indicator** is off by default and stored per launcher profile.
When enabled, it shows the generic text **Audio playing** if Android's `AudioManager.isMusicActive()`
reports active music. It checks every two seconds only while Home is resumed. Leaving Home removes the
callback and hides the label. The indicator has no focusable control and no extra service.

It cannot identify a song or source app, and is not a Cast, HDMI or playback-success indicator. There
is no notification, media-session, recording or usage-access permission. A wallpaper's own muted
soundtrack uses UI-sonification audio attributes at zero gain, mapped separately from music; no system
volume or audio focus is changed. This avoids reporting the silent background as music
([Android attribute mapping](https://source.android.com/docs/core/audio/attributes)). Device-specific
external audio paths may not be reflected by Android's music-active signal.

## Desktop app editor

Run from the repository root with the existing authorized ADB tools:

```sh
python tools/layout_editor.py --tv <tv-ip> --package com.gathin.tvhome --label "Living room"
```

Use `--repo <tools-checkout>` when the existing `tools/tv.py`, venv and authorized key live in a separate
checkout. Use the exact installed package; a preview build can have a different application ID.
`--demo` opens a disconnected example instead. `--port` selects a local port; `--no-browser` only prints
the URL. An optional `--wallpaper <local-still.jpg>` changes the computer's illustrative preview only.

The browser page can arrange 1–32 favorite apps, hide/unhide tiles and edit their displayed names.
Its picture is an app-order sketch, not a pixel-exact renderer of every theme, scaling or layout.
Theme/media files, Home selection, permissions, services, system settings and streaming-app data are
outside its write contract. In particular, it never calls the theme-push scripts that reset manual
launcher appearance choices.

The editor also offers **Top / Middle / Bottom** app position. This is a profile-scoped alignment
choice: it does not change icon dimensions, app order, wallpaper or TV display scaling. **Use theme
position** removes that override. Existing app-only clients and imported four-field app drafts preserve
the current position. Position is reviewed alongside app changes and included in guarded Undo; app-draft
exports continue to contain only app order, names and visibility.

The Home illustration includes clock/date, weather and header buttons. An authorized read captures
only allowlisted text from this launcher's visible Home, with a capture time; **Reload from TV** refreshes
it. It does not capture another app's screen or poll weather. Weather appears when the installed Home
has a weather header/companion; this does not add weather networking to the public launcher. If Home is
not visible, the page labels the clock as PC time and weather as unavailable. Inputs and Settings reflect
the current header configuration, and the depicted buttons do not open system settings. Tile artwork,
responsive sizing and complex themes remain illustrative; an optional wallpaper is a local still.

On the TV, open **Launcher settings → Home layout → App position**. The three choices start a 20-second
preview with **Keep / Revert**; Revert is focused first. Timeout, cancellation, leaving Home or process
death discards an unkept preview. The saved position stays unchanged on upgrade until explicitly chosen.
Theme-default reset removes the override. Header clearance is retained on crowded layouts; a layout
too tall for three distinct alignments remains scrollable rather than overlapping the clock.

1. Load the selected TV/profile and edit a local draft.
2. Preview the changes. The confirmation names the target and lists the changes.
3. Apply only the exact reviewed draft. A private JSON backup is written first.
4. Undo restores the previous app state and position, provided nothing relevant has changed since that Apply.

Backups default to ignored `product/private/layout-backups/`; use `--backups` to choose another local
directory. They include package names and personal app labels: do not commit or publicly upload them.
An exported draft contains only the four-field layout, not device credentials, media or system settings.

Import validates a JSON draft into the browser; it does not apply it. An assistant can prepare this
same format for review, but **there is no built-in AI provider, model SDK, cloud call or credit charge**.
The app catalogue is the source of valid installed IDs; never invent them. Profiles use the separate
[profile library](ANDROID-PROFILES.md); this editor edits the currently selected profile and does not
switch Netflix/Google accounts or create a parental-control boundary.

```json
{"schema":1,"favorites":["org.example.video"],"hidden":[],"labels":{"org.example.video":"Video"}}
```

### Preservation and conflict handling

The native provider serializes requests with the UI/profile picker, rejects edits during on-TV reorder
or a style preview, and fingerprints preferences, profile, theme configuration and native order. Stale
drafts and stale Undo are refused. Reload and review again; a timeout is not proof a write failed.
The native write is one atomic preference transaction. A failed commit repairs the prior in-memory
state. The original `order.txt`, base configuration and media are not rewritten by the editor.

A name/visibility-only change preserves native ordering and dock-fill behavior. Explicit desktop
favorite moves take precedence over old native ranks, while preserving the relative order of remaining
apps. A later saved on-TV order takes precedence again. Undo restores the prior override, rather than
reconstructing an approximate base layout. Cards, empty docks, arbitrary grid ordering and individual
component aliases are outside this first app-package-only editor.

The server binds only `127.0.0.1`; Host/Origin checks and a random session token protect API calls. It
serves fixed local assets under a restrictive CSP. The TV provider separately enforces the shell's
`WRITE_SECURE_SETTINGS` permission; ordinary apps cannot call it. The launcher itself does not receive
that permission. Do not expose the editor through port forwarding or a network bind.

See [the SHOWCASE review](SHOWCASE-REVIEW-2026-10.md), [source register](sources.json) and
[validation limits](VALIDATION.md). Remote keyboard behavior, native settings routes and playback still
need acceptance on each model; a successful app launch alone does not establish protected playback.
