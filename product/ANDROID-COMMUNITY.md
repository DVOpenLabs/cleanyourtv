# Full native Home features

The optional community build extends the existing Android launcher in this fork. It uses one APK,
package, signing path and data directory. It does not install Projectivy, a second Home or a web server.
The interaction design is informed by the [Projectivy review](PROJECTIVY-REVIEW.md); its code and artwork
are independent. See [validation](VALIDATION.md) for dated device results and remaining acceptance.

## Settings that work with a remote

Alpha 12 uses a seven-section rail: Appearance, Apps & rows, Wallpaper, Profiles, Remotes & inputs,
Backup & restore and System. Appearance brings Home experience, Layout, Text & clock, Focus & motion
and Setup help together. All sections fit the ordinary TV viewport; controls and contextual help use
the remaining width instead of a large empty page and tiny footer illustrations.

Outfit uses regular body text, medium controls and semibold headings with the existing locale fallback.
Shared palette resources, consistent outline icons and stable focus bounds define the presentation.
The initial remote focus is restored when the window gains focus. Saved selections stay quieter than
focused controls. No Home preference changes just by updating or browsing the settings.

Rows align icons, names, current values and actions. A context panel explains the selected control and
shows bounded dark/light still illustrations for visual settings. Larger font scales and short screens
prioritize readable help over illustrations. Hold OK or press Info for the complete description.
The profile name remains visible and shared controls are identified separately. Illustrations do not
open a video decoder or establish contrast for every moving wallpaper frame.

**Search settings** searches local labels, help and category paths, including aliases such as HDMI for
Inputs and font for Text sizes. Queries are limited to 120 characters and results to 24. A result opens
the owning section and focuses its control; it never executes that control or stores search history.

**Icon size** is now beside **App spacing** under **Appearance → Layout**. Preview Current, Small,
Medium or Large before Apply. Current preserves the exact existing theme dimensions; the named sizes
scale banners and icons within the safe row width. App order, apps per row, display density and
wallpaper stay unchanged. Large is the largest size allowed by the row; fewer apps per row allows
larger tiles. The choice is per profile and included in saved setups and recovery. The still preview
keeps its TV aspect ratio and never plays or replaces the wallpaper.

**App position**, **Apps per row**, **Rows below favorites**, **Favorites background** and **App
spacing** use labelled segmented choices where applicable. Left/Right changes the local draft; OK
confirms it. A check denotes the saved choice, while the brighter chip is the preview. An inherited
position is labelled as following configuration rather than falsely marking Top as saved. Position
retains its existing timed Home preview and Keep/Revert. Spacing uses the existing profile key and
rejects an intervening profile/revision change. Back discards these unsaved drafts. Existing immediate
save controls retain their previous save behavior; the page does not imply a global Apply button.

Animation/live-video switches display their on/off state without a nested focus target. Numeric,
position and spacing previews do not save on horizontal navigation. Less-used effects are inside
Advanced under Focus & motion. Unsupported controls are omitted. Up/Down clamps at each rail/list edge,
Back returns toward categories; Left does the same outside segmented controls. Back from categories
closes settings. Horizontal direction
follows the existing RTL configuration. Browsing writes no launcher preferences or history.

Text sizes offers separate clock, header and app-name previews at 85%, 100%, 110% and 120%, with all
four choices visible for each control instead of opening another picker. Apply saves
the current profile; Cancel keeps its previous sizes. Theme sizes removes the override. Existing app-name
visibility and solid light/dark/automatic theme text remain available. Automatic contrast follows theme
configuration, not every changing video frame; test busy wallpapers from the seating position.

**Arrange apps** now opens a native favourite-order draft from Clean, Glass and Cinema. Move up/down
buttons work with the remote; Cancel discards the draft and Apply saves only the current profile. The
editor preserves labels, hidden apps, artwork and media. It uses the existing bounded layout store,
checks the profile/preferences/native order stamp and edit lock again before saving, invalidates stale
Undo and notifies layout observers. Classic and category layouts retain their existing editors.
The three new styles now read the named focus style correctly, including Strong black and white.

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

## Home experiences (alpha 8)

Settings → Appearance → Home experience previews three native presentations of the same Home state:

- **Freedom Glass:** compact header and a floating dock.
- **Freedom Clean:** an open app row with solid, readable names.
- **Freedom Cinema:** a quieter app ribbon and a title for the focused app.

The existing grid/category layout remains the default. Preview is process-local and returns to Settings
after 20 seconds unless Keep is chosen. Revert, Back or leaving a preview discards it. The preview bar
allows D-pad navigation through the real apps; Up returns to Revert/Keep. Keep saves only the profile's
experience choice and change metadata, preserving its app ordering, media and other appearance choices.
Settings, profile management and app customization wait until the trial is kept or reverted; trying a
look does not grant its edit surfaces access to the saved Home. App launches remain available.
The selection also participates in saved setups and automatic recovery.

