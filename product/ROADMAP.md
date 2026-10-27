# Integration roadmap

**27 October follow-up:** Top/Middle/Bottom alignment and the fuller desktop Home preview are
implemented. The next lightweight controls add optional recently opened apps, direct profile access
when profiles are enabled, and native wallpaper dimming. No automatic profile or appearance change is
part of an upgrade. See [controls](ANDROID-COMPANION.md) and dated [validation](VALIDATION.md).

**27 October alpha-5 update:** [Quick actions, local app search, Bluetooth settings/audio status and
the guarded desktop app editor](ANDROID-COMPANION.md) are implemented. Sony and Streamer native tests
and remaining acceptance are in [VALIDATION.md](VALIDATION.md). The [current SHOWCASE review](SHOWCASE-REVIEW-2026-10.md)
and [LG phase plan](LG-NEXT.md) refine the earlier roadmap below; complete LG packaging/hardware
acceptance and the public weather-header adapter remain pending.

## 1. Shared local profiles and settings

The desktop exporter and first Android native picker/import/persistence integration are implemented.
Next, verify the Android UI in isolation and connect the same profile semantics to webOS. Switching must preserve each person's
favorites, hidden apps, layout and selected media; unrelated TV settings remain global. Cancelling or an
interrupted save must retain the previous profile. Migration must preserve the owner's current layout.

LiquiTvOs is a useful profile UI reference, but selecting a visible Google account is not a substitute for
separate launcher data. Do not read account credentials or claim to switch streaming-service profiles.

## 2. Integrate the webOS platform

Review the pinned no-root implementation from Razdva122, including its helper, recovery, runtime
requirements, third-party notices and update paths. Vendor only reviewed source with its licence notices
into a platform directory; keep the origin/revision history. Do not introduce a floating install/download.
The product's Windows build path should produce both platform packages from this repository.

The first [helper review](LG-HELPER-REVIEW.md) found a recovery exit-code mismatch. Alpha 2 supplies a
pinned patch and locally passing filter/startup/controller tests in `platforms/webos/`. Full helper
packaging, target-runtime tests and LG hardware acceptance are still pending; nothing was installed.

LG model and firmware inventory is still missing. The current helper's C3/C4/other-model evidence has
limits; a published release is not acceptance on the owner's TVs. Do not use root as the default path.
Automatic on-TV Home handling, standby/wake and the recovery shortcut need model-specific testing.

## 3. Layouts, focus and shared appearance

Alpha 3 adds optional dual-contrast outlines and extra gutters in the existing adapter. Global HDMI
visibility and the TV-button chooser are also integrated without new services or permissions. Their
source/persistence tests pass; real native rendering, physical remotes and firmware input behavior
remain pending. See [Android usability](ANDROID-USABILITY.md). Whole-row navigation and other renderers
remain research candidates, not imported features.

The first native bottom carousel is implemented with the existing preview/Undo and adapter. Evaluate
Home Styles' Aurora/Cinema/Orbit and Header Grid's focus/spacing implementation next. Preserve the
existing native app model rather than importing whole competing launchers. The bottom carousel must
support reorder/library behavior, clear focus, all requested favorites and a grid fallback. Retain the
Sony's tested 20-second preview and Undo behavior. Verify RTL, reduced motion and scaling.

## 4. Theme library and silent live backgrounds

The restricted offline ZIP importer and permanently silent local-video adaptation are implemented.
Validate native picker/player behavior next. Imported paths/resources are bounded; continue to
preserve manual preferences, and keep user artwork outside the public repository. Every live theme needs
a still fallback and silent behavior. Release the player while streaming apps are in front.
Test corrupt/unsupported media, process recreation, standby and actual streaming playback on Sony and
Streamer before handover. LG playback support must be tested rather than inferred from Android.

## 5. Useful optional services

Weather can use the existing remote-friendly add-on on Android; cross-platform Home integration needs
its own opt-in provider boundary. The US alert viewer is outside upstream's new-add-on policy. Treat it
as an optional component of this product only after its packaging and lifecycle are integrated.

Potential AI work: plain-language configuration proposals with a preview and Undo, or finding a title
across installed services when a provider supports reliable deep links. Review provider cost, data sent
and user value before implementing. No AI dependency is needed for launching apps or choosing profiles.

## Release acceptance

Record source revision, signing identity, version and installed artifact hash per device. Test through
real app transitions, restart and physical remote/playback acceptance. Keep the Streamer's stock Google
components required by casting. Publish source and documentation with an honest compatibility matrix;
only add a SHOWCASE entry after a reproducible build and real-device validation.
