# UltraMusic Changelog

## v2.3.0
- **Library Doctor: Duplicate albums now merges editions instead of leaving them alone.** Two copies of an album that each had tracks the other lacked (a standard album and its Expanded or Collector's Edition, for example The Marshall Mathers LP 2, Night Visions and The Miracle) used to be left alone because tracks were compared by number, and editions number their tracks differently (one even uses CD 01 / CD 02 folders). Tracks are now matched by title (case, punctuation and "(feat. ...)" ignored). The copy with more of the album stays; a track only the other copy has is moved in (next free number if its own is taken); a track that is in a better format in the other copy (FLAC over MP3) replaces the worse file, which goes to quarantine; then the emptied folder goes to quarantine too. The plan is shown before anything moves, nothing is deleted, and **Undo last tidy** puts every file back. Desktop and Android.
- **Library Doctor window (desktop):** the report now word-wraps instead of running off the edge, and the buttons are on two rows so they are no longer squeezed together.

## v2.2.2
- **Two spellings of one artist folder are now one artist (AC-DC / AC_DC).** An older version of the app saved a "/" in an artist's name as "_", so a library can hold both `AC-DC` and `AC_DC`. The scan only looked inside the folder it was scanning, so `AC-DC` (which held just `Live`) was reported as missing 21 albums that were sitting in `AC_DC`. Now an artist folder is searched together with its other spelling (folders whose names are equal once punctuation is ignored), both in Library Scan and New releases and when downloading, so albums found in either are counted as owned and missing tracks are filled in where the album already is. A release found for both spellings is listed once. Nothing is moved or renamed; new albums still go to the folder named the current way. Desktop and Android.

## v2.2.1
- **Library Doctor: Tidy now reads vinyl-style names.** Files named like `a1-elvis_presley-heartbreak_hotel_(take_5).flac` ... `b7-...` (a side letter and a track number) used to be left alone as having no track number. When every file in a folder has a side letter and number and none repeats, Tidy now numbers them in order (side A first, side B carrying on after it), removes the artist prefix, turns underscores into spaces and capitalises the words: `01 - Heartbreak Hotel (Take 5).flac`. A folder where only some files fit the pattern is still left alone. Like every tidy it can be undone. Desktop and Android.

## v2.2.0
- **Carry on after a restart.** A download's queue is saved as it runs, so a reboot, a crash or a closed window no longer loses it: the next start asks "Carry on with your last download?" and picks up at the next album (albums already downloaded are skipped; answering No forgets the queue). Stopping a batch with Stop, a limit or a full disk keeps what was left, and finishing one clears it. A single imported song is not carried over (resuming it without its exclusions would download its whole album). Desktop and Android.
- **New: 🆕 New releases.** Compares every artist in your library with what the last check saw of their YouTube Music discography and lists only what has appeared since, in the same result screen as Library Scan (tick, download, mark owned). It is quick because it asks only for each artist's release list (no track counts except for the few new ones) and never searches for an artist or asks a question: an artist the app does not know yet is skipped until one Library Scan has found it. The first check of an artist only records what is there. A new edition of an album you already have, an album already in your library, and artists or albums you unticked are not listed. Optional **Check for new releases weekly** (off by default) runs the check quietly when the app opens and only speaks up if something is new. Desktop and Android.
- **Library Doctor: Duplicate albums.** Finds an album that exists twice for one artist (same title once the year and edition words are ignored, for example "LOOM" and "LOOM (2024)") and keeps the better copy: the one with more of the album, then the better format (FLAC, Opus or M4A over MP3), then the bigger files. The other folder moves to the quarantine folder, and only when the kept copy has every one of its tracks; if each copy has tracks the other lacks, the pair is left alone and listed so you can merge by hand. Nothing is deleted, you see the plan and confirm, and **Undo last tidy** puts the folder back. Desktop and Android.
- **↻ Retry failed.** After a batch with failed tracks, one button runs those albums again. Tracks already on disk are skipped, so only the failed ones are fetched (tracks YouTube reports as removed are still skipped, as in any run). Desktop (header button, shows the count) and Android (button at the top of the list).
- **New: 🔗 Playlist link.** Paste a YouTube or YouTube Music playlist link and its songs are listed like an imported song list: tick the ones you want and download them (each song on its own, not its whole album). A video with no album is looked up by search so it can be downloaded as a song of its album; what cannot be used is listed in the log. Public and unlisted playlists work; private ones and personal mixes cannot be read, and the app says so. Desktop and Android.

## v2.1.2
- **A folder that is another release of the same artist is no longer claimed by the release with the shorter name.** The scan matched "Hail to the Thief" (the studio album, 14 tracks) to your folder "Hail to the Thief (Live Recordings 2003-2009)" (12 tracks) only because the studio title is the start of the folder name, then skipped it as "a different edition". Now, when the artist has another release whose title is exactly that folder name, the folder belongs to that release; the studio album is listed as missing and downloads into its own "Hail To the Thief" folder next to the live one, and the live folder is left alone. A hand-kept edition that is NOT another release (for example "Abbey Road Anniversary Edition" for "Abbey Road") is still recognised as before. This applies to a download that follows a Library Scan (the scan is what knows the artist's other releases); a single album downloaded by hand behaves as before. Desktop and Android.
- **Correction:** the v2.0.0 notes said an emulator tests each release build. It did not work yet (the release APK has only ARM libraries and the CI emulator is x86_64). A weekly job now builds a separate x86_64 copy of the app just for that test; the shipped APK is unchanged.

## v2.1.1
- **Library Scan is now much faster, and "use saved results" finally makes a difference.** Measured on a 1,300-album library on Google Drive: a scan that used the saved results still took 194 seconds, because almost all of it (168 s) was spent reading the library folders: it made about 93,000 separate file-status requests (roughly 2 ms each on a cloud drive) and listed each artist's folder again for every one of its albums. Folders are now listed once and files are counted without any extra request per file. The same scan takes 34 seconds the first time after updating and **7 seconds** after that, with identical results (the 34 s is YouTube searches for each artist's name, which are now remembered for a week too). Desktop and Android; downloading and tidying get the same faster folder counting.

## v2.1.0
- **New: "Accept the closest version" (off by default; desktop and Android).** Some compilations (Queen Forever, the Beatles Anthologies, R.E.M. Complete Rarities) list their tracks only as music videos, whose length differs from the album's listed length, while the audio-only studio version lives on the original album a few seconds longer or shorter. Until now those tracks were reported as "wrong length" and left out. With this option on, when no exact-length version exists the app uses the closest audio-only version of the same song: same title, same artist, not live or remixed, within 12% (at least 5 seconds) of the wanted length, checked against its own length so a truncated download is still refused. The log says which version was used and how its length differs (for example "3:33 instead of 3:15"). Off, nothing changes.
- **Expired cookie files are now recognised.** YouTube rotates the cookies of the browser session an exported file came from, often within hours, so a file saved a few hours ago can be dead even though it lists expiry dates a year away. The app used to report age-restricted tracks as "blocked even with your cookies", and after two runs skipped them for good. It now detects YouTube's "cookies are no longer valid" message, says plainly that the cookie file has expired and how to export a fresh one (private/incognito window, then close it), never remembers those tracks as blocked, and on desktop switches to your browser's live cookies when it found some.

## v2.0.2
- **Library Doctor no longer counts long paths as a problem.** The health check used to list every path over 240 characters among the problems, although the app cannot fix them (shortening a name would make it look like the track is missing and download it again) and they work in Plex, on cloud drives and in this app. Now only paths over Windows' real limit (259 characters) are mentioned, in a separate "FYI, not counted" note, and a clean library shows 0 problems. Desktop and Android.

## v2.0.1
- **Library Doctor: Tidy no longer says "every file is already named NN - Title" when the health check has just listed files that are not.** Files with no track number it can read (for example scene-style names like `a1-elvis_presley-heartbreak_hotel_(take_5).flac`) are left alone, and Tidy now says exactly that: how many files in how many folders, an example, and that they need renaming by hand (the desktop window also lists them). The health check section is now titled "FOLDERS WITH FILES NOT NAMED ..." because its number counts folders, not files. Desktop and Android.

## v2.0.0
- **Android: your music now goes to the phone's own Music folder.** With Android's "All files access" (a one-time switch the app asks about; **🩺 Doctor → Storage…** shows the state) downloads are saved to `Music/UltraMusic`: Plex, music players and file managers can see them, and they survive an uninstall. Without the permission nothing changes (the app-private folder). Once you allow it, the app offers to move the music already in the private folder, never overwriting a file. Settings, lists and caches stay app-private. This changes where files are saved, hence the new major version.
- **Android: tested on an emulator.** After each release build an emulator installs the APK, grants the permission, launches the app and checks that it starts without errors and picks the shared folder (the parts that only exist on a phone: native libraries, the permission switch, scoped storage).
- **Safer and easier to keep up to date.** GitHub Actions are pinned to exact commits, Dependabot watches the Python pins and the Actions weekly (the repeated yt-dlp / ytmusicapi pins follow from `requirements.txt` automatically), and a failing weekly check now opens an issue.
- **More tested.** The download engine both apps share (retries, rate limits, cookies, blocked and unavailable tracks, the wrong-length search) went from about half covered to 94%, Library Scan and the phone code gained tests too, and CI now fails if the shared code's coverage drops below 88%.
- **Smaller files.** The desktop window code is split by screen (`gui_scan.py`, `gui_doctor.py`, `gui_lists.py`, `gui_import.py`, `gui_updates.py`, `gui_theme.py`) and so is the Android screens file (`screens_*.py`, `widgets.py`), with the existing screen tests unchanged.

## v1.0.79
- **Android: the permanent signing key is live.** This is the first APK signed with the app's own permanent key (fingerprint `848bb2a2...a52e`), and the build now refuses to publish an APK signed with any other key. From the release after this one, a phone updates the app in place and keeps its music, settings and lists. **Moving from an earlier APK to this one needs one uninstall first**, because earlier builds were signed with throwaway keys; uninstalling deletes the app's own music folder and settings, so copy anything you want to keep out of the app's folder before you do.

## v1.0.78
- **Your lists can no longer be wiped by a crash.** The owned list, skipped list, unticked list, saved artist matches and settings are written safely now (to a temporary file, flushed to disk, then renamed over the old one, with the previous good version kept as a `.bak`). If a file is found damaged anyway it is moved aside, the last good copy is restored, and the log says so, instead of silently starting from an empty list that the next save would write over the real one. Desktop and Android.
- **Library Scan is much faster the second time.** Each artist's album list and track counts are saved for a week. Scanning again asks whether to use them (fast, and far fewer requests to YouTube, which helps with rate limits) or to ask YouTube again; your own folders are always read fresh, so what you have is always up to date. Desktop and Android.
- **Copy diagnostics** (in the log window on desktop, in the Log screen on Android): copies versions, settings, list sizes and the recent log to the clipboard for a bug report, with your library folder, data folder and user name replaced by placeholders. No cookies or tokens are ever included. Log lines in the log file now carry the date, and the file rotates at 2 MB instead of growing forever.
- **Android: a permanent signing key.** Every Android build used to be signed with a different throwaway key, so a phone could not update the app in place (and uninstalling deletes its music and settings). The build now re-signs with one permanent key kept in the repository secrets, refuses to publish an APK signed with any other key, and `tools/make_android_keystore.ps1` creates the key once. Until that one-time setup is done the APK is still signed with a throwaway key.
- Behind the scenes: the desktop screens (Library Scan, the unticked list, song import, Library Doctor, diagnostics) are now tested automatically on a virtual display in CI, next to the Android screens; a test also catches stray control characters that a backslash escape once put into scripts and the README.

## v1.0.77
- **New: 🩺 Library Doctor (Windows, Linux and Android).** A read-only health check of your library (failed writes, leftover temp files, empty or artwork-only folders, gaps and duplicates in track numbers, a track kept as both MP3 and FLAC, oddly named and stray files, over-long paths). It only reads folder listings and file sizes. **Tidy** renames files to `NN - Title` (including `Artist - Album - 01 - Title` and `01. Title`) and moves stray rip files (`.m3u .sfv .nfo .cue .log .txt`) and artwork-only folders to a quarantine folder next to the library. Nothing is deleted or overwritten, every change is journaled to its own undo file, and **Undo last tidy** puts everything back. You can save the report as a text file.
- **Android now retries a wrong-length download from another version**, like the desktop app already did (music-video cuts, the same recording in another release or region). The yt-dlp download, rate-limit and throttle handling, cookies and wrong-length search are now one shared engine used by both apps instead of two copies that had drifted apart.
- **Android builds are more robust:** native-library sources (freetype, x264) are pre-fetched from mirrors, the build retries once, and the release script re-runs a failed Android build once before giving up. Behind the scenes: tests now fail instead of silently skipping, a lint check and a headless run of the Android screens are part of CI, and the one-off library and Plex helper scripts moved to a `tools` folder.

## v1.0.76
- **A throttled YouTube no longer gets real tracks skipped for good.** YouTube answers rate-limited requests with "Video unavailable", the same message it gives for a removed track. A track that failed that way on two runs was marked "removed" and skipped from then on, so two throttled runs could wipe out playable tracks (confirmed with a real one from an R.E.M. run). Now an "unavailable" result does not count toward that skip while the app suspects a throttle (it announced one in the last 10 minutes, or 4 came back within a minute with no success between). The log and the batch summary say "unavailable while YouTube was throttling — try again later" instead of "removed", and nothing is added to the Skipped list. Tracks that are really gone are still learned in calm stretches. On desktop and Android alike.
- If an earlier run already marked playable tracks as removed, use 🚫 Skipped Tracks → Reset all (retry everything); only the ones that are really gone will fail again.

## v1.0.75
- **Android: untick single tracks inside a release, and import a song list.** Open a release with the ▸ button and untick the tracks you do not want; they are left out of that release's download. **📄 Import list** reads a .txt (one song per line, `#` for comments), matches each line on YouTube Music, shows what it found and what it could not, and downloads just the songs you leave ticked (not their whole albums). Both already existed on the desktop; the Android app now has everything the desktop has except auto-update and browser-cookie detection, which a phone cannot do.
- The song-list reading, matching and "this song only" logic is now shared code used by both apps; a list that starts with a byte-order mark no longer leaves it stuck to the first song.

## v1.0.74
- **The Android app now matches the desktop app (and will from now on).** New on the phone: **Library Scan** (compares your music folder with each artist's YouTube Music albums, asks which artist a oddly named folder is, and lists what is missing); **Download selected** for an artist's releases or scan results; **Original (no re-encode)** and the honestly named FLAC/MP3 formats; **Stop after N albums / N GB** and **Pause per artist**, plus automatic stops; **↺ Reset unticked** (artists and albums you untick in a scan are remembered and left out of later scans until you reset); an **Owned albums** list you can edit and import into; **Retry cookie-blocked only** in Skipped; and a low-storage warning. Android and desktop now run the same shared code for the scan, the artist album list and the batch controls.
- The Windows app uses that shared code too; behaviour is unchanged (checked against a 19-scenario transcript).
- `release.ps1` now runs the unit tests in the dev environment (all 310 run; before, a fifth silently skipped on the bare system Python), and the README's release instructions had a corrupted script name that is fixed.

## v1.0.73
- **Library Scan remembers what you unticked.** Untick an artist, or a single album, in the scan results and later scans no longer check or list it, so you do not have to untick it again every time. The status line says how many were left out, and the new **↺ Reset unticked** button (with a count) brings them all back for a complete scan. Ticking an item again restores it, and nothing is hidden from direct downloads or from the Skipped / Owned list.

## v1.0.72
- **New format: "Original (no re-encode)".** YouTube only serves lossy audio (Opus at about 134 kbps, or AAC). Original keeps that stream exactly as YouTube sent it (`.opus`, or `.m4a` when only AAC exists), fully tagged with cover art: nothing is re-encoded, and the files are about 12 times smaller than FLAC (measured on one real track: 4.7 MB against 59.1 MB). It only fills what is missing; it never replaces an MP3 and a FLAC is never swapped for it. Plex reads both containers.
- **FLAC and MP3 are now named for what they are.** The old "FLAC (lossless container)" suggested lossless audio; since the source is lossy, FLAC is a conversion of it, not an upgrade (the "24-bit" is padding). The menu now says "FLAC (re-encoded, big)" and "MP3 320 kbps (re-encoded)", and choosing one logs a one-line explanation.
- **Every download now carries a `SOURCE` tag** (for example `YouTube Opus 135 kbps 48 kHz`), so a converted file can always be told from a genuinely lossless one later. Applies to Original, FLAC and MP3, on desktop and Android.
- **Failures in plain words.** Errors are classified (network, rate limit, sign-in/age, unavailable, a YouTube page this version can't read, a file or folder that changed under the app, drive full) and shown as one short sentence instead of a long exception, and a batch ends with a "By reason" count (for example `3 x marked removed or region-blocked; 2 x wrong length`). A folder renamed or moved by another program mid-run is reported as exactly that and the run carries on.
- **Android runs the same album pipeline as the desktop app.** The two copies had drifted; it is now one module used by both. The phone gains the desktop's handling of greyed-out tracks and podcast episodes, `cover.jpg`, per-track failure reasons and the clearer log lines, and says "wasn't downloaded" instead of "all tracks downloaded" when an album was skipped.
- A download rejected for any reason other than length (an empty file, a bad header) is no longer reported as "wrong length".
- Under the hood: dependencies are pinned (`requirements.txt`, `requirements-dev.txt`, and yt-dlp/ytmusicapi in the Android build); a weekly job tries the newest yt-dlp and ytmusicapi against a real download, conversion and validation, so a YouTube-side change is noticed before a release; the album pipeline has its own 25 tests that need no display, plus format, error and packaging tests (285 in all); `finish_release.ps1` attaches the APK and syncs the changelog.

## v1.0.71
- **Albums whose declared track count is higher than what YouTube Music actually lists no longer stay incomplete forever.** Eagles' Legacy declares 116 tracks but lists 113 (and BULLY declares 18 but lists 17), so a complete folder was reported as short on every scan. Scan Library now compares your folder with the tracks the album really lists. Very large albums (200+ tracks) are still compared with the declared count.

## v1.0.70
- **Every album now logs its result.** After an album is processed the log says how many of its tracks are on disk and which tracks YouTube Music lists with no video (greyed out), instead of staying silent when an album is already complete. Use it to see why an album is a track or two short.

## v1.0.69
- **Albums that are one or a few tracks short no longer stay on the list forever.** YouTube Music lists some tracks with no video at all (greyed out), and the downloader skipped them without a word, so the album stayed incomplete on every scan (for example Metallica's Load and ReLoad box sets, The Complete Stevie Wonder, Elton John's Duets). The log now says `⏭ '<track>' has no playable video on YouTube Music (greyed out) — nothing to download.`, and an album missing only such tracks counts as complete from the next scan after you download it once.

- **An album filed under a different artist is no longer reported as missing under the first one.** Downloads such as Biggie's "Unsolved" (credited to Biggie and 2Pac, filed under 2Pac) were listed as missing in the Notorious B.I.G folders on every scan, because the scan only looked inside each artist's own folder. The app now remembers where each album was downloaded; download an affected album once more (it is skipped as complete) and the scan finds it.

- **Two library folders matched to the same artist are flagged.** If two folders use the same saved YouTube Music artist (for example Bruce Springsteen's E Street Band and Sessions Band folders, which then list the same releases), the scan log now warns and names the file to edit to fix the wrong one.

## v1.0.68
- **Scan Library says why an album is listed as incomplete.** Under each artist it now logs, per album, how many tracks you have against YouTube Music's count, which folder it matched, and how many tracks are known-unavailable (up to 12 albums per artist). Use it to see why an artist with only a few missing tracks never clears.

## v1.0.67
- **A track with no video of the right length is no longer reported on every run.** When the only video that exists is a different length than the track (for example P!NK "There You Go" on Greatest Hits, listed at 3:30 but only available as a 3:51 music video) and no other version can be found, it used to fail on every batch. After two runs with the same result it is now skipped like other unavailable tracks, albums missing only such tracks count as complete, and it appears in 🚫 Skipped Tracks as "only available at the wrong length" where you can retry it. Changing your cookies does not release it.
- **A library folder with no findable artist is now matched through its own albums.** YouTube Music's search returns nothing at all for a name made only of symbols such as `¥$` (Kanye West / Ty Dolla $ign), so the folder was reported as "no similar artist". When the artist search finds nothing, the app now searches for up to three of the albums in the folder and uses the artist credited on the result whose name matches the folder (the artist's own page, or failing that the albums found for them, are then used for the scan).

## v1.0.66
- **Artists whose name is only symbols are found.** The Kanye West / Ty Dolla $ign duo `¥$` was reported as "no similar artist on YouTube Music" because every character of its name counts as punctuation, which the matching threw away. Artist names with no letters or digits are now compared by their symbols.
- **The log says why no right version was found.** When a wrong-length track can't be fixed, a `🔎` line lists what was tried: whether a counterpart video was listed, whether the search matched, and the length of each candidate that was downloaded and rejected.
- **The log says what the search returned** when a library folder has no matching artist.

## v1.0.65
- **Tracks skipped because of bad cookies are released automatically.** A track marked "blocked even with your cookies" is a verdict about those cookies, not about the track. The app now remembers which cookies each verdict was made with; when your cookies change (a new export, or after you import a file with 🍪 Provide Cookies) every such track is tried again, with a log line saying how many. Tracks that were removed or region-blocked stay skipped. Verdicts recorded by older versions count as stale, so the first run after updating retries them all once.
- **New "Retry only cookie-blocked tracks" button** in 🚫 Skipped Tracks, for doing it by hand without clearing the removed/region-blocked ones.
- **Removed or region-blocked tracks also look in the current region.** Availability can differ per release, so the same recording on another album may be playable here. Matching is as strict as before (same title, version, artist and length).
- **Artists whose page can't be read are found by search.** Jon Bon Jovi (and similar artists served as plain channels) failed with a `twoColumnBrowseResultsRenderer` error in Scan Library; their albums are now found by searching the artist's name. Other errors, such as rate limits, are still reported as before.
- **Fewer double downloads.** For tracks whose video isn't the plain audio track (music videos and other uploads), the app checks the video's length before downloading and goes straight to the right version, instead of downloading the wrong one first. The retry log line now shows the video type, so the next log shows which tracks needed it.

## v1.0.64
- **"Please sign in" is now recognised as a sign-in problem.** It used to be retried three times and reported as a generic failure; it now fails fast, is remembered like other blocked tracks, and counts toward the new end-of-batch hint (also on Android).
- **Wrong-length downloads get a second chance.** When a download comes back a different length than the track (typically the music-video cut or another version), the app now looks for the audio-only counterpart and a matching recording in the catalogs, and uses one only if it passes the same length check. A track with no right version is still rejected, and nothing wrong is saved.
- **Fixed `cover.jpg` write errors (and false track failures).** Several tracks of one album wrote the same `cover.jpg.part` at once, giving "Permission denied" / "Invalid argument" on Google Drive, and a cover failure could mark a good track as failed. Cover art is now written once per album, and a cover that can't be written is only a warning.
- **The failed-track summary now says why.** Each failed track in "Batch finished with N failed track(s)" carries its reason (needs an age-verified account, wrong length, no longer available, and so on). If any tracks were age-restricted or sign-in only, the log ends with how to fix the account and cookies.

## v1.0.63
- **Fixed good files being thrown away as "the wrong size" on Google Drive.** Cloud-synced virtual drives (Google Drive for desktop) can report a stale, smaller size for a moment after a write, so some perfectly good copies were rejected and deleted (and retried, and failed again) depending on timing. The copy is now flushed to disk and a size mismatch is re-checked for up to 20 seconds before it counts; a file that really is short is still rejected, and the message now says how many bytes arrived.
- **Scan Library now asks where to download.** Downloads go to the saved Download Folder, not to the folder you scanned, so scanning one library while a stale setting pointed at another quietly filled the wrong place. If the scanned folder differs, you are asked whether to use it as the Download Folder.

## v1.0.62
- **The Windows build now bundles yt-dlp's YouTube solver components** (`yt-dlp[default]`, including `yt-dlp-ejs`), which formats on some videos depend on. Includes everything from v1.0.61.

## v1.0.61
- **A library drive that rejects writes now stops the run.** If 5 files in a row can't be written to the library folder (a cloud-synced folder such as Google Drive over its upload quota, or a drive that is full, offline or failing), the run stops once with a clear message instead of retrying every remaining track. Nothing corrupt is kept. Run Scan Library again once the drive is fixed or the quota has reset; only the missing tracks are fetched.

## v1.0.60
- **Fixed the Windows installer crashing on start** in the v1.0.58 and v1.0.59 builds (missing modules). Includes everything from v1.0.58.

## v1.0.59
- **Fixed the Windows installer crashing on start** (`No module named 'tkinter.font'`) in the short-lived v1.0.58 build. Includes everything from v1.0.58.

## v1.0.58
- **Blocked tracks no longer stop the run.** Tracks blocked even with your cookies (e.g. explicit tracks the signed-in account can't access) are skipped and reported as before; after 5 in a row you get one warning instead of the run stopping. Tip: make sure the YouTube account behind your cookies is age-verified.

## v1.0.57
- **Fixed albums claiming the wrong folder.** The app accepted any folder whose name *starts with* the album title and took whichever the operating system listed first, so `Red` could claim `Red (Taylor's Version)` even with a real `Red` folder beside it (and the reverse in scans and upgrades). An exact match (also ignoring a trailing year like `(2012)` or old punctuation) now always wins; the "starts with" match is only a fallback and picks the closest name.

## v1.0.56
- **A failed or invalid download can no longer cost you a file.** Every download is checked before it may replace anything: not empty, the right file signature (FLAC/MP3), parses, its length is within a few seconds of the length YouTube Music reports, and (desktop) it decodes end to end with ffmpeg. Files are written to a temporary `.part` name, size-checked and renamed into place, so a failed copy (full disk, Drive hiccup) never leaves a zero-byte or truncated file and never touches an existing one. The old MP3 is deleted only after the new FLAC is validated, fully written and verified at its final name.
- **Zero-byte files never count as "already have it"**, and are deleted automatically (with leftover `.part` files) when an album is processed. Fixes the empty FLACs left behind by the out-of-space errors.
- **Different editions are left alone.** If a folder's numbered files don't match YouTube Music's release by number *and* title (or are too oddly named to match), the album is skipped untouched and reported ("looks like a different edition") instead of having tracks replaced or added. Missing or upgradeable tracks are only fetched when every file on disk matches.
- **Controlled batches.** New settings next to Keep existing files: **Stop after N albums / N GB** (stops cleanly between albums; the rest stay queued) and **Pause after each artist** (press Resume for the next). A run now also stops itself after 5 tracks in a row are blocked even with your cookies.
- **Disc subfolders** named `Digital Media NN`, `Vinyl NN` and `12 Vinyl NN` now count like `CD NN`/`Disc NN`.
- **Bulk "Mark owned" import:** Skipped / Owned → Owned albums → **Import list…** reads a text file, one per line: `Artist - Album` (or `Artist | Album`, or an `MPREb_…` album ID).
- Symbol-only track titles (`$`, `★`) are now matched correctly.

## v1.0.55
- Fixed the "What's New" list shown in the installer: versions 1.0.50–1.0.54 were listed out of order. No app changes.

## v1.0.54
- **"Keep existing files (no MP3→FLAC upgrades)" is now OFF by default** (v1.0.53 shipped it on). With it off, downloading as FLAC upgrades an existing MP3 to FLAC and removes the MP3, as in earlier versions; tick it to leave existing files alone. The setting is unchanged if you already chose one. Mark owned is the way to protect individual albums from refilling or upgrading.

## v1.0.53
- **Owned albums.** New **Mark owned** button on every album row (and **Mark all owned** on a Scan Library artist). An owned album is checked before anything on disk is counted, so it is never downloaded, re-downloaded, refilled or listed by Scan Library — trimmed albums and albums you deleted on purpose stay exactly as you left them. It is keyed by the YouTube Music album ID, and also recognised by artist + title if the same album turns up under another ID (another region's catalog). Reversible: **🚫 Skipped / Owned → Owned albums** lists them with Remove / Clear all. Desktop and Android (Owned button on album rows; Clear owned in the 🚫 Skipped popup).
- **Existing files are no longer upgraded by default.** New option **Keep existing files (no MP3→FLAC upgrades)**, ON by default: a track that already exists in any format is left alone, so downloading as FLAC no longer replaces your MP3s with FLACs and deletes the MP3s (this is what rewrote a hand-trimmed Eminem album). Missing tracks are still filled in. Untick it to get the old upgrade behaviour. Desktop and Android.
- **Multi-disc albums kept in `CD 1` / `Disc 2` subfolders** now count toward an album being complete (previously only files directly in the album folder counted, so such albums looked incomplete and were refilled).

## v1.0.52
- **Tracks blocked even with your cookies now go on the skipped list too.** If a download is refused (age-restricted, private, members-only) while a cookie file or detected browser session is in use, the track counts like a removed one: after two separate runs it is skipped from then on, shown on the 🚫 Skipped Tracks list with its reason. Without cookies nothing is remembered, since a cookie file could fix it. After adding a new cookie file, use Reset all on the Skipped Tracks list to try them again. Desktop and Android.

## v1.0.51
- **Low-disk-space warning.** Before a download starts, the app checks the library drive and, if under 2 GB is free, asks whether to start anyway (a cloud-drive folder can report inaccurate free space, so it is only a warning). The run still stops by itself if a write actually fails.
- **Tracks that keep failing are remembered and skipped.** A track that fails as removed/region-blocked on two separate runs is marked unavailable and skipped from then on, so scans and downloads stop retrying it (rate limits, full disks and network errors are never counted). An album missing only such tracks no longer shows up as incomplete in Scan Library. A track that later downloads fine is forgotten. Reversible: the new **🚫 Skipped Tracks** button lists them, lets you retry one, or **Reset all (retry everything)** to do a complete scan again. Desktop and Android (🚫 Skipped button).

## v1.0.50
- **A full drive now stops the download run instead of failing every remaining track.** When the library drive (or the temp folder) ran out of space, each track retried the write four times, failed, and the run carried on to the next one — a Scan Library run could grind through hundreds of tracks like that before anyone noticed. A "No space left on device" error now stops everything at once with one clear message (💾 Out of disk space), removes the half-written file so it can't pass for a finished track, and leaves the albums in the list so you can download them again after freeing space — only the missing tracks are fetched. Applies to desktop/Linux and Android.

## v1.0.49
- Fixed the Scan Library progress bar jumping back to 50% over and over: loading each artist's track counts was resetting the shared progress bar. The bar now advances steadily with the number of folders scanned.

## v1.0.48
- Fixed the update prompt showing the new version with a doubled "v" ("vv1.0.47").

## v1.0.47
- **Scan Library now offers near-matches for folders it could not match exactly.** When a library folder has no artist with exactly the same name on YouTube Music (a different spelling, "The" prefix, punctuation, a typo), the scan asks before skipping it: a window lists each such folder with the closest artists and their similarity, pre-selecting a candidate only when it is a very close match (80%+) and otherwise defaulting to Skip. Nothing is applied until you press Continue scan. Your answers are remembered (artist_matches.json in the app data folder), so each folder is only asked about once. Folders with nothing similar are listed in the log as before.

## v1.0.46
- **Fixed Scan Library glitching the whole window on big libraries.** A scan that found over a thousand missing releases built a card (thumbnail, checkbox, buttons) for every one at once, which exhausted Windows' graphics handles — the window then painted garbage outside its frame. Results are now one collapsed row per artist ("Artist • N missing or incomplete") with a checkbox and a ▸ button; an artist's albums are only drawn when you expand it, where you can untick individual ones. A ticked artist you never expand is downloaded in full.

## v1.0.45
- **New: 🔍 Scan Library.** Pick your music folder and the app compares every artist folder with that artist's YouTube Music discography, then lists every release that's missing or only partly downloaded (with "you have N" next to the track count), grouped by artist, ready to tick and send through Download Selected Releases. Only exact artist-name matches are scanned — a folder with no exact match is skipped and listed in the log rather than guessed at, so one artist's albums can never be downloaded into another's folder. An album that exists but whose track count couldn't be checked (rate limit) is not re-queued, since there's no way to tell if it's complete. Downloads from the scan file under the same artist folder they were found in. Stop works during a scan. Desktop/Linux only for now.

## v1.0.44
- **Tracks removed or blocked in your region are now sourced from other regions.** When YouTube Music says a track is unavailable (or a download comes back "no longer available"), the app searches the United Kingdom, Canada, Australia, Germany, France, Japan, Brazil and Mexico catalogs for the *same recording* released under a different ID, and downloads that instead. The match is deliberately strict — same title, same version (a live/demo/remix/acoustic cut never stands in for the studio track and vice versa), a credited artist in common, and a length within 3 seconds — because a wrong substitute silently putting the wrong audio in your library is worse than a clearly reported missing track. The file is still named and tagged with the original track's title and album, and each substitution is logged (🌍). If a long run of lookups finds nothing, the app stops searching other regions for the rest of the session. Tracks already on disk are no longer reported as failures just because YouTube flags them. Only YouTube Music catalogs are searched — regular YouTube uploads are not used. Applies to desktop/Linux and Android.
- **Fixed View Log needing two clicks the first time.** The log window opened and was immediately buried under the main window (Windows re-activates the main window after the click finishes). It's now an owned window of the main one, which Windows always keeps on top of it. Verified with real clicks, including with the main window maximized.

## v1.0.43
- **Albums hit by a rate limit are no longer lost.** Two separate things made them disappear, so a stopped artist run could never be finished: (1) an album that lost tracks to the limit was still treated as "done" and removed from the release list, and (2) when the artist's releases were loaded again, any release whose track-count lookup failed (also usually the limit) was recorded as "0 tracks" and silently filtered out. Now an album with missing tracks stays in the list with a visible "⚠ N track(s) didn't download" note — download it again and only the missing tracks are fetched — and a failed lookup is retried, then kept in the list with a "?" track count instead of being dropped. If the artist's album list itself can't load because of the limit, it now says so instead of "No releases found."
- Android release builds now actually use their build cache (a release is built on a fresh tag each time, and tag builds could never see each other's caches, so every release rebuilt everything — about 30 minutes instead of 7). A master-branch build keeps a cache warm for them.

## v1.0.42
- **Stopped filing albums under the wrong artist** (e.g. Kanye West's *DONDA 2* ending up in a `DONDA` artist folder, tagged Artist and AlbumArtist "DONDA"). The app trusted the *first* artist YouTube Music credits, and for some releases that's a stand-in entity named after the project (*DONDA 2* is credited `DONDA, Kanye West, Ye`). The artist is now chosen from better evidence, in order: the artist whose page you're downloading from; a credited artist that already has a folder in your library; the first credit that isn't a project-named stand-in. The per-track Artist tag gets the same treatment, while ordinary collaborations still keep their first-credited performer. When the app files something under a different name than the first credit, it says so in the log (🎯). Applies to both the desktop/Linux and Android apps.
- A stand-in folder that already exists in your library (like the `DONDA` one) is deliberately *not* treated as evidence, so it can't attract more albums.

## v1.0.41
A reliability pass over the five highest-impact problems found in a code review.
- **Downloads no longer stall on genuinely removed tracks.** The v1.0.35 "is YouTube rate-limiting me?" check treated every dead track as a possible throttle once six came back unavailable in a row, pausing all downloads for 30 seconds each. For a catalog with hundreds of removed tracks that meant hours. It now pauses and retries once per suspected burst, and a track that's still unavailable after the pause counts as confirmed gone — which also raises the bar for the next pause. A 400-track run of removals now costs about 7 pauses instead of hundreds.
- **Fewer duplicates in partly-downloaded albums.** Tracks were recognised only by the exact filename the current version would produce, so tracks saved by an older version (`Who Believes In Angels_.mp3`) were downloaded again next to themselves whenever the album wasn't 100% complete. They're now matched by track number plus a punctuation-insensitive title, in any supported format, and a lower-quality copy is replaced rather than duplicated. `cover.jpg` is now written by whichever track lands first, not only track 1.
- **Updates are signature-checked.** The auto-updater used to run whatever installer it downloaded. Releases are now signed with a key that stays on the maintainer's machine, and the app refuses to run an installer whose signature doesn't verify (or that has none). Applies from this version onward — copies older than 1.0.41 still update unverified this one time. Also fixed: if an update failed, the "Update failed" dialog never actually appeared (a Python scoping bug swallowed it).
- **Fixed non-Latin album names matching each other.** Folder matching stripped every non-ASCII character, so any two Japanese (or Korean, etc.) album folders under one artist compared as identical.
- **Android:** cookie import now handles the `content://` file locations Android's picker returns, three error messages that were silently dropped now show up, there's a Log button (and a log file) for diagnosing problems on the phone, and "unavailable" bursts are handled like on desktop. Not yet verified on a physical device.
- Under the hood: the logic shared by the desktop and Android apps now lives in one module with 42 automated tests (run on every push), instead of two hand-synced copies — and a test now guards against the Linux package missing a module the app imports, which is what broke the 1.0.31 `.deb`.

## v1.0.40
- Fixed "View Log" opening behind the main window on the first click (looked like it opened and immediately closed) — a freshly created log window was never explicitly raised/focused, only a second click did that, via the "already open" path. Verified directly: now shows in front on the very first click.

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
