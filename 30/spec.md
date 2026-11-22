# Spec — content distribution & asset policy for the cherokeelessons.org stand-up (umbrella)

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/30

> **Recomposition (2026-11-14, developer direction "remediate by splitting"):** the over-fat spec was recomposed into per-concern parts. The app-page rewrites (including the Bound Pronouns page and all per-app SCs) moved to **CherokeeLessons#20**; the lesson-media → YouTube embed migration (R-2/SC-4/SC-15) to **CherokeeLessons#54**; the ebook store-link removals (R-7/SC-16) to **CherokeeLessons#55**. All three are sub-issues of this ticket. This spec retains what only it owns: the LPT surface removals, the tracking additions (R-2/R-3/R-4), the vestigial-artifact and retired-binary hygiene, and the site-wide build/size gates. Superseded content is deleted (not struck): the relocated requirements, their SCs, and the per-app scope rows no longer appear here — each part's spec is its single source of truth.

## Problem

The site carries ~3.49 GB of untracked-but-site-required assets, and its distribution model is changing by developer decision (2026-11-11, revised per-page 2026-11-15/16): app/game distribution moves to itch.io (`cherokee-language.itch.io`); lesson media divides per page — the Syllabary-Dictation videos to the developer's YouTube channel (`@-cherokeelanguage7554`), the two lesson-audio pages to local tracked AAC `.m4a` — and the ebook to the site only (directives of record live on the owning parts' specs — #55's Problem section carries the 2026-11-12 directive text; #54's Problem carries the 2026-11-15/16 media-method decisions). This umbrella owns the cross-cutting remainder: three page-less direct-URL surfaces from the private Language-Preservation-Technologies repos are slated for removal, the TTS demo audio, book TeX sources, and Google verification file are untracked (so a fresh-clone build is incomplete), and retired-asset hygiene is pending. An executed stand-up gate (2026-11-11, shallow clone + pinned-zola build: 1,547 files / 0.48 GiB, exit 0) shows the fresh-clone build omits every untracked asset class; the same gate informed CherokeeLessons#31's sequencing, which executes first and clears the four over-limit tracked files itself (its R-8/R-9).

## Provenance

- **Developer directives (2026-11-11, chat, in order):** "we are moving to itch-io for publishing apps etc."; factor in the YouTube channel overlap; "the unpublished apps that do not have real pages on the cherokeelessons.com site for language preservation technologies need to be re-scoped for removal from the site"; "track the tts demo audio — … determine what the text sources are for or contain"; "track them" (the tex sources); "file specs as needed to make the needed edits so that cherokeelessons.org becomes the new proper site".
- **Recomposition directive (2026-11-14, chat):** "remediate by splitting" — issued on the observed separation-of-concerns defect in #30's post-relocation shape.
- **LPT sources (local repos, verified 2026-11-11):** `intro_to_animals` export presets name the site artifacts exactly (`export/html5/index.html`, `export/Mac-OSX/animals-say-and-see.zip`, `export/Win-x64/…exe`, `export/linux-x64/…x86_64`); the VN is `mi_ma_ale_mo_uniyo_wesa` (Ren'Py 7.3.5.606 build; `options.rpy` title byte-identical to the site's page title). Both repos private (GitHub 404 unauthenticated).
- **Zero internal references** to the three LPT surfaces (source greps over `content/`, `templates/`, `config.toml`, and `themes/` return empty — independently re-verified during validation — and SC-5 structurally re-checks the post-change state).
- **Text sources for the TTS demos (verified 2026-11-11):** generation text lives in public repos (`CherokeeLanguage/Cherokee-TTS` archived: `data/tests/animals/animals-game-mco.txt`; `CherokeeLanguage/cherokee-audio-data` active: tracked contents include `ced-mco.txt`, `voice-ids.txt`, and the generator scripts) or the tracked pages themselves (the Update-3 stories are printed in full in `index.md`) — tracking the audio completes reproducibility with no further text work.

## Parts and seams

Parts (each closes on its own delivered work; this umbrella closes only after every part has closed):

