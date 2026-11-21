# Spec — 14 Syllabary-Dictation pages migrate their dead local-video embeds to `{{ youtube(id=…) }}`

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/54 · part of the CherokeeLessons#30 recomposition (2026-11-14) · scope re-cut 2026-11-20 — the lesson-audio halves moved to their own parts (CherokeeLessons#58, CherokeeLessons#59), leaving the dictation slice
<!-- artifacts: .issues/54/ -->

## Intent

**Problem Statement.** The 14 Syllabary-Dictation pages (`content/lessons/Syllabary-Dictation/**/index.md`) embed 20 `{{ video(video="Dictation …mp4") }}` embeds whose mp4 sources are untracked — the repo's `*.mp4` ignore rule predates the embeds — so every fresh-clone/deployed build renders all 20 embeds dead (observed: the executed 2026-11-11 stand-up-gate build omitted every untracked asset class). Visitors to cherokeelessons.org get no playable dictation media.

**Root Cause / Motivation.** The dictation mp4 media was never tracked in git — the sources exist only on the working machine (21 untracked mp4s verified in the working tree; none in git) — leaving every deployable tree without the referenced files. The migration lands now as the dictation part of the CherokeeLessons#30 recomposition making the site deployable from fresh clones.

**Approach Chosen.** Each page's old embeds are replaced by `{{ youtube(id=…) }}` embeds carrying the verified mirror IDs — the mechanically derived page↔video assignment — and the ~1.7 GB of untracked working-tree mp4s is deleted. No theme/shortcode/infra changes: the `{{ youtube() }}` shortcode is pre-existing (tracked since 2023-05) and `static/_headers` frame-src already carries the YouTube origin (`https://www.youtube.com`).

**Alternatives Considered & Why Discarded.**
- Track the mp4s in git and keep the local embeds — discarded: the delivery method of record for the dictation class is YouTube embeds (developer decision 2026-11-15/16, recorded on this issue), and the long sessions run against the Workers 25 MiB-per-file asset cap.
- External re-host of the mp4s (R2 delivery) — discarded: R2 is not a valid delivery path for this repo (ruling of record 2026-11-12).
- Remove the media from the pages — discarded: the pages' purpose is the dictation exercise; dropping playable media abandons the visitor need the migration serves.

