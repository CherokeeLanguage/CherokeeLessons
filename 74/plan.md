# Plan — CherokeeLessons#74: drop the 45 superseded source mp3s and the dead CLL-1 mp3 zip — m4a is the site standard

## Goal

Implement the CherokeeLessons#74 spec: untrack and delete all 45 tracked mp3s (41 demo-audio sources across three TTS-demo dirs + 4 espeak-ng clip sources — each with an exact-name tracked `.m4a` counterpart), remove the four stale `!*.mp3`-only per-dir `.gitignore` files and the cll1 dir's `*.zip`-only `.gitignore`, delete the untracked `cll1-v3-audio.zip`, refresh AGENTS.md's media-facts statements (the "45 tracked mp3s" account, the `!*.mp3` negation guidance, the mp3-tracking guidance paragraph, the stale "176 tracked `.m4a`" figure, and the "TTS demo audio → tracked mp3s" ruling prose superseded in place), and run the full-build gate. Answers CherokeeLessons#74; touches the four content dirs, AGENTS.md, and `.issues/74/research/` (nothing else — no page edits, no m4a changes).

## Execution surface

Repo: the session's project root (CherokeeLessons, Zola static site). Branch: `feature/74-m4a-only-cleanup` off verified trunk tip `7de0dd0` (master == origin/master, verified live 2026-11-22). Tools: `./build.sh` (the sanctioned build; `public/` is gitignored build output — stale residue there is pruned locally per the spec's Edge Cases, carries no git delta). Issue-store work (plan, research records, audit records) happens in the `.issues/` linked worktree on its own `issues-data` branch. No registered submodules exist — pointer gates N/A.

## Items — one per success criterion

**Item 1 — media retirement, rule cleanup, preservation audit, build gate (SC-1, SC-2, SC-3, SC-4, SC-6)**
- **Deliverable:** all 45 tracked mp3s untracked (`git rm`) and gone from the working tree; the four `!*.mp3`-only `.gitignore` files removed; the cll1 dir's `*.zip`-only `.gitignore` removed; `cll1-v3-audio.zip` deleted from the working tree; stale `public/` residue (mp3/zip copies under `public/content/…` mirroring the removed files) pruned so the local serve state matches the tracked tree; the 199 tracked `.m4a` untouched; the embed invariant re-audited on the composed state; `./build.sh` executed on the composed state. The root `.gitignore`'s global `*.mp3` rule survives untouched (the spec intends it).
- **RED:** on the current tree `git ls-files '*.mp3' | wc -l` = 45 (≠ 0 — SC-1's arm fails), `test -f …cll1-v3-audio.zip` true (SC-4's arm fails), the four `!*.mp3` negation files exist (SC-3's arm fails), `find content -path 'content/lesson-audio/*' \( -name '*.zip' -print \)` shows the zip, and `git diff master..HEAD --stat` is null so the no-`.m4a`-rows preservation assertion is unestablishable (SC-2's arm fails) — the composed state does not exist.
- **GREEN:** `git rm -q` the 45 mp3s; `git rm` the four `.gitignore` files; `git rm content/lesson-audio/cherokee-language-lessons-1---audio-exercises/.gitignore`; `rm` the zip; prune the matching mp3/zip paths under `public/content/…`; commit the deletions; run the audit and the build on the committed state. No other change.
- **Instrument:** SC-1: `git ls-files '*.mp3'` empty and `find content -path 'content/lesson-audio/*' \( -name '*.mp3' -print -o -name '*.zip' -print \)` empty (spec's corrected parenthesized form — the zip arm scoped to lesson-audio so the excluded fonts/NumbersMaze zips are never matched). SC-2: `git ls-files '*.m4a' | wc -l` = 199; `git diff master..HEAD --stat` shows no `.m4a` row; embed audit greps over `content` (both `audio (file=` and `audio(file=` variants, `.md` pages and `exercises.txt`): every value ends `.m4a`, `.mp3` count 0, each value resolves to a tracked counterpart. SC-3: `git ls-files` empty on each of the four negation paths and the cll1 path; `git grep -lF '!*.mp3' -- '*.gitignore'` empty. SC-4: `test ! -f …cll1-v3-audio.zip`; page zip references 0. SC-6: `./build.sh` exit 0. `git check-ignore` on an m4a name exits non-zero (rule state unchanged).
- **Instrument class:** fact checks (git listings, finds, greps, executed build).

**Item 2 — AGENTS.md docs currency (SC-5)**
- **Deliverable:** AGENTS.md media-facts statements updated in place: the "45 tracked mp3s"/negation-pattern account and the mp3-tracking guidance paragraph rewritten to the post-cleanup truth (zero tracked mp3s, no per-dir mp3 negation pattern as a live mechanism, git history as the recovery path); the "176 tracked `.m4a` across the site" figure corrected to 199; the 2026-11-16 ruling-of-record prose "TTS demo audio → tracked mp3s" superseded in place by the developer's 2026-11-22 m4a-only disposition (the historical account corrected, not annotated as surviving).
- **RED:** grep AGENTS.md: "45 tracked mp3s" present; "176 tracked" present; `git grep -F 'TTS demo audio → tracked mp3s' -- AGENTS.md` non-empty — all three statements contradict the state Item 1 delivers.
- **GREEN:** targeted edits to the four statements — no restructuring of the surrounding SOP text beyond the superseded disposition line.
- **Instrument:** grep AGENTS.md: "45 tracked mp3s" absent; "176 tracked" absent; "199 tracked" present; `git grep -F 'TTS demo audio → tracked mp3s' -- AGENTS.md` empty.
- **Instrument class:** fact checks (greps).

## Dependency order

1 → 2: Item 1 performs the deletions, executes the preservation audit and the build against the composed state; Item 2's AGENTS.md figures (0 tracked mp3s, 199 tracked m4a) describe exactly Item 1's delivered state and follow it so the doc records the true state, never an anticipation. No cycles; each item's RED is a real failing assertion on the pre-item state.

## Traceability

| Item | SC(s) |
|---|---|
| 1 | SC-1, SC-2, SC-3, SC-4, SC-6 |
| 2 | SC-5 |

Every SC covered once; every item traces to ≥1 SC.

## Verification summary

- Fact checks (git listings, finds, greps): Items 1–2.
- Executed build: Item 1.
- Recorded audit outputs (embed invariant, diff assertion, build output): `.issues/74/research/cleanup-audit.md` on the issues-data branch.
- No authored tests (Zola static site — no test suite; `./build.sh` exit 0 is the verification gate per AGENTS.md).
- Cycle-time runs: Items 1–2 in order; heavy tiers N/A.

## Status boundary

No status fields in this body; implementation state lives in the pipeline and the issue's validation record.
