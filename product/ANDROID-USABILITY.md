# Android usability options - alpha 3

This source-only batch adds optional focus outlines, extra tile spacing, an HDMI list filter and a
picker for the remote's TV button. **No TV was contacted, installed, reconfigured or restarted.**
The version is `3.0-community-alpha3` (11). Native remote/input/visual acceptance is still pending.

## Opt-in and unchanged defaults

These base configuration switches default to false:

```json
{"appearance_controls": true, "input_visibility_choice": true, "tv_button_picker": true}
```

The example is for a future isolated test configuration; it is not pushed automatically. Without opt-in,
the new settings rows are hidden and saved overrides are dormant. `tile_gap` defaults to zero and
`hide_inactive_hdmi` defaults to false. No install scripts, grants, accessibility settings or Home
selection are changed. The existing manifest, runtime dependencies and minSdk 26 are retained.

## Focus and spacing

- Launcher settings → Focus outline: Follow theme, Standard rim, or Strong black and white.
- Launcher settings → Extra tile spacing: Follow theme, 0, 8, 16 or 24 dp.
- Configuration/theme packs may specify `glass_style: "double"` and `tile_gap: 0..32` directly.

The strong outline is adapted from Header Grid: opaque white outside the selected tile and black inside
its edge. It uses the existing cached GlassView, with two strokes and no additional glow, animation,
bitmap asset or rendering engine. An explicit double frame survives a layout preset. The existing
reduced-motion behavior is retained; the outline does not require focus zoom to be visible.

Spacing adds gutters within the existing grid cells. Tiles narrow by the requested extra amount until
the existing 80 dp minimum is reached; app identity, columns and order remain unchanged. Vertical grids
add the same extra amount between app rows, with no trailing gap under the last row. Single-row/carousel
layouts gain horizontal space but no empty vertical row. This is **extra spacing**, not a promise of an
exact final edge-to-edge gap: legacy layout, labels, dock decoration, zoom and minimum tile size still
contribute. Zero preserves the old sizing. No whole-row navigation rewrite was imported in this batch.

Focus/spacing choices belong to each local profile. Follow theme in Launcher settings clears those
appearance overrides. An explicit external theme push retains the launcher's existing appearance-reset
semantics. Selecting a library pack keeps manual choices, as before. Theme ZIPs may set the visual keys,
but cannot enable controls or write remote/input/boot preferences.

## HDMI list filter

Launcher settings → HDMI input list offers Follow configuration, Show all and Hide inactive bare ports.
The selection is global, independent of profiles and appearance reset. `hide_inactive_hdmi: true` is also
an explicit base-configuration default; the filter itself is off otherwise.

Only bare HDMI ports with a known standby/disconnected state are hidden. Connected ports, unknown
states, the existing unreliable-state fallback, known CEC device rows, AV and Live TV keep their existing
paths. Known CEC rows remain available to wake a sleeping device. A hotplug/state-read failure becomes
unknown rather than making that input disappear. If anything was filtered, a **Show all inputs** row
reopens the menu without filtering for that visit; it does not change the saved choice. Inputs hidden
in the TV's own settings and configured nonexistent ports remain excluded even in Show all.

The filter changes only this launcher's menu. It does not disconnect devices, change CEC, rename ports,
change native input settings, or filter a vendor's own source panel. Firmware state quality determines
how useful the filter is; Sony firmware that reports every port connected may show no difference.

## Remote TV button

Launcher settings → Remote TV button offers Follow configuration, Launcher Inputs menu, Live TV and
enabled launchable apps sorted by locale. Back/Cancel makes no write. Missing selections are retained
and shown as unavailable. A failed save repairs the in-memory choice/revision; real disk-failure
durability still needs native testing.

Only Android `KEYCODE_TV` is affected. Explicit choices take priority over configured mappings for that
key. Follow configuration leaves the existing key handling intact. `KEYCODE_TV_INPUT`, Home, Input,
Settings, volume and other dedicated/scancode mappings are outside this picker. Live TV opens the
configured TV app, with its configured action fallback when no launcher entry is available; it does
not select a tuner channel automatically. Unavailable/restricted destinations show a message rather
than silently opening another app.

On Home, no helper is needed. Across apps, the existing InputKeyService or HomeKeyService must already
be enabled and filtering keys. The settings status row links to the native Accessibility screen only
when clicked. It never grants access or enables/rebinds a helper. No additional service, receiver,
package watcher, polling loop or key timer is introduced. Existing helpers read base enablement when
their normal configuration is loaded; runtime picker choices use the shared in-memory preference cache.

The complete-press gate is shared across the activity and both helpers in the application process.
It ignores auto-repeat, cancelled/orphan releases, duplicate helper deliveries and delayed older events;
a changed choice at release cancels the override press. Devices/scancodes keep separate bounded state.
Remote and HDMI choices are global and survive profile/appearance resets. They are preference controls,
not parental restrictions, and the picker does not switch streaming-service accounts.

## Acceptance still required

Local tests cover policy and persistence, not Android rendering or actual firmware key interception.
Before any separately authorized device handover, test strong focus over bright/dark banners with zoom
and animation off/on; spacing at 4–7 columns, scale, RTL, carousel and library/reorder edges; all-hidden,
CEC standby, hotplug and Show all input cases; the physical TV button from Home and another app, both
helpers together, missing apps and return from Accessibility settings; then physical Home/Input/Settings,
held volume and real streaming playback. Do not auto-install this branch over the owner's current Home.

See [validation](VALIDATION.md), [source register](sources.json) and [attribution](THIRD-PARTY.md).
