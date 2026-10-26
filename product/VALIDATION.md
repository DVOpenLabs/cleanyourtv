# Local integration checks - 26 October 2026

Android version: **3.0-community-alpha2 (10)**. This identifies a product test build, not an upstream release.
Feature switches remain off by default. No TV installation, settings write, Home handover or ADB action
was performed for this integration.

## Android

From `launcher/`, with JDK 17 and Android SDK 34:

```sh
./gradlew --offline --no-daemon testReleaseUnitTest lintRelease assembleRelease
```

- Release build succeeds.
- **302 JVM tests pass**, zero failures/errors/skips. Alpha 2 adds 18 cases: carousel geometry (2),
  playback lifecycle/memory gate (3), theme archive/library (12), profile-scoped theme/save repair (1).
- Lint: **zero errors, 95 warnings**, plus 130 informational entries. This is not a warning-free build.
- Release APK: **561,521 bytes** (548.4 KiB), **13,552 bytes** above alpha 1 (547,969 bytes), below
  the existing **589,824-byte** limit by 28,303 bytes. Binaries and personal media are not committed.
- SHA-256: `7775f5ee82cd6ac6f654888068e7b20c7b5bb1cee8ab8398290fca3d231b4412`.
- The manifest and runtime dependencies are unchanged. No new permission, Internet access or service.
- `git diff --check` passes.

The tests use ordinary JVM data, temporary filesystem ZIPs and a SharedPreferences stand-in. They
establish geometry, callback-generation gates, import quotas, unsafe/control input rejection,
CRC failure cleanup, interrupted staging, immutable publication, isolation and failed-save repair.
They do not exercise Android BitmapFactory, MediaPlayer, TextureView, real disk durability, native
dialog focus, file-picker availability, physical remotes or playback. The bitmap/decoder paths compile;
their native execution is pending. The size delta does not establish idle RAM, CPU or decoder performance.

From the repository root:

```sh
python -m unittest discover -s product/tests -v
```

All **eight** desktop profile/exporter tests pass. The full inherited Python suite was not rerun for
this product integration; no all-repository/CI pass is claimed.

## LG

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
