# Spec — tracked TTS-sample pages: page mp3s convert in place to tracked AAC `.m4a`, pages gain per-entry download links

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/67 · filed from the developer directive (2026-11-21) · sibling of the lesson-audio conversions CherokeeLessons#58 and CherokeeLessons#59 (closed — the #59 disposition sets the download-link form) · seam with CherokeeLessons#30 R-2 (its "4 espeak clips" item supersedes into this spec)
<!-- artifacts: .issues/67/ -->

## Intent

**Problem Statement.** Four TTS-sample pages carry 126 embedded samples as mp3 — 122 tracked today (AGENTS.md's recorded count; espeak 96, tacotron2 18, here-is-why 5, tts-update--2 3) and 4 untracked espeak clips — and carry no per-sample download affordance. The pages are inconsistent with the converted lesson-audio set (CherokeeLessons#58/#59) in both media format and page treatment, and the 4 untracked clips are additionally dead in every fresh-clone/deployed build (the executed 2026-11-11 stand-up-gate build omitted every untracked asset class).

**Root Cause / Motivation.** These four pages predate the lesson-audio conversions and kept tracked mp3 media with no download affordance; the lesson-audio pages' 2026-11-21 disposition (recorded on CherokeeLessons#59: direct `.m4a` anchors with the HTML `download` attribute, arrow glyph + "download", no per-file zips) now defines the delivery form of record across the site's sample-media pages. The developer's 2026-11-21 directive extends that consistency to these pages: the tracked TTS-sample mp3s convert to `.m4a` and the pages gain the same per-entry download links.

**Approach Chosen.** In-place transcode of the 126 page mp3s to tracked AAC `.m4a` through the per-track bitrate ladder (AAC-LC, mono, 48 kHz; 128 kb/s target falling back 112 → 96 kb/s on encode-and-measure until each file is ≤ 24 MiB under the site's 25 MiB-per-file asset cap), all embed and media-link references updated to the `.m4a` names, each media entry gaining a direct-download anchor in the #59 form, the 122 tracked mp3s retired from the working tree (git-history recoverable), the 4 unrecoverable clip sources TRACKED via the repo's demo-audio negation pattern (per the developer's 2026-11-21 ruling — untracked audio is lost audio; provenance recorded as well), the inert tacotron `!*.mp3` negation cleared, and the umbrella seam recorded (#30 R-2's espeak-clips item descopes here).

**Alternatives Considered & Why Discarded.**
- Leaving the tracked mp3s as-is (the status quo) — discarded: the developer directive converts them; page treatment stays inconsistent with the #58/#59 delivery form of record.
- YouTube embeds — discarded: the disposition of record (developer decisions 2026-11-15/16, recorded on CherokeeLessons#54) keeps sample media local; the channel mirror is provenance, not the delivery method (the espeak/tacotron sample clips have no verified channel mirror at all).
- External re-host (R2 delivery) — discarded: R2 is not a valid delivery path for this repo (ruling of record 2026-11-12).
- Per-file zip downloads — discarded by the #59 disposition: a single-file zip of an AAC file gains nothing; direct `.m4a` anchors only.
- Deleting the 4 clip sources after conversion — discarded (safety + ruling): they have no channel mirror and no regenerable recorded configuration; deleting them destroys their only copy. Retaining them untracked is equally discarded (the 2026-11-21 ruling: untracked audio is lost audio — untracked retention preserves nothing a fresh clone can see). This spec TRACKS the sources via the negation pattern and records their provenance.

**Key Design Decisions.** (1) In-place per-track transcode with recorded ladder transcripts — the transcripts are the evidence for the ladder-exercised criterion; final encoded size, not nominal rate, decides the rung (owning definition: audio-lessons-generator-python#16 — its trials bound the ladder's method). At these sources' KB-to-sub-MB sizes the 128k rung is the expected landing — recorded per track, not assumed. (2) The 122 tracked mp3s retire from the working tree with git history as the recovery path; the 4 untracked espeak clips' source mp3s are the only media in this change with no recovery path (untracked today, no channel mirror for the espeak clip class — the mirror map's 69-video census carries no espeak-clip section) — per the developer's 2026-11-21 ruling ("an untracked audio file is a lost audio file") they are not left untracked and not deleted: they are TRACKED via a per-dir `!*.mp3` negation (the repo's demo-audio pattern) and retained on disk with sha256 provenance in the research artifacts. (3) No generator scripts exist for these pages (verified) — the change touches the pages directly; no generator-emission surface is in scope. (4) The word-link pages' Cherokee-word entry links (espeak 100, here-is-why 5) are content, not download affordances — they retarget to `.m4a` with their labels unchanged, and the download affordance is the added `⤓ download` anchor per entry (the two roles stay distinct). (5) The 4 espeak clips leave CherokeeLessons#30 R-2's scope through this conversion (they arrive as tracked `.m4a` here, not as tracked mp3 there) — the seam is recorded as R-4 and revised into the umbrella's spec.

**User Intent / Original Prompt.**
- (2026-11-21, developer directive) "the tracked mp3s for the tts samples also needs to have a spec to convert and to have the page updated to be consistent with #58 and #59; update/create specs as needed" — this spec, filed accordingly.
- (2026-11-21, disposition set on CherokeeLessons#59, extended across sample-media pages by this directive) direct download links per media entry (no per-file zips — a single-file zip of AAC gains nothing), arrow glyph + the word "download", tested.
- (2026-11-11, observed failure evidence class) the executed stand-up-gate build omitted every untracked asset class — the 4 untracked espeak clips among them.
- (2026-11-21, developer ruling) "an untracked audio file is a lost audio file" — correcting this spec's original retention design: the 4 clip sources are not kept on disk untracked (which preserves nothing — untracked media is invisible to every fresh clone); they are tracked via the repo's established demo-audio negation pattern.

## Not Included

- The three demo-audio dirs' 41 mp3s (cherokee-animal-names-tts-demo-audio-1 24, cherokee-animal-names--ims-toucan-tts- 9, cherokee-tts-update-3--ims-toucan- 8) and their `SPEAKER|AUDIO MP3` tables — outside this spec's four-page scope; their conversion, source tracking, and page treatment are owned by CherokeeLessons#68 under the 2026-11-21 new-audio-standard directive (R-4's seam record).
- The lesson-audio pages (converted under CherokeeLessons#58/#59) and the espeak/TTS generation tooling that produced the samples.
- A "download all" affordance — per-entry links only (the #59 form); no aggregate-download artifact is created.

## Requirements

- **R-1 — In-place conversion.** The 126 page mp3s SHALL be transcoded in place to `*.m4a` — AAC-LC in MP4 container, mono, 48 kHz — through the per-track ladder: 128 kb/s target falling back 112 → 96 kb/s on encode-and-measure (the encoded file's measured size, not the nominal rate, decides) until each file is ≤ 24 MiB (25,165,824 B — the margin below the 25 MiB per-file asset cap). The 126 `.m4a` files SHALL be tracked in git (including the 4 espeak clips). The 122 tracked mp3s SHALL be deleted from the working tree (git history preserves them); the 4 untracked clip sources (`oo1sdee3gaa_.mp3`, `sdvv2goo2hvv23-ii_.mp3`, `sgwoohl2gaa_.mp3`, `sgwoohl2gii_.mp3` in `content/content/espeak-ng-and-ced-phonetics/`) SHALL NOT be deleted — per the developer's 2026-11-21 ruling ("an untracked audio file is a lost audio file") they SHALL be TRACKED via a new per-dir `content/content/espeak-ng-and-ced-phonetics/.gitignore` carrying `!*.mp3` (the repo's demo-audio negation pattern), with their sha256 digests and sizes recorded in the research transcripts. The per-track ladder transcript (rungs attempted, measured sizes at each, chosen rung), the transcode command, and tool provenance SHALL be recorded in this issue's research artifacts (`.issues/67/research/`).
- **R-2 — Pages updated: renamed references + per-entry download links.** All `{{audio (file=…)}}` embeds on the four pages (counts: espeak 100, tacotron2 18, here-is-why 5, tts-update--2 3) SHALL reference the `.m4a` names; the two word-link pages' media links (espeak 100, here-is-why 5) SHALL retarget to the `.m4a` names with their Cherokee-word labels unchanged; and each media entry SHALL gain a direct-download anchor in raw-HTML form `<a href="<file>.m4a" download>⤓ download</a>` (direct `.m4a` link with the HTML `download` attribute; arrow glyph plus the word "download"), placed with its entry. No per-file zip artifacts are created.
- **R-3 — Tracking-state artifacts.** The tracked `content/content/tacotron2-and-cherokee-tts/.gitignore` (its sole `!*.mp3` negation), left covering nothing by R-1's retirement, SHALL be deleted; a new `content/content/espeak-ng-and-ced-phonetics/.gitignore` carrying `!*.mp3` SHALL be created (R-1's source tracking). `.m4a` is not globally ignored (`git check-ignore` verified non-zero 2026-11-21); Cherokee diacritic filenames (here-is-why's `ạ²ma_.mp3` class) are handled with `git -c core.quotepath=off` and quoted paths per the repo's recorded media pattern.
- **R-4 — Umbrella seam (CherokeeLessons#30 R-2).** The umbrella's R-2 descopes its "4 espeak clips" item into this spec (the clips arrive as tracked `.m4a` through R-1 and their source mp3s become tracked here through R-1's negation — the clips' tracking moves to this spec entirely); R-2's remaining scope is the three demo dirs' 41 mp3s — superseding to CherokeeLessons#68 (the 2026-11-21 new-audio-standard directive) per the umbrella's own revision. The umbrella's spec carries the same seam revision.

## Success criteria

| ID | Criterion | Class | Instrument |
|---|---|---|---|
| SC-1 | The four pages reference only `.m4a` media: per-dir embeds = 100/18/5/3, all names `*.m4a`; `.mp3` name references = 0 on the four pages; the two word-link pages' media links (espeak 100, here-is-why 5) all target `.m4a` | structural | grep counts on the four page sources |
| SC-2 | Tracking truth: `git ls-files` per dir lists exactly 100/18/5/3 `.m4a` (espeak's 100 including the 4 clips) and— espeak only — the 4 `.mp3` clip sources (tracked via the new negation; the other three dirs carry no tracked mp3); on-disk `find` for `.mp3` across the four dirs returns exactly the 4 recorded clip names and no other mp3; `git check-ignore` on an m4a exits non-zero (not ignored); `git check-ignore -v` on a clip source shows the `!*.mp3` negation rule | structural | `git ls-files`, `git check-ignore`, `find` against the 4-name whitelist |
| SC-3 | Every m4a is AAC-LC, mono, 48 kHz, ≤ 25,165,824 B, in an MP4 container | structural | per file: `ffprobe` shows `codec_name` aac, `channels` 1, `sample_rate` 48000, `format_name` containing mov/mp4; `stat -c %s` ≤ 25,165,824 |
| SC-4 | The conversion exercised the ladder per track — each file's final encode is the highest rung (128 → 112 → 96 kb/s) whose encoded output measured ≤ 24 MiB; no track silently pre-lowered | behavioral | the recorded per-track transcripts in `.issues/67/research/`; per-file cross-check: `ffprobe` `bit_rate` corresponds to the recorded chosen rung |
| SC-5 | Every media entry carries the download anchor in the #59 form: counts 100/18/5/3 per page, each anchor `<a href="<file>.m4a" download>` carrying the `download` attribute and the `⤓ download` label; the retargeted word-links keep their Cherokee-word labels unchanged (label-set preserved) | structural | grep counts + label-preservation check (pre/post label lists equal) on the four page sources |
| SC-6 | A clicked download anchor serves the entry's `.m4a` for download (browser save, not navigation) | behavioral | browser test on the built pages: click one anchor per page (4 total), observe the download events with matching filenames |
| SC-7 | The residue is cleared: `content/content/tacotron2-and-cherokee-tts/.gitignore` is absent from `git ls-files` and the working tree | structural | `git ls-files` + `test -e` check |
| SC-8 | The pages build and serve their converted media | behavioral | `./build.sh` exit 0; the four pages' `public/` dirs present carrying exactly the 100/18/5/3 `.m4a` files |
| SC-9 | The umbrella carries the seam record: CherokeeLessons#30's spec R-2 names the 4 espeak clips' descope into CherokeeLessons#67 and its own supersession to CherokeeLessons#68 under the 2026-11-21 new-audio-standard directive, and its Parts list carries both the #67 and the #68 entries | structural | grep checks on `.issues/30/spec.md` for those recorded elements |

## Dependencies

| Reference | Relationship | Status |
|---|---|---|
| CherokeeLessons#59 (closed, completed 2026-11-21) | pattern of record for R-2's anchor form, SC-5/SC-6's instruments, and the executed download-event evidence class | satisfied (PR #61 merged; evidence verified 2026-11-21) |
| CherokeeLessons#30 (umbrella) | R-4's seam — R-2's espeak-clips item descopes here; the umbrella's site-wide gates execute after the last part lands | pending (umbrella open) |
| CherokeeLessons#58 (open sibling) | the conversion method record (ladder, per-track transcripts, verification form) — no ordering constraint | pending (approval states) |
| audio-lessons-generator-python#16 | owning definition of the ladder/encoder method R-1 imports | satisfied (fetched 2026-11-20) |
| ffmpeg / ffprobe toolchain | the transcode's tool — `/snap/bin/ffmpeg` present; `ffprobe` absent (verified 2026-11-21) and provisioned at implement time | pending (implement-time provisioning) |
| Git history | recovery path for the 122 retired tracked mp3s | satisfied (all 122 tracked at HEAD) |

## Traceability

| Requirement | Success criteria |
|---|---|
| R-1 | SC-1, SC-2, SC-3, SC-4, SC-8 |
| R-2 | SC-1, SC-5, SC-6 |
| R-3 | SC-7 |
| R-4 | SC-9 |

## Documentation Sources

| Source | Type | Location | Verification |
|---|---|---|---|
| Completed #59 download-column form (pattern of record) | sibling spec + repo file + executed evidence | CherokeeLessons#59 (closed; `.issues/59/spec.md` R-3/SC-7/SC-8); `content/lesson-audio/animal-names/index.md` (completed 3-column table); `.issues/59/research/download-column.md` | read 2026-11-21; executed browser-download evidence verified 2026-11-21 |
| Page and media census | working tree | the four page dirs — embeds 100/18/5/3; word-links 100/5; tracked mp3s 96/18/5/3 (122 total), on-disk 100/18/5/3 (126; the 4 untracked clips named in R-1); sizes 1.5 MB / 712 KB / 72 KB / 1.4 MB | greps, `git ls-files`, `find`, `du` executed 2026-11-21 |
| Page structures: both embed forms (`audio (file=` and `audio(file=`), 0 tracked generators, 0 session tables | repo files | the four `index.md` + `git ls-files` per dir | greps executed 2026-11-21 |
| The 4 untracked clips | working tree | `git ls-files` vs `find` diff on the espeak dir — 4 names, each referenced twice on the page (link + embed) | comm diff executed 2026-11-21 |
| Owning definition of the ladder/encoder method | remote spec | audio-lessons-generator-python#16 | fetched live via `gh` 2026-11-20 |
| Channel-mirror absence for the espeak clip class | research artifact | `.issues/30/research/youtube-video-id-map.txt` — the map's 69-video channel census carries Syllabary-Dictation, CLL 1, and Animals sections; no espeak-clip or TTS-demo section | read 2026-11-21 |
| Asset cap + ignore rules | repo facts | Workers 25 MiB/file (`AGENTS.md`); global `*.mp3`/`*.mp4` ignore, m4a unignored — `git check-ignore` verified non-zero 2026-11-21 | read/verified 2026-11-21 |
| Umbrella seam | spec revision | CherokeeLessons#30 R-2/SC-3 and part list revised 2026-11-21 | edited in place 2026-11-21 |

## Cost Frame

- SC-1 / SC-2 / SC-5 / SC-7: greps, git listings, and the label-preservation diff — minutes; skipping risks dead or stale references surviving the deploy, a mis-named regen, or label loss (content destruction the change never authorized).
- SC-3 / SC-4: 126 encodes with per-file probes — the conversion's own runtime (KB-to-sub-MB sources; minutes of wall time); skipping risks over-cap files or unverifiable ladder behavior.
- SC-6: one browser click-through per page (4 total) — the #59 download-column evidence class; an untested anchor risks a link that navigates instead of saving.
- SC-8: one full site build; skipping risks shipping the pages without their media or stale embed forms.

## Edge Cases

| Condition | Expected behavior | Resolution |
|---|---|---|
| A track's encoded size exceeds 24 MiB even at the 96k floor | not expected — sources are KB-to-sub-MB scale | if it occurs: the transcript records the measured floor size and the track's completion waits on the owning definition gaining a lower rung — this conversion does not widen the ladder unilaterally |
| A page's media filename contains Cherokee diacritics (here-is-why's `ạ²ma_.mp3` class) | quoted-path handling (`git -c core.quotepath=off`, quoted shell paths) per the repo's recorded pattern | recorded in R-3; the rename keeps the basename, extension only |
| `git add` of the new m4a silently no-op'd | SC-2's `ls-files`/`check-ignore` assertions verify tracking truth, not disk presence | the repo's recorded media-add pattern (AGENTS.md): confirm with `git status`/`git check-ignore` |
| The 4 clip sources accidentally deleted | SC-2's `ls-files` (they are tracked) + the whitelist `find` catch the shortfall before completion | the completion gate keeps the change all-or-nothing; re-encode is not possible from a fresh clone — the sources have no mirror or recorded generator configuration, which is why R-1 tracks them |
| The espeak page's word-link label set drifts during the edit | SC-5's label-preservation check (pre/post label lists equal) catches it | labels are content — restored before completion |
| Site build re-copies the media dirs into `public/` | SC-8's built-tree checks count the served m4a per page dir | `./build.sh` exit 0 plus per-dir counts verify the deployable surface |

## Completion gate

Every SC passes before the deliverable is complete — all-or-nothing: the conversion is complete only when all 126 files are converted, tracked, ladder-transcribed, page-referenced, anchor-complete, and built, with the 122 tracked mp3s retired and the 4 clip sources retained-and-recorded; a partial state (some files converted, some pending) fails SC-2 as a set.

<!-- artifacts: .issues/67/ -->
