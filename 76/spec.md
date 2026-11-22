# Spec — complete the conversions for consistency: the 45 source-backed m4a deliverables re-encoded at the 192k rung from their git-history originals

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/76 · filed 2026-11-22 from the developer directive ("create spec to complete the conversions for consistency. implement spec. approved for PR.") completing the same-day dispositions ("convert to m4a"; "as the originals are already small, higher bitrates are acceptable") · program context: CherokeeLessons#58/#59/#67/#68 (the conversions), #74 (the sole-format drop this pass operates on), #30 (umbrella)
<!-- artifacts: .issues/76/ -->

## Intent

**Problem Statement.** After the conversion program (CherokeeLessons#58/#59/#67/#68) and the sole-format drop (CherokeeLessons#74), the site ships one audio format — 199 tracked `.m4a`, zero tracked mp3s, all pages on the `SESSION|M4A FILE|DOWNLOAD` form — but the 45 source-backed tracks (animal-names demo 24, ims-toucan demo 9, tts-update-3 8, espeak-ng clips 4) still carry their first-pass encodes at the 128k-rung ladder, measured 2026-11-21 at effective 48–128 kb/s (animal demo 48–59k, ims-toucan 57–68k, tts-update-3 120–128k, espeak clips 72–117k). The developer's higher-bitrate disposition — "as the originals are already small, higher bitrates are acceptable" (2026-11-22) — was not executed before #74 dropped the source mp3s, so the conversion thread it names is open. The originals are recoverable: every dropped mp3 is in git history (AGENTS.md, post-#74: "the mp3s of any past commit remain recoverable from git history").

**Root Cause / Motivation.** The program's source-handling ran as track-then-drop; the higher-rung encode the developer accepted mid-program never landed before the drop. Completing the conversions for consistency = producing the final, higher-rung encodes of the 45 source-backed tracks from their retained originals — the last thread of the conversion program.

**Approach Chosen.** Recover the 45 originals from the pre-drop commit, re-encode each into its exact-name tracked `.m4a` deliverable at the **192 kb/s nominal target** (the picked rung under the developer's acceptability grant; 2× the program's nominal rung while staying far inside the 25 MiB cap — the largest source track is 13.55 MB (`animals-0007.mp3` at 1,788.96 s — four tracks, 10.8–13.55 MB, are the cap-relevant set)), with the cap-floor rule (a track whose 128k-floor encode still exceeds 24 MiB retains its existing deliverable unchanged, exclusion recorded with all measured rung sizes — no new rung is invented), replace the deliverables in place under unchanged filenames (zero page edits), record per-track provenance, and keep the #74 state (no mp3 re-tracking, no negation files).

**Alternatives Considered & Why Discarded.**
- Leave the first-pass encodes (no work) — discarded: the developer's directives name the higher-rung conversion as wanted work; the quality pass is the thread they directed to complete.
- Re-encode all 199 tracks — discarded for this pass: the 154 source-retired tracks' certified ladder encodes stand; their originals' re-obtain is heavier (git history + channel mirror) and no developer statement ties the pass to them (the "originals are already small" wording binds to the retained-sources thread; a site-wide pass is a separate disposition if later wanted).
- Higher than 192k (e.g. 256k) — discarded for this pass: the measured originals' information caps (their mp3 rates: 28.9–64 kb/s) put any gain from 256k in pure file inflation; 192k nominal overshoots every original's rate while cutting generation loss.
- Convert-side-file semantics (keep both an m4a deliverable and a separate m4a copy of the source) — discarded: contradicts the #74 sole-format standard (no dupes of the same audio).

**Key Design Decisions.** (1) In-place replacement semantics: the exact-name `.m4a` deliverable is the re-encode's destination — pages, `exercises.txt`, anchors, and tables are untouched by construction (no name changes → no reference surface edits). (2) The 192k rung is the spec's picked value under the developer's acceptability grant (a 2026-11-22 encoder-parameter decision, recorded here). The program's measured record shows the encoder's rate control follows content (the 128k rung yielded 48–128k effective per file), so encoded sizes are decided by encode-and-measure, never by projection: the cap gates the rung per track, and the cap-floor rule (R-1) handles the four long tracks, 10.8–13.55 MB / ~30 minutes, whose 192k landing is genuinely size-relevant. (3) Transient originals: the history-recovered mp3s live in a temp directory (`/tmp/opencode`) and are never added to git — `git show <sha>:<path>` per file; the git delta contains media bytes only in the 45 m4a paths. (4) The m4a deliverables keep their container/codec form (AAC-LC, MP4, mono, 48 kHz, faststart) — only the rate changes. (5) #68's record is reconciled to the remote-anchored closed state in the same workflow (the superseded R-2/SC-2 annotated in the reconciliation record); #30's seam-2 stale wording gets the correction here (#74's record chose to flag rather than edit; this completion spec carries the umbrella edit — the program's closing edit).

**User Intent / Original Prompt (2026-11-22, chat).**
- "we need to look at tracking everything feasible, using the new standard for audio" (the program's completion context); "convert to m4a" (the sources converted, not merely dropped); "as the originals are already small, higher bitrates are acceptable" (the rung freedom); "create spec to complete the conversions for consistency. implement spec. approved for PR." (this spec + full pipeline authorization).

## Not Included

- The 154 source-retired tracks (lesson-audio: CLL-1 23 + animal-names 9; the #67 classes: espeak 96, tacotron2 18, here-is-why 5, tts-update-2 3): their certified encodes stand; a site-wide higher-rung pass would re-obtain originals from git history and the verified channel mirror — a separate disposition if the developer later directs it.
- The Cherokee Phoenix page's 55 external `data.cherokee.org … .mp3` links — dead at fetch time (verified 2026-11-22: HTTP 404 on both probed URLs); any link repair is its own disposition, not a conversion.
- Any page/affordance/wording edits (the display form of record stands; the filenames do not change).
- Fixing or removing `fix-mp3-tags.sh` (dead tooling, flagged in #74's KDD-3 — flag stands).

## Requirements

- **R-1 — Higher-rung re-encode, in place.** The 45 original mp3s SHALL be recovered from git history from the pre-drop commit (the exact commit SHAs per dir recorded; the 45 paths and their sha256 digests verified), and each SHALL be re-encoded into its exact-name tracked `.m4a` deliverable — AAC-LC in MP4 container, mono, 48 kHz, `-movflags +faststart`, via the static `.tools/ffmpeg` 7.0.x build with `-nostdin` — at the **192 kb/s nominal target**, falling back 160 → 128 kb/s encode-and-measure (the encoded file's measured size decides) while the track's encode exceeds 24 MiB (25,165,824 B — the margin below the 25 MiB per-file asset cap). A track whose 128k-floor encode still exceeds the cap retains its existing deliverable unchanged, its transcript recording every attempted rung's measured size with the exclusion — no new rung is invented, no over-cap file is emitted. The decided re-encode set SHALL be replaced in place under unchanged filenames; no other m4a SHALL be touched.
- **R-2 — Site invariants preserved.** The 199 tracked-m4a count SHALL be unchanged; zero mp3s tracked and zero on disk; the four per-dir `!*.mp3` negation files stay absent; every page's embed/word-link/anchor/table counts and the `exercises.txt` m4a-only fragment state are unchanged; the history-recovered mp3s live in a temp location and never enter git.
- **R-3 — Records currency.** `AGENTS.md`'s media-tooling record SHALL gain the 192k statement for this pass (the sole-format statement already stands); the umbrella CherokeeLessons#30's seam-2 wording SHALL be corrected to the post-#74 truth: the demo-audio class ships tracked `.m4a` with per-entry download anchors, the sources were dropped by CherokeeLessons#74 (single-format standard; history-retained), and #76's pass re-encoded the source-backed deliverables at the 192k rung.
- **R-4 — Provenance and verification.** Per-track transcripts (source commit SHA + path, source sha256, rung, measured encoded size at each attempted rung, the transcode command, tool version — `.tools/ffmpeg` and `ffprobe`) SHALL be recorded in `.issues/76/research/`; every re-encoded file SHALL pass `ffprobe` verification (codec aac, channels 1, sample_rate 48000, mov/mp4, size ≤ 25,165,824 B); one browser click-through on a served demo page anchor SHALL observe the download event with the re-encoded file (browser save, not navigation — the program's `.issues/59/research/download-column.md` evidence class).

## Success criteria

| ID | Criterion | Class | Instrument |
|---|---|---|---|
| SC-1 | Only `.m4a` media paths + AGENTS.md carry byte changes: the re-encoded set exactly as the transcript's decided list records (all 45, minus any cap-floor exclusions, which retain their prior bytes and are recorded); zero page, fragment, or script changes; deliverable names unchanged. The `.issues/30/spec.md` seam-2 correction is store-worktree content (verified by SC-7's grep, not the main-branch diff) | structural | `git diff --name-only` vs HEAD (main-branch paths only: the decided m4a set + AGENTS.md); every changed media path matches `.*\.m4a$`; zero non-listed paths |
| SC-2 | Every re-encoded file is AAC-LC, mono, 48 kHz, ≤ 25,165,824 B, in an MP4 container | structural | per file: `ffprobe` shows `codec_name` aac, `channels` 1, `sample_rate` 48000, `format_name` containing mov/mp4; `stat -c %s` ≤ 25,165,824; 45/45 pass |
| SC-3 | Each track's encode decision is executed as recorded: the transcript names the rung(s) attempted with measured sizes and the outcome (re-encoded at 192k / fallback / floor-excluded) for every one of the 45 | behavioral | the per-track transcripts in `.issues/76/research/` — the recorded command + measured-size columns are the evidence (rate columns informational; encoder rate control follows content, so bit_rate alone distinguishes nothing) |
| SC-4 | The site invariants hold: 199 tracked `.m4a`; zero tracked and on-disk mp3s; zero `!*.mp3` negation files tracked; embed counts (100/18/5/3, 12/9/8, 9, 23), anchor counts (100/18/5/3, 12/9/8, 9, 23), `SPEAKER|…|DOWNLOAD` table count (4), and `exercises.txt` 12-line m4a-only state unchanged | structural | `git ls-files '*.m4a'` count, `find` mp3 check, `git ls-files` negation check, grep counts |
| SC-5 | The provenance record is complete: per-track source commit SHA + path + source sha256, rung, sizes, commands, tool versions (45 entries) in `.issues/76/research/` | structural | transcript file present; 45 entries; source sha256s match the history-recovered bytes |
| SC-6 | The site builds and serves the re-encoded media | behavioral | `./build.sh` exit 0; the four dirs' `public/` copies present carrying the 100/18/5/3 and demo 24/9/8 m4a |
| SC-7 | AGENTS.md and the umbrella carry the post-pass truth | structural | grep AGENTS.md: the 192k statement present with the #76 reference; grep `.issues/30/spec.md`: seam-2 names #74's drop and #76's pass |
| SC-8 | A clicked demo-page anchor serves the re-encoded file for download (browser save, not navigation) | behavioral | browser test on the served built page (one anchor), download event with the matching filename |

## Dependencies

| Reference | Relationship | Status |
|---|---|---|
| Developer directives 2026-11-22 (chat) | the authorizing intent; the rung freedom; the full-pipeline grant ("approved for PR") | satisfied (quoted in Intent) |
| CherokeeLessons#74 (closed) | the sole-format state this pass operates on; the source mp3s' git-history recovery path | satisfied (merged; SHAs resolved in the transcripts) |
| CherokeeLessons#58/#59/#67/#68 | the conversion program whose final pass this is; the display form of record | satisfied (merged) |
| CherokeeLessons#30 (umbrella) | R-3's seam-2 correction; site-wide gates execute after the last part lands | pending (umbrella open) |
| ffmpeg / ffprobe static toolchain | `.tools/ffmpeg` + `.tools/ffprobe` present (verified 2026-11-22); `-nostdin` mandatory (AGENTS.md: the non-interactive stdin hang) | satisfied (in place) |
| Channel mirror | not needed by this pass (history-recovery suffices); unchanged rollbacks | n/a to this pass |

## Traceability

| Requirement | Success criteria |
|---|---|
| R-1 | SC-1, SC-2, SC-3 |
| R-2 | SC-1, SC-4 |
| R-3 | SC-7 |
| R-4 | SC-2, SC-3, SC-5, SC-6, SC-8 |

## Documentation Sources

| Source | Type | Location | Verification |
|---|---|---|---|
| Effective-rate census of the first-pass encodes | executed measurements | per-dir ffprobe runs 2026-11-21 (animal demo 48–59k vs 28.9k originals; ims-toucan 57–68k vs 57.3k; tts-update-3 120–128k vs 64k; espeak clips 72–117k vs ~64k) | `.tools/ffprobe` executed 2026-11-21 | 
| The drop record and recovery guarantee | spec + repo facts | CherokeeLessons#74 spec (R-1/R-4, its SCs, merged state); AGENTS.md "the mp3s of any past commit remain recoverable from git history" | read 2026-11-22 |
| Static toolchain record | repo facts | AGENTS.md media-tooling section (`.tools/` ffmpeg/ffprobe 7.0.2-class, `-nostdin`, the transcode form) | read/verified 2026-11-22 (tools in place: `ls .tools/`) |
| Ladder/encoder method owning definition | remote spec | audio-lessons-generator-python#16 | fetched live via `gh` 2026-11-20 |
| Asset cap | repo facts | Workers 25 MiB/file (`AGENTS.md`) | read 2026-11-22 |
| The display form of record (verified standing) | repo files | the 3-column tables and anchors on all demo/lesson pages (grep-verified 2026-11-22) | executed 2026-11-22 |

## Cost Frame

- SC-1 / SC-4 / SC-5 / SC-7: greps, git listings, the transcript — minutes; skipping risks a partial pass or an unrecorded provenance chain (no transcript is no evidence).
- SC-2 / SC-3: 45 encodes with per-file probes — the conversion's own runtime (largest source 13.55 MB; minutes of wall time); skipping risks unverifiable rung behavior.
- SC-6: one full site build; skipping risks shipping stale `public/` media.
- SC-8: one browser click-through — the program's evidence class; an untested anchor risks navigation instead of save.

## Edge Cases

| Condition | Expected behavior | Resolution |
|---|---|---|
| A track's encode exceeds 24 MiB at a rung | the ladder falls back 192 → 160 → 128 encode-and-measure — the measured size decides | the cap floor rule (R-1): a 128k-floor over-cap track retains its existing deliverable, its transcript records all measured rung sizes and the exclusion; the four long tracks (10.8–13.55 MB / ~30 min) are the set this rule is for |
| The pre-drop commit does not carry one of the 45 paths | the pass halts for that dir (all-or-nothing per dir) | the transcript records the miss; the recovery SHAs are resolved at implement time and recorded — history is verified before the first encode |
| `git show` of a history path silently returns a non-identical file | sha256s verify the recovered bytes against the recorded digests | the transcript's source sha256 column is the check; mismatch halts the track |
| The encode hangs (stdin consumption) | `-nostdin` is mandatory (AGENTS.md recorded hang) | the recorded command form carries `-nostdin` per track; SC-5's transcript shows the exact command |
| A page count drifts between census and implement | invariants halt completion | the SC-4 counts are the census of record; drift re-census before SCs run |
| Post-merge deploy weight | the 45 current deliverables measure ≈ 326 MB (animal-demo 221 MB, ims-toucan 94 MB, tts-update-3 11 MB, clips ~0.1 MB); a higher-rung pass grows them roughly half again — up to ≈ +160 MB — with the true total decided by the transcripts | recorded here for the developer's awareness (the 25 MiB per-file cap is untouched); deploy proceeds as normal |

## Completion gate

Every SC passes before the deliverable is complete — all-or-nothing: the pass is complete only when all 45 tracks are decided per the cap floor rule — re-encoded at their recorded rung or floor-excluded with all measured rung sizes recorded — each decided track verified and provenance-transcribed, the site invariants clean, built, and click-checked; an undecided track fails SC-5 as a set.

<!-- artifacts: .issues/76/ -->
