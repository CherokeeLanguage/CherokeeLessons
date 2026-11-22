# Spec — CherokeeLessons#74: drop the 45 superseded source mp3s and the dead CLL-1 mp3 zip — m4a is the site standard

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/74 · filed 2026-11-22 from the developer's chat directives of the same date · related: CherokeeLessons#30 (umbrella; resolves its seam-5 bundle open decision as dropped), #58, #59, #67, #68 (the conversions this cleanup follows)
<!-- artifacts: .issues/74/ -->

## Intent

**Problem Statement.** After CherokeeLessons#58/#59/#67/#68, every audio page ships tracked AAC `.m4a` — 182/182 `{{ audio }}` embeds reference `.m4a`, zero embed mp3s — but 45 mp3s remain tracked in git (41 demo-audio sources in three TTS-demo dirs + 4 espeak-ng clip sources), each with an exact-name `.m4a` counterpart in the same directory, and a 250 MB untracked `cll1-v3-audio.zip` sits in the CLL-1 dir with its page link already dead since #58. The repo carries two copies of the same audio against the developer's stated single-format standard.

**Root Cause / Motivation.** The mp3s were tracked deliberately during the #30-recomposition conversions as source copies pending a single-format decision; the developer has now made that decision: m4a is the current site standard and both formats of the same audio are not kept.

**Approach Chosen.** Untrack and delete the 45 mp3s from the working tree, remove the four stale per-dir `!*.mp3` negation `.gitignore` files and the cll1 dir's stale `*.zip` ignore (the zip is deleted; the dir ships no zip), delete `cll1-v3-audio.zip`, and refresh AGENTS.md's media-facts statements to the post-cleanup state.

**Alternatives Considered & Why Discarded.**
- Keep sources for fidelity (m4a is lossy) — discarded: the developer's explicit disposition is single-format m4a; the decision is the developer's product decision and is recorded here.
- Untrack without deleting from disk — discarded: retires the guarantee while keeping a disk artifact that fresh clones never see; contradicts "cleanup".
- External re-host / R2 backup for the mp3s — discarded: R2 is not a valid delivery path for this repo (ruling of record 2026-11-12).

**Key Design Decisions.** (1) The 4 espeak-ng clips are included in the deletion: each has an exact-name `.m4a` counterpart (verified `oo1sdee3gaa_.mp3`/`.m4a`, `sdvv2goo2hvv23-ii_`, `sgwoohl2gaa_`, `sgwoohl2gii_`), and the developer's "no desire to keep both mp3 and m4a of the same audio" names the class. (2) The deletion is unconditional working-tree removal (not `git rm --cached`): the git history retains the mp3s for forensic recovery at any past commit. (3) The `fix-mp3-tags.sh` script in the demo dir becomes dead tooling with no mp3s to tag — left in place and flagged, not removed (script removal is not named by the directive). (4) The Cherokee Phoenix page's off-site mp3 links (data.cherokee.org) and the fonts/NumbersMaze zips are outside scope (not local media, not mp3 media).

**User Intent / Original Prompt (2026-11-22, chat).** "we need to go ahead and cleanup where the mp3s are no longer needed"; "same for the zips of the mp3s"; "I have no desire to keep both mp3 and m4a of the same audio, especially as the m4a is the current standard for the site".

## Not Included

- Removing `fix-mp3-tags.sh` or any other tooling (flagged only).
- Touching the Cherokee Phoenix page's external mp3 links, the fonts zip, or the NumbersMaze zip.
- Any new media conversion or re-encoding.
- The CherokeeLessons#30 umbrella's site-wide gates (they execute after the last part lands — a separate, open decision surface).

## Requirements

