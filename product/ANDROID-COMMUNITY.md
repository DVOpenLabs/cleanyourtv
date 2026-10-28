# Full native Home features

The optional community build extends the existing Android launcher in this fork. It uses one APK,
package, signing path and data directory. It does not install Projectivy, a second Home or a web server.
The interaction design is informed by the [Projectivy review](PROJECTIVY-REVIEW.md); its code and artwork
are independent. See [validation](VALIDATION.md) for dated device results and remaining acceptance.

## Settings that work with a remote

Settings has seven short sections: Appearance, Apps & rows, Wallpaper, Profiles, Remotes & inputs,
Backup & restore, and System. Appearance opens Layout, Text & clock, and Focus & motion. The context
pane explains the focused choice; rows show their current value. Back returns one level, preserving the
focused row. Browsing a page writes nothing. Existing preview, confirmation and save handlers remain
behind the new navigation.

Text sizes offers separate clock, header and app-name previews at 85%, 100%, 110% and 120%. Apply saves
the current profile; Cancel keeps its previous sizes. Theme sizes removes the override. Existing app-name
visibility and solid light/dark/automatic theme text remain available. Automatic contrast follows theme
configuration, not every changing video frame; test busy wallpapers from the seating position.

## Optional category rows

Apps & rows → Categories creates up to eight named horizontal rows. Each row can contain up to 32 apps;
all rows together are bounded to 128 memberships. Rows and apps can be ordered, renamed, shown or hidden.
The virtual Favorites row uses the existing Home order. Categorizing an app does not move or remove it
from the original layout, and the same app can be in multiple categories.

Preview category layout opens a temporary view. Use categories is a separate confirmation and applies
only to the current launcher profile. Use existing layout returns to the saved grid. Category rows have
stable per-row focus, app-name/artwork choices, silent local wallpaper, real passthrough Inputs where
available, the existing remote-button controller and resumed-only cinematic idle. Idle retains the
clock/header and consumes the first navigation press to wake. Volume and device controls pass through.

Classic app position and Home-style previews apply to the grid. The category settings explain this,
route Arrange apps to its category editor, and omit unsupported dock/extra-grid-row effects. Category
mode currently has no equivalent of every grid long-press action or vendor tuner/source menu. Returning
Home can briefly draw the original grid before the internal category activity opens. Keep it optional
until the intended device and physical remote have been accepted.

## App names and artwork on the TV

Customize apps lists current favorites first, then other installed apps. Rename and Use app’s original
name preserve app placement and accounts. Artwork choices include the original art, a landscape app-icon
card, bundled TV banners and a local image through Android’s file picker. A local image has a preview and
an explicit Use image step; Cancel leaves the current art alone. The grid long-press menu also opens the
selected app’s editor.

Overrides are profile-scoped, capped at 64 apps and 16 KiB of metadata. Imported art is validated before
decoding, normalized to a bounded JPEG, and stored using a content hash. The source limit is 8 MiB/4K;
stored output is at most 960 × 540 and 1 MiB. The total art store is capped at 64 files/32 MiB. Home decodes
smaller thumbnails. Saved setups and recovery entries retain their asset references; no unsafe automatic
deletion is used. This is a local image picker, not an icon-pack marketplace.

## Visual wallpaper browsing

Browse wallpapers previews local pictures from the current theme. Browse themes shows imported packs
using their preview or wallpaper fallback. Browsing, moving focus and Back do not choose a different
wallpaper. Apply is explicit and guarded against a stale profile. Thumbnail work uses a single worker
and bounded decoding. The browser does not auto-play videos. Existing import, silent live playback,
wallpaper dimming and reduced-motion behavior stay in their existing controllers.

## Wallpaper playlists

Wallpaper → Wallpaper playlist rotates between up to eight imported local backgrounds. Import packs
through Manage offline themes first; each supplies a still picture and may include a silent video.
Choose wallpapers, arrange their order, then Apply. The editor defaults to 15 minutes and also offers
1, 5, 30 and 60 minutes, sequential order or shuffle without immediate repeats. Cancel discards the draft.
The feature is off by default. Turning it off returns to the profile's original single wallpaper.

Rotation changes only the background. It does not select the other pack's theme configuration, app
order, typography, row position or profile. Each profile retains its own playlist, current entry and
remaining interval. Saved setups and Recent changes include playlist choices, but not playback progress.
Referenced packs are protected from removal while a profile, setup or recovery entry still uses them.

The interval counts resumed Home time. Opening another app pauses the clock and releases the video
player; returning continues the same entry and remaining time. There is one active video player, with
a still fallback if video is disabled, reduced motion is enabled or playback is unsupported. The
playlist also works in the optional category preview. It needs no network, service or new library.
Personal videos are separate local imports and are not included in the APK or this repository.

## Recent changes

Backup & restore → Recent changes keeps up to five automatic points per launcher profile, within a
256 KiB history budget. Before a supported real edit, a pending snapshot is staged; an unchanged or
repaired failed edit does not create an entry. This history is separate from named Saved setups.
It covers allowed launcher preferences and the bounded app-order/library files. Restoring validates
values and referenced media, saves the current setup, and uses a rollback journal for preferences,
files and history. An interrupted journal is handled before ordinary product edits on startup.

This is bounded Home recovery, not a complete device or media backup. It excludes streaming accounts,
recent-viewing consent/history, helpers, system settings and media bytes. Missing/overwritten media
cannot be recovered. If storage prevents staging a checkpoint, the original edit may still proceed
without a history entry. Native normal restore has been tested; power-loss/disk-failure injection has
not been device accepted.

## Building and compatibility

From `launcher/`, build the full feature set with the existing Android SDK and JDK:

```sh
./gradlew --no-daemon -PcommunityFeatures=true testReleaseUnitTest lintRelease assembleRelease
```

On Windows use `gradlew.bat`. The application-ID override remains available for an explicitly selected
device identity; an update must use the same package and signing certificate as the installed app.
Build locally from source; no APKs or proprietary assets are published in this repository.

Without `communityFeatures`, the reference build retains its original controls and tiny no-op product
hooks. Its existing 589,824-byte gate and CI are unchanged. The full product intentionally exceeds that
reference limit with the owner’s approval. No new runtime library, manifest permission, service, network
client or polling loop was added. Timers for clock/idle run only with their Home activity resumed.

Public weather/header integration, content-provider permission design for genuine Continue Watching,
and LG webOS packaging remain separate work. Recent apps is not viewing progress, and profiles do not
switch Netflix/Google accounts. Optional AI needs a useful task and explicit provider/data consent;
there is no always-running assistant or paid inference in this build.
