# Plan — CherokeeLessons#76: complete the conversions for consistency (the 45 source-backed tracks re-encoded at the 192k rung)

## Goal

Implement CherokeeLessons#76's spec: re-encode the 45 source-backed `.m4a` deliverables (animal-names demo 24, ims-toucan demo 9, tts-update-3 8, espeak-ng clips 4) at the 192k rung from their git-history-recovered original mp3s, replacing them in place under unchanged filenames; record per-track provenance; keep the #74 state (zero mp3s, no negation files) and every display invariant; record the 192k standard in AGENTS.md and correct the umbrella's stale seam-2; verify by probes, build, and browser click-through.

## Execution surface

Repository: CherokeeLessons (this repo, trunk `master`). Branch: `feature/76-192k-reencode` (verified per the git-workflow-branch card at branch time). Tool home: `.tools/ffmpeg` and `.tools/ffprobe` (static 7.0.x, `-nostdin` mandatory per AGENTS.md's recorded hang). Temp workspace for recovered originals: `/tmp/opencode/76-recovered/` (never enters git). Media dirs: `content/content/{cherokee-animal-names-tts-demo-audio-1, cherokee-animal-names--ims-toucan-tts-, cherokee-tts-update-3--ims-toucan-, espeak-ng-and-ced-phonetics}/`. Build gate: `./build.sh`. No authored tests: this repo has no test suite — every instrument is a direct executed check (fact checks, greps, probes, build, browser observation); scratch observations live out of test files (ephemeral class).

## Items — one per SC

**Item 1 — History recovery + provenance skeleton (SC-5).**
- **Deliverable:** the 45 original mp3s recovered from the pre-drop commit into `/tmp/opencode/76-recovered/` (sha256-verified against the pre-drop tree) and the provenance transcript skeleton in `.issues/76/research/encode-transcript.md` (one row per track: source commit SHA, path, sha256, [rung/sizes pending item 2]).
- **RED:** the transcript file does not exist and `/tmp/opencode/76-recovered/` is empty — no recovered set has been verified against the pre-drop tree.
- **GREEN:** resolve the drop commit (`git log --all --diff-filter=D -- <path>` — this names the deletion-commit SHA itself, e.g. 27191cf for #74's drop), then recover from its **parent**: `git cat-file -p <drop-sha>^:<path>` (the blob lives at the parent ref, verified live 2026-11-22 — `git cat-file -p <drop-sha>:<path>` errors `path does not exist`), sha256-verify each against the pre-drop blob, record the rows.
- **Tier marks:** none (no authored tests; instrument class: fact checks + recorded file).
- **Instrument:** the transcript contains 45 rows with a resolved SHA; `sha256sum` of each recovered file matches the blob recorded in the transcript row; `ls /tmp/opencode/76-recovered | wc -l` counts 45.

**Item 2 — The 45 encodes at the recorded rung (SC-2, SC-3).**
- **Deliverable:** `/tmp/opencode/76-encoded/<dir>/<name>.m4a` for all 45 (or the decided subset), via `.tools/ffmpeg -y -nostdin -loglevel error -i IN.mp3 -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT.m4a`, the cap gate applied per track (fallback 160 → 128 encode-and-measure; a 128k-floor over-cap track is floor-excluded — its existing deliverable retained — with all attempted rungs' measured sizes recorded), and the transcript rows completing the rung/size/outcome columns.
- **RED:** the encoded set is absent (item 1's precondition holds).
- **GREEN:** encode each recovered original; measure (`stat`, `.tools/ffprobe`); apply the cap gate; complete the transcript rows (rung(s) attempted, sizes, outcome, command, tool version).
- **Tier marks:** none (fact checks + recorded file).
- **Instrument:** the transcript's outcome column is filled for 45/45 rows (re-encoded / fallback / floor-excluded); each file's probe (codec aac, channels 1, sample_rate 48000, mov/mp4, size ≤ 25,165,824 B) passes for the re-encoded set.

**Item 3 — In-place replacement + invariants (SC-1, SC-4).**
- **Deliverable:** the decided `.m4a` set copied over the tracked deliverables (exact names); a clean `git status` showing only the decided m4a paths modified (plus AGENTS.md and the umbrella note from item 4, staged separately); all SC-4 invariants re-measured.
- **RED:** `git diff --stat` is empty (no media change yet) — the in-place state does not exist.
- **GREEN:** `cp` the decided encoded set over the working-tree deliverables; confirm names unchanged; `git status --porcelain` shows exactly the decided m4a paths; re-run the SC-4 count battery.
- **Tier marks:** none (fact checks).
- **Instrument:** `git diff --name-only` = the decided m4a set (+ AGENTS.md after item 4); the SC-4 greps/counts (199 tracked m4a, 0 mp3s, anchors/tables/fragment unchanged) all hold.

**Item 4 — Records currency (SC-7).**
- **Deliverable:** AGENTS.md's media-tooling section gains the 192k statement ("the source-backed tracks' 2026-11-22 pass: -b:a 192k per CherokeeLessons#76; higher bitrates acceptable per the developer's disposition; the 128k example form remains the generic template"); `.issues/30/spec.md`'s seam-2 corrected to the post-#74 truth (sources dropped, history-retained; #76's pass re-encodes the source-backed deliverables at the 192k rung).
- **RED:** grep AGENTS.md for the 192k statement fails; `.issues/30/spec.md` seam-2 still reads the pre-#74 source-preservation wording.
- **GREEN:** the two edits.
- **Tier marks:** none (grep fact checks).
- **Instrument:** `grep -c '192k' AGENTS.md` ≥ 1 with the #76 reference in the sentence; grep `.issues/30/spec.md` for "CherokeeLessons#76" in seam 2 present.

**Item 5 — Build gate (SC-6).**
- **Deliverable:** a clean build of the composed state.
- **RED:** the working tree's media changes are unverified in a build (the last build predates item 3).
- **GREEN:** run `./build.sh`; check `public/` carries the four dirs' m4a (100/18/5/3 and 24/9/8) with the re-encoded bytes present (`public` copies the working tree's current files).
- **Tier marks:** none (build gate + built-tree checks).
- **Instrument:** `./build.sh` exit 0; `ls public/content/content/cherokee-tts-update-3--ims-toucan-/*.m4a | wc -l`-style counts match 24/9/8/100 per dir.

**Item 6 — Browser click-through (SC-8).**
- **Deliverable:** the download-event observation on a served demo page anchor with the re-encoded file, recorded to `.issues/76/research/click-through.md`.
- **RED:** no click-through record exists for the re-encoded state.
- **GREEN:** serve `public/` (http.server), playwright click one demo-page anchor (e.g. animal-demo `#0001` row), observe `Downloading file Animals_2021-05-31_0001.m4a`, md5-verify the download against the tracked deliverable, close the browser session.
- **Tier marks:** none (recorded dispatch; ephemeral observation).
- **Instrument:** the recorded events (download + matching filename + md5 identity) in `.issues/76/research/click-through.md`.

**Item 7 — Final SC-1 sweep + completion record (SC-5 closure).**
- **Deliverable:** the transcript finalized (45/45 rows complete; outcome columns filled; exclusions, if any, carry all attempted rung sizes) and the SC-1 diff sweep recorded.
- **RED:** SC-1's `git diff --name-only` battery has not been executed against the final staged state.
- **GREEN:** run the battery; confirm only the decided m4a paths + AGENTS.md on the main branch; the store edits (umbrella note) ride the issues-data worktree separately.
- **Tier marks:** none (fact checks).
- **Instrument:** the sweep output recorded; SC-1's zero-non-listed-paths check passes; 45/45 transcript rows complete.

## Dependency order

1 → 2 → 3 → 4 → 5 → 6 → 7, strictly: item 2 needs item 1's recovered originals; item 3 needs item 2's encoded set (the decided list is item 2's transcript output); item 5 needs item 3's replaced media so the built tree is the composable state; item 4 is independent of 1–3's media but ordered after 3 so AGENTS.md's 192k statement records the delivered state; item 6 needs item 5's served build; item 7's battery is the completion sweep over 1–6's final state. No cycles.

## Traceability

| Item | SCs |
|---|---|
| 1 | SC-5 |
| 2 | SC-2, SC-3 |
| 3 | SC-1, SC-4 |
| 4 | SC-7 |
| 5 | SC-6 |
| 6 | SC-8 |
| 7 | SC-1, SC-5 (closure) |

## Verification summary

Fact checks (items 1, 3, 4, 7: greps, sha256, git diff/status, git ls-files/find), probes (item 2: ffprobe/stat per file), build gate (item 5: `./build.sh` + built-tree counts), recorded dispatch (item 6: browser observation to `.issues/76/research/click-through.md`), recorded file (items 1–2: the encode transcript). No authored tests — the repo has no test suite; every instrument is a directly executed check per the spec's SC instruments.

## Status boundary

No status fields in this plan; implementation state lives in the pipeline, and measured loop history belongs to the issue's validation record.
