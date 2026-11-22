# Plan — CherokeeLessons#58: CLL-1 audio exercises — 23 session mp3s convert to tracked AAC `.m4a` with the #59 table form

## Goal

Implement the CherokeeLessons#58 spec: transcode the 23 untracked `cll1-v3-NNNN.mp3` sources (419 MB, verified in the working tree) in place to tracked AAC `.m4a` through the per-track bitrate ladder, retire the mp3s from the working tree, retarget the page's 23 `{{audio}}` embeds to `.m4a` names, remove the dead `cll1-v3-audio.zip` line (the zip file itself stays on disk untracked — the bundle's disposition is the umbrella's open decision, CherokeeLessons#30 seam 5), update the session table to the completed CherokeeLessons#59 form (`SESSION|M4A FILE|DOWNLOAD` with per-row `⤓ download` anchors), update the retained generator to emit that form with `.m4a` names and no zip emission, and record the ladder transcripts. Answers CherokeeLessons#58 (split from CherokeeLessons#54, part of the CherokeeLessons#30 recomposition); touches `content/lesson-audio/cherokee-language-lessons-1---audio-exercises/` (23 files converted, `index.md`, `process_end_notes.py`) and `.issues/58/research/`.

## Execution surface

Repo: the session's project root (CherokeeLessons, Zola static site). Branch: `feature/58-cll1-audio-m4a` off verified trunk tip `a58df4d` (master == origin/master, verified live 2026-11-21). Tools: `./build.sh` (the sanctioned build); ffmpeg/ffprobe are the vendored static 7.0.2 builds at `.tools/ffmpeg` / `.tools/ffprobe` (AGENTS.md: `/snap/bin/ffmpeg` is broken in this environment and never used; `-nostdin` is always passed — the #67 stdin-consuming hang). Issue-store work (`plan.md`, research transcripts) happens in the `.issues/` linked worktree on its own `issues-data` branch, not on the feature branch. No registered submodules exist (.gitmodules absent — pointer gates N/A). The ladder's owning definition is audio-lessons-generator-python#16 (AAC-LC, mono, 48 kHz; 128 → 112 → 96 kb/s encode-and-measure; ≤ 24 MiB target under the 25 MiB Workers cap). The #59 conversion's transcript and download-column evidence (`.issues/59/research/`) are the pattern of record.

## Items — one per success criterion

**Item 1 — media conversion, tracking, and mp3 retirement (SC-2, SC-3, SC-4)**
- **Deliverable:** the 23 `cll1-v3-NNNN.mp3` transcoded in place to `cll1-v3-NNNN.m4a` (AAC-LC in MP4 container, mono, 48 kHz, each ≤ 25,165,824 B) via the per-track ladder 128 → 112 → 96 kb/s encode-and-measure — the highest rung whose encoded output measured ≤ 24 MiB is kept, no track pre-lowered; the 23 `.m4a` tracked in git (m4a is not ignored — `git check-ignore` confirmed non-zero exit per SC-2); the 23 mp3s deleted from the working tree (working-tree-only retirement per spec key-decision 3 — the git delta carries only page + script + new m4a; the global `*.mp3` ignore means mp3 deletion produces no git change, verified by `git status` staying clean of mp3 rows); per-track ladder transcripts (rungs attempted, measured size at each, chosen rung, the ffmpeg command, tool provenance `.tools/ffmpeg` static 7.0.2 with `-nostdin`) recorded to `.issues/58/research/ladder-transcript.md` on the issues-data branch.
- **RED:** `git ls-files 'content/lesson-audio/cherokee-language-lessons-1---audio-exercises/*.m4a'` is empty; `find <dir> -name '*.m4a'` is empty; `find <dir> -name '*.mp3'` shows 23 (pre-conversion state).
- **GREEN:** per-track loop: for each of the 23 tracks, run `.tools/ffmpeg -y -nostdin -loglevel error -i cll1-v3-NNNN.mp3 -ac 1 -ar 48000 -c:a aac -b:a <rung> cll1-v3-NNNN.m4a` starting at 128k, `stat -c %s` the output, fall back 112 → 96 on > 24 MiB; `git add` the 23 m4a (confirm with `git status` — ignored-media add is a silent no-op); `rm` the 23 mp3 sources only after the completion-gate's all-or-nothing state is met (all 23 converted, probed, tracked).
- **Instrument:** per file: `.tools/ffprobe -v error -select_streams a:0 -show_entries stream=codec_name,channels,sample_rate,bit_rate -show_entries format=format_name -of default=noprint_wrappers=1` (aac / 1 / 48000 / mov,mp4,m4a) and `stat -c %s` ≤ 25,165,824; `git ls-files` lists exactly `cll1-v3-0001.m4a`…`cll1-v3-0023.m4a`; `find <dir> -name '*.mp3'` empty; `git check-ignore <one m4a>` exit non-zero; transcript cross-check: `ffprobe` `bit_rate` corresponds to each file's recorded chosen rung.
- **Instrument class:** fact checks + a recorded artifact (the transcript file).