- **CherokeeLessons#20** — Bound Pronouns app page: itch conversion (R-1) + presentation refresh (R-2; wording/layout/screenshots/itch widget), bound-pronouns colocated binary retirement.
- **CherokeeLessons#54** — Syllabary-Dictation pages: the 14 dictation pages migrate 20 `{{video}}` embeds to `{{ youtube(id=…) }}`; the dictation mp4s retire from the working tree (spec re-cut 2026-11-20 — its dictation spec is the single source of this part's record).
- **CherokeeLessons#58** — CLL-1 audio exercises: the 23 session mp3s convert in place to tracked AAC `.m4a` per-track ladder (owning definition audio-lessons-generator-python#16); its `process_end_notes.py` retained and updated to the `.m4a` form; the dead `cll1-v3-audio.zip` link removed (split from #54, 2026-11-20 — its spec is the single source of the record).
- **CherokeeLessons#59** — animal-names page: the 9 session mp3s convert in place to tracked AAC `.m4a` via the same ladder; its `process_end_notes.py` retained and updated (split from #54, 2026-11-20 — its spec is the single source of the record). [closed, completed 2026-11-21]
- **CherokeeLessons#67** — tracked TTS-sample pages (espeak-ng-and-ced-phonetics, tacotron2-and-cherokee-tts, here-is-why, tts-update--2): the four pages' 126 mp3s convert in place to tracked AAC `.m4a` via the same ladder; pages gain per-entry `⤓ download` anchors (the #59 form); the 4 untracked espeak clips arrive tracked-as-m4a here with their source mp3s tracked via negation (the 2026-11-21 ruling: untracked audio is lost audio), descoping them from the original R-2 (filed from the developer's 2026-11-21 directive — its spec is the single source of the record).
- **CherokeeLessons#68** — TTS demo-audio pages (cherokee-animal-names-tts-demo-audio-1, cherokee-animal-names--ims-toucan-tts-, cherokee-tts-update-3--ims-toucan-): the class's 41 mp3s convert to tracked AAC `.m4a` with per-entry download links, the `SPEAKER|AUDIO MP3` tables take the 3-column form, and the unrecoverable mp3 sources track via negation — the new audio standard applied (filed from the developer's 2026-11-21 directive; supersedes the original R-2 — its spec is the single source of the record).
- **CherokeeLessons#55** — book pages: external ebook-store links removed (15 pages + `new-page.sh` scaffold lines).

Seams the umbrella names (each part builds on them; nothing part-local is restated here):

