# Touchline Tracker r18

## Changes in r18
- Fixed Saved matches rendering: removed a stale reference to the deleted current-match banner that stopped the history list from rendering.
- After full time, the Full time control becomes a trophy button; tap it to choose or change Player of the Match. The award is stored as a match timeline event and counted in player statistics.
- Added a persistent **Notes:** field to the live match setup. Notes autosave with the current match and are included in shared/exported match reports.
- Existing local match data is retained; no reset is required.

# Touchline Tracker — Complete website package

This ZIP contains the files needed to host Touchline Tracker as a static website / Progressive Web App (PWA):

- `index.html` — the app and its interface (includes the app JavaScript)
- `manifest.webmanifest` — PWA name, launch settings and icons
- `sw.js` — service worker for offline caching and updates
- `icon-192.png`, `icon-512.png`, `icon.svg` — app icons
- `app.js` and `inline-check.js` — supporting copies of the app script for reference/validation; the current `index.html` runs its embedded script

## Publish on GitHub Pages

1. Back up your Touchline Tracker data from inside the app before updating.
2. Extract this ZIP.
3. Upload **all the files in this ZIP** to the root of your existing GitHub Pages repository, keeping the filenames unchanged. `index.html` should be at the published root, not buried inside another folder.
4. Commit the changes and wait for GitHub Pages to publish.
5. Open the site using its HTTPS GitHub Pages URL, then refresh/reopen the app. HTTPS is required for service-worker/PWA installation; opening `index.html` directly as a local file will not enable offline PWA features.

## Visual formation controls

- Tap a player, then tap another player to swap positions.
- Tap a substitute and then the player to replace, or tap the player first and then the substitute.
- Save formation and Save starting lineup save the changes and close their popups.

## Data safety

The app stores team and match data in the browser's local storage on each device. Updating the website should not require clearing browser storage. **Do not clear site data/storage** as part of the update. Export a backup regularly, and especially before replacing the hosted files.


## Latest reliability fixes (r6)
- Corrected saved-match team/date/season/tournament filtering with metadata fallbacks.
- Visual formation redraws the existing selection after a swap/substitution.
- Penalty shootout records and redraws the next kick state immediately.
- Start-period buttons start the match clock when pressed before kick-off.
- Yellow feedback is triggered on click rather than pointer-down, so scrolling should not flash buttons.
- Live page order is players, match timeline, player time, then event totals.
- Team backup filenames include the selected team name and export date.


## Latest reliability fixes
- Added a live-page toggle to switch between single-button and player-card layouts.
- Visual formation swaps and substitutions now redraw the pitch in the open popup; the popup stays open until Save formation & close is pressed.
- Penalty shootout score, recorded kick list, turn and available takers update inside the existing popup after each kick.
- Yellow feedback is limited to actual clicks on match action buttons rather than pointer-down gestures that can begin scrolling.
- Updated the service-worker cache version so the revised files can be picked up after publishing.


## Team page saved-team selection update
- The Team page no longer requires a separate Load team button. Choose a saved team in the Saved teams dropdown to load its players, kit colours, shirt numbers and age-group setup into the editable fields below.
- If the current team has unsaved changes, the app asks whether to save or discard them before switching.
- Saved team records now retain the age group as well.


### Popup refresh reliability fix (v10)
- Visual formation now updates the pitch inside the existing popup without rerendering the whole app while selections are being made. Changes remain in the popup until Save formation & close.
- Penalty shootout has a stable modal content container; each kick updates score, recorded kicks, turn and available takers in place without rebuilding the underlying live screen.
- Bumped service-worker cache to ensure updated files can be fetched.


Reliability update: undo snapshots are now capped by both count and total size. Saving first trims undo history and, if browser storage is full, retries after clearing the current match undo history. Visual formation and shootout saves use the same persistence routine. Older undo steps may be discarded to protect the match data.


## Build and diagnostics (2026-10-15)

- Release build ID: `TT-2026.10.15-r1`
- Service-worker cache: `touchline-tracker-20261015-r1`
- The app opens diagnostics after five taps on the top/bottom match banner within about 1.8 seconds. It reports build identifiers, active service worker, cache names, IndexedDB availability, and record counts.
- IndexedDB database: `touchline-tracker-db` (version 1), object stores `app` and `meta`. Existing localStorage data is retained and mirrored to IndexedDB; if localStorage is absent, the app attempts to restore the IndexedDB snapshot. This release deliberately keeps localStorage compatibility while IndexedDB rollout is tested.
- Statistics fixes normalise saved match records and ensure a selected team does not hide the saved-match statistics merely because the *current unsaved match* belongs to another team.

### Verify the running build
1. Publish every file in the ZIP to the repository root and commit.
2. Open the GitHub Pages site, tap the top/bottom match banner five times quickly, and compare the reported build ID and cache name above.
3. Check that the active service worker is present and IndexedDB says Available.
4. Create two teams and save one match with a goal and assist for the first team. Select that team in Statistics and History; its saved match and player event counts should appear. Select the second team; the first team's match should be excluded.

