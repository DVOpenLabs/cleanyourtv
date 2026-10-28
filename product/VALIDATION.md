# Local integration checks

## Wallpaper playlists — 27 October 2026

The full build adds an off-by-default, profile-specific local-background playlist. See
[the feature contract](ANDROID-COMMUNITY.md#wallpaper-playlists). Public full release: **633,395 bytes**,
SHA-256 `63354d87d611af0a55e9f6b2680dae7f0ba6012416999fef5199b8c83bf37412`.
Personalized pilot: **635,491 bytes**, SHA-256
`d80211cb594429533e1d602c8a2c95d449ee223e4305495fbbbaf20fb9089b83`.
Both pass **424 JVM tests** with zero lint errors (public: 134 warnings, 235 information; pilot:
140 warnings, 241 information). The reference build passes **392 JVM tests**, zero lint errors
(111 warnings, 228 information), and remains **588,531 bytes**, within the unchanged 589,824-byte gate.
Its SHA-256 is `2f01133504d603ab3321f53f68ed48804e063dde8786ddeba9e325c1c1c21994`.
No runtime dependency, manifest permission, service, network client or workflow was added.

Native injected-input tests on Sony BRAVIA 4K VH2 / Android 12 and Google TV Streamer / Android 14 used
temporary launcher profiles, then restored the owner profile and deleted those test profiles:

- The default interval was 15 minutes. A changed draft canceled without preference or recovery writes.
- Applying two silent clips at a one-minute interval allocated one native video player. Opening Netflix
  released it; 70 seconds outside Home consumed no playlist time. Home resumed the same entry with the
  saved remaining interval, then an actual deadline changed to the second clip with one player.
- Category preview played the same playlist; leaving released its player. Returning to the grid shared
  the saved playlist position. Rotation did not change the selected theme, app order or row position.
- Original owner preferences, profiles, app layout and saved order/library files matched after cleanup.

Two additional silent landscape clips were imported without selecting their theme settings. Both
started in the native player on each TV in a separate temporary profile, with a real timed transition
between them. After cleanup, the owner's four-background sequence was applied at 15 minutes through
the native editor, with each TV's previous wallpaper first. All other captured owner preferences,
app layout, row position and saved order/library files matched the baseline. The short-interval test
exercises the timer; this is not a separate measured 15-minute wait. All source clips contain no audio
stream. Visual acceptance of every generated character effect remains with the owner.

These checks establish the tested lifecycle and timer behavior, not a CPU/RAM benchmark, protected
streaming playback, every video codec, seamless video loops, physical remote acceptance or new reboot
acceptance. Largest-font/RTL layout and other TV models remain unaccepted. Media and device screenshots
remain private; the default public package was built but not installed on either personalized pilot.

## DV Freedom Launcher branding — 27 October 2026

The full product now uses the selected **DV Freedom Launcher** name in its application label,
settings breadcrumb, About page and Home-only helper description. About includes the original Gathin
MIT notice and source credits. The reference build, package/component IDs, signing identity and
Home/helper behavior are unchanged. The public fork keeps its existing repository URL.

Both release builds pass 416 existing JVM tests and lint with zero errors. Public APK: **627,307 bytes**,
SHA-256 `838b2292e757c13ece907b1a3529b012e07fcc677fb23e6824638155ef28de03` (123 warnings, 235 information).
Personalized pilot: **628,999 bytes**, SHA-256 `7a20d01b68164ffabb1ca3001a8bcd3eaafe2de0139a2df6660fd27de19e82ce`
(129 warnings, 241 information). Both compiled application labels were inspected before deployment.

Native Sony and Streamer checks opened the renamed settings, About/tagline and full MIT credits,
then returned through Home. Owner preferences/history/profiles, favorites/position, app cards and
wallpaper dimming matched their saved pre-branding states. The protected Home/helper, display,
ADB, screensaver, boot count, package set, UID and configuration/media/recovery checks passed.
No temporary test profiles, settings changes or media replacement were required. On Sony, the first
test expected the settings root but found its remembered Appearance page; returning one level and
rerunning the checks passed. Branding is not a new acceptance claim for physical remotes or playback.

## Full native community features — 27 October 2026

See [the feature contract](ANDROID-COMMUNITY.md) and [expanded Projectivy review](PROJECTIVY-REVIEW.md).
The owner approved a larger full product; useful features are no longer restricted to the reference
APK ceiling. One optional source set extends the same app identity. It adds no runtime dependency,
manifest permission, service or launcher network client. The reference build/gate and CI are unchanged.

- Full public release: **624,915 bytes**, SHA-256 `7597dfdb21d93f9007e771288c4fdcbd0cade628f2e90b334c94258cebc98baf`.
  Same-feature personalized pilot: **626,831 bytes**, SHA-256 `4380d6006b982e372ebc17d306f6df9f6a71f4ab059d9f75746bb2207b8e79e5`.
  **416 JVM tests** pass in each release and **28 product tests** pass. Full public debug assembly also
  succeeds. Public lint has zero errors, 123 warnings and 228 information entries; pilot has zero
  errors, 129 warnings and 234 information entries. These are not warning-free builds.
- Reference release with no full-feature flag: **588,527 bytes**, SHA-256
  `b3436d8d18294ceab4baea96f77e82a526ac1c71872a3ea7016d2a8f2433ff43`, **392 JVM tests**, zero lint errors,
  111 warnings and 228 information entries. Its original 589,824-byte ceiling remains intact.
- REA/JADX evidence now includes selected settings view/Back-dispatch and preference-screen methods,
  along with prior package, class, category and preference controls. Twenty-one complete CLI records
  and their digest index remain private. This is selected static inspection, not execution of every
  Projectivy feature, and no original code or assets are published.

### Native Sony and Streamer pilot checks

On Android 12 Sony and Android 14 Google TV Streamer, the existing personalized pilot was updated in
place with package/signing/installed-hash guards. Both use the full feature source; the public default
package itself was not installed. Tests created disposable launcher profiles, restored Current layout,
and deleted those test profiles. The original owner's app layout, selected media and profile choices
were compared before and after, rather than replacing them with demonstration defaults.

- All seven settings sections, nested Appearance navigation and Back passed with no preference/history
  writes while browsing. Screenshots were inspected for menu hierarchy, current values and focus.
- A text-size draft canceled without a write. Explicit Apply created a profile-specific automatic point;
  native Recent changes restored the previous text choice, left no pending journal and retained the
  existing order/library file hashes. Those files were unchanged by this text edit; this is not a
  native recovery test of deliberately damaged order/library data.
- Native rename/original-name reset and icon artwork persisted within the test profile while preserving
  favorites/order/position. The final catalog puts current Home favorites first. Local-image SAF
  import/confirmation is implemented and policy-tested but has not been file-picker accepted on-device.
- Local wallpaper/theme browsers opened, changed preview focus and canceled without a write. Still
  preview screenshots were retained for visual review. Browsing does not enable or auto-play video.
- Categories passed create/choose-apps, rename, hide/show, preview without writes, horizontal/vertical
  row focus, opening Inputs without selecting a port, explicit enable/Home routing, category Arrange,
  grid-only preview guidance, omission of unsupported grid controls, and return to the existing layout.
  The category idle preview hid rows/actions and retained the clock/weather; first OK woke without
  launching an app. Automatic idle timing uses the existing tested policy; physical remote/system-key
  combinations and a native timed-idle matrix are not established by that preview.

An initial Streamer editor run was interrupted by Android's HDMI active-source-lost activity; its
cleanup restored the owner profile, and the rerun passed. Earlier native test-harness problems included
tracking only the latest nested dialog and tapping a clipped list row. The private own-window fixture
now tracks showing focused dialogs and the app catalog favors current favorites; the reruns passed.
The Streamer's existing Home-only helper was kept; no UIAutomator or broad accessibility capture was used.

Remaining acceptance includes physical remote buttons/held volume, protected streaming playback,
casting, larger fonts/RTL/large catalogs, imported-image selection, category/grid long-press parity,
native timed-idle combinations and real storage/power-loss failures. Category mode can briefly show
the original grid on Home before its internal activity opens; it stays opt-in. This is bounded Home
recovery, not media/device backup. The LG webOS launcher was neither installed nor changed.

## REA Projectivy review and guided appearance controls — 27 October 2026

See [the inspection and selection record](PROJECTIVY-REVIEW.md). REA 6.8.0/JADX 0.7.1 inspected the official 4.72 APK, class inventories, selected methods and incoming-reference coverage. Seventeen complete CLI Evidence records and their digest index are retained privately. Static findings do not establish Projectivy runtime behavior; no proprietary source/assets are copied into this fork.

- **392 JVM tests** pass in each build; **28 product tests** pass. New tests cover explicit text ink, default/selected/hidden label opacity, invalid saved values and profile-scoped storage.
- Public APK: **588,563 bytes**, SHA-256 `64e41908eb5857dfeff60c0dad302e462885b6864537195a3c8f1a210fca002c`. Signed private pilot: **589,143 bytes**, SHA-256 `13c70f79472655761e07cce1889f1d6362cf122bfa3eaa2b7fc0fb6341d58175`. Both pass the unchanged 589,824-byte ceiling. Public lint: zero errors, 111 warnings, 228 information entries; pilot: zero errors, 117 warnings, 234 information entries.
- SDK resource-value deduplication retains all 31 prior locale/density configurations, named view IDs and runtime banner names. The bundled font file matches its source bytes. No permission, runtime dependency or service was added.
- Native Android 12 Sony and Android 14 Streamer checks passed for all four App names modes, automatic/light/dark Home text, contextual help, section focus memory, Cancel, and selection-decoration alignment across all eight apps. The original theme-renderer option was not separately visually accepted in this batch. Existing layout, app order, positions, dimming, profiles and restore-point names were restored/retained. Browsing and canceling leave the settings revision unchanged.
- Device protection checks match the prior selected Home/accessibility helper, size/density, ADB, screensaver/timeout, boot count, disabled package set, app UID, recovery launcher and original configuration/media hashes. No reboot or helper force-stop was used.
- Injected Home from Netflix returned to the selected pilot on Sony and through the existing narrow helper on Streamer. Two Home-only background captures per device changed within a crop excluding the header/app row, confirming continued Luffy/Pokémon background motion. This is not a protected-playback or audio-silence measurement.

An initial resource-path-shortening attempt failed Sony color-state-list inflation because Android required an XML suffix. The optimization was removed; the repaired build passed native Home and settings checks before being installed on the Streamer. Resource paths/extensions are preserved in the final artifacts. A touch-driven test also returned without a focused card; the final selection checks use actual D-pad navigation and explicit Home routing. This is not acceptance of pointer navigation on every device.

Screenshots, raw decompilation, private test fixtures and recovery artifacts remain private. Physical-remote/from-seat acceptance, protected streaming playback, casting, largest-font/RTL layout and broader hardware acceptance remain pending. The standard public package itself was built/tested statically; native evidence comes from its existing personalized pilot identity. Neither LG's webOS setup was changed.

## Theme readability, native settings and saved Homes — 27 October 2026

See [the feature contract](ANDROID-POLISH.md). This independent fork adds opt-in theme-aware solid text, refined focus motion, cinematic idle, on-TV profile creation and five named restore points per profile. Settings now uses a contextual menu/detail flow with five sections. An explicit Inputs shortcut offers real sources on an integrated TV and guidance on an external streamer. No runtime dependency, manifest permission, receiver or service was added by this batch.

- **389 JVM tests pass** in both builds. Added cases cover exact idle deadlines, first-key wake/repeat/up consumption, system-key passthrough, solid theme contrast, snapshot validation and atomic repair, and profile-scoped choices. Snapshot column bounds also accept the existing imported two/three-column profile format.
- **28 product Python tests pass**, including reachable grouped controls and desktop guards. The broader inherited suite was not rerun; its known failures below remain unresolved.
- Standard lint: **0 errors, 109 warnings, 217 information entries**. Private trial: 0 errors, 115 warnings, 223 information entries. Not a warning-free build.
- Standard APK **586,615 bytes**, SHA-256 `49d4785adb2730e023bb02d9c31e166f8107072e0f61fba385c50b32c397b7b5`. Private trial **587,391 bytes**, SHA-256 `ce4bc1a7a7f21404b1e43c3bba8c0f7ae4f9cf532872b5c5923c3c662e46bbd6`. Both remain below the unchanged 589,824-byte gate.
- SDK AAPT2 resource-name compaction occurs before normal packaging/signing. All 31 locale/density configurations present in the previous installed artifact remain present, as do named view IDs and runtime banner names. Font files and translations were not stripped. Matching R8 mappings and resource-ID symbols are retained privately.

### Native pilot evidence and limits

The signed private pilot was updated in place on Sony BRAVIA 4K VH2 / Android 12 and Google TV Streamer (`kirkwood`) / Android 14. Its private weather/appearance and shell-only own-window readback are not shipped as public assets. This does not establish acceptance of the unmodified standard package or all theme/model combinations.

On both devices, native D-pad checks and screenshots verified solid app-name glyphs, full-opacity header text, the focused card and focus-decoration alignment across all eight apps and back, all five settings detail pages, Back navigation, remote focus recovery after keyboard/touch interaction, and an unchanged editor snapshot/revision while browsing (excluding the live clock/weather header). Sony opened its existing real Inputs list without selecting a source. Streamer opened the explicit remote-control guidance; it does **not** enumerate or switch the attached LG's ports. Hardware-backed wallpaper posters safely skip CPU pixel access; enabling the option and returning Home were checked with the owner's existing media.

Named-setup tests saved the baseline, changed launcher-only dim/position, restored the prior apps/order/position/dim, cleared stale Undo, renamed, cancelled deletion and deleted a test entry. These controller tests preceded the menu presentation change; final menu traversal was checked separately. A copied on-TV profile retained the same readable design and favorites, did not inherit recent-history consent, kept its saved setups separate from Current layout, and could be renamed/deleted. Test profiles were removed. This is not an import/file-picker, all-16-profile or real storage-failure acceptance result.

Manual cinematic preview and an actual 30-second inactivity deadline hid apps, controls and the focus decoration while retaining the silent wallpaper. The first OK only woke Home and kept focus; it did not launch the hidden selection. Automatic idle was left **Off** for the owner's visual review. Theme-aware text and refined focus motion are enabled in this pilot. Each device retains **Original setup** plus a new **My setup** restore point for its finished appearance. Recent apps remain enabled in Current layout.

Home from Netflix returned to the applied Home on both devices using injected input. Sony's pilot remains its selected Home, with its original launcher available as recovery. Streamer retains Google Home/setup/Cast and the already selected Home-only helper. Original config/media hashes, UID, saved Home/accessibility choice, display size/density, disabled package set, ADB, screensaver/timeout and boot count match the protection baselines after cleanup. No reboot or force-stop was used. Native launch/return checks do not prove protected video playback, casting or physical remote behavior.

Real-remote/from-seat acceptance, actual protected playback, RTL/largest-font layout, fresh boot/casting and wider hardware combinations remain owner/release checks. No comparative CPU/RAM/FPS improvement is claimed. Personal artwork, screenshots, mappings, addresses and recovery APKs stay private. Neither LG was changed.

## Recent apps and wallpaper controls — 27 October 2026

Community alpha 5 follow-up, independently implemented from the Projectivy/Home Deck/profile guides:

- **376 JVM tests pass** in public and private trial builds. New cases cover bounded/deduplicated MRU,
  malformed storage, per-profile history naming and choice isolation, dim fallback, exact failed-save
  repair, and failed history clear. **28 product Python tests pass**, including settings reachability
  and the native Quick actions scroll container. The broader inherited suite was not rerun; its known
  failures remain recorded below.
- Public lint: **0 errors, 106 warnings, 196 information entries**. Trial: 0 errors, 112 warnings,
  202 information entries. Public APK **583,055 bytes**, 6,769 bytes below the unchanged gate;
  SHA-256 `54d5864e1526fbc7fc1655ba4d518bc91627bb617157834730b39051fe8ebe43`.
  No added runtime dependencies, permissions, Home helper, polling service or launcher networking.
  Matching R8 mappings are retained locally, outside Git.
- Native tests passed on Sony VH2 Android 12 and Google TV Streamer Android 14: choose 30% wallpaper
  dimming and restore Follow configuration; explicitly enable recent apps; launch Netflix and STB
  through favorite tiles; verify newest-first package records; reopen STB from the recent menu with
  no duplicate; clear the list; disable it; open Netflix again and confirm no new record. Original app
  layout, actual tile dimensions, Top position and personal media were retained. History was left off
  and empty, and wallpaper dimming at its original theme value.
- The direct profile shortcut is capability-gated. Both owner configurations keep `local_profiles`
  off, so no feature flag/library/account was changed to expose it. Profile preference isolation and
  failed selection remain JVM-tested; live multi-profile switching/import through the new shortcuts
  is **not claimed** as hardware-tested.
- Native app transitions released and resumed the silent wallpaper player. Protected Home/helper,
  display scaling/density, screensaver, timeout, disabled packages, UID, boot count, enabled ADB and
  original launcher/media hashes were unchanged. No reboot, casting acceptance or actual protected
  playback result is inferred from app launches. Physical remote, large-font/RTL and every style
  combination remain acceptance checks.
- Browsing all five settings categories, entering controls and returning with Back left saved layout,
  profile and revision unchanged on both devices. The editor header's capture time was excluded from
  that equality check because it is read-only and time-varying.

Current SHOWCASE description refresh: blob `57f490a24c7ebf23818e05330791ce8a799ea688`, 140 projects;
the three descriptions added after the earlier 137-project review were triaged. This is not a source
or hardware audit of all projects; see [the review](SHOWCASE-REVIEW-2026-10.md).

## App position and Home preview — 27 October 2026

Community alpha 5 follow-up, using the same application identity and permissions:

- **364 JVM tests pass** in public and private trial builds; six additional placement/rollback cases.
- Public lint: **0 errors, 103 warnings, 184 informational entries**. Trial: 0 errors, 109 warnings,
  190 informational entries. Neither build is warning-free.
- Public APK: **578,759 bytes**, SHA-256 `25701e2856610514e914d6a2f4cbd9f0358657c1a73c5bb31189f31de68d9f86`,
  11,065 bytes below the unchanged 589,824-byte gate. R8 now shortens names as well as shrinking code;
  the exact build's `mapping.txt` must be retained locally to decode crash traces. No runtime reflection
  or name-based class loading was found in the launcher source. Manifest/XML entry points retain AGP's
  standard keep rules, and installed activity/provider/helper routes were exercised after the change.
- **27 product Python tests pass**, including old app-only client compatibility, position-only review,
  Apply/Undo, stale drafts and rejection of unreviewed changes. The inherited full Windows suite has
  known failures recorded below; it was not rerun or declared green for this follow-up.
- The connected web UI reviewed a position-only Bottom change on the Streamer, applied it with a
  private backup, then restored the starting Top/theme position using Undo. The final UI showed weather,
  clock/date and Settings with no pending draft. Inputs absence was explained by the current config.
- On Sony BRAVIA VH2 Android 12 and Google TV Streamer Android 14, Top, Middle and Bottom were each
  applied and undone. Evidence checks actual card coordinates after focus movement, not padding alone.
  All eight favorites remained visible and reachable by injected D-pad events; icon dimensions, names
  and ordering stayed unchanged. Both native selectors defaulted to Revert; explicit Revert and the
  20-second timeout restored geometry without changing saved preferences/revision.
- Captured Home header text included clock/date, both weather units and Settings. Sony Inputs was
  visible; the Streamer retained its existing hidden Inputs configuration. Weather data stays in its
  separate companion; the editor reads the rendered label and labels its capture time.
- Native Netflix/STB launches released the wallpaper player and return to Home resumed it on both
  devices; Streamer return exercised the existing narrow Home helper. Launch checks do not prove
  protected streaming playback. No reboot was performed.
- Home selection/helper, scale/density, screensaver, timeout, enabled debugging, boot count, disabled
  packages, streaming-app data, UID and personal theme/media hashes were preserved. Sony's selected
  original Home remained unchanged; updates were confined to its separate trial package.

Fresh-boot, physical-remote, largest-font, RTL and every theme/carousel combination remain separate
acceptance checks. Personal images, device snapshots, APKs, mappings and local addresses are not in Git.

## Earlier checks — 26 October 2026

Android version: **3.0-community-alpha3 (11)**. This identifies a product test build, not an upstream release.
Feature switches remain off by default. No TV installation, settings write, Home handover or ADB action
was performed for this integration.

## Android

From `launcher/`, with JDK 17 and Android SDK 34:

```sh
./gradlew --offline --no-daemon testReleaseUnitTest lintRelease assembleRelease
```

- Release build succeeds.
- **324 JVM tests pass**, zero failures/errors/skips. Alpha 3 adds 22 cases over alpha 2: spacing (3),
  double-frame style selection (1), profile/global persistence and failed remote-save repair (2), visual
  pack/control boundaries (1), HDMI state policy (3), restricted remote destinations (2), TV-button
  press lifecycle and cross-receiver deduplication (10).
- Lint: **zero errors, 97 warnings**, plus 151 informational entries. This is not a warning-free build.
- Release APK: **570,257 bytes** (556.9 KiB), **8,736 bytes** above alpha 2 (561,521 bytes), below
  the existing **589,824-byte** limit by 19,567 bytes. Binaries and personal media are not committed.
- SHA-256: `88a7c4527a73e796a00ad69c4f4133b5611a119df7d08e33de87fc6a7774df95`.
- The manifest and runtime dependencies are unchanged. No new permission, Internet access or service.
- `git diff --check` passes.

The tests use ordinary JVM data, temporary filesystem ZIPs and a SharedPreferences stand-in. They
establish geometry, callback-generation gates, import quotas, unsafe/control input rejection,
CRC failure cleanup, interrupted staging, immutable publication, isolation and failed-save repair.
They do not exercise Android BitmapFactory, MediaPlayer, TextureView, real disk durability, native
dialog focus, file-picker availability, physical remotes or playback. The bitmap/decoder paths compile;
their native execution is pending. The size delta does not establish idle RAM, CPU or decoder performance.

Alpha 3 policy tests also cover unchanged zero-gap geometry, minimum tile sizing, avoiding a trailing row
gap, unknown HDMI states, follow-configuration pass-through, repeats/cancellation/orphan releases,
independent remotes, changed choices and delayed/duplicate events. They do not render GlassView,
exercise real TvInputManager/CEC or Show all popup flows, intercept a physical TV key, resolve native
app intents or test real accessibility settings. These [native usability checks](ANDROID-USABILITY.md)
remain required before device handover. The complete inherited Python/CI suite was not rerun.

From the repository root:

```sh
python -m unittest discover -s product/tests -v
```

All **eight** desktop profile/exporter tests pass. The full inherited Python suite was not rerun for
this product integration; no all-repository/CI pass is claimed.

## LG

The following alpha 2 evidence is retained unchanged; no LG source or helper behavior was modified or
retested in the alpha 3 Android batch.

The pinned source is reproducibly extracted by `platforms/webos/prepare_helper.py`; `git apply --check`
and patch application succeed against a fresh output. A second call against existing output is refused.

- Portable C11 filter and startup tests compile and pass on Windows/MSVC: triple-Home exit 22 versus
  overflow 27, timing/holds/repeats/interruption/cursor/pass-through; held-key release wait, deadlines,
  destination busy, simulated ioctl failure, controller loss and stop.
- Node 24 executes the pinned controller's real `finish` function with mocked OS/filesystem/Luna/cleanup:
  recovery opens LG Home after cleanup, overflow does not, in-flight launch waits are bounded and repeat
  callbacks do not duplicate cleanup/launch.
- Ten selected upstream JavaScript test files passed in the earlier review. One Linux-dependent
  controller test file remains unvalidated on Windows; no full upstream-suite pass is claimed.

No Linux relay/ioctl execution, target-Node compatibility or physical LG acceptance is claimed. This is
a source patch layer, not a complete helper/launcher package. Full platform packaging and deployment
remain pending; the root edition is excluded. See [the review](LG-HELPER-REVIEW.md).

## Photo screensaver temperature units — 27 October 2026

The optional `Show both Fahrenheit and Celsius` setting is off by default. It converts the raw Celsius
reading independently into each rounded display value, and prevents a cached single-unit label being
reused after the option changes. No dependencies, permissions, system settings or media writes were added.

The add-on passes all 11 JVM tests, including six new conversion tests, and release lint has zero errors.
Both the standard build and a Sony build enabling the new option by resource default compile successfully.
The Sony APK is 38,408 bytes. It was installed over the existing signed add-on on a BRAVIA 4K VH2,
Android 12, after backing up its APK and verifying the signing certificate. UID, permissions, selected
screensaver, timeout and external media hashes were preserved.

Native screenshots show the enabled setting and both units in the slideshow preview, which shares its
Scene with the Dream service. This batch did not force the system Dream service to start; its selection
and timing were left unchanged. The owner confirmed both units were comfortably readable on the
launcher and screensaver preview, without overlap. Screenshots and personal
artwork remain private. The launcher header's dual-unit trial is still in the isolated local preview,
not the public product launcher; its existing 324 JVM tests and release lint pass.

## Streamer persistent Home — 27 October 2026

Alpha 4 adds an optional Home-only accessibility helper. It is inactive until selected; no installer
or default configuration silently selects it. The standard build passes 332 JVM tests (eight new policy
tests), release lint has zero errors, and its release APK is 571,989 bytes, below the 589,824-byte gate.
No runtime dependency or uses-permission was added. A Streamer trial build is 570,113 bytes; its private
weather/appearance harness and personal media are not bundled in the public product.

On a Google TV Streamer (`kirkwood`), Android 14, firmware UTTK.260317.003, the trial was updated in place
after saving the previous APK and checking its signing certificate. Its UID was retained. Only the
previous custom Home watcher was replaced in the saved accessibility list. The new bound service
reports capabilities=0; the previous watcher is no longer selected. Google launcher/setup/Cast components,
display overrides, disabled packages, animation scales, screensaver selection and power timeout were
preserved. No Sony APK or configuration was changed in this rollout.

Native smoke checks passed: Netflix/STB EMU launch, wallpaper-player release while those apps are in
front, injected Home returning to the fork, repeated Home, native Settings remaining open, and automatic
Home plus muted Pokémon playback after a reboot. Actual protected playback and physical remote behavior
still require owner acceptance. The final duplicate-coalescing adjustment also passes the policy tests;
the full reboot test preceded that small adjustment. The final in-place update retained the selected
helper and its bound capabilities=0 state; a native rapid injected-Home smoke check also passed on that
final installed revision. This is not a claim that casting or every remote button was
physically tested. See [Home mode and Undo](STREAMER-HOME.md).

## Alpha 5: everyday controls and categorized settings — 27 October 2026

See [the feature contract](ANDROID-COMPANION.md) and [dated SHOWCASE review](SHOWCASE-REVIEW-2026-10.md).
The selected implementations are independent app-only desktop editing, native settings shortcuts,
generic audio activity, offline app search and categorized settings. No Android runtime dependency,
uses-permission, receiver or service was added relative to alpha 4. The optional Home-only helper
from alpha 4 remains available with the same activation boundary.

### Reproducible local checks

- **358 JVM tests pass**, zero failures/errors. This includes app-draft validation, stale/profile
  conflict checks, failed-save repair, scoped preferences, saved-order precedence and local search.
- Standard release lint: **zero errors, 102 warnings**, with 181 informational entries. Not a
  warning-free build. The trial's private harness has additional warnings.
- Standard release APK: **589,347 bytes** (575.5 KiB), below the unchanged **589,824-byte** limit by
  **477 bytes**. This leaves very little room for further base-APK features; optional companions/tools
  and a new size review are preferable to silently raising the gate. It is 17,358 bytes above alpha 4.
- Standard APK SHA-256: `828c8bbf8964c2beebf1efdff0643740f1599b4c69d6774b7dc6f94c2301a927`.
- **23 product Python tests pass**: exporter, desktop editor/HTTP guard/backup/Undo and settings-control
  reachability. Affected inherited source/menu/tool/settings-documentation checks also pass (28 tests).
- The full inherited suite was run on Windows: **1,707 tests, 121 failures, 27 errors, 206 skipped**.
  An archived prior-commit source baseline under the same environment had 123 failures, 28 errors,
  207 skipped. This is not a green CI result. Existing translation, shell/PowerShell, theme/recovery
  and fixture assumptions remain unresolved. The unchanged LAN-address test module varied by case
  between runs; this comparison alone does not prove every remaining failure environmental.
  New menu documentation, tool indexing, text encoding and the menu-label translation regressions
  were resolved and their focused checks rerun. No broad inherited-suite pass is claimed.
- `git diff --check` passes. Existing runtime dependencies and SDK floor remain unchanged.

APK size excludes personal images/video, optional add-ons and desktop Python memory. No new comparative
CPU, RAM, latency or jank benchmark was performed; the footprint gate and lifecycle checks do not
establish a performance improvement.

### Native device evidence

Trial package `com.dvopenlabs.cytv.usabilitytest` was updated in place on Sony BRAVIA 4K VH2 / Android 12
and Google TV Streamer (`kirkwood`) / Android 14, using the existing certificate and UID. Final trial
APK: **588,971 bytes**, SHA-256
`c79324513570c64a11d100655f231fdb5f0711a030f196958919235a7beb61a0`.
Its private weather/appearance/readback fixtures are not in the public source package. These results
do not establish hardware acceptance of the unmodified standard package or every feature combination.

On both devices:

- Native adapter readback and screenshots verified a temporary favorite swap and custom app name;
  Undo restored the exact prior rendered tiles and app-only snapshot. A separate rename/Undo,
  stale-draft rejection and ordinary-app-UID permission denial also passed. These editor/security
  checks preceded the categorized-settings-only change; their implementation did not change afterward.
- A silent PCM fixture at zero player gain exercised Android's music-active signal and displayed the
  indicator above Settings. Cleanup released the fixture and restored the prior indicator preference.
  This is not a test of every streaming app's background-audio path. The option remains off unless
  the owner enables it. A final native comparison on each device used zero-gain PCM on both the music
  route and the background's UI-sonification route: only the music route set music-active. Both
  fixtures released and restored the prior indicator preference. Live video now explicitly uses the
  latter route at permanently zero gain so a muted soundtrack cannot masquerade as background music.
  Launch/player-release/resume and protected-state checks passed again on this final build.
- The native Bluetooth route opened Sony's exported Remotes/accessories slice. Streamer could not
  resolve a direct supported route and used ordinary TV Settings with a hint. No device was selected
  or paired, and no Bluetooth permission was granted. Route checks preceded the category-only UI change.
- The on-TV reorder collision guard rejected desktop editing while reorder was active; no apps moved.
- Native local search returned only Netflix, handled no matching apps, cleared back to the full
  catalogue, and closed without changing the layout. Tests used injected D-pad/native keyboard and
  screenshot-confirmed dialog buttons, not an assertion about the owner's physical remote.
- All five settings categories were reached; Right entered controls; Back returned to categories;
  Back from the rail closed settings. The full editor snapshot **and revision** were identical before
  and after browsing. Current eight-column layouts show 8 selected; no layout choice was applied.
- Netflix/STB EMU launch, wallpaper-player release behind those apps and local-video resume on return
  passed. Streamer also returned through the existing helper after injected Home. Actual protected
  playback is not established by these checks.
- Package UID/certificate, saved Home/accessibility selection, display size/density, ADB state,
  screensaver/timeout, disabled packages, original-launcher APK and external config/media hashes were
  preserved. Sony's original theme hashes also matched. Boot counts did not change; idle screensavers
  were dismissed to test the UI without changing their configuration. **No additional reboot** was run.

Sony's original selected Home remains unchanged; its new features are in the separate preview. On the
Streamer the existing narrow Home redirect continues to select this trial as custom Home. Stock Google
launcher/setup/Cast remain enabled. Debugging remains on as requested during ongoing development.

Screenshots, app inventories, device addresses, personal media and recovery APKs remain private.
Real remote buttons, actual Netflix/STB playback, RTL, largest-font layout, new cold-boot/casting tests
and broader model acceptance remain pending. No LG was connected or modified; [LG-NEXT.md](LG-NEXT.md)
is a staged plan, with exact firmware/model inventory still required.
