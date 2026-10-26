# Local integration checks — 26 October 2026

Android version: **3.0-community-alpha1 (9)**. This identifies a product test build, not an upstream release.
The feature switch remains off by default. No TV installation, settings write, Home handover or ADB action
was performed for this integration.

## Android

From `launcher/`, with JDK 17 and Android SDK 34:

```sh
./gradlew --offline --no-daemon testReleaseUnitTest lintRelease assembleRelease
```

- Release build succeeds.
- **284 JVM tests pass**, zero failures/errors/skips; 13 tests cover the new profile model/preferences.
- Lint: **zero errors, 92 warnings**. This is not a warning-free build.
- Release APK is below the existing 589,824-byte limit; binaries are not committed.
- The manifest and runtime dependencies are unchanged. Profiles add no permission or Internet access.
- `git diff --check` passes.

The profile tests use ordinary JVM data and a SharedPreferences stand-in. They establish mapping,
isolation, editor commit behavior, failure repair and input bounds; they do not establish Android dialog
focus, real disk durability, file-picker availability, physical remote behavior or playback.

From the repository root:

```sh
python -m unittest discover -s product/tests -v
```

All **eight** desktop profile/exporter tests pass. The full inherited Python suite was not rerun for
this product integration; no all-repository/CI pass is claimed.

## LG

The pinned helper source was reviewed, with ten selected upstream JavaScript test files passing on
Windows/Node 24. One Linux-dependent controller test file remains unvalidated here. The targeted controller
simulation found the recovery exit-code mismatch described in [the review](LG-HELPER-REVIEW.md).

No C relay execution, target-Node compatibility or physical LG acceptance is claimed. Helper import and
deployment remain pending; the root edition is not part of this integration.