**Important technical limitation:** this is a transitional IndexedDB-backed release: localStorage remains the synchronous compatibility store because the existing application reads its state synchronously at startup. Data is mirrored into IndexedDB and can be restored from it when localStorage is absent, but this is not yet a complete localStorage-free IndexedDB-only rewrite.


## Release TT-2026.10.16-r6

- Penalty shootout first-kicker choice is now inside the shootout setup popup.
- Match substitutions update formation slots so the visual formation follows live substitutions.
- Team data import accepts both selective team backup files and full Touchline Tracker JSON backups.
- Statistics player table now aggregates filtered saved/current match records and honours team, season/tournament, and date filters.
- Check `version.json` and open Settings and tap the small version link at the very bottom to open diagnostics.

File version IDs: `index.html`, `app.js`, `inline-check.js`, and `manifest.webmanifest`: `TT-2026.10.16-r6`; `sw.js` cache: `touchline-tracker-20261015-r6`. `version.json` contains SHA-256 hashes for the shipped files.


## Release TT-2026.10.16-r6

- Replaced the five-tap top-banner gesture with a discreet “version TT-2026.10.16-r6” text link at the bottom of Settings. Tapping it opens build diagnostics.
- Updated the service-worker cache identifier so the new build can replace the cached release.


Release r4 fixes the Settings version diagnostics link, wires the Team page import button to the device file picker, and rebuilds History / Stats filters to use team, season and competition type only (no date filters or filter opt-out checkbox).


## r6 fixes

- The version link opens diagnostics immediately and then fills in browser storage/cache details.
- Penalty shootout first-team selection has a direct Start shootout action.
- Statistics and History use the same team, season and competition filters; All includes every saved match.
- Shot/free-kick/penalty outcome buttons record the chosen outcome and close the popup.
- Save formation and Save starting lineup close the modal before rerendering.


## r6 reliability fixes
- Version link label is generated from the active build ID and opens diagnostics through one delegated click handler.
- Startup reconciles localStorage with the IndexedDB mirror and restores the mirror when it contains more saved match records than localStorage.
- History and statistics share a normalized saved-match reader; history explicitly reports the number shown versus stored and filters only by the selected team, season, and competition.


## r6 reliability fixes
- Version label is generated from the active build ID and opens diagnostics.
- Startup reconciles localStorage with the IndexedDB mirror and restores the mirror when it contains more saved match records than localStorage.
- History and statistics share normalized saved-match data; history reports matches shown versus stored and filters only by the selected team, season, and competition.


## Release TT-2026.10.16-r10

- The Settings version link is bound as soon as the script starts, so a later startup error cannot leave it inert.
- The service worker uses network-first for page navigation, reducing the chance that GitHub Pages keeps displaying an older embedded script after publishing.
- The Saved matches panel now separately shows an active, eventful current match when it has not yet been saved to history, with a direct “Save current match to history” action. It is clearly labelled as current and is not counted as a saved record until the action is used.
- Storage writes include a timestamp; startup can prefer a newer IndexedDB mirror when the saved-match counts are equal, while still preferring a copy with more saved matches.
- No IndexedDB file is needed in the ZIP: the browser creates the database.


## r10 data integrity fixes
- Factory reset clears the app's localStorage entries and IndexedDB object-store records before reloading, preventing old match data from being restored after reset.
- Saved-match deletion no longer re-archives the same current match when that record is deleted.
- Statistics filter options are refreshed before calculating player-stat totals to avoid stale selections.
- Player statistics heading clarifies that totals apply to filtered matches.
- Service worker cache version incremented and app code resources use network-first with offline fallback.


## r10 saved-match recovery

This build restores the complete r8 application codebase (rather than the reduced r9 script) and applies a narrow saved-match fix. Live Save Match, the History page save button, and New Match now use a shared match-content check and archive routine. A named team/match can be saved even at 0–0, and a meaningful previous match is archived before starting a new one. The rest of the team, live match, formation, penalty shootout, import/export and settings handlers are retained from r8.


## History and storage recovery (r11)

- Saved matches from the current local-storage key, known legacy local-storage keys, and the IndexedDB mirror are reconciled into one saved-match list instead of one store replacing another.
- Match history is grouped by team, then season, then tournament/competition, while the existing team, season and competition filters apply to both reports and statistics.
- Browser data sources are read and merged without clearing them. Keep exporting a backup before updates; local-file and GitHub Pages storage are separate browser origins.


## r12 History/statistics visibility recovery
- Normalises current and legacy saved-match wrappers before display.
- If filters unexpectedly hide every record while saved matches exist, history shows all records with a recovery notice instead of an empty list.
- Player statistics use the same saved-match set and include current squad names when older reports lack squad snapshots.
- Service-worker cache bumped to r12.