**Item 2 — page embed retargeting, zip-line removal, table copy (SC-1, SC-7 page part)**
- **Deliverable:** `index.md` — the 23 `{{audio (file="cll1-v3-NNNN.mp3")}}` embeds retargeted to `.m4a` names; the zip line 51 (`Download all files as a single zip: [cll1-v3-audio.zip](cll1-v3-audio.zip).`) removed; the header corrected `SESSION|MP3 FILE` → `SESSION|M4A FILE|DOWNLOAD`; the divider extended to `---|--|--`; each of the 23 session rows gains the third cell `<a href="cll1-v3-NNNN.m4a" download>⤓ download</a>`; the 15 interleaved note rows (`----| Complete the lesson material…`) stay two-cell, unchanged. No other prose changes.
- **RED:** grep counts on `index.md`: `audio (file=` = 23 all `.mp3`; `cll1-v3-audio.zip` = 1; `MP3 FILE` = 1; `DOWNLOAD` = 0; `⤓ download` anchors = 0.
- **GREEN:** edit the page: rename the 23 embed names, delete the zip line, re-header the table, extend the divider, add the 23 anchor cells in the spec's exact form (the #59 `animal-names/index.md` form of record).
- **Instrument:** grep counts — `audio (file=` = 23 all `cll1-v3-NNNN.m4a`, `.mp3` name count = 0, `cll1-v3-audio.zip` count = 0, `M4A FILE` present, `MP3 FILE` count = 0, `DOWNLOAD` header present, 23 anchors each `href="cll1-v3-NNNN.m4a" download>` with the `⤓ download` label, note-row count 15 with no anchor content.
- **Instrument class:** fact checks (greps).

