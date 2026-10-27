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

## Optional audio indicator

**Launcher settings → Audio-playing indicator** is off by default and stored per launcher profile.
When enabled, it shows the generic text **Audio playing** if Android's `AudioManager.isMusicActive()`
reports active music. It checks every two seconds only while Home is resumed. Leaving Home removes the
callback and hides the label. The indicator has no focusable control and no extra service.

It cannot identify a song or source app, and is not a Cast, HDMI or playback-success indicator. There
is no notification, media-session, recording or usage-access permission. Device-specific external
audio paths may not be reflected by Android's music-active signal.

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

1. Load the selected TV/profile and edit a local draft.
2. Preview the changes. The confirmation names the target and lists the changes.
3. Apply only the exact reviewed draft. A private JSON backup is written first.
4. Undo restores the previous app-only state, provided nothing relevant has changed since that Apply.

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