## r13 Saved-match statistics and filter fix
- Added the missing safe `seasonForDate` helper; this helper was referenced by history/statistics rendering but was absent, so rendering could stop with a JavaScript error.
- History and statistics now honour selected team, season and competition filters strictly. They no longer silently fall back to showing all teams when a filter has no matches.
- Empty filter results now give a clear message instead of mixing unrelated saved matches into the result.
- Service-worker cache bumped to r13.


## Release TT-2026.10.16-r14 — shot statistics fix

- Player and team shot statistics now count recorded shot outcomes (`shot_saved`, `shot_post`, `shot_crossbar`, `shot_offtarget`, and other `shot_*` events), explicit `shot` events, and goals recorded with `sourceType: "shot"`.
- A shot resulting in a goal is counted once as a shot attempt, while still counting as a goal.
- Updated the service-worker cache to r14 and refreshed the SHA-256 file hashes in `version.json`.
- Validation: JavaScript syntax checks, embedded-script consistency checks, and ZIP integrity checks passed. This release has not been tested end-to-end on a physical Android device.


## r15 — Player of the Match and custom event management (2026-10-17)
- Custom event buttons now appear in Event button settings with editable labels, button order, and Delete controls. Deleting a custom event removes it from current settings/statistics; existing match timeline records are retained.
- After full time (including ending the final half/quarter), the app prompts to choose Player of the Match. The selection is saved with the match, can be changed later from the live screen, appears in match reports, and is counted in player statistics.
- Existing local data is preserved during update; do not clear browser storage.


## r16 update
- Removed the Player of the Match button from the live controls to preserve mobile layout.
- Final-time prompt lets you select None, a starter, or a substitute; selection is recorded in the timeline and player statistics.
- Removed the persistent unsaved-current-match banner from History / Stats.


## r17 update

- Removed legacy custom “Player of the Match” / “Man of the Match” event buttons from saved settings automatically, including their button-order entries. The award is selected only in the full-time picker.
- Player of the Match choice is recorded as a single `playerOfTheMatch` event in the match timeline and counted in player statistics; selecting None clears the award event.
- Existing match and squad data are preserved.


## r19 settings and cache refresh
- Settings lists render independently at the beginning of the main render cycle so another view's rendering error cannot prevent Event button settings and Clubs, kits and competitions from populating. Required settings arrays are normalised after storage recovery.
- The service worker cache name is bumped to r19. Navigation and app code are fetched network-first with cache bypass, and the worker update is checked on page load. The diagnostics “Reload latest version” action adds a unique query string.
- Browser HTTP cache directives are included for index.html. GitHub Pages/CDN and browser behaviour can still vary; the in-app version diagnostics are the reliable check.


## r20 — Training sessions and multi-user roadmap

- Added a Live match dropdown with a Training page.
- Record date, location, attendance across multiple saved teams, Star of Training and notes.
- Edit/delete saved training sessions; sessions are included in the combined chronological history and can be filtered separately from matches.
- Training statistics show session attendance and Star of Training counts by player/team.
- Training sessions are saved locally in this browser and included in full JSON backups. This release does not provide cloud sync or multi-user access; see the accompanying response for a recommended backend architecture.


## r21 — Training page navigation fix

Fixed the Live match / Training dropdown handler. The dropdown is no longer bound to the button-tab click handler, which could hide all pages when the select was clicked. Selecting Training now explicitly shows the Training section and renders its saved sessions and form. Existing local data is preserved.


## r22 update
- Fixed navigation state so switching to another tab resets the Live match/Training selector; returning to Training reliably shows the Training page.
- Added Training sessions to selective JSON backup export and restore. Full JSON backups already include the trainingSessions array.
- Training import merges sessions by ID instead of replacing existing sessions.


## r24 — Game/Train dropdown placeholder
- The first tab selector starts with “＋ Game/Train”, followed by “Live match” and “Training”.
- Switching to Team, History / stats, or Settings resets the selector to “＋ Game/Train”.
- Choosing Live match or Training explicitly opens that page. Selecting the placeholder does not hide the current page.
- Navigation-only change based on r22; other workflows and saved data structures are unchanged.


## r30 timeline, totals and full backup update (24 October 2026)
- Add events to the match timeline after full time, with player, assist, period and match-time fields.
- Timeline displays events in chronological match-time order.
- Match event totals show goal and assist totals per player plus other recorded event counts.
- Starting a new period automatically inserts a missing end-of-previous-period marker before the new start marker.
- Settings includes a complete app backup export and full replacement import for all app data.


## r30 timeline events, time-played editing and totals (24 October 2026)
- Add timeline events for start/end of a quarter or half and substitutions, in addition to goals and other events.
- Match event totals use a defensive renderer and include goalscorers and assists.
- Time played can be manually edited or reset to the calculated value from the match page.
- Complete app backup controls moved to the bottom of Settings, immediately above Factory reset.


## r30 changes
- Match reports now include Match Event Totals, including goals, opposition goals, assists, goalscorers/assists, and other recorded event totals.
- History & Stats match report popup includes a Share report action (native share where supported, clipboard fallback otherwise).
- Loading a saved match closes the confirmation/report popups before saving and switches to the Live Match page with the selected match loaded.