**Item 3 — generator retained and updated (SC-5, SC-7 generator part)**
- **Deliverable:** `process_end_notes.py` retained (developer decision 2026-11-16), its session-row emission gaining the third-cell anchor and `.m4a` names, its table header emitted as `SESSION|M4A FILE|DOWNLOAD` with the three-column divider, its zip-link text line and `pathlib.Path("cll1-v3-audio.zip").unlink(...)` + `subprocess.run(["zip", …])` steps dropped. Page regeneration stays format-agnostic over `end-notes.txt`; note-row emission unchanged. The zip file itself is not deleted from the working tree (its disposition is the umbrella's open decision).
- **RED:** grep on `process_end_notes.py`: `cll1-v3-{number}.mp3` emission present; `cll1-v3-audio.zip` present on three lines — line 84 the zip-link text, line 116 the `pathlib.Path(...).unlink()` step, line 117 the `subprocess.run(["zip", …])` step (grep -c = 3); `zip -j` present; `SESSION|MP3 FILE` present; `.m4a` absent.
- **GREEN:** minimal edits per spec: the emitted header/divider to the 3-column form, `below_fold_md +=` row emission producing `{number}|{{audio (file="cll1-v3-{number}.m4a")}}|<a href="cll1-v3-{number}.m4a" download>⤓ download</a>`, note-row emission untouched, zip link line and unlink+zip subprocess lines removed.
- **Instrument:** `git ls-files` lists the script; greps: `.mp3` name emission = 0, `cll1-v3-audio.zip` = 0, `zip -j` = 0, `SESSION|M4A FILE` and `.m4a` emission present, `⤓ download` present in emission strings.
- **Instrument class:** fact checks (greps + git listing).

**Item 4 — browser download-behavior check (SC-8)**
- **Deliverable:** executed evidence that a clicked download anchor serves the row's `.m4a` as a download (browser save, not navigation) — one of the 23 anchors on the built, served page; recorded to `.issues/58/research/download-column.md` on the issues-data branch.
- **RED:** run the browser check against the current state (or assert statically first): the page carries 0 `⤓ download` anchors and no `.m4a` files exist in the dir, so the behavioral check "click one of the 23 anchors, observe a download event with the matching filename" fails today — the executed browser click cannot observe such an event.
- **GREEN:** none in the repo — executed observation evidence, recorded to the research file (ephemeral scratch per the test-discipline posture; never an authored test artifact).
- **Instrument:** browser test on a local serve of the built site (playwright-cli managed browser; serve `public/` via `python3 -m http.server`, per the #59 evidence pattern; the build precondition is Item 5's executed build): click one session anchor, observe the download event with the matching `cll1-v3-NNNN.m4a` filename, page URL unchanged; size/byte-compare the downloaded file against the tracked source. Tier: none authored (observation only).
- **Instrument class:** recorded dispatch evidence (playwright session output pasted to the research file).

**Item 5 — full-build gate (SC-6)**
- **Deliverable:** a clean `./build.sh` (exit 0) on the feature branch; `public/lesson-audio/cherokee-language-lessons-1---audio-exercises/` present with the page and exactly 23 `.m4a` files; no `.m4a` omitted by the asset pipeline.
- **RED:** the current build's `public/…/` carries no `.m4a` files (the untracked mp3 era produced none).
- **GREEN:** follow-on effect of Items 1–2; the item executes and records the build.
- **Instrument:** `./build.sh` exit status; `find public/lesson-audio/cherokee-language-lessons-1---audio-exercises/ -name '*.m4a' | wc -l` = 23 and the page present.
- **Instrument class:** fact checks + executed build output.

## Dependency order

1 → 2 → 3 → 5 → 4: Item 1 produces the `.m4a` files every later reference targets; Item 2's page anchors depend on Item 1's names and its removal of the mp3 embeds; Item 3's emission change must match the delivered page form established in Item 2 (spec R-4: generator emits the same form); Item 5's build gate runs after 1–3 so the built state is the composed media-bearing state; Item 4's browser click is Item 5's build's direct consumer — its execution reads Item 5's build output as its precondition. No cycles: no item's RED or GREEN depends on a later item's change (Item 4 authors no repo change; its evidence is recorded observation only).

## Traceability

| Item | SC(s) |
|---|---|
| 1 | SC-2, SC-3, SC-4 |
| 2 | SC-1, SC-7 (page part) |
| 3 | SC-5, SC-7 (generator part) |
| 4 | SC-8 |
| 5 | SC-6 |

Every SC covered: SC-1(2), SC-2(1), SC-3(1), SC-4(1), SC-5(3), SC-6(5), SC-7(2+3), SC-8(4). Every item traces to ≥1 SC.

## Verification summary

- Fact checks (greps, git listings, ffprobe, stat, check-ignore): Items 1–3, 5.
- Recorded dispatch evidence (playwright download observation): Item 4.
- Recorded artifact (ladder transcript, research file): Item 1.
- No authored tests exist (this is a Zola static site — no test suite; `./build.sh` exit 0 is the verification gate per AGENTS.md). Behavioral SC-4 verifies through the recorded transcripts + ffprobe cross-check; behavioral SC-8 through executed browser observation; behavioral SC-6 through the executed build.
- Cycle-time runs: Items 1–5 in dependency order; the ladder transcripts and download-column evidence paste into `.issues/58/research/` on the issues-data branch before PR prep.

## Status boundary

No status fields in this body; implementation state lives in the pipeline and the issue's validation record.