**Key Design Decisions.** (1) Embed form `{{ youtube(id=…) }}` — pre-existing shortcode, no new template surface, no CSP change (`frame-src` precondition verified; the same iframe class ships in production on the existing comics-talk embed). (2) IDs single-sourced from the recorded assignment (`.issues/30/research/page-video-assignment.yaml`; map `.issues/30/research/youtube-video-id-map.txt`) — no per-page re-derivation at implement time. (3) The mp4 retirement is working-tree-only (untracked files; no git-history change). (4) The 2026-11-20 re-cut keeps this spec a single work class — a mechanical page edit — with the lesson-audio transcodes, generator updates, and zip-link removal in their own parts (CherokeeLessons#58, CherokeeLessons#59).

**User Intent / Original Prompt.**
- (2026-11-11, observed failure) the executed stand-up-gate build omitted every untracked asset class; the dictation embeds among them.
- (2026-11-15/16, developer decisions recorded on this issue) disposition of record, per-page: dictation videos → YouTube embeds; the two lesson-audio pages → local tracked `.m4a` via the ladder (audio-lessons-generator-python#16); TTS demo audio → tracked mp3 — the per-page split governs.
- (2026-11-21 UTC, review comment on this issue) "Compare with: https://www.youtube.com/playlist?list=PLo2dw3Y__5_LM3UrVTJoFSxNErQ_yEdCk" — the playlist crosscheck (`.issues/54/research/youtube-playlist-crosscheck.txt`, executed 2026-11-21) found a clean bijection: the developer's curated "Syllabary Dictation" playlist holds exactly the map's 21 dictation IDs in identical order.
- (2026-11-20, developer directives) "the changes related to non-dictation videos needs to be moved into their own specs so as to prevent conflation and cross-concerns" (this re-cut); "proceed with spec revisions".

## Not Included

- The CLL-1 audio-exercises conversion (23 sessions → tracked `.m4a` ladder; dead zip-link removal; its generator's update) — CherokeeLessons#58, its own part since the 2026-11-20 re-cut.
- The animal-names conversion (9 sessions → tracked `.m4a` ladder; its generator's update) — CherokeeLessons#59, its own part since the re-cut.
- The `cll1-v3-audio.zip` bundle's own disposition (drop vs re-issue) — the umbrella's open decision (CherokeeLessons#30 seam 5); the dead page link leaves with CherokeeLessons#58.
- The four TTS demo-audio dirs — the umbrella's tracking work (CherokeeLessons#30 R-2, tracked mp3).
- Assigning Dictation 14-02 — it has no current page reference (mechanical truth of the assignment derivation); its mp4 still retires with R-2.
- Any YouTube-side work beyond embedding (playlist edits, privacy settings, uploads).
- The AGENTS.md content-facts update (the former R-5) — recorded work: commit 1fc9732 on `feature/sop-lesson-media-dispositions` (validated 2026-11-20, this issue's validation record), waiting on the branch's human merge — not part of the dictation migration.

## Requirements

- **R-1 — Embed migration.** The 14 dictation pages SHALL replace their 20 `{{ video(video="Dictation …mp4") }}` embeds with `{{ youtube(id=…) }}` embeds whose per-page ID sets equal the recorded assignment — `.issues/30/research/page-video-assignment.yaml`'s dictation entries: 20 IDs assigned across the 14 pages; the map's remaining dictation entry (Dictation 14-02, `pNOCN-6abhk`) assigned to no page. Page structure is otherwise unchanged.
- **R-2 — Working-tree media retirement.** The untracked dictation mp4 sources (`content/lessons/Syllabary-Dictation/**/*.mp4` — 21 files) SHALL be deleted from the working tree after R-1 replaces the embeds that reference them (untracked media; no git-history change; re-obtainable from the verified channel mirror).

## Success criteria

| ID | Criterion | Class | Instrument |
|---|---|---|---|
| SC-1 | The 14 pages carry exactly the recorded assignment: zero `video(video=` embeds anywhere in the 14; each page's `youtube(id=` ID set equals its recorded set; the pages' union equals the 20 assigned IDs; the 14-02 ID appears in no page | structural | per-page greps joined to `.issues/30/research/page-video-assignment.yaml`'s dictation entries; union and orphan checks (`pNOCN-6abhk` count = 0 across the 14) |
| SC-2 | The untracked dictation mp4s are absent from the working tree | structural | `find content/lessons/Syllabary-Dictation -name "*.mp4"` is empty |
| SC-3 | The migration's commits touch only the 14 dictation pages under content/ | structural | each migration commit's file list (`git show --name-only`) contains only dictation `index.md` paths under `content/lessons/Syllabary-Dictation/` |
| SC-4 | The built pages serve the migrated embeds without CSP frame violations | behavioral | `./build.sh` exit 0; each built `public/lessons/Syllabary-Dictation/**/index.html` contains its recorded IDs' iframe embed URLs; a browser-rendered preview reports no Content-Security-Policy `frame-src` violation in the console (the evergreen iframe-CSP gate — the blank-box failure class, #22 precedent) |

## Dependencies

| Reference | Relationship | Status |
|---|---|---|
| CherokeeLessons#30 (umbrella) | this part closes into the umbrella's composition; its site-wide gates execute after the last part lands | pending (umbrella open) |
| `.issues/30/research/page-video-assignment.yaml` · `youtube-video-id-map.txt` | the ID evidence R-1 consumes | satisfied (read 2026-11-20) |
| `.issues/54/research/youtube-playlist-crosscheck.txt` | playlist corroboration of the dictation set | satisfied (executed 2026-11-21) |
| `static/_headers` frame-src | iframe-origin precondition — `https://www.youtube.com` pre-existing | satisfied (verified 2026-11-20) |
| CherokeeLessons#58 · CherokeeLessons#59 | sibling parts born of the re-cut — independent content sets; no ordering constraint | pending (approval states) |

## Traceability

| Requirement | Success criteria |
|---|---|
| R-1 | SC-1, SC-4 |
| R-2 | SC-2, SC-3 |

## Documentation Sources

| Source | Type | Location | Verification |
|---|---|---|---|
| Page↔video assignment | research artifact | `.issues/30/research/page-video-assignment.yaml` (dictation entries — 20 assigned IDs across 14 pages) | read 2026-11-20 |
| Channel mirror map | research artifact | `.issues/30/research/youtube-video-id-map.txt` (Syllabary-Dictation section — 21 entries, verified on channel 2026-11-11) | read 2026-11-20 |
| Playlist crosscheck | research artifact | `.issues/54/research/youtube-playlist-crosscheck.txt` | executed 2026-11-21 (live playlist fetch; bijection) |
| Embeds/media state of record | working tree | 14 pages: 20 `video(video=` embeds; 21 untracked mp4s | grep/find executed 2026-11-20 |
| CSP frame-src | repo file | `static/_headers` (frame-src carries `https://giscus.app https://www.youtube.com https://itch.io`) | read 2026-11-20 |
| iframe-CSP evergreen gate (failure precedent) | ops governance | `AGENTS.md` build & verification section (#22 itch-widget precedent) | read 2026-11-20 |

## Cost Frame

- SC-1 / SC-2 / SC-3: greps, find, and commit file lists — minutes; skipping risks wrong IDs on live pages (visitors see dead or wrong-media embeds) or collateral edits elsewhere.
- SC-4: one site build plus one browser-rendered preview — build time; skipping risks the silent blank-box CSP failure reaching the deployed site (#22 precedent: the embed renders empty with no build-time signal).

## Edge Cases

| Condition | Expected behavior | Resolution |
|---|---|---|
| A page keeps an old embed alongside a new one (partial swap) | SC-1 fails (per-page `video(video=` count ≠ 0) | the swap completes per page before completion |
| Two pages carry equal counts but swapped IDs | SC-1 fails (per-page set inequality — a total-count check would miss it) | per-page set equality holds |
| The 14-02 ID lands on any page | SC-1 fails (orphan check) | the unassigned entry stays unbaked |
| The iframe is blocked by CSP at runtime | SC-4's console check fails — the silent blank box (#22 class) | the frame-src precondition is verified; the check observes the served page, not the config |
| The mp4s are deleted before their pages are migrated | sequencing fault | R-2 orders retirement after R-1; the channel mirror re-obtains if violated |

## Completion gate

Every SC passes before the migration is complete — all-or-nothing: a state with some pages migrated and others not fails SC-1 as a set, and retirement without migration (or the reverse) is incomplete.

<!-- artifacts: .issues/54/ -->
