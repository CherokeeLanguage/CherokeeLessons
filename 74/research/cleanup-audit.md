# Cleanup audit evidence — CherokeeLessons#74

Executed 2026-11-22 on `feature/74-m4a-only-cleanup` (commits `fa698c6` deletions — note `7489a1f`-class fix-up not applicable — and `0ea7700` docs), against spec SC-1..SC-6.

## SC-1 (mp3/zip zero on the tracked-content surface)

- `git ls-files '*.mp3'` → empty (0 rows).
- `find content -path 'content/lesson-audio/*' \( -name '*.mp3' -print -o -name '*.zip' -print \)` → empty.
- `find content -name '*.mp3'` → empty (45 pre-change → 0).

## SC-2 (m4a preservation + embed invariant)

- `git ls-files '*.m4a' | wc -l` → 199 (pre-change 199; unchanged).
- `git diff master..HEAD --stat | grep -c m4a` → 0 (no .m4a row in the delivered diff).
- mp3 embeds (`audio ?\(file="[^"]*\.mp3"` over content) → 0.
- Embed audit (both `audio (file=` and `audio(file=` variants across 13 files incl. `exercises.txt`): **199 embeds, 0 untracked/missing counterparts** (per-occurrence resolution against the page dir + `git ls-files --error-unmatch`).

## SC-3 (stale ignore rules)

- `git ls-files` on the four negation paths and `content/lesson-audio/cherokee-language-lessons-1---audio-exercises/.gitignore` → empty.
- `git grep -lF '!*.mp3' -- '*.gitignore'` → exit 1 (no match; root `.gitignore`'s global `*.mp3` survives untouched as intended).
- `git check-ignore` on an m4a name → non-zero (exit 1, not ignored).

## SC-4 (mp3 zip gone, unreferenced)

- `test ! -f content/lesson-audio/cherokee-language-lessons-1---audio-exercises/cll1-v3-audio.zip` → true (261,214,882 B removed from the working tree; it was untracked — no git delta).
- `grep -c 'cll1-v3-audio.zip' …index.md` → 0 (the #58 state retained).

## SC-5 (AGENTS.md currency)

- "45 tracked mp3s" → 0 hits; "176 tracked" → 0 hits; "199 tracked" → present (the Content-facts count statement now reads 199 and records the #74 deletion).
- `git grep -F 'TTS demo audio → tracked mp3s' -- AGENTS.md` → exit 1; the 2026-11-16 ruling prose now records the m4a delivery form and names the 2026-11-22 superseding disposition (m4a sole kept format).

## SC-6 (build gate)

- `./build.sh` → exit 0 ("Done in 1.5s"; composed post-deletion tree).
- Public/ stale residue pruned (`find public/content -name '*.mp3' -delete`; the cll1 zip's local copy removed) — no git delta.
