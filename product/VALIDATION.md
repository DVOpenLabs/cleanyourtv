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
