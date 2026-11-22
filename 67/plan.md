# Plan — CherokeeLessons#67: tracked TTS-sample pages → tracked AAC `.m4a` + per-entry download links

## Goal

Convert the four TTS-sample pages' 126 mp3s in place to tracked AAC `.m4a` (per-track ladder), retarget every embed and word-link, add the #59-form `⤓ download` anchors, retire the 122 tracked mp3s, track the 4 espeak clip sources via negation, clear the tacotron negation residue, and record the umbrella seam verification. Spec: `.issues/67/spec.md` (single source of truth — every item below traces to its SCs).

## Execution surface

- Working tree, feature branch off verified trunk tip `9884cf0` (master == origin/master, verified 2026-11-21; no registered submodules).
- Dirs: `content/content/espeak-ng-and-ced-phonetics/`, `content/content/tacotron2-and-cherokee-tts/`, `content/content/here-is-why/`, `content/content/tts-update--2/`.
- Tools: `/snap/bin/ffmpeg` (present); `ffprobe` (absent per spec — provisioned as item 1's setup step, spec Dependencies table row "ffmpeg / ffprobe toolchain").
- Issue-store artifacts (`.issues/67/research/`) are workspace-local issue-store data — not feature-branch commits.

## Dependency order (why each item precedes its dependents)

Item 1 precedes item 2 because the ladder's `.m4a` set must exist (and its transcripts be recorded) before the tracking-state assertions can count it. Item 2 precedes item 3 because the pages must reference files whose tracked names exist. Item 3 precedes item 4 because the pages' `.m4a` references must resolve against the working tree before a build can serve them. Item 4 precedes item 5 because SC-6's browser click-through runs against `public/`. Item 6 is independent (read-only grep at any point before verify); the fresh-context verify (after item 5) consumes all prior state.

## Items

1. **Item 1 — SC-3 + SC-4: transcode the 126 mp3s via the per-track ladder.**
   - Deliverable: 126 `.m4a` files beside their sources, one per mp3 (sources' basenames, extension changed), plus the per-track ladder transcript.
   - Setup step (rides this item; no separate item): provision a static `ffprobe` binary — `ffmpeg` is present at `/snap/bin/ffmpeg` but `ffprobe` is absent (spec Dependencies, verified 2026-11-21). Instrument for the setup: `ffprobe -version` exits 0 before the batch runs.
   - RED: current media is mp3 — ffprobe reports codec `mp3` on a sample and `git ls-files` shows 0 tracked `.m4a` (fails SC-3 today).
   - GREEN: for each of the 126 tracks, AAC-LC in MP4 container, mono, 48 kHz, rungs 128 → 112 → 96 kb/s, encode-and-measure at each rung until the encoded file's measured size is ≤ 25,165,824 B per spec R-1 (the encoded size — not nominal rate — decides the rung; 128k expected landing at these sizes). Append each track's transcript (track, rungs attempted, measured size at each, chosen rung, transcode command, tool provenance) to `.issues/67/research/ladder-transcript.md`.
   - Tier notes: verification commands are ephemeral structural observation (run in-session, output captured) — no authored test files; the transcript is a spec-owned research artifact (SC-4's instrument).
   - Instrument: batch ffprobe over **all 126** `.m4a` outputs — `codec_name=aac`, `channels=1`, `sample_rate=48000`, `format_name` containing mov/mp4; `stat -c %s` ≤ 25,165,824 B per file; per-file `bit_rate` cross-checked against its recorded chosen rung (SC-4).
2. **Item 2 — SC-2 + SC-7: tracking-state artifacts.**
   - Deliverable: `content/content/espeak-ng-and-ced-phonetics/.gitignore` (carrying `!*.mp3`) added and the 4 clip source mp3s tracked; `content/content/tacotron2-and-cherokee-tts/.gitignore` deleted; 122 tracked mp3s deleted from the working tree; 126 `.m4a` tracked; clip sha256 digests + sizes recorded in `.issues/67/research/clip-provenance.md`.
   - RED: `git ls-files` in the four dirs shows 122 tracked mp3 / 0 tracked `.m4a`; the 4 clip sources are untracked; `git ls-files` shows the tacotron `.gitignore` tracked (fails SC-2/SC-7 today).
   - GREEN: gitignore creation/deletion and media adds per AGENTS.md's recorded media-add pattern — every `git add` confirmed with `git status`/`git check-ignore` before being counted.
   - Instrument: spec SC-2's exact assertions — `git ls-files` per dir = 100/18/5/3 `.m4a` (espeak also carrying exactly the 4 named clip-source mp3s; the other three dirs 0 tracked mp3); `find` on-disk `.mp3` across the four dirs = exactly the 4 whitelist names; `git check-ignore` on an m4a exits non-zero; `git check-ignore -v` on a clip source shows the `!*.mp3` negation; the tacotron ignore absent from `git ls-files` and the tree (`test -e`).
3. **Item 3 — SC-1 + SC-5: page edits.**
   - Deliverable: the four `index.md`s fully retargeted + anchor-completed. Before this item runs, extract the two word-link pages' current Cherokee-word label lists (pre-edit snapshot) for the preservation diff.
   - RED: the four pages reference `.mp3` in every embed (100/18/5/3) and carry 0 download anchors (fails SC-1/SC-5 today).
   - GREEN: every `{{audio …}}` embed retargets `*.mp3` → `*.m4a`; the two word-link pages' media links (espeak 100, here-is-why 5) retarget with labels unchanged; each media entry gains `<a href="<file>.m4a" download>⤓ download</a>` placed with its entry; no zip artifacts created.
   - Instrument: grep counts on the four page sources — `.mp3` references = 0; `.m4a` embeds = 100/18/5/3; anchors = 100/18/5/3 each in the #59 form; pre/post label lists equal (SC-5's preservation check).
4. **Item 4 — SC-8: build.**
   - Deliverable: a clean build serving the converted media.
   - RED: n/a as an assertion — the build is green pre-change; the SC's meaning is post-change. RED basis recorded: build exit 0 at trunk tip (verified when the branch is cut).
   - GREEN: `./build.sh` exits 0 after all prior items land.
   - Instrument: exit code 0; the four pages' `public/` dirs carry exactly the 100/18/5/3 `.m4a` files.
5. **Item 5 — SC-6: browser download test.**
   - Deliverable: executed browser click-through evidence.
   - RED: the download anchors are a new affordance — no clicked-download evidence can exist for them (SC-6 untested today).
   - GREEN: local serve of `public/` (browser via the playwright-cli card's vendored tooling), click one anchor per page (4 total), observe the download events with matching filenames. Evidence saved to `.issues/67/research/download-evidence.md`.
   - Tier: behavioral, on-demand for this change's verification only — no authored test file.
   - Instrument: the four download events observed.
6. **Item 6 — SC-9: umbrella seam verification.**
   - Deliverable: executed grep evidence that `.issues/30/spec.md` carries the seam.
   - RED basis recorded: the seam elements were verified present during planning (2026-11-21) — if a grep fails now it is a finding, not a new edit; this item is verification-only (no GREEN change to the umbrella beyond what its own 2026-11-21 revision already records).
   - GREEN: n/a — verification-only item; any failure halts for direction (the umbrella is another issue's artifact).
   - Instrument: grep `.issues/30/spec.md` for R-2's #67 descope, the #68 supersession under the 2026-11-21 new-audio-standard directive, and both Parts entries (#67 and #68).

## Traceability

| Spec requirement | Item(s) |
|---|---|
| R-1 (SC-1, SC-2, SC-3, SC-4, SC-8 — conversion/Tracking/Build) | Items 1, 2, 4 |
| R-2 (SC-1, SC-5, SC-6 — page references + anchors) | Items 3, 5 |
| R-3 (SC-7 — residue cleared) | Item 2 |
| R-4 (SC-9 — umbrella seam) | Item 6 |

## Verification summary

Structural instruments (greps, git listings, ffprobe batch, build) run in-session with captured output; SC-6 runs the browser click-through (playwright-cli); SC-4's behavioral bar is the recorded transcript cross-check. Then the fresh-context `verify` PASS per the PR card's checklist.

## Out of scope

The #68 demo-audio class (41 mp3s), lesson-audio pages, any generator scripts (none exist — verified), any aggregate-download artifact.
