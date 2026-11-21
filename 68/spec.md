# Spec — TTS demo-audio pages: 41 demo mp3s convert in place to tracked AAC `.m4a`, unrecoverable mp3 sources tracked, pages gain per-entry download anchors

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/68 · filed from the developer directive (2026-11-21) "we need to look at tracking everything feasible, using the new standard for audio", extending the same-day ruling "an untracked audio file is a lost audio file" · the repo-wide audio census (executed 2026-11-21) found every audio surface covered by existing specs except this class · transfers from CherokeeLessons#30's original R-2 (track-as-mp3 negation delivery) under the new standard (the CherokeeLessons#58/#59/#67 form) · part of the CherokeeLessons#30 recomposition (2026-11-14)
<!-- artifacts: .issues/68/ -->

## Intent

**Problem Statement.** Three TTS demo-audio page dirs carry 41 embedded-or-referenced mp3s (≈ 210 MB), all untracked — every one dead in each fresh-clone/deployed build (the executed 2026-11-11 stand-up-gate build omitted every untracked asset class):

- `content/content/cherokee-animal-names-tts-demo-audio-1/` — 24 mp3s, 118 MB: 12 embedded on the page (`Animals_2021-05-31_000N`, the "## Exercises" list) + 12 non-embedded (`Animals_2021-05-29_000N`, referenced by the tracked fragment `exercises.txt` — the recorded both-generations drift)
- `content/content/cherokee-animal-names--ims-toucan-tts-/` — 9 mp3s, 86 MB (embedded `animals-000N`)
- `content/content/cherokee-tts-update-3--ims-toucan-/` — 8 mp3s, 5.7 MB, laid out in four `SPEAKER|AUDIO MP3` tables (two-column, `--|--` dividers)

No page carries a download affordance, and the media format diverges from the converted lesson-audio set's standard.

