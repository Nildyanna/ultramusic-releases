# UltraMusic Changelog

## v1.0.39
- Fixed real duplicate downloads: an album already on disk in one format (e.g. MP3, from before the FLAC option existed or a change of format setting) was invisible to the "already have this" check, which only ever looked for the currently-selected format's file extension — so switching format, or just revisiting an old album, silently redownloaded every track and dropped the new copies in the same folder alongside the originals. Every track now checks for an existing copy in any supported format: an equal-or-better existing file is left alone (never downgraded, never duplicated), and downloading in a genuinely higher-quality format now replaces the old lower-quality file instead of sitting next to it. Applies to both the desktop/Linux app and the Android app.
- Android also gained the album-level "already complete, skip" fast path desktop already had — it never existed there before, so a fully-downloaded album still re-fetched its thumbnail and looped every track's existence check instead of recognizing upfront there was nothing to do.

## v1.0.38
- **Android app brought to feature parity with desktop/Linux**: FLAC option, lyrics embedding, region selector, Retag Library + new Retag Artist (with live progress and a Stop button), and manual cookie import via the system file picker. Also picked up the readable-filename fix and the cookiejar-corruption fix from earlier desktop releases, which the Android port had predated and still carried the old versions of.
- All three platforms now remember whichever of Song/Album/Artist you searched with last and reopen with that preselected, instead of always defaulting back to Album.

## v1.0.37
- Fixed a hard, unrecoverable track failure when the destination drive briefly drops out mid-write (confirmed with a real case: a USB hard drive momentarily disconnecting/reconnecting under sustained multi-track write load — Windows itself logged the disk error and recovered within about a second, but the app had already permanently failed every track that happened to be writing at that exact moment, with zero retry). Writing the finished file into the library folder now retries a few times with backoff before giving up, the same courtesy already given to network errors elsewhere in the app — a genuinely dead/still-disconnected drive still fails correctly after retries are exhausted.

## v1.0.36
- Retag Library ran with no visible progress and no way to stop it. It now shows a live count ("Retagging... 120/840 checked, 6 fixed") on the app's regular progress bar, and the Stop button cancels it cleanly.
- New **Retag Artist** button — scopes the same AlbumArtist fix to one artist's folder instead of requiring a full library rescan every time.
- Fixed illegal Windows filename characters (`: * ? " < > | /`) all collapsing to a single `_`, mangling real titles beyond recognition ("Nebraska '82: Expanded Edition" became "Nebraska '82_ Expanded Edition", "Who Believes In Angels?" became "Who Believes In Angels_"). Each character now gets a readable substitute instead (`:` → ` -`, `/`/`\`/`|` → `-`, `"` → `'`, `<`/`>` → `(`/`)`, `?`/`*` removed).
- Fixed a duplicate-download risk that fixing the above would otherwise have caused: folders already on disk under the old, more heavily-mangled naming (e.g. from before this fix, or from any other punctuation quirk) are now matched against the newly-computed name using a punctuation-insensitive comparison, so an existing album is still recognized as already downloaded instead of getting a second copy under its corrected name.

## v1.0.35
- Found the actual cause of the "removed or deleted" mass-failures v1.0.33/v1.0.34 were chasing: it's real YouTube-side rate-limiting, confirmed directly against yt-dlp itself — the exact same block shows up as a generic "Video unavailable" through one internal player client and as an explicit "rate-limited by YouTube for up to an hour" through another, for the same video, at the same time. The app was treating the vaguer message as a confirmed permanent deletion instead of a rate limit. **This release can't undo YouTube's own block** — only waiting (up to an hour, per YouTube's own message) or switching networks does that. What it fixes: a burst of 6+ "unavailable" results in a row with zero successes between them is no longer trusted individually — it's now treated as suspected throttling, backed off, and reported once clearly instead of flooding the failure list with hundreds of individually "removed" tracks that are actually just fine.

## v1.0.34
- Actually fixed the "removed or deleted" false-failure bug from v1.0.33 — that fix's own recheck was the thing silently failing. It ran concurrently with several other tracks' real downloads sharing the same connection pool, so under real load the recheck request itself kept timing out, and a failed recheck was being treated the same as a confirmed-still-unavailable one — quietly recreating the exact mass-failure bug it was supposed to fix (confirmed: a real 400+ track re-run produced zero recheck-success AND zero recheck-error log lines, meaning every single recheck was erroring out invisibly). A recheck that can't complete is no longer treated as evidence of anything — it now falls through to attempting the real download, where a genuinely unavailable track still gets caught and reported correctly by the app's existing download-failure classification.

## v1.0.33
- Fixed real songs getting reported as "removed or deleted" when they weren't. YouTube Music's album-listing API occasionally marks a track unavailable even though it plays fine (confirmed: a ~0.3% false-positive rate even under normal conditions), and that rate can spike much higher during a transient bad response — a real report showed an artist's entire 400+ track discography, including massive hits like "Rocket Man" and "Tiny Dancer," all flagged unavailable in one run, then working again on a fresh request seconds later. Every "unavailable" track now gets one cheap, independent recheck before being reported as gone — a track that's actually fine proceeds to download instead of failing; a track that's genuinely removed still fails the same as before.

