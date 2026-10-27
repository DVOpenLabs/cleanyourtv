# LG phase: one product, a separate webOS implementation

Android and webOS can share a repository, app/profile model and visual conventions. Android APKs,
accessibility services and package IDs do not run on LG webOS. An external Android streamer remains
an independent source: installing its launcher does not change the television's native LG Home.

## Start with the simpler supported entry

The first LG build should be a small Developer Mode web app, opened from LG's app bar or a Quick Access
number key. Preserve LG Home and the owner's TV/input/power/picture/sound/panel-care settings. Keep
automatic Home interception out of the first installation. On a TV already using a Google TV Streamer,
the owner can continue using the Android Home while evaluating the native LG app separately.

[SamuelSHernandez's webOS edition](https://github.com/SamuelSHernandez/cleanyourtv-webOS/tree/4be30bddd3429258aac9f755581242f05cde43eb/webos)
is a promising baseline: plain ES5/CSS, native app-launch adapter, dock/library model, Magic Remote
handling, a constrained CSP and a journalled install/restore tool. Its README, setup, compatibility
record and root MIT licence were reviewed. Only one webOS-22 model is reported as experimental hardware;
Home routing is unvalidated. Full launcher/installer source and transitive-licence audit are still
required before importing it. It is not already integrated into this product.

The existing [Razdva122 helper review](LG-HELPER-REVIEW.md) and recovery patch remain useful for a later
optional automatic-Home path. That helper depends on compatible input devices, native runtime and
firmware behavior; C3/C4 evidence does not certify an LG B3 or another model. No input checks should be
bypassed. Keep triple-Home recovery, watchdogs and an independent return to LG Home as acceptance gates.
No helper installation or root setup is authorized by this plan alone.

## Phased work

| Phase | Deliverable | Acceptance before proceeding |
|---|---|---|
| 0. Inventory | Exact model suffix/region, firmware, webOS/SDK/engine, remote, native apps and input list | Read-only report; distinguish model family from installed OS; no private identifiers in public logs |
| 1. Source and package | Pinned web shell/TV adapter, licences, reproducible IPK and restore journal in this repository | Source/manifest/bridge review, bounded launch allowlist, no imported binaries/root scripts, simulated failure/restore tests |
| 2. Static Home | Clock, current favorites, clear focus, top/bottom placement, app search and settings shortcuts | D-pad and Magic Remote, Back, app launch/exit, HDMI, ordinary LG Home recovery; current settings unchanged |
| 3. Shared preferences | Map logical apps to actual LG IDs; profiles, names/order/hidden tiles, themes and desktop draft editor | No Android-ID guessing, no streaming-account claims; migrations/failed writes preserve previous profile; apply/readback/Undo |
| 4. Optional services | Weather beside clock/forecast, explicit location and units | Separate provider/network budget, offline/cache state; no credentials in public profiles |
| 5. Silent live media | Local H.264 still-fallback pack, then any compatible additional format | Bounded decode dimensions, permanently silent, pause/release on hidden Home; native streaming, standby/wake and OLED care unchanged |
| 6. Optional automatic Home | Separately audited helper plus patched recovery | Actual model/runtime compatibility; triple-Home/long Back, held volume, pointer, controller crash/loss, standby and cold boot; manual resume/recovery |

The UI should share the Android vocabulary: Favorites, App library, Quick actions, Profiles, Themes and
Undo. Share schemas and behavior tests rather than copying Java UI or adding an Android compatibility
layer. Native Bluetooth/settings and media state are capability-dependent; hide unsupported controls
or state the fallback instead of drawing a switch that cannot work.

## Size and lifecycle budget

- First web package: no Web framework, custom font bundle, online poster catalogue or shader engine.
  Icons come from the TV inventory; user images/video stay optional and outside the base package.
- Establish a measured package/startup/memory baseline on the first LG model before choosing numeric
  budgets. Android's APK size and decoder results do not establish webOS performance.
- No hidden Home timers, fetches or video playback. Respect reduced motion and existing OLED protection.
- Keep both a static fallback and a manual launch/restore route that does not depend on a running PC.
  A PC watcher is not equivalent to a self-contained on-TV Home helper.

## Developer Mode and owner inputs

Before a hardware trial, obtain both exact LG model codes and installed firmware/webOS versions. LG's
Developer Mode app requires an LG developer account and a local network connection; the owner signs
in on the TV. Enabling Dev Mode restarts the TV, so do it at an agreed time. The displayed remaining
session time is authoritative. Expiry/disabling Developer Mode can remove its sideloaded apps; retain
the package/configuration for reinstall and extend the session through LG's app while still valid.
See [LG's official guide](https://webostv.developer.lge.com/develop/getting-started/developer-mode-app).

The currently known first model family is OLED B3. Exact suffix and installed firmware are still
needed; the other LG is not yet identified. No LG has been connected, paired, rooted or modified by
this Android batch. This document is a plan, not a completed LG implementation or compatibility claim.