- **R-1 — mp3 retirement.** All 45 tracked mp3s (`cherokee-animal-names--ims-toucan-tts-` ×9, `cherokee-animal-names-tts-demo-audio-1` ×24, `cherokee-tts-update-3--ims-toucan-` ×8, `espeak-ng-and-ced-phonetics` ×4) SHALL be untracked (`git rm`) and absent from the working tree. Their exact-name `.m4a` counterparts SHALL be untouched.
- **R-2 — ignore-rule cleanup.** The four per-dir `.gitignore` files whose sole content is `!*.mp3` ("cherokee-animal-names--ims-toucan-tts-", "cherokee-animal-names-tts-demo-audio-1", "cherokee-tts-update-3--ims-toucan-", "espeak-ng-and-ced-phonetics") SHALL be removed; the cll1 dir's `*.zip`-only `.gitignore` SHALL be removed with the zip itself (R-3).
- **R-3 — mp3 zip removal.** `content/lesson-audio/cherokee-language-lessons-1---audio-exercises/cll1-v3-audio.zip` SHALL be deleted from the working tree; no page reference reappears (the #58 state — zero `cll1-v3-audio.zip` references — is retained). The zip is untracked, so this is a working-tree delete only. The CherokeeLessons#30 seam-5 bundle decision is superseded as "dropped".
- **R-4 — docs currency.** AGENTS.md's media-facts statements (the "45 tracked mp3s" account, the per-dir `!*.mp3` negation pattern, and the mp3-tracking guidance paragraph) SHALL be updated to the post-cleanup state: zero tracked mp3s, 199 tracked m4a as the sole kept audio format.

## Success criteria

| ID | Criterion | Class | Instrument |
|---|---|---|---|
| SC-1 | Zero tracked mp3s and zero mp3s on disk | structural | `git ls-files '*.mp3'` empty; `find content -name '*.mp3' -o -name '*.zip' -path '*lesson-audio*'` empty |
| SC-2 | All m4a preserved: 199 tracked `.m4a` unchanged and every page's audio embeds resolve to tracked m4a | structural | `git ls-files '*.m4a' \| wc -l` = 199 (against the pre-change working-tree count); `git diff master..HEAD --stat` shows no `.m4a` rows; embed count 182/182 `.m4a` and 0 `.mp3` (grep) |
| SC-3 | Stale ignore rules gone: the four `!*.mp3`-only negation files and the cll1 dir's `*.zip`-only file are absent; no tracked file's ignore rules include `!*.mp3` | structural | `git ls-files` on each of the four paths empty; `git grep -l '!*.mp3' -- '*.gitignore'` empty |
| SC-4 | The mp3 zip is gone and stays unreferenced | structural | `test ! -f …cll1-v3-audio.zip`; page zip reference count 0 (`grep -c 'cll1-v3-audio.zip' content/lesson-audio/cherokee-language-lessons-1---audio-exercises/index.md` = 0) |
| SC-5 | AGENTS.md media facts state the post-cleanup truth: no "45 tracked mp3s" retention claim, no `!*.mp3` negation guidance as a live pattern | structural | grep AGENTS.md: "45 tracked mp3s" absent; statements consistent with zero tracked mp3s / m4a-only audio |
| SC-6 | The site builds clean on the composed state | behavioral | `./build.sh` exit 0 |

## Dependencies

| Reference | Relationship | Status |
|---|---|---|
| Developer directives 2026-11-22 (chat) | the authorizing intent; the single-format disposition | satisfied (quoted in Intent) |
| CherokeeLessons#58/#59/#67/#68 | the conversions whose tracked mp3s this cleanup retires; m4a counts and forms carried forward | satisfied (merged) |
| CherokeeLessons#30 (umbrella) | seam-5 bundle disposition superseded by R-3 as "dropped" — recorded here, umbrella edit not this spec's surface beyond the flag | umbrella open (out of scope) |

## Traceability

| Requirement | Success criteria |
|---|---|
| R-1 | SC-1, SC-2 |
| R-2 | SC-3 |
| R-3 | SC-1, SC-4 |
| R-4 | SC-5 |
| (build gate) | SC-6 |

## Documentation Sources

| Source | Type | Location | Verification |
|---|---|---|---|
| The 45 tracked mp3s and their exact-name m4a counterparts (all 45 stem-verified 2026-11-22) | working tree / git | `git ls-files '*.mp3'`; per-stem counterpart check executed in session | executed 2026-11-22 |
| Embed state (182/182 m4a, 0 mp3) | working tree | grep audit executed in session | executed 2026-11-22 |
| Media tooling + mp3/m4a tracking facts | repo facts | `AGENTS.md` (Build & verification, Content facts) | read 2026-11-22 |
| Zip bundle disposition origin | issue store | `.issues/58/spec.md` key-decision 4 / Not-Included (seam 5 deferral), #30 spec | read 2026-11-22 |

## Cost Frame

- SC-1–SC-5: greps and git listings — minutes; skipping risks a tracked mp3 surviving (the standard contradicted) or a stale ignore rule re-including a re-added mp3.
- SC-6: one full build; skipping risks shipping a broken composed state after mass deletions.

## Completion gate

Every SC passes before the deliverable is complete. SC-2's m4a preservation makes the change all-or-nothing with respect to the kept format: no m4a may be removed or modified by this cleanup's commits.

<!-- artifacts: .issues/74/ -->
