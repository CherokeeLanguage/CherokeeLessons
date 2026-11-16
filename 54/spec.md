# Spec — lesson media embeds migrate to YouTube (dictation videos, animal-names audio); CLL-1 audio converts to tracked .m4a

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/54 · part of the CherokeeLessons#30 recomposition (2026-11-14)

## Problem

14 Syllabary-Dictation pages embed 20 `{{ video(video="Dictation …mp4") }}` embeds and two lesson-audio pages (CLL-1 audio exercises: 23, animal-names: 9) embed `{{audio(file=…)}}` referencing untracked mp3/mp4 sources — these are dead in every fresh-clone build (the executed 2026-11-11 stand-up gate's build omitted every untracked asset class). All 53 items are verifiably mirrored on the developer's YouTube channel (`@-cherokeelanguage7554`, verified live 2026-11-11) with a stable-title-prefix + duration correspondence mapping (`research/youtube-video-id-map.txt`), and the CLL-1 audio page carries a `cll1-v3-audio.zip` download link that is dead in every fresh-clone build for the same reason.

**Disposition change (developer decision, 2026-11-15):** the CLL-1 audio-exercises page does **not** migrate to YouTube. The exercise sessions stay local: deployed as tracked AAC `.m4a` with a per-track bitrate ladder (the owning definition of the encoder settings is `audio-lessons-generator-python#16` — AAC-LC mono 48 kHz, 128 kb/s target falling back 128 → 112 → 96 kb/s per file until it fits the 24 MiB margin under the site's 25 MiB-per-file asset cap; universal browser playback incl. Safari/iOS; source's own quality ceiling ~57 kb/s mono makes higher encodes inaudible and, on the hour-long sessions, cap-breaking). The 14 dictation pages and the animal-names audio page keep the YouTube disposition (their embeds are video/mirror content, and the CLL-1 conversion choice reflects the exercises' download/offline-usable role rather than channel distribution).

## Scope

This repository — content pages and the CLL-1 media set: the 14 Syllabary-Dictation pages, `content/lesson-audio/animal-names/index.md`, the CLL-1 page `content/lesson-audio/cherokee-language-lessons-1---audio-exercises/index.md` plus its 23 session mp3s and their `.m4a` replacements, the two tracked `process_end_notes.py` generator scripts, and working-tree retirement of the migrated media files. No theme or shortcode changes (the `{{ youtube() }}` shortcode is pre-existing, tracked since 2023-05; the existing `{{audio}}` shortcode form is kept for CLL-1).

## Requirements

- **R-1 — YouTube embeds.** The 14 dictation pages SHALL switch their 20 `{{ video(video="Dictation …mp4") }}` embeds, and the animal-names audio page its 9 `{{audio(file=…)}}` embeds, to `{{ youtube(id=…) }}` using the verified mapping (`.issues/30/research/youtube-video-id-map.txt` — pre-split umbrella artifact); page→video assignment derives from each page's current media references. The untracked mp4 sources of the dictation class and the animal-names mp3s SHALL be deleted from the working tree.
- **R-2 — Dead zip link.** The CLL-1 page's `cll1-v3-audio.zip` download link SHALL be removed with the embed conversion — the link is dead in every fresh-clone build (the zip is untracked and never deploys). The zip file itself stays untracked on disk; its disposition is deferred (out of scope here, recorded on the umbrella CherokeeLessons#30).
- **R-3 — Generator scripts deleted.** The two tracked page-generator scripts `content/lesson-audio/cherokee-language-lessons-1---audio-exercises/process_end_notes.py` and `content/lesson-audio/animal-names/process_end_notes.py` SHALL be deleted: they regenerate the old embed form — and, for CLL-1, the dead zip link plus mp3-name embeds — from the now-deleted local media, the build serves them verbatim as non-markdown content files (shipping stale link/extension text), and re-running them post-conversion would overwrite the migrated pages (for animal-names) and reference the deleted mp3s (for CLL-1) — tracked, so history-recoverable. CLL-1's session renewal path is the generator's own assembly (`audio-lessons-generator-python#16`), not the page script.
- **R-4 — CLL-1 media conversion.** The CLL-1 page's 23 session mp3s SHALL be transcoded in place to `cll1-v3-NNNN.m4a` — AAC-LC, mono, 48 kHz, nominal 128 kb/s with the per-track fallback ladder (112, then 96 kb/s) applied on encode-and-measure until the file is ≤ 24 MiB — per the recommended settings (`audio-lessons-generator-python#16`). The 23 `.m4a` files SHALL be tracked in git (replacing the untracked mp3s), the page's `{{audio (file=…)}}` embeds SHALL reference the `.m4a` filenames, and the 23 mp3s SHALL be deleted from the working tree. Transcode command and tool provenance SHALL be recorded in this issue's research artifacts.

## Success criteria

| ID | Criterion | Class | Instrument |
|----|-----------|-------|------------|
| SC-1 | The migrated pages use `{{ youtube(id=…) }}` with exactly their recorded per-page assignment; their old embeds (`video(video=` and `audio (file=`) are gone; all seven other `{{audio}}` users are untouched at their measured baselines | structural | per migrated page: `video(video=` and `audio (file=` counts = 0; the page's `youtube(id=` ID set equals its recorded set in `.issues/30/research/page-video-assignment.yaml` (52 embeds derived mechanically from the current pages; map entry Dictation 14-02 has no current page reference and is unassigned); unchanged baselines: `content/content/espeak-ng-and-ced-phonetics/index.md` `audio (file=` = 100; `content/content/cherokee-animal-names-tts-demo-audio-1/index.md` = 12; `content/content/cherokee-animal-names--ims-toucan-tts-/index.md` = 9; `content/content/cherokee-tts-update-3--ims-toucan-/index.md` = 8; `content/content/here-is-why-i-m-so-keen-on-getting-espeak-ng-speaking-cherokee---/index.md` `audio(file=` (no-space form) = 5; `content/content/cherokee-tts-update--2--ims-toucan-/index.md` = 3; `content/content/tacotron2-and-cherokee-tts/index.md` = 18 |
| SC-2 | The two tracked generator scripts are deleted | structural | `git ls-files "**/process_end_notes.py"` is empty; `test -f` on both script paths fails |
| SC-3 | The migrated media sources are absent from the working tree | structural | `find content/lessons/Syllabary-Dictation -name "*.mp4"` is empty; `find content/lesson-audio -name "*.mp3"` is empty |
| SC-4 | Pages build and serve the migrated embeds and the CLL-1 media without template errors | behavioral | `./build.sh` exit 0; the migrated pages and `public/lesson-audio/cherokee-language-lessons-1---audio-exercises/` (with its 23 `.m4a` files) present in the built tree |
| SC-5 | The CLL-1 page references only `.m4a` media and no zip link | structural | on the CLL-1 page: `audio (file=` embed count = 23, `.m4a` name count = 23, `.mp3` name count = 0, `cll1-v3-audio.zip` count = 0 |
| SC-6 | The 23 `.m4a` files are tracked, correctly encoded, and cap-compliant | structural | `git ls-files` lists exactly 23 `cll1-v3-*.m4a` in the page directory; `ffprobe` each: `codec_name= aac`, `channels= 1`, `sample_rate= 48000`; `stat -c %s` ≤ 25,165,824 bytes each |

(Site-wide build/size gates SC-7/SC-8 remain on the umbrella CherokeeLessons#30.)

## Out of scope

- The `cll1-v3-audio.zip` bundle's own disposition (drop vs re-issue) — deferred to the umbrella; with the sessions tracked as `.m4a` the bundle is locally restorable from them, which the umbrella's disposition decision weighs.
- The four TTS demo-audio dirs (CherokeeLessons#30's tracking work R-2, umbrella).
- Page↔video assignment beyond the mechanical derivation above (Dictation 14-02 stays unassigned).
- Any YouTube-side work beyond embedding (playlist edits, privacy settings).
- The generator's own export change (`audio-lessons-generator-python#16` — separate repository's spec; this conversion imports its settings, it does not implement the generator's code).

## Approach

Mechanical rewrite of the 15 migrated pages to `{{ youtube(id=…) }}` per the derived assignment; the CLL-1 page: delete the zip link, transcode the 23 mp3s to `.m4a` per the ladder, update the embeds to the `.m4a` names, `git add -f`-style explicit adds (note: `.gitignore` covers `*.mp3` but not `*.m4a`, so the new files track normally — verify with `git status`/`git check-ignore` anyway per the media-add pattern); delete the migrated media files and the two generator scripts from the working tree. CSP check: the `{{audio}}`/`{{youtube}}` shortcode output renders `<audio>`/`<iframe>` — the iframe class already covered by `static/_headers` frame-src (itch widgets land there, YouTube is pre-existing; no new iframe origins enter through this change). Rollback: revert the commit set; migration media re-obtainable from the YouTube channel and local mirror (`cherokee-audio-mirror`), CLL-1 mp3s from the same mirror.

## Impact

Consumers: dictation and animal-names page visitors gain playable embedded mirror media on the deployed site (the local-media embeds they replaced are dead-on-deploy today); the CLL-1 page keeps fully playable exercise media, now deployment-surviving rather than dead — exercises remain downloadable as individual files with no channel dependency. The build tree gains ~250 MB of tracked `.m4a` (23 files, max ~21 MB, every file under the 25 MiB asset cap); ~2 GB of working-tree media is retired locally across the other classes. Risk: low — content plus a mechanical transcode; shortcodes pre-existing; the conversion imports settings defined by the generator spec.

> Full spec and plan artifacts: https://github.com/CherokeeLanguage/CherokeeLessons/tree/issues-data/54
<!-- artifacts: .issues/54/ -->
