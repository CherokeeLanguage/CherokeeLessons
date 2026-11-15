# Spec — lesson media embeds migrate to YouTube (dictation videos, CLL-1 + animal-names audio)

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/54 · part of the CherokeeLessons#30 recomposition (2026-11-14)

## Problem

14 Syllabary-Dictation pages embed 20 `{{ video(video="Dictation …mp4") }}` embeds and two lesson-audio pages (CLL-1 audio exercises: 23, animal-names: 9) embed `{{audio(file=…)}}` referencing untracked mp3/mp4 sources — these are dead in every fresh-clone build (the executed 2026-11-11 stand-up gate's build omitted every untracked asset class). All 53 items are verifiably mirrored on the developer's YouTube channel (`@-cherokeelanguage7554`, verified live 2026-11-11) with a stable-title-prefix + duration correspondence mapping (`research/youtube-video-id-map.txt`), and the CLL-1 audio page carries a `cll1-v3-audio.zip` download link that is dead in every fresh-clone build for the same reason.

## Scope

This repository — content pages only: the 14 Syllabary-Dictation pages, `content/lesson-audio/cherokee-language-lessons-1---audio-exercises/index.md`, `content/lesson-audio/animal-names/index.md`, the two tracked `process_end_notes.py` generator scripts, and working-tree retirement of the migrated media files. No theme or shortcode changes (the `{{ youtube() }}` shortcode is pre-existing, tracked since 2023-05).

## Requirements

- **R-1 — YouTube embeds.** The 14 dictation pages SHALL switch their 20 `{{ video(video="Dictation …mp4") }}` embeds, and the CLL-1 audio page (23) and animal-names audio page (9) their `{{audio(file=…)}}` embeds, to `{{ youtube(id=…) }}` using the verified mapping (`.issues/30/research/youtube-video-id-map.txt` — pre-split umbrella artifact); page→video assignment derives from each page's current media references. The untracked mp3/mp4 sources of these classes SHALL be deleted from the working tree.
- **R-2 — Dead zip link.** The CLL-1 page's `cll1-v3-audio.zip` download link SHALL be removed with the embeds — the link is dead in every fresh-clone build (the zip is untracked and never deploys). The zip file itself stays untracked on disk; its disposition is deferred (out of scope here, recorded on the umbrella CherokeeLessons#30).
- **R-3 — Generator scripts deleted.** The two tracked page-generator scripts `content/lesson-audio/cherokee-language-lessons-1---audio-exercises/process_end_notes.py` and `content/lesson-audio/animal-names/process_end_notes.py` SHALL be deleted: they regenerate the old embed form (and, for CLL-1, the dead zip link) from the now-deleted local media, the build serves them verbatim as non-markdown content files (shipping the dead-link text), and re-running them post-migration would overwrite the migrated pages and fail on the deleted mp3s — tracked, so history-recoverable.

## Success criteria

| ID | Criterion | Class | Instrument |
|----|-----------|-------|------------|
| SC-1 | The migrated pages use `{{ youtube(id=…) }}` with exactly their recorded per-page assignment; their old embeds (`video(video=` and `audio (file=`) and the CLL-1 page's dead zip link are gone; all seven other `{{audio}}` users are untouched at their measured baselines | structural | per migrated page: `video(video=` and `audio (file=` counts = 0; the page's `youtube(id=` ID set equals its recorded set in `.issues/30/research/page-video-assignment.yaml` (52 embeds derived mechanically from the current pages; map entry Dictation 14-02 has no current page reference and is unassigned); `cll1-v3-audio.zip` count = 0 on the CLL-1 page; unchanged baselines: `content/content/espeak-ng-and-ced-phonetics/index.md` `audio (file=` = 100; `content/content/cherokee-animal-names-tts-demo-audio-1/index.md` = 12; `content/content/cherokee-animal-names--ims-toucan-tts-/index.md` = 9; `content/content/cherokee-tts-update-3--ims-toucan-/index.md` = 8; `content/content/here-is-why-i-m-so-keen-on-getting-espeak-ng-speaking-cherokee---/index.md` `audio(file=` (no-space form) = 5; `content/content/cherokee-tts-update--2--ims-toucan-/index.md` = 3; `content/content/tacotron2-and-cherokee-tts/index.md` = 18 |
| SC-2 | The two tracked generator scripts are deleted | structural | `git ls-files "**/process_end_notes.py"` is empty; `test -f` on both script paths fails |
| SC-3 | The migrated media sources are absent from the working tree | structural | `find content/lessons/Syllabary-Dictation -name "*.mp4"` is empty; `find content/lesson-audio/cherokee-language-lessons-1---audio-exercises content/lesson-audio/animal-names -name "*.mp3"` is empty |
| SC-4 | Pages build and serve the migrated embeds without template errors | behavioral | `./build.sh` exit 0; migrated pages present in the built tree |

(Site-wide build/size gates SC-7/SC-8 remain on the umbrella CherokeeLessons#30.)

## Out of scope

- The `cll1-v3-audio.zip` bundle's own disposition (drop vs itch re-issue) — deferred, umbrella-recorded.
- The four TTS demo-audio dirs (CherokeeLessons#30's tracking work R-2, umbrella).
- Page↔video assignment beyond the mechanical derivation above (Dictation 14-02 stays unassigned).
- Any YouTube-side work beyond embedding (playlist edits, privacy settings).

## Approach

Mechanical rewrite of the 17 pages to `{{ youtube(id=…) }}` per the derived assignment; delete the migrated media files from the working tree; delete the two generator scripts. Rollback: revert the commit set; media re-obtainable from the YouTube channel and local mirror (`cherokee-audio-mirror`).

## Impact

Consumers: dictation and lesson-audio page visitors gain playable embedded mirror media on the deployed site (the local-media embeds they replaced are dead-on-deploy today); the build tree loses the mp3/mp4 embed dependencies. Risk: low — content-only; shortcode pre-existing. ~2.1 GB of working-tree media retired locally.

> Full spec and plan artifacts: https://github.com/CherokeeLanguage/CherokeeLessons/tree/issues-data/54
<!-- artifacts: .issues/54/ -->