1. **App-binary retirement split:** the bound-pronouns colocated binaries bind to #20 (SC-3); the animals (2 files) and cherokee-numbers-maze (3 files) colocated binaries and the untracked `static/HTML-Games/NumbersMaze` web build bind to this umbrella (R-5/SC-6) — their pages' link edits already landed via #22/#25.
2. **Media-file retirement split:** the dictation mp4s retire on migration (CherokeeLessons#54, its SC-2); the lesson-audio mp3s delete on their in-place `.m4a` conversions (CherokeeLessons#58 SC-2, CherokeeLessons#59 SC-2); the tracked TTS-sample mp3s retire through CherokeeLessons#67's in-place conversion (122 deleted, git-history recoverable — its espeak clip sources and the demo sources subsequently dropped by CherokeeLessons#74, 2026-11-22: the sole-format standard, m4a kept, mp3s and the mp3 zip removed, history-retained); the TTS demo-audio class ships tracked `.m4a` with per-entry download anchors — its source-backed deliverables were re-encoded at the 192k rung by CherokeeLessons#76 (2026-11-22, from the #74-dropped originals recovered from git history).
3. **Site-wide gates:** SC-7/SC-8 (fresh-clone build; over-limit count) execute after the last part lands — they verify the composed whole and cannot pass early. No part carries them.
4. **Wording-approval gate:** #20's wording change carries a developer-approval-before-landing clause — an approval gate, not a seam, recorded there for single-sourcing.
5. **Open decisions (unchanged):** the `cll1-v3-audio.zip` bundle's own disposition (drop vs itch re-issue — CherokeeLessons#58 removes only the page's dead link to it); the Lulu paperback links and `content/app-ads.txt` reseller entries (open developer decisions, recorded, criterion set does not touch them); Hangman1 (#33).

## Scope

| Surface | Change |
|---|---|
| `static/Visual-Novels/mi-ma-mo-uniyo-wesa/`, `static/HTML-Games/LPT-IntroToAnimals/`, `static/downloads/animals-say-and-see/` (untracked) | deleted from the working tree |
| `static/Visual-Novels/.gitignore`, `static/downloads/.gitignore` (tracked) | deleted (vestigial) |
| per-dir `.gitignore` files for the three demo-audio dirs | superseded — the class's tracking work moves to CherokeeLessons#68 (the 2026-11-21 new-audio-standard directive: page audio tracked `.m4a` with download links; the unrecoverable mp3 sources tracked via negation) |
| 3 `tex-source/.gitignore` files | rules removed; 3 `.tex` + 3 `.md` tracked (~1.6 MB) |
| `content/.gitignore` | google verification rules removed; file tracked |
| untracked colocated app binaries (animals: 2 files; cherokee-numbers-maze: 3 files) + untracked `static/HTML-Games/NumbersMaze` web build | deleted from the working tree (their pages' link edits landed via #22/#25) |

No hosting/CDN/Workers changes (CherokeeLessons#31); no theme changes; no itch.io project creation (ops); no app-page or book-page edits (parts #20/#54/#55).

## Requirements

- **R-1 — LPT surfaces removed.** The three untracked directories SHALL be deleted from the working tree; the two vestigial tracked ignore artifacts SHALL be deleted.
- **R-2 — TTS demo-audio class: transferred (superseded by CherokeeLessons#68).** The demo-audio class's work — the three dirs' 41 untracked mp3s (`cherokee-animal-names-tts-demo-audio-1` 24, `cherokee-animal-names--ims-toucan-tts-` 9, `cherokee-tts-update-3--ims-toucan-` 8) and their page treatment — moves to CherokeeLessons#68, filed on the developer's 2026-11-21 directive ("tracking everything feasible, using the new standard for audio", extending the same-day ruling "an untracked audio file is a lost audio file"): the standard is tracked AAC `.m4a` page audio with per-entry download links (the #58/#59/#67 form), and the unrecoverable mp3 sources (no channel mirror for the class) are tracked via the `!*.mp3` per-dir negation pattern this requirement originally carried. The original R-2's negation-delivery method (mp3s tracked as the page audio, no conversion) is superseded. The 4 espeak clips left this scope earlier the same day through CherokeeLessons#67.
- **R-3 — Track the book TeX sources.** The 3 `.tex` files and their 3 `.md` conversions SHALL be tracked by removing the `tex-source/.gitignore` rules.
- **R-4 — Track the Google verification file.** `content/google593a568777046d5c.html` SHALL be tracked (the `content/.gitignore` google rules removed) — required for Search Console verification on the new host, consistent with R-2/R-3's reproducibility intent.
- **R-5 — Retired app-binary and web-build hygiene.** The untracked colocated binaries in the animals and cherokee-numbers-maze app dirs (2 + 3 files) and the untracked `static/HTML-Games/NumbersMaze` web build (7 files) SHALL be deleted from the working tree (their pages' link edits landed via #22/#25 — the artifacts are no longer referenced; no repo change — untracked).

## Success criteria

| ID | Criterion | Class | Instrument |
|---|---|---|---|
| SC-1 | `static/downloads/` carries no tracked files — the jars deleted by CherokeeLessons#31's R-9 (executes first), the vestigial `.gitignore` deleted by this spec's R-1 | structural | `git ls-files static/downloads` is empty |
| SC-2 | The three LPT directories are absent from the working tree; the two vestigial ignore artifacts are removed | structural | `test -d` fails for all three; `git ls-files static/Visual-Novels` and `git ls-files static/downloads/.gitignore` are empty |
| SC-3 | The demo-audio transfer is recorded and resolvable: the store links CherokeeLessons#68 as a sub-issue of #30, and #68's spec carries the transferred class (3 dirs, 41 mp3s), the source-tracking requirement (`!*.mp3` negation for the unrecoverable originals), and the supersession record naming this R-2 | structural | `local-issues` sub-issue listing / `.issues/30/links.yaml` contains 68; grep checks on `.issues/68/spec.md` for the transferred scope and the source-tracking requirement |
| SC-4 | The 3 tex + 3 md companions are tracked | structural | `git -c core.quotepath=off ls-files '*tex-source/*'` finds the 6 content files |
| SC-5 | No content references the removed LPT surfaces; the Google verification file is tracked | structural | `grep -rnE "mi-ma-mo-uniyo-wesa|LPT-IntroToAnimals|animals-say-and-see" content templates config.toml` is empty; `git ls-files content/google593a568777046d5c.html` lists it; `grep -c google content/.gitignore` = 0 |
| SC-6 | The retired untracked app binaries (animals + cherokee-numbers-maze dirs) and the `static/HTML-Games/NumbersMaze` web build are absent from the working tree | structural | `find content/apps/animals content/apps/syllabary* content/apps/cherokee-numbers-maze* -name "*.apk" -o -name "*.jar" -o -name "*.x86_64" -o -name "*.exe" -o -name "*.ipa"` is empty; `test -d static/HTML-Games/NumbersMaze` fails |
| SC-7 | Fresh shallow-clone build succeeds post-change (executed after the last part lands) | behavioral | executed `git clone --depth 1` + `./build.sh`; exit 0 |
| SC-8 | Post-change fresh-clone `public/` contains no files over 25 MiB (executed after the last part lands) | structural | post-build `find public -type f -size +25M \| wc -l` = 0 in the SC-7 build (the jars and PDFs are already cleared by CherokeeLessons#31's R-8/R-9; this criterion guards the tracking additions against introducing over-limit files) |

## Interdependencies

- Depends on: none (first in the filing sequence); CherokeeLessons#31 executes first per the revised sequencing (2026-11-12) and clears the four over-limit tracked files itself.
- Parts: #20 (Bound Pronouns page), #54 (dictation embeds — re-cut 2026-11-20 into #54 dictation slice, #58 CLL-1, #59 animal-names), #55 (ebook links), #67 (tracked TTS-sample pages, 2026-11-21), #68 (TTS demo-audio class, 2026-11-21 — absorbs the original R-2) — sub-issues; closure order per the Parts section.
- Independent: CherokeeLanguage/cll1-gdx#5 (the CLL-1 build/publish — no dependency in either direction; #32 consumed its deliverable, and #32 has landed).

## Out of scope

- The Cloudflare Workers hosting stand-up (CherokeeLessons#31) — including the lemmatizer retirement + `_redirects` rule (kept whole there) and the oversized-PDF delivery disposition (the R2 plan was ruled invalid 2026-11-12; the disposition decision is the developer's).
- All app-page and book-page content edits — parts #20/#54/#55.
- The CLL-1 itch.io project creation, build/publish, and page link — landed via #32 (2026-11-15).
- The `cll1-v3-audio.zip` bundle's own disposition — open decision (recorded, seam 5).
- The Lulu paperback links and `content/app-ads.txt` reseller entries — open developer decisions (recorded, seam 5; no criterion touches them).
- Widget-embed presentation on the itch-backed pages — the per-page requirements live on their owning tickets (#20; landed #22/#23/#25/#32).
- The store-link retention policy beyond removing the verified-wrong links.

## Approach

Follow the developer's distribution decisions of record — external channels for apps (itch.io), the Syllabary-Dictation videos (YouTube) with the lesson-audio pages local (`AAC .m4a`, per the 2026-11-15/16 revisions recorded in #54), and the ebook (site-only); removal for the page-less LPT surfaces; tracking for site-required sources. The pre-split alternatives-weighed record (legacy `.htaccess` conversion rejected; self-hosted binaries alongside itch rejected; keeping store links rejected) transfers with its owning concerns to the parts — its "local re-encoding instead of YouTube rejected" item was decided at the 2026-11-11 stand-up frame and is superseded for the lesson-audio pages by the developer's later per-page method decisions; this umbrella adds none (the superseded item no longer transfers as a constraint): tracking and untracked-surface deletion are the repo's own established patterns, weighed against leaving the assets untracked (rejected: the executed gate shows every untracked class is absent from fresh-clone builds — the reproducibility defect of record).

## Impact

Consumers: the site build (a fresh-clone build becomes complete for all retained surfaces — the executed gate's omissions for the tracked classes disappear except the deferred zip class); the local working tree (shrinks further: ~388 MB LPT surfaces + ~50 MB animals/NumbersMaze binaries + web build; the media-weight effect of the transferred and converted parts is recorded on their own specs — CherokeeLessons#67: 122 mp3s retired git-history-recoverable, 4 clip sources tracked, ~3.7 MB m4a added; CherokeeLessons#68: 41 m4a deliverables plus the 41 retained mp3 sources tracked for durability, its Key Design Decisions (5) flagging the ~2× media weight for the three demo dirs for the developer's approval gate — the superseded R-2's ~212 MB figure no longer applies). The umbrella is the last ticket in the stand-up's content-policy chain to close: SC-7/SC-8 verify the composed whole after #20/#54/#55 land. Rollback: revert the commit set; untracked deletions re-obtainable per class (LPT from the private repos, app artifacts from itch, tex/md from history).

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)
