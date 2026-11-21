# Plan — CherokeeLessons#68: TTS demo-audio pages convert to tracked AAC `.m4a` with per-entry download links

## Goal

Implement the CherokeeLessons#68 spec: transcode the 41 untracked demo mp3s across three TTS page dirs in place to tracked AAC `.m4a` through the per-track bitrate ladder, track the 41 mp3 sources via per-dir `!*.mp3` negation (nothing deleted), retarget page embeds and the `exercises.txt` fragment references to `.m4a` names with per-entry download anchors, update the fragment generator's glob, and record the supersession seam in the CherokeeLessons#30 umbrella. Answers CherokeeLessons#68; touches the three content dirs, `make-exercises-fragment.sh`, the three per-dir `.gitignore`s, and `.issues/68/research/` + `.issues/30/spec.md`.

## Execution surface

Repo: the session's project root (CherokeeLessons, Zola static site). Branch: `feature/68-tts-demo-m4a` off verified trunk tip `4362e81` (master == origin/master). Tools: `./build.sh` (sanctioned build), ffmpeg at `/snap/bin/ffmpeg`; `ffprobe` is absent today and is provisioned at implement time (apt `ffmpeg` package or equivalent static binary — recorded in research). Issue-store work (`plan.md`, `research/`, `.issues/30/spec.md` seam edits) happens in the `.issues/` linked worktree on its own `issues-data` branch, not on the feature branch. No registered submodules exist (`.gitmodules` absent). The ladder's owning definition is audio-lessons-generator-python#16; the per-track transcripts are recorded to `.issues/68/research/` on that issue's repo — this plan's instruments reference the file this work writes there (`.issues/68/research/ladder-transcripts.md` in the CherokeeLessons store).

## Items — one per success criterion

