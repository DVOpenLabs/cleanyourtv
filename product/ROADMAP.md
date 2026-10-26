# Integration roadmap

## 1. Shared local profiles and settings

The initial desktop exporter is implemented. Next, add native profile persistence and a remote-friendly
picker on Android, then connect the same profile semantics to webOS. Switching must preserve each person's
favorites, hidden apps, layout and selected media; unrelated TV settings remain global. Cancelling or an
interrupted save must retain the previous profile. Migration must preserve the owner's current layout.

LiquiTvOs is a useful profile UI reference, but selecting a visible Google account is not a substitute for
separate launcher data. Do not read account credentials or claim to switch streaming-service profiles.

## 2. Integrate the webOS platform

Review the pinned no-root implementation from Razdva122, including its helper, recovery, runtime
requirements, third-party notices and update paths. Vendor only reviewed source with its licence notices
into a platform directory; keep the origin/revision history. Do not introduce a floating install/download.
The product's Windows build path should produce both platform packages from this repository.

LG model and firmware inventory is still missing. The current helper's C3/C4/other-model evidence has
limits; a published release is not acceptance on the owner's TVs. Do not use root as the default path.
Automatic on-TV Home handling, standby/wake and the recovery shortcut need model-specific testing.

## 3. Layouts, focus and shared appearance

Evaluate Home Styles' Aurora/Cinema/Orbit and Header Grid's focus/spacing implementation. Preserve the
existing native app model rather than importing whole competing launchers. The bottom carousel must
support reorder/library behavior, clear focus, all requested favorites and a grid fallback. Retain the
Sony's tested 20-second preview and Undo behavior. Verify RTL, reduced motion and scaling.

## 4. Theme library and silent live backgrounds

Review the offline ZIP importer and local-video player separately. Validate imported paths/resources,
preserve manual preferences, and keep user artwork outside the public repository. Every live theme needs
a still fallback and mute-by-default behavior. Release the player while streaming apps are in front.
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
