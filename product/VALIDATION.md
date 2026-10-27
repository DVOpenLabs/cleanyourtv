# Local integration checks - 26 October 2026

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