**Item 1 — media conversion and source tracking (SC-1 embed-name part of, SC-2, SC-3, SC-4, SC-8 media part of)**
- **Deliverable:** the 41 mp3s in the three dirs transcoded in place to `*.m4a` (AAC-LC, mono, 48 kHz, MP4 container, ≤ 25,165,824 B each) via the per-track ladder 128 → 112 → 96 kb/s (encode-and-measure: the encoded file's measured size, not nominal rate, decides each rung); all 41 `.m4a` plus all 41 retained `.mp3` sources tracked in git; per-dir negation rules: new `.gitignore` with `!*.mp3` in `cherokee-animal-names--ims-toucan-tts-/` and `cherokee-tts-update-3--ims-toucan-/`; `cherokee-animal-names-tts-demo-audio-1/.gitignore`'s `Animals*.mp3` rule replaced with `!*.mp3`; per-track ladder transcripts (rungs attempted, measured size at each, chosen rung, ffmpeg command, tool provenance) recorded to the research file.
- **RED:** `git ls-files` on the three dirs shows zero `.m4a` files and (for two dirs) no negation rule at all — `git check-ignore -v` on a source mp3 reports the global `*.mp3` ignore, not a dir negation.
- **GREEN:** the per-track transcode loop plus the three `.gitignore` edits plus `git add` of both classes (plain add of ignored media is a silent no-op — confirmed with `git status`/`git check-ignore` after each add).
- **Instrument:** per file `ffprobe` (`codec_name`=aac, `channels`=1, `sample_rate`=48000, `format_name` mov/mp4) and `stat -c %s` ≤ 25,165,824; per-direction `git ls-files` counts 24+24 / 9+9 / 8+8 and zero deleted mp3s; `git check-ignore -v` shows the dir negation; transcript cross-check per file (`ffprobe` `bit_rate` vs the recorded chosen rung).
- **Instrument class:** fact checks + a recorded artifact (the transcript file).

**Item 2 — page embed retargeting and anchors (SC-1, SC-5)**
- **Deliverable:** the three pages' embed surface — 12 `{{audio}}` embeds on `cherokee-animal-names-tts-demo-audio-1/index.md` and 9 on `cherokee-animal-names--ims-toucan-tts-/index.md` retargeted to `.m4a` names each with `<a href="<file>.m4a" download>⤓ download</a>` beside the entry; `cherokee-tts-update-3--ims-toucan-`'s four tables re-headered `SPEAKER|M4A FILE|DOWNLOAD` with three-column dividers and a third-cell download anchor per row. No zip artifacts; no other prose changes.
- **RED:** grep on the three page sources shows `.mp3` references in the embeds and no `⤓ download` anchors; the four tables carry `SPEAKER|AUDIO MP3` headers.
- **GREEN:** sed/edit per page replacing names and adding anchors in the spec's exact form.
- **Instrument:** grep counts — embeds 12/9/8 all `*.m4a`, `.mp3` name references = 0, anchor counts 12/9 with the `download` attribute and the `⤓` label, four `SPEAKER|M4A FILE|DOWNLOAD` headers with extended dividers, `SPEAKER|AUDIO MP3` = 0.
- **Instrument class:** fact checks (greps).

**Item 3 — fragment and generator (SC-7)**
- **Deliverable:** `exercises.txt`'s 12 `{{audio}}` lines retargeted to the 05-29 set's `.m4a` names; `make-exercises-fragment.sh`'s glob changed from `Animals*mp3` to `Animals*m4a`.
- **RED:** grep on `exercises.txt` shows 12 `.mp3` (05-29) lines; the script contains `Animals*mp3`.
- **GREEN:** replace the 12 names in the fragment; update the script's glob.
- **Instrument:** greps — fragment has 12 `.m4a`/05-29 lines, `.mp3` = 0, no anchors; script contains `Animals*m4a`.
- **Instrument class:** fact checks.

**Item 4 — umbrella seam (SC-9)**
- **Deliverable:** `.issues/30/spec.md` — original R-2 superseded and replaced by a transfer record naming CherokeeLessons#68 under the 2026-11-21 directive (converted deliverables, source tracking, page treatment); SC-3 records the transfer as resolvable (the #68 link + the transferred class); the Parts list carries the #68 entry; `.issues/30/links.yaml` gains the 68 link.
- **RED:** grep on `.issues/30/spec.md` shows no #68 supersession/transfer text and no Parts entry; `links.yaml` lacks 68.
- **GREEN:** edit `.issues/30/spec.md` (R-2 transfer record, SC-3 note, Parts entry) and run the local-issues `link` command; commit on the `issues-data` branch worktree.
- **Instrument:** greps on `.issues/30/spec.md` + `local-issues read --number CherokeeLessons#30` showing the 68 link.
- **Instrument class:** fact checks (issue-store workspace data — the store's workspace guide authorizes these edits).

**Item 5 — browser download-behavior check (SC-6)**
- **Deliverable:** executed evidence that a clicked download anchor serves the entry's `.m4a` as a download (save, not navigation) — one anchor per page, 3 total.
- **RED:** not authored as a test — the pages cannot show download behavior until Items 1–2 land and the site is served.
- **GREEN:** none in the repo — this is executed observation evidence, recorded to `.issues/68/research/` (ephemeral scratch per the test-discipline posture; never an authored test artifact).
- **Instrument:** browser test on a local serve of the built site (playwright-cli; the managed browser) — click one anchor per page, observe download events with matching `.m4a` filenames. Tier: none authored (observation only).
- **Instrument class:** recorded dispatch evidence (playwright session output pasted to the research file).

**Item 6 — full-build gate (SC-8)**
- **Deliverable:** a clean `./build.sh` (exit 0) on the feature branch, and the three pages' `public/` dirs present holding exactly 24 / 9 / 8 `.m4a` plus the retained `.mp3` sources.
- **RED:** the current build's `public/` carries none of the `.m4a` conversion output.
- **GREEN:** follow-on effect of Items 1–2; the item executes and records the build.
- **Instrument:** `./build.sh` exit status + `find public/content/content/<dir> -name '*.m4a' | wc -l` per dir (24/9/8).
- **Instrument class:** fact checks + executed build output.

## Dependency order

1 → 2 → 3 → 4 → 5 → 6, strictly: Item 1 produces the `.m4a` files every later name reference targets; Item 2's page anchors depend on Item 1's names; Item 3's fragment retarget depends on Item 1's names (and shares the 05-29 set decision Item 1 preserves); Item 4's seam names the delivered state of 1–3; Item 5's browser click needs built, deployed-media pages (1–2); Item 6's build gate runs last so the verified state is the composed one. Item 4 (issue-store worktree) is independent of 2–3's file state but ordered after them so the transfer record describes the final names; no cycles.

## Traceability

| Item | SCs |
|---|---|
| 1 | SC-2, SC-3, SC-4 (media/tracking/ladder), SC-1 name-retarget enabler, SC-8 media |
| 2 | SC-1, SC-5 |
| 3 | SC-7 |
| 4 | SC-9 |
| 5 | SC-6 |
| 6 | SC-8 |

## Verification summary

- Fact checks: Items 1–4, 6 (greps, `git ls-files`, `git check-ignore`, `ffprobe`, `stat`, build counts).
- Recorded artifact: Item 1's ladder transcripts + Item 5's browser-click record, both to `.issues/68/research/`.
- Authored tests: none — this repo has no test suite; every instrument is a direct executed check against the repo/build (per-SC instruments from the spec).
- Cycle time: Items 1–6 all execute in this cycle; Item 5's browser observation runs against a local serve of the built site; SC-8's build is the PR-check-time structural gate as well.

## Status boundary

No status fields in this plan; implementation state lives in the pipeline, and measured loop history belongs to the issue's validation record.
