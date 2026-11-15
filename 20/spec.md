# Spec — Bound Pronouns app page: itch conversion + presentation refresh

CherokeeLessons.com#20 · local-authoritative · sibling of BoundPronouns#59

> **Re-scope (2026-11-14, recomposition split of CherokeeLessons#30, developer direction "remediate by splitting"):** this ticket is the Bound Pronouns per-app site-update ticket, matching the #22–#25/#32 shape. It absorbs #30's R-1/R-1a (SC-1/SC-14) — the itch-link conversion, self-hosted-binary-link removal, store-link removal (all previously absorbed here from this ticket per #30's 2026-11-14 relocation note, now returned), plus the presentation refresh: wording, layout, screenshots, itch widget. #30's spec no longer carries Bound-Pronouns-page content. The original #20 R-1–R-4 wording (narrow additive listing) is superseded by the absorbed, broader R-1/R-1a below.

## Provenance

- **Developer statements (2026-11-05/06, chat):** "the app should also be listed in ../CherokeeLessons.com"; "the app is now only available via itch.io" (2026-11-06 — the page's Play/Apple/Amazon listing entries are superseded; itch.io is the sole current channel).
- **Developer directives (2026-11-11, chat, via #30):** "we are moving to itch-io for publishing apps etc."; "only itch.io is the new release platform" — all store links removed, itch-only.
- **Presentation-refresh directives (2026-11-14, chat, via #30's R-1a):** "update the layout and text wording — the app pages don't describe the apps very well"; "probably also need to use the widget from itch.io"; "adding and including 1 or more screenshots is acceptable; each app repo should have screen shots" (confirmed acceptance 2026-11-14).
- **Observed repo facts (verified 2026-11-06):** `content/apps/bound-pronouns/index.md` carries description, feature prose, support URL (github.com/CherokeeLanguage/BoundPronouns/issues), store links (Google Play, Apple, Amazon — including dead Amazon link `B00TCP955U`, probe matrix `research/store-link-probe.txt`), and direct-download links to stale artifacts (`BoundPronouns-3.41.jar`, `BoundPronouns-3.41-release.apk`; repo also holds 3.40 and 3.30 APKs), last app-update note "(App updated 2022-07-18)".
- **itch.io facts (verified 2026-11-05):** https://cherokee-language.itch.io/cherokee-bound-pronouns is live with `win-linux-mac` and `android` channels carrying the current release.
- **App-repo screenshots (verified loc 2026-11-14):** `/home/muksihs/git/BoundPronouns/artwork/screen-shots/` carries in-repo screenshots — the R-3 screenshot source; never fabricated site-side.

## Problem

The page predates the itch listing: it links stale 2022 self-hosted binaries and dead store links, has no itch link/widget, and its prose under-describes the app (developer, 2026-11-14).

## Scope

This repository: the Bound Pronouns app page `content/apps/bound-pronouns/index.md`, the static screenshots it embeds, and retirement of the bound-pronouns colocated untracked binaries from the working tree. No other app pages (those own their per-app tickets); BoundPronouns#59 owns the itch description text.

## Requirements

- **R-1 — itch conversion (itch-only, absorbed from #30's R-1).** The page SHALL reference its itch.io project URL (https://cherokee-language.itch.io/cherokee-bound-pronouns) as the distribution target, replacing the self-hosted binary links in their actual forms (relative colocated `BoundPronouns-3.41.jar`, `BoundPronouns-3.41-release.apk`) and removing the `### App Stores` and `### Direct Download` link lists — every `play.google.com`, `apps.apple.com`, and `amazon.com` link (including dead Amazon `B00TCP955U`). With the rewrite, the untracked colocated binaries in `content/apps/bound-pronouns/` (6 files) SHALL be deleted from the working tree (no repo change).
- **R-2 — Presentation refresh (absorbed from #30's R-1a).** (a) **Wording** — the page's descriptive text SHALL be expanded into accurate prose, drafted from the app's release notes and the page's existing prose, carrying no factual claim absent from those sources; developer-approved before landing. (b) **Layout** — a prominent distribution section presenting the itch project replaces the direct-download/store link lists. (c) **Screenshots** — ≥ 1 in-app screenshot embedded, sourced from the Bound Pronouns app repo's screenshots (`artwork/screen-shots/`), origin + md5 recorded; missing screenshots are app-repo work, never fabricated site-side. (d) **Itch widget** — the itch.io embed widget for this project (embed code via the live project page's footer "Embed" link; light/dark variant is a developer-review decision). Fallback: if the embed proves incompatible with the site's build/rendering, the plain itch link stands and the disposition is recorded — never silently dropped.

## Out of scope

- Removing the stale 3.41 jar/APK binary files from repo history — working-tree deletion only (they are untracked).
- The itch description text itself (BoundPronouns#59's deliverable).
- Front-matter `tags`/`title` restructuring (precedent: tags left unchanged on #25/#32, flagged in PR body for developer review).
- New-fact invention beyond release notes, README, itch page, and prior page prose.

## Success criteria

| ID | Criterion | Class | Instrument |
|----|-----------|-------|------------|
| SC-1 | The page references its itch project URL, retains no self-hosted binary link (any form), retains no store links, and is fully itch-first | structural | `grep -c "cherokee-language.itch.io/cherokee-bound-pronouns" content/apps/bound-pronouns/index.md` ≥ 1; `grep -nE "\.(apk\|jar)\)\|/downloads/.*\.jar\)" content/apps/bound-pronouns/index.md` is empty; `grep -cE "play\.google\.com\|apps\.apple\.com\|amazon\.com" content/apps/bound-pronouns/index.md` = 0 |
| SC-2 | Presentation refresh (R-2): ≥ 1 embedded in-app screenshot with recorded origin (app-repo sourced) and the itch.io embed widget (or recorded fallback); prose carries no factual claim absent from the app's release notes and the page's prior prose; prominent itch distribution section replaces the direct-download/store link lists; developer wording approval recorded | structural + content | `grep -niE "!\[.*\]\(.*\)" content/apps/bound-pronouns/index.md` present with recorded origin; `grep -c "itch.io/embed/" content/apps/bound-pronouns/index.md` ≥ 1 (fallback disposition otherwise); `grep -cE "^### (App Stores\|Direct Download)" content/apps/bound-pronouns/index.md` = 0; side-by-side prose read + approval record |
| SC-3 | The bound-pronouns colocated binaries are absent from the working tree | structural | `find content/apps/bound-pronouns -name "*.apk" -o -name "*.jar" -o -name "*.x86_64" -o -name "*.exe" -o -name "*.ipa"` is empty |
| SC-4 | The page builds clean and the widget embed carries no CSP frame violation | behavioral | executed `./build.sh` exit 0; browser-rendered served page console shows no "frame-src" CSP violation for the widget iframe (the #22 itch-widget pattern; `static/_headers` frame-src already carries `https://itch.io`) |

(Site-wide build/size gates remain on the umbrella CherokeeLessons#30; SC-4's build clause re-executes the umbrella gate on this repo's sanctioned build path.)

## Approach

Same shape as #22/#23/#25's R-S2 and #32's R-1/R-2: itch-first page rewrite (widget + link + distribution section), screenshots from the app repo, wording drafted from release notes and approved. Rollback: revert the commit set.

## Impact

One page + 1–2 static images + working-tree binary retirement. Risk: low — static content; the widget iframe pattern is proven (#22–#32). The page becomes the last of the five app pages carrying self-hosted-binary links; after it lands, the itch-first migration is complete.

---

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)
