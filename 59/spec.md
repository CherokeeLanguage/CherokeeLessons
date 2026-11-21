# Spec — animal-names page: 9 session mp3s convert in place to tracked AAC `.m4a`

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/59 · split from CherokeeLessons#54 (2026-11-20 scope re-cut — conflation/cross-concern prevention) · part of the CherokeeLessons#30 recomposition (2026-11-14)
<!-- artifacts: .issues/59/ -->

## Intent

**Problem Statement.** The animal-names audio page (`content/lesson-audio/animal-names/index.md`) embeds 9 `{{ audio (file="animals-NNNN.mp3") }}` sessions whose mp3 sources are untracked — the 9 embeds render dead in every fresh-clone/deployed build (observed: the executed 2026-11-11 stand-up-gate build omitted every untracked asset class). The animal-name audio sessions are absent from the deployed site.

**Root Cause / Motivation.** The lesson-audio mp3s were never tracked in git — the repo's global `*.mp3` ignore rule predates the deploy environment's asset requirements, and the sources exist only on the working machine (9 mp3s, 31 MB, verified in the working tree; none in git). It must be solved now as the animal-names part of the CherokeeLessons#30 recomposition making cherokeelessons.org deployable from fresh clones.

**Approach Chosen.** In-place transcode of the 9 sessions to tracked AAC `.m4a` through the per-track bitrate ladder (AAC-LC, mono, 48 kHz; 128 kb/s target falling back 112 → 96 kb/s on encode-and-measure until each file is ≤ 24 MiB under the site's 25 MiB-per-file asset cap), the page's 9 embeds updated to the `.m4a` names, the mp3s retired from the working tree, and the tracked generator script updated to emit the new names.

**Alternatives Considered & Why Discarded.**
- YouTube embeds — discarded: the disposition of record (developer decisions 2026-11-15/16, recorded on CherokeeLessons#54 — animal-names joined the local-conversion method 2026-11-16) keeps the lesson-audio sessions local; the channel mirror is provenance, not the delivery method.
- External re-host of the media (R2 delivery) — discarded: R2 is not a valid delivery path for this repo (ruling of record 2026-11-12).
- Track the mp3s in place via `!*.mp3` per-dir negation (the demo-audio class's method, CherokeeLessons#30 R-2) — discarded: the method of record for the lesson-audio page class is the AAC-LC conversion through the ladder (developer decision 2026-11-16); mp3-tracking is the demo-audio class's separate disposition.
- Remove the audio from the page — discarded: abandons the page's purpose (the animal-name sessions).
- Wait for the generator repository's own export change and regenerate through it — discarded: this conversion imports the owning definition's settings inline (the recorded design of the CherokeeLessons#54 disposition); the page generator here is the page-editing tool, not the production deck pipeline.

**Key Design Decisions.** (1) In-place per-track transcode with recorded ladder transcripts — the transcripts are the evidence for the ladder-exercised criterion; final encoded size, not nominal rate, decides the rung (owning definition: audio-lessons-generator-python#16 — its 2026-11-15 trials: quiet mono speech undershooting nominal targets, effective ~46–59 kb/s, source information ceiling ~57 kb/s). (2) The generator script is retained (developer decision 2026-11-16 — regeneration tooling needed regardless of media format; the change touches emitted names only) — no zip emission exists in this script (verified: no zip reference on the animal-names page or in scope). (3) The mp3 retirement is working-tree-only; the git delta is page + script + new tracked m4a.

**User Intent / Original Prompt.**
- (2026-11-11, observed failure) the executed stand-up-gate build omitted every untracked asset class — animal-names' 9 embeds among them.
- (2026-11-16, developer decision recorded on CherokeeLessons#54) animal-names joins CLL-1's conversion method: local tracked `.m4a` via the per-track ladder — not YouTube.
- (2026-11-16, developer decision) the lesson-audio generator scripts are retained — the m4a/mp3 distinction touches emitted names only.
- (2026-11-20, developer directives) "the changes related to non-dictation videos needs to be moved into their own specs so as to prevent conflation and cross-concerns" (this split); "proceed with spec revisions".
- (2026-11-21, PR-review feedback) "the page reads 'mp3'" (stale table header copy); "we need a way for users to download these files easily"; disposition set same day: direct download links (no per-file zips — a single-file zip of AAC gains nothing), arrow glyph + the word "download", tested, in a 3rd column of the session table.

## Not Included

- The dictation migration (CherokeeLessons#54's remaining dictation slice) and the CLL-1 conversion (CherokeeLessons#58 — sibling part from the same re-cut; CLL-1 alone carries the zip-link removal).
- The TTS demo-audio dirs — the umbrella's tracking work (CherokeeLessons#30 R-2, tracked mp3).
- The generator's own export change (audio-lessons-generator-python#16) — separate repository; this conversion imports its settings inline.
- The umbrella's site-wide gates (CherokeeLessons#30 SC-7/SC-8 — execute after the last part lands).
- A "download all" affordance — the bundle's own disposition (drop vs re-issue) stays the umbrella's open decision (CherokeeLessons#30 seam 5); this spec adds per-session direct-download links only.

## Requirements

- **R-1 — In-place conversion.** The 9 `animals-NNNN.mp3` (measured source: 31 MB) SHALL be transcoded to `animals-NNNN.m4a` — AAC-LC in MP4 container, mono, 48 kHz — through the per-track ladder: 128 kb/s target falling back 112 → 96 kb/s on encode-and-measure (the encoded file's measured size decides) until the file is ≤ 24 MiB (25,165,824 B — the margin below the 25 MiB per-file asset cap). The 9 `.m4a` files SHALL be tracked in git; the page's 9 embeds SHALL reference the `.m4a` names; the 9 mp3s SHALL be deleted from the working tree. The per-track ladder transcript (rungs attempted, measured sizes at each, chosen rung), the transcode command, and tool provenance SHALL be recorded in this issue's research artifacts (`.issues/59/research/`).
- **R-2 — Generator retained and updated.** The tracked generator script `content/lesson-audio/animal-names/process_end_notes.py` SHALL be retained and SHALL emit `.m4a` embed names matching R-1's filenames. Page regeneration stays format-agnostic over its page-structure inputs (`end-notes.txt`; `index-template.txt`); media production remains the generator's own assembly (audio-lessons-generator-python#16).
- **R-3 — Session-table copy: header correction + download column.** The page's session table SHALL carry the header row `SESSION|M4A FILE|DOWNLOAD` (the stale `MP3 FILE` header corrected to the converted format), the divider row extended to three columns, and each of the 9 session rows SHALL gain a third cell — a direct-download anchor in raw-HTML form `<a href="animals-NNNN.m4a" download>⤓ download</a>` (direct `.m4a` link with the HTML `download` attribute; arrow glyph plus the word "download") — per the developer's 2026-11-21 disposition: no per-file zip artifacts are created. `index-template.txt`'s table surface stays the generator's input; the generator (R-2) SHALL emit the same header and row form.

## Success criteria

| ID | Criterion | Class | Instrument |
|---|---|---|---|
| SC-1 | The page references only `.m4a` media: `audio (file=` embeds = 9, all 9 names `animals-NNNN.m4a`, `.mp3` name count = 0 | structural | grep counts on the page source |
| SC-7 | The session table carries the corrected header and the download column: `M4A FILE` header present, `MP3 FILE` count = 0 on the page and in both generator strings, `DOWNLOAD` header present, 9 anchors — each `<a href="animals-NNNN.m4a" download>` carrying the `download` attribute and the `⤓ download` label, one per session row | structural | grep counts on the page source and the generator's emitted-row form |
| SC-8 | A clicked download anchor serves the row's `.m4a` for download (browser save, not navigation) | behavioral | browser test on the built page: click one of the 9 anchors on the served page, observe the download event with the matching filename |
| SC-2 | Exactly the 9 converted files are tracked and the mp3s are gone from the working tree | structural | `git ls-files 'content/lesson-audio/animal-names/*.m4a'` lists exactly `animals-0001.m4a`…`animals-0009.m4a`; `find <page dir> -name "*.mp3"` is empty; `git check-ignore` on an m4a exits non-zero (not ignored) |
| SC-3 | Every m4a is AAC-LC, mono, 48 kHz, ≤ 25,165,824 B, in an MP4 container | structural | per file: `ffprobe` shows `codec_name` aac, `channels` 1, `sample_rate` 48000, `format_name` containing mov/mp4; `stat -c %s` ≤ 25,165,824 |
| SC-4 | The conversion exercised the ladder per track — each file's final encode is the highest rung (128 → 112 → 96 kb/s) whose encoded output measured ≤ 24 MiB; no track silently pre-lowered | behavioral | the recorded per-track transcripts in `.issues/59/research/`; per-file cross-check: `ffprobe` `bit_rate` corresponds to the recorded chosen rung |
| SC-5 | The generator is retained and emits the new form | structural | `git ls-files` lists the script; source greps: `.mp3` name emission = 0, `.m4a` emission present |
| SC-6 | The page builds and serves its converted media | behavioral | `./build.sh` exit 0; `public/lesson-audio/animal-names/` present with the page and exactly 9 `.m4a` files |

## Dependencies

| Reference | Relationship | Status |
|---|---|---|
| CherokeeLessons#30 (umbrella) | this part closes into the umbrella's composition; its site-wide gates execute after the last part lands | pending (umbrella open) |
| audio-lessons-generator-python#16 | owning definition of the encoder settings this conversion imports (AAC-LC/mono/48 kHz; 128→112→96 ladder; trials bound the landing) | satisfied (fetched 2026-11-20) |
| ffmpeg / ffprobe toolchain | the transcode's tool — `/snap/bin/ffmpeg` present; `ffprobe` absent (verified 2026-11-20) and provisioned at implement time | pending (implement-time provisioning) |
| Channel mirror (`@-cherokeelanguage7554`, Animals map section) | source of record for re-obtain/rollback | satisfied (verified live 2026-11-11) |
| CherokeeLessons#54 (parent) · CherokeeLessons#58 (sibling) | the re-cut's hierarchy — independent content sets; no ordering constraint | pending (approval states) |

## Traceability

| Requirement | Success criteria |
|---|---|
| R-1 | SC-1, SC-2, SC-3, SC-4, SC-6 |
| R-2 | SC-5 |
| R-3 | SC-7, SC-8 |

## Documentation Sources

| Source | Type | Location | Verification |
|---|---|---|---|
| Owning definition of the encoder settings (trials, ladder, cap margin) | remote spec | audio-lessons-generator-python#16 | fetched live via `gh` 2026-11-20 |
| Animals track mirror map | research artifact | `.issues/30/research/youtube-video-id-map.txt` (Animals section — 9 entries, verified 2026-11-11) | read 2026-11-20 |
| Source media measurements | working tree | `content/lesson-audio/animal-names/*.mp3` — 9 files, 31 MB | `ls`/`du` executed 2026-11-20 |
| Page embed form | repo file | `content/lesson-audio/animal-names/index.md` (embeds line 41+; no zip reference) | grep executed 2026-11-20 |
| Asset cap + ignore rules | repo facts | Workers 25 MiB/file (`AGENTS.md`); global `*.mp3`/`*.mp4` ignore, m4a unignored — `git check-ignore` to confirm at implement time | read 2026-11-20 (check-ignore pending implement-time run, asserted by SC-2) |

## Cost Frame

- SC-1 / SC-2 / SC-5: greps and git listings — minutes; skipping risks dead embeds surviving the deploy or a regeneration re-emitting names that no longer exist.
- SC-3 / SC-4: 9 encodes with per-file probes — the conversion's own runtime; skipping risks over-cap files or unverifiable ladder behavior.
- SC-6: one full site build; skipping risks shipping the page without its media or a template error in the new names.

## Edge Cases

| Condition | Expected behavior | Resolution |
|---|---|---|
| A track's encoded size exceeds 24 MiB even at the 96k floor | doubly unexpected — the 12.8–29.9-minute tracks project to the 128k rung (≈ 5.1–11.2 MB per the owning definition's 2.3–2.4× trial measurements) | if it occurs: the transcript records the measured floor size and the track's completion waits on the owning definition gaining a lower rung (audio-lessons-generator-python#16) — this conversion does not widen the ladder unilaterally |
| `ffprobe` absent from the environment | implement-time provisioning per the Dependencies table | ffmpeg is present (`/snap/bin/ffmpeg`); `ffprobe` joins it before the verification run |
| `git add` of media silently no-op'd by the global ignore rules | SC-2's `ls-files`/`check-ignore` assertions verify tracking truth, not disk presence | the repo's recorded media-add pattern (AGENTS.md): confirm with `git status`/`git check-ignore` |
| An mp3 missing from the working tree at implement time | the mirror re-obtains it first | the map's Animals section is the correspondence record; the completion gate keeps the conversion all-or-nothing |
| Generator re-run after the conversion | page regeneration emits the `.m4a` names (post-R-2) | the generator's media-assembly output is unchanged by this spec (owning definition governs) |

## Completion gate

Every SC passes before the deliverable is complete — all-or-nothing: the conversion is complete only when all 9 files are converted, tracked, ladder-transcribed, page-referenced, and built; a partial state (some tracks converted, some pending) fails SC-2 as a set.

<!-- artifacts: .issues/59/ -->