**Root Cause / Motivation.** The demo class predates the deploy-environment asset requirements and the new audio standard; the class has no channel mirror (the YouTube map's 69-video census carries Syllabary-Dictation, CLL 1, and Animals sections only — no demo/TTS section) and no git history (never tracked): once lost, these files are unrecoverable. The developer's 2026-11-21 ruling ("an untracked audio file is a lost audio file") and directive ("tracking everything feasible, using the new standard for audio") bring the class under the standard: tracked AAC `.m4a` page audio with per-entry download links, and tracked sources for the unrecoverable originals.

**Approach Chosen.** In-place transcode of the 41 demo mp3s to tracked AAC `.m4a` through the per-track ladder; all 41 mp3 sources tracked via the per-dir `!*.mp3` negation pattern (nothing deleted — the ruling leaves no untracked-retention option, and deletion destroys the only copies); page embeds and the fragment's references retargeted to the `.m4a` names with per-entry download anchors (the `SPEAKER|M4A FILE|DOWNLOAD` 3-column form on the four tables); the fragment generator's emission updated to `.m4a` names; the umbrella's original R-2 superseded by the transfer record.

**Alternatives Considered & Why Discarded.**
- The original R-2 method (track the mp3s as the page audio via negation, no conversion) — superseded by the developer's 2026-11-21 directive: the new standard (tracked `.m4a` page audio + download links) replaces the mp3-delivery form; its negation mechanics are recycled here as the source-preservation mechanism.
- Convert-and-delete the sources (the lesson-audio pattern) — discarded: CherokeeLessons#58/#59's source retirement rests on recovery paths (verified channel mirrors / the recordings' provenance); this class has neither, so deletion is permanent loss (the ruling's exact case).
- Retain sources on disk untracked — discarded by the 2026-11-21 ruling: untracked audio is lost audio (invisible to every fresh clone; retention preserves nothing).
- YouTube embeds — discarded: the disposition of record keeps sample media local; the class has no channel mirror anyway.
- External re-host (R2 delivery) — discarded: R2 is not a valid delivery path for this repo (ruling of record 2026-11-12).
- Per-file zip downloads — discarded by the #59 disposition (a single-file zip of AAC gains nothing).

**Key Design Decisions.** (1) In-place per-track transcode with recorded ladder transcripts (owning definition: audio-lessons-generator-python#16); final encoded size, not nominal rate, decides the rung. (2) Source preservation is the ruling's application: nothing in this class is ever untracked or deleted again — sources track via negation alongside the `.m4a` deliverables, and the both-generations drift is preserved names-only. (3) The tracked fragment generator `make-exercises-fragment.sh` is retained with its glob updated to `Animals*m4a` (its re-run emits one entry per tracked `Animals*m4a` — the committed fragment's 12-entry count vs the glob's 24 is the generator's own pre-existing behavior, preserved, not resolved). (4) `fix-mp3-tags.sh` is untouched — its `*.mp3` glob operates on the retained sources. (5) The deploy consequence of source durability (~2× media weight for these three dirs, both sets ship in `public/`) is flagged for the developer's approval gate — accepted as the price of preserving unrecoverable originals, not silently optimized away.

**User Intent / Original Prompt.**
- (2026-11-21, developer ruling) "an untracked audio file is a lost audio file" — no audio remains untracked or untracked-retained anywhere it can be tracked.
- (2026-11-21, developer directive) "we need to look at tracking everything feasible, using the new standard for audio" — the repo-wide audio census left this class as the only uncovered surface; this spec carries it.
- (2026-11-15/16, disposition of record superseded today) the class's track-as-mp3 negation-delivery method (CherokeeLessons#30 R-2) — superseded by the new standard; the umbrella records the supersession.
- (2026-11-11, observed failure evidence class) the executed stand-up-gate build omitted every untracked asset class — all 41 demo mp3s among them.

## Not Included

- Resolving the tracked both-generations drift (`exercises.txt` references 05-29 while `index.md` embeds 05-31) — preserved names-only, per the recorded stance.
- `fix-mp3-tags.sh` — operates on the retained sources; no name surface changes.
- Any "download all" affordance — per-entry links only (the #59 form); no aggregate-download artifact.
- The lesson-audio pages (CherokeeLessons#58/#59) and the tracked TTS-sample pages (CherokeeLessons#67).
- Deploy-weight optimization (excluding mp3 sources from `public/` would be a build-framework change) is not part of this spec — the deploy-weight consequence is flagged in Key Design Decisions (5) and accepted-for-flagging here for the developer's approval decision.

## Requirements

- **R-1 — In-place conversion.** The 41 demo mp3s (24 + 9 + 8; measured sources ≈ 210 MB) SHALL be transcoded in place to `*.m4a` — AAC-LC in MP4 container, mono, 48 kHz — through the per-track ladder: 128 kb/s target falling back 112 → 96 kb/s on encode-and-measure (the encoded file's measured size, not the nominal rate, decides) until each file is ≤ 24 MiB (25,165,824 B — the margin below the 25 MiB per-file asset cap). All 41 `.m4a` (both animal generations among them) SHALL be tracked in git. The per-track ladder transcript (rungs attempted, measured sizes at each, chosen rung), the transcode command, and tool provenance SHALL be recorded in this issue's research artifacts (`.issues/68/research/`).
- **R-2 — Source preservation (the ruling applied).** The 41 mp3 sources SHALL be tracked: a new per-dir `.gitignore` carrying `!*.mp3` SHALL be created in `cherokee-animal-names--ims-toucan-tts-/` and `cherokee-tts-update-3--ims-toucan-/` (both verified none today), and the tracked `Animals*.mp3` rule in `cherokee-animal-names-tts-demo-audio-1/.gitignore` SHALL be replaced with `!*.mp3`. No mp3 SHALL be deleted: the sources have no channel mirror (map-census evidence) and no git history — the only durable copies that can exist are tracked ones. The both-generations drift is preserved names-only.
- **R-3 — Pages updated: renamed references + per-entry download anchors.** All `{{audio (file=…)}}` embeds on the three pages (12/9/8) SHALL reference the `.m4a` names; every embedded entry SHALL gain a direct-download anchor in raw-HTML form `<a href="<file>.m4a" download>⤓ download</a>` (direct `.m4a` link with the HTML `download` attribute; arrow glyph plus the word "download") placed with its entry — for `cherokee-tts-update-3--ims-toucan-` that means each of the four tables carries the header `SPEAKER|M4A FILE|DOWNLOAD` with its divider extended to three columns and a third-cell anchor per row, replacing the `SPEAKER|AUDIO MP3` headers; the list pages' anchors sit beside each entry. `exercises.txt`'s 12 `{{audio}}` lines SHALL retarget to the 05-29 `.m4a` names (a non-rendered fragment — no anchors). The tracked generator `make-exercises-fragment.sh` SHALL be retained and SHALL emit `.m4a` embed names (its `Animals*mp3` glob SHALL become `Animals*m4a`). No per-file zip artifacts are created.
- **R-4 — Umbrella seam (CherokeeLessons#30 R-2).** The umbrella's original R-2 SHALL be superseded and replaced by a transfer record naming this spec (the converted deliverables, the source tracking, the page treatment); the umbrella's Parts list SHALL carry the #68 entry, and the seam-2 record SHALL carry the class's new disposition (converted deliverables tracked `.m4a`, sources tracked via negation, nothing deleted).

## Success criteria

| ID | Criterion | Class | Instrument |
|---|---|---|---|
| SC-1 | The three pages reference only `.m4a` embeds: embeds = 12/9/8, all names `*.m4a`; `.mp3` name references = 0 on the three pages | structural | grep counts on the three page sources |
| SC-2 | Tracking truth: `git ls-files` counts 24+24 / 9+9 / 8+8 (`.m4a` + retained `.mp3` sources per dir); no mp3 deleted from any dir; `git check-ignore -v` on a source mp3 shows the dir's `!*.mp3` negation rule | structural | `git ls-files` per dir, `git check-ignore -v` |
| SC-3 | Every m4a is AAC-LC, mono, 48 kHz, ≤ 25,165,824 B, in an MP4 container | structural | per file: `ffprobe` shows `codec_name` aac, `channels` 1, `sample_rate` 48000, `format_name` containing mov/mp4; `stat -c %s` ≤ 25,165,824 |
| SC-4 | The conversion exercised the ladder per track — each file's final encode is the highest rung (128 → 112 → 96 kb/s) whose encoded output measured ≤ 24 MiB; no track silently pre-lowered | behavioral | the recorded per-track transcripts in `.issues/68/research/`; per-file cross-check: `ffprobe` `bit_rate` corresponds to the recorded chosen rung |
| SC-5 | Download affordances present: `cherokee-tts-update-3`'s four tables each carry `SPEAKER|M4A FILE|DOWNLOAD` with the extended divider and one per-row anchor; the list pages carry 12 and 9 anchors; every anchor carries the `download` attribute and the `⤓ download` label; `SPEAKER|AUDIO MP3` count = 0 | structural | grep counts on the three page sources |
| SC-6 | A clicked download anchor serves the entry's `.m4a` for download (browser save, not navigation) | behavioral | browser test on the built pages: click one anchor per page (3 total), observe the download events with matching filenames |
| SC-7 | Reference surfaces updated: `exercises.txt` references only `.m4a` names (12 lines, 05-29 set, `.mp3` count = 0, no anchors); `make-exercises-fragment.sh` emits `Animals*m4a` names | structural | greps on the fragment and the script |
| SC-8 | The pages build and serve their converted media | behavioral | `./build.sh` exit 0; the three pages' `public/` dirs present carrying exactly the 24/9/8 `.m4a` (plus the retained `.mp3` sources the build copies) |
| SC-9 | The umbrella carries the seam record: `.issues/30/spec.md` R-2 names the supersession to CherokeeLessons#68 under the 2026-11-21 directive, its SC-3 records the transfer as resolvable (the #68 link + the transferred class), and its Parts list carries the #68 entry | structural | `local-issues` sub-issue listing / `.issues/30/links.yaml` contains 68; grep checks on `.issues/30/spec.md` for those recorded elements |

## Dependencies

| Reference | Relationship | Status |
|---|---|---|
| CherokeeLessons#59 (closed, completed 2026-11-21) | pattern of record for R-3's anchor form, SC-5/SC-6's instruments, and the executed download-event evidence class | satisfied (PR #61 merged; verified 2026-11-21) |
| CherokeeLessons#30 (umbrella) | R-4's seam — the original R-2 supersedes into this spec; the umbrella's site-wide gates execute after the last part lands | pending (umbrella open) |
| CherokeeLessons#67 (open sibling) | the ruling record and the conversion-method record (ladder, transcripts, verification form); no ordering constraint | pending (approval states) |
| audio-lessons-generator-python#16 | owning definition of the ladder/encoder method R-1 imports | satisfied (fetched 2026-11-20) |
| ffmpeg / ffprobe toolchain | the transcode's tool — `/snap/bin/ffmpeg` present; `ffprobe` absent (verified 2026-11-21) and provisioned at implement time | pending (implement-time provisioning) |
| Channel mirror | none exists for this class — the map census is the evidence R-2's source-preservation rests on | verified absent (map read 2026-11-21) |

## Traceability

| Requirement | Success criteria |
|---|---|
| R-1 | SC-1, SC-2, SC-3, SC-4, SC-8 |
| R-2 | SC-2 |
| R-3 | SC-1, SC-5, SC-6, SC-7 |
| R-4 | SC-9 |

## Documentation Sources

| Source | Type | Location | Verification |
|---|---|---|---|
| Completed #59 download-column form (pattern of record) | sibling spec + repo file + executed evidence | CherokeeLessons#59 (closed; `.issues/59/spec.md` R-3/SC-7/SC-8); `content/lesson-audio/animal-names/index.md`; `.issues/59/research/download-column.md` | read 2026-11-21; executed download evidence verified 2026-11-21 |
| Page and media census | working tree | the three dirs — mp3s 24/9/8 (118 MB / 86 MB / 5.7 MB); embeds 12/9/8; animal-demo page embeds the 05-31 set (12), `exercises.txt` references the 05-29 set (12); `fix-image-links.sh`/`fix-mp3-tags.sh`/`make-exercises-fragment.sh` tracked; tts-update-3's 4 `SPEAKER|AUDIO MP3` tables; the other two dirs carry no tracked generator scripts | greps, `git ls-files`, `find`, `du` executed 2026-11-21 |
| Fragment generator mechanism | repo file | `make-exercises-fragment.sh` — `for x in Animals*mp3` glob emitting `{{audio (file=…)}}` lines | read 2026-11-21 |
| Negation-file states | repo facts | animal-demo `.gitignore` carries `Animals*.mp3` (tracked); the other two dirs have none (`ls` + `git ls-files` verified 2026-11-21) | executed 2026-11-21 |
| Channel-mirror absence for the class | research artifact | `.issues/30/research/youtube-video-id-map.txt` — 69-video channel census, Syllabary-Dictation/CLL 1/Animals sections only | read 2026-11-21 |
| Owning definition of the ladder/encoder method | remote spec | audio-lessons-generator-python#16 | fetched live via `gh` 2026-11-20 |
| Asset cap + ignore rules | repo facts | Workers 25 MiB/file (`AGENTS.md`); global `*.mp3`/`*.mp4` ignore, m4a unignored — `git check-ignore` verified 2026-11-21 | read/verified 2026-11-21 |

## Cost Frame

- SC-1 / SC-2 / SC-5 / SC-7 / SC-9: greps, git listings, the sub-issue link check — minutes; skipping risks dead embeds surviving the deploy, a negation rule that silently fails to track, or an unrecorded seam.
- SC-3 / SC-4: 41 encodes with per-file probes — the conversion's own runtime (multi-MB sources; the heaviest tracks 5–12 MB); skipping risks over-cap files or unverifiable ladder behavior.
- SC-6: one browser click-through per page (3 total) — the #59 evidence class; an untested anchor risks navigation instead of save.
- SC-8: one full site build; skipping risks shipping pages without their media or stale embed forms.

## Edge Cases

| Condition | Expected behavior | Resolution |
|---|---|---|
| A track's encoded size exceeds 24 MiB even at the 96k floor | not expected — the largest demo tracks are 5–12 MB source-scale | if it occurs: the transcript records the measured floor size and the track's completion waits on the owning definition gaining a lower rung — this conversion does not widen the ladder unilaterally |
| The fragment generator re-run after conversion | emits one entry per tracked `Animals*m4a` (24 entries — both generations; the committed 12-entry count is the generator's own pre-existing divergence, preserved) | regeneration behavior is not governed by this spec's criteria beyond name format (SC-7); the count divergence is the pre-existing generator's |
| `git add` of the new m4a or the re-included mp3 sources silently no-op'd | SC-2's `ls-files`/`check-ignore -v` assertions verify tracking truth, not disk presence | the repo's recorded media-add pattern (AGENTS.md): confirm with `git status`/`git check-ignore -v` |
| The `Animals*.mp3` rule replaced but sources still excluded by the global ignore | `!*.mp3` in the dir file negates the global rule for that dir — verified per-SC by `check-ignore -v` showing the `!` rule | implement-time instrument, already the SC-2 check |
| An mp3 missing from the working tree at implement time | conversion is all-or-nothing over the 41 | the census is the correspondence record; missing files halt completion rather than partial conversion |
| A demo page's table rows or list entries gain/lose entries between census and implement | the SC counts anchor on census numbers (12/9/8; 4 tables) — drift halts completion | re-census at implement time; the page-editing keeps the count-set consistent before SCs run |

## Completion gate

Every SC passes before the deliverable is complete — all-or-nothing: the class's conversion is complete only when all 41 files are converted, tracked (deliverables and preserved sources), ladder-transcribed, page-referenced, anchor-complete, reference surfaces retargeted, and built; a partial state fails SC-2 as a set.

<!-- artifacts: .issues/68/ -->