An explicit Top/Middle/Bottom choice is shared between experiences. Without one, Clean starts at the top
and Glass/Cinema at the bottom; this does not write a new position. Apps per row and extra spacing remain
available. Controls belonging only to the legacy grid are omitted while a new experience is selected.
Switching to Current layout restores access to those controls and its saved appearance.

Names use solid ink, never outlined letters. Automatic ink uses a still poster's header/app region and
supporting shading; it does not alternate colors on every video frame. Manual light/dark choices remain
available. Focus surrounds the selected card. Settings includes a bounded still illustration, not a second
wallpaper video. The optional playlist uses the same resumed-Home timer and one active decoder.

Inputs retains the existing passthrough/HDMI filtering and device-specific fallback. Apps opens local
label/package search over the shared visible catalog; it does not query viewing history or an online
content service. The internal experience screen is not another installed launcher or Home helper.

`product/experience-preview/` contains a desktop design review. Its weather, app launches and system
actions are simulated; its optional `/media/` routes must be supplied by a local development server.
Private artwork and videos are deliberately absent from this repository. The existing owner pilot's
weather bridge is separate from the public product's still-pending weather-header packaging.

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
reference limit with the owner’s approval. Alpha 8 added no runtime library, manifest permission, service, network
client or polling loop. Alpha 9 adds only the optional full-build TV-listings permission described below. Timers for clock/idle run only with their Home activity resumed.

Public weather/header packaging and LG hardware acceptance remain separate work. The alpha-9
content-provider boundary is described below. Recent apps is not viewing progress, and profiles do not
switch Netflix/Google accounts. Optional AI needs a useful task and explicit provider/data consent;
there is no always-running assistant or paid inference in this build.


## Guided setup, profiles and content (alpha 9)

**Setup help** offers four optional steps: look, apps, comfort and review. Back/Cancel never saves;
Apply defaults to a new local profile. Imported AI drafts use the same allowlist and exact app inventory,
are staged rather than applied by the desktop bridge, and reject stale Home/profile revisions. Nothing
is sent to an AI service by the launcher. See [AI setup help](AI-SETUP.md).

**Profiles** now has a local avatar chooser, a separate switcher and an optional edit PIN. The PIN is
salted PBKDF2, profile-scoped and excluded from saved setups, history, profile exports and AI templates.
Five failed attempts impose a thirty-second retry delay; a successful unlock lasts five minutes.
This is a convenience edit lock, **not** a streaming-account/content restriction or Android security
boundary. Other profiles and TV system settings remain accessible. Keep your PIN yourself; Android's
app-data controls can reset it. No owner PIN is configured by an upgrade.

**Scene schedules** live under Wallpaper and start off. Create up to eight non-overlapping local-time
windows from existing playlist wallpapers. Midnight windows belong to their start day. A schedule
changes only the displayed background while Home is visible; it neither wakes the TV nor switches
profiles, apps or theme choices. Normal rotation pauses during a scene and resumes afterward. Removing
a scheduled wallpaper from the playlist is rejected until its windows are removed. No new scheduler,
alarm, background service or second video player is installed.

**Content shelves** starts off in every new profile and is excluded from copied/exported setups.
The full build declares Android's dangerous `READ_TV_LISTINGS` permission, requested only through
its explicit content settings. It does not request usage access, all-EPG privileged access or account
permissions. After granting access, choose Show shelf separately for that profile. Disabling the shelf
does not silently revoke Android's app-wide permission; revoke it in Android App permissions if desired.
The reference manifest remains unchanged and has no TV-listings permission.

The shelf reads searchable [Watch Next metadata](https://developer.android.com/reference/android/media/tv/TvContract.WatchNextPrograms)
once per foreground session, with at most 96 rows inspected and 12 cards retained in memory. It filters
hidden/unavailable apps, old/future entries, invalid links and nearly-complete Continue items when
progress is available. Types remain labelled Continue / Up next / New episode / Watchlist; no progress
is invented. A deep link is rebuilt without foreign components, grants, selectors or arbitrary extras
and must resolve inside the originating app. Failures are reported without opening an unrelated app.

Glass/Clean/Cinema show a row in free space outside favorites; the existing grid retains its geometry
and offers Browse shared items from content settings. Empty/unavailable shelves collapse. The first
version uses local app artwork and provider titles, not downloaded posters. It stores no watch history,
sends no titles off-device and cancels work/clears cards when hidden. Actual coverage depends on each
app/provider and firmware; there is no promise of Netflix or any other service exposing its items.

Settings omits controls belonging only to the old grid when a new experience is active, retains current
values and focus memory, and gives modal controls a consistent high-contrast style. The still settings
illustration does not open another video decoder.
