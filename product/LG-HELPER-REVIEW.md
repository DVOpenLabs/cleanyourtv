# LG no-root helper: source review

Reviewed Razdva122/clean-your-tv-lg-webos at
[`d5804f6a7ce5d4eb35c0093ca19ae011e5a6df7f`](https://github.com/Razdva122/clean-your-tv-lg-webos/tree/d5804f6a7ce5d4eb35c0093ca19ae011e5a6df7f).
This is a targeted source/protocol review, not a full security audit or hardware acceptance. No LG TV was
contacted. No helper or root component was installed, and no TV settings were changed.

## How it works

The no-root implementation runs a native input relay and Node controller on the TV. The relay looks for
the named LGE M-RCU source/destination devices, checks capabilities and held keys, then captures the source
and forwards ordinary input. Home is handled by the controller; other remote events pass through. This is
more involved than drawing a web home screen. A successful installer/runtime check is not evidence that
the owner's model exposes compatible input devices.

The relay has a two-second controller heartbeat deadline. The controller has a separate 2.5-second worker
deadline. Cleanup releases held keys and the input grab; failure paths keep a blocked latch. The ordinary
Helper tile opens explanatory UI; an explicit resume action starts capture. Local SSAP foreground/power
monitoring uses the paired TV connection. The source includes key-file ownership, group/mode, no-symlink
and atomic-replacement checks; those Linux behaviors were read, not validated on LG hardware here.

## Recovery finding and product patch

The relay maps `RELEASE` to exit code **27** in
[`relay.c`](https://github.com/Razdva122/clean-your-tv-lg-webos/blob/d5804f6a7ce5d4eb35c0093ca19ae011e5a6df7f/native/prototype/relay.c#L76).
The filter returns that action for the third qualifying Home release, and also for input overflow.
The controller's cleanup explicitly launches LG Home only for code **22** in
[`native.js`](https://github.com/Razdva122/clean-your-tv-lg-webos/blob/d5804f6a7ce5d4eb35c0093ca19ae011e5a6df7f/native/app/native.js#L97).

A local VM check ran the unmodified controller `finish` function with fake filesystem/process/Luna and
cleanup adapters. Code 22 requested LG Home; code 27 set the stop latch and ran cleanup, but requested no
LG Home launch. This supports a source-level mismatch with the documented triple-Home return behavior.
It does not establish what a real webOS window manager ultimately displays. The physical remote may
be released successfully while the current launcher remains visible until another Home press.

The [product patch layer](../platforms/webos/README.md) now separates `EXIT_HOME` (22) from `RELEASE`
(27), preserving the old action values. The relay uses the tested mapping; the existing controller
requires no change. A fresh pinned source extraction accepts the patch. Portable C tests and controller
simulation distinguish intentional recovery from overflow, including an in-flight launch and repeated
finish callbacks. No public bug report or upstream patch was sent. Full helper import and hardware
acceptance remain pending.

## Local checks

Ten selected upstream JavaScript test files passed on Windows/Node 24: runtime/power/screen policies,
screen watcher, launcher bootstrap, boot state, helper entry, Home-control persistence, its bridge and
the controller Home protocol. The two real-filesystem tests needed a writable workspace TEMP directory;
their first sandbox-temp runs failed and the corrected environment runs passed.

`tests/controller-resume-home.cjs` could not complete on Windows: its current test setup reaches
`process.getuid()` in the Linux permission module. This remains unvalidated here; no shim was used to
claim a pass. Alpha 2 compiles and runs the portable filter and startup tests with MSVC C11 on Windows.
The Linux relay itself was not compiled/executed. Node 0.12 compatibility, real UID/GID handling,
Developer Mode expiry, standby/cold boot and actual remote behavior remain untested.

## Integration decision

Keep this as the leading no-root webOS candidate, with target-runtime recovery acceptance as a gate
before deploying the helper. Review/build source rather than importing the committed IPKs. Keep the root
edition outside the default path. Retain the MIT notices plus the font, WebSocket, compatibility-runtime
and any redistributed desktop Qt notices. Preserve recovery controls independently of profiles/themes.

Inventory both LG model/firmware combinations before hardware testing. The upstream compatibility table
contains version-specific reports and unresolved limits; it does not validate the owner's TVs. Changing
application/service IDs later also requires reviewing the helper's hard-coded paths and Luna destinations.
