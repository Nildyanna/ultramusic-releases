# UltraMusic Changelog

## v1.0.15
- Fixed a "Failed to load Python DLL" crash some people hit on launch — switched the Windows build to a more reliable packaging mode that doesn't re-extract files on every startup.

## v1.0.14
- Fixed the automatic YouTube cookie bypass for age-restricted videos — it wasn't working for most people because Chrome/Edge lock their cookie file while running. The app now retries automatically and gives clearer guidance if it still can't get cookies.

## v1.0.13
- **Auto-update** — the app checks for new versions on its own and can install them for you (Windows).
- **Import from a text file** — turn an exported playlist (Spotify, Amazon Music, etc.) into downloads.
- **Region selector** — search other countries' YouTube Music catalogs (e.g. Japan, for anime music not on the US catalog).
- Installer now cleanly removes the old version before installing a new one.
- Fixed the results list looking "stuck" after a search with fewer results than before.

## v1.0.12
- Albums can now be expanded to select individual tracks — same feature now works from a direct album search, not just an artist's full discography.

## v1.0.11
- Your download folder, cookies, and log now survive reinstalls/upgrades (moved to a proper per-user settings location instead of wherever the app happened to be launched from).

## v1.0.10
- Fixed very large albums/box sets (500+ tracks) being silently capped at ~200 tracks.

## v1.0.9
- ffmpeg setup made much simpler — just drop `ffmpeg.exe` next to the app instead of messing with Windows PATH/winget.

## v1.0.8
- ffmpeg detection no longer depends on Windows PATH working correctly — checks common install locations directly.

## v1.0.7
- Deleted/removed YouTube videos now fail instantly instead of wasting time retrying something that can never succeed.

## v1.0.6
- Distinguishes real download failures from tracks that were never actually music (bonus videos, podcast episodes, etc. mixed into an album listing).

## v1.0.5
- Album search results are now expandable to select individual tracks (previously only worked from an artist's discography view).

## v1.0.4
- Age-restricted videos no longer get wrongly treated as "rate limited" (was wasting ~48 seconds per track).
- Automatic cookie sourcing from a logged-in browser, so age-restricted/private videos mostly just work.
- Failures now show up clearly in the app (a red "⚠ N Failed" badge), not just buried in a log file.
- New: expand an artist's albums to select/deselect individual tracks before downloading.

## v1.0.3
- Fixed downloads failing on very long track/album names (Windows path length limit).
- Fixed ffmpeg sometimes not being found even when correctly installed.

## v1.0.2
- Fixed a bug where one bad album in a batch download could silently stop the whole batch.

## v1.0.1
- Clearer guidance when ffmpeg isn't installed yet.

## v1.0.0
- First public release: search and download albums/songs from YouTube Music with tags and cover art, batch-download an artist's discography, pause/resume/stop, and a redesigned interface.