## v1.0.32
- Actually fixed the "does not look like a Netscape format cookies file" corruption bug from v1.0.29 — that fix only protected successful downloads. On a track that failed (rate-limited, unavailable, network error — most of what happens during a big batch), the protection got skipped entirely, leaving every failed track free to corrupt the cookies file for everyone else running at the same time. This showed up as a real user report: downloading an artist's whole catalog produced hundreds of false "no longer available" failures, because cookies broke partway through and the rest of the run went out anonymously into YouTube's rate limiting. Verified directly: reproduced the corruption with pure failures under the old code (49 errors), reproduced zero errors under the same conditions with the fix.

## v1.0.31
- Fixed the Linux `.deb` installing but then crashing immediately on launch (`ModuleNotFoundError: No module named 'version'`). The package builder never included `version.py`, even though the app imports it directly — it was left out because the file list that builds the package and the file list that computes checksums were two separate, hand-maintained copies that had drifted apart. Merged into one list so this can't happen again.

## v1.0.30
- Fixed the Linux `.deb` failing to install entirely ("unable to create '/opt/ultramusic/ultramusic_gui.py.dpkg-new': No such file or directory"). The package builder never wrote explicit directory records into the archive, only file records — some dpkg configurations don't reliably auto-create missing parent directories from that alone. The archive now includes proper directory entries for every folder it installs into, matching what `dpkg-deb` itself produces.

## v1.0.29
- Fixed cookies randomly breaking mid-download on large batches ("does not look like a Netscape format cookies file"). yt-dlp rewrites the whole cookies file on every single track it finishes, and with 4 tracks downloading in parallel all sharing that one file, one track's write could land while another was mid-read, corrupting it. This could silently drop your signed-in session partway through a big run, making otherwise-available tracks fail. Downloads no longer trigger that rewrite — cookies still load and work exactly the same, just without the race.

## v1.0.28
- Actually fixed the "Change Folder" truncation from v1.0.27 — widening the button alone wasn't the real problem. The header row had grown too crowded (five buttons plus the folder pill competing for the same row), so the last-packed element got clipped regardless of its own width. Split the header into two rows — folder path/button on top, action buttons below — so nothing gets squeezed out again as more buttons are added.

## v1.0.27
- Fixed the "Change Folder" button text getting clipped on some systems (font/DPI-scaling dependent) — widened the button.

## v1.0.26
- **Retag Library** — new button in the header that rescans an existing music folder and fixes AlbumArtist on tracks that were downloaded before v1.0.25's tagging fix, without re-downloading anything.
- **FLAC option** — a new Format dropdown lets you download as FLAC instead of MP3 320kbps. Note this re-encodes YouTube's source stream into a lossless container; it avoids MP3's extra lossy transcoding step but doesn't exceed the source's actual quality.
- **Lyrics embedding** — new "Embed lyrics" checkbox (on by default) fetches and embeds plain lyrics into each downloaded track via lrclib.net, best-effort — a missing lyrics match never fails or slows down the download.

## v1.0.25
- Fixed AlbumArtist tagging on soundtracks/compilations with guest performers — a track by a guest artist (e.g. Kenny G on a Whitney Houston soundtrack) was getting tagged with AlbumArtist = the guest, not the album's actual credited artist. This fragmented the album into separate folders/artists in Plex and similar library software. AlbumArtist is now always the album's credited artist across every track, while Artist still correctly reflects the individual track's performer.

## v1.0.24
- Fixed the "Provide Cookies" import silently accepting the wrong file. It now checks the file is actually a valid Netscape-format cookies.txt before saving it, and tells you right away if it isn't — instead of the file being accepted and downloads failing later with a confusing "does not look like a Netscape format cookies file" error.

## v1.0.23
- Fixed "Requested format is not available" failures on some tracks — YouTube requires a JavaScript runtime to properly decode certain videos' audio streams, which the downloader wasn't using even when one (like Node.js) was already installed on your system.

## v1.0.22
- Replaced the cookie-export popup (which kept freezing the app on Windows across several attempts to fix it) with a plain "🍪 Provide Cookies" button in the header — click it anytime. If you don't have an exported cookies file yet, it walks you to the browser extension that creates one; once you do, it lets you select it directly. Same outcome as the old popup, none of the freeze risk.

## v1.0.21
- Fixed a real hard freeze in the cookie-export popup (confirmed: even "Close window" from the taskbar had no effect) caused by a Windows-specific quirk in the visibility fix from 1.0.20. Removed the risky call; the popup still comes to the front, just via a safer method.

## v1.0.20
- The cookie-export popup can no longer hang the app under any circumstance — it now gives up automatically after 5 minutes with no response, and clicking Stop closes it immediately. Also made the popup harder to miss (it was likely opening behind other windows, which looked exactly like a freeze).

## v1.0.19
- Fixed the new cookie-prompt popup being able to hang the app entirely if it failed to open correctly.

## v1.0.18
- When cookies are needed but can't be auto-extracted, the app now pauses and shows a popup with a button to get the cookie-export extension and select your exported file directly — instead of just failing and burying the reason in the log.

## v1.0.17
- Fixed misleading advice when Chrome/Edge cookies can't be auto-extracted due to their newer built-in encryption (a security feature those browsers added — closing them doesn't help, unlike the other cookie issue fixed in 1.0.14). The app now says so clearly and suggests Firefox or a manual cookie export instead.

## v1.0.16
- Fixed the installer pointing people at a changelog they couldn't access. What's new is now shown directly in the installer itself, and the full changelog is public here.

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
