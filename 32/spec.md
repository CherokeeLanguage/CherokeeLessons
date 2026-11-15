# Spec — CLL-1 app page itch link (depends on the cll1-gdx publish)

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/32

## Problem

After the distribution-policy migration (CherokeeLessons#30) removes every store link — dead by probe or retired by the itch-only policy — and after the cll1-gdx publish (CherokeeLanguage/cll1-gdx#5) delivers the CLL-1 itch project, the site's CLL-1 page (`content/apps/cherokee-language-lessons-1.md`) is the only app page left with no distribution link. The developer's itch-only decision (2026-11-11: "only itch-io is the new release platform") makes `https://cherokee-language.itch.io/cherokee-language-lessons-1` the page's distribution target. This spec adds that link only once it can resolve — the site never ships a dead itch link.

## Provenance

- **Developer directives (2026-11-11):** "the cll1 app needs an itch-io page setup"; "only itch-io is the new release platform"; "pre-existing store links are all dead"; "file a spec to go ahead and migrate to itch-io and to publish the latest version with the cll1-gdx repo"; "all specs must be filed in the correct repos and not have work which crosses repo boundaries. proper interdependencies must be declared between specs".
- **Page baseline (probed 2026-11-11, `research/cll1-page-probe.txt`):** the page carries 3 store links (Play 404, Apple 404, Amazon 503) + 1 desktop-jar link (`/downloads/CLL1-1.72.jar`) and zero itch links today; #30 removes the store and jar links, cll1-gdx#5 delivers the itch project.
- **Cross-repo filing rule:** this spec's scope is this repository's files only; the build/publish deliverable lives in CherokeeLanguage/cll1-gdx#5 (that repo's agent triages it before plan/implementation).

## Interdependencies

- Depends on: **CherokeeLanguage/cll1-gdx#5** — its itch.io project live at slug `cherokee-language-lessons-1` with the 1.82 desktop jar and Android APK published (that spec's SC-3/SC-5). R-1 cannot execute until that deliverable is verifiably live.
- Sequenced after: **CherokeeLessons#30** (this page's store links and desktop-jar link are removed there; this spec's criteria re-assert those end-states rather than re-own them).
- Dependents: none.

## Scope

`content/apps/cherokee-language-lessons-1.md` — this repository's only file. Nothing else. The itch.io project creation, the 1.82 build, and the butler publish live in CherokeeLanguage/cll1-gdx#5; site-side work never crosses the repo boundary.

## Requirements

- **R-1 — Itch link and widget (revised 2026-11-14, developer direction: the itch.io embed widget is spec-level content, not plan-level presentation).** The CLL-1 page SHALL reference `https://cherokee-language.itch.io/cherokee-language-lessons-1` as its distribution link, and SHALL embed itch.io's embeddable widget for this project — a self-contained iframe that pulls the project's cover and description from itch (embed code is obtainable from the live project page's footer "Embed" link, no dashboard access required; light/dark variants (per itch's docs) are available and the presentation choice is a developer-review decision). Fallback: if the embed proves incompatible with the site's build/rendering at implementation, the plain itch link stands and the disposition is recorded — the widget is never silently dropped.
- **R-2 — Page presentation refresh — layout, wording, screenshots (added 2026-11-14, developer direction: "update the layout and text wording — the app pages don't describe the apps very well"; "adding and including 1 or more screenshots is acceptable; each app repo should have screen shots").** Executed together with the R-1 edit, the page's rewrite SHALL go beyond the link addition: (a) **Wording** — the page's descriptive text SHALL be expanded into prose that accurately describes what the app does and its notable capabilities — drafted by the agent from the app's release notes and the page's existing prose, carrying no factual claim absent from those sources, reviewed/approved by the developer before the edit lands; (b) **Layout** — a prominent distribution section presenting the itch project (widget per R-1); (c) **Screenshots** — the page SHALL embed one or more in-app screenshots sourced from the cll1-gdx repo's screenshots; if a needed screenshot does not yet exist, its production is app-repo work and this ticket declares the dependency — never fabricates or substitutes site-side images.

## Success criteria

| ID | Criterion | Class | Instrument | Traces to |
|---|---|---|---|---|
| SC-1 | The CLL-1 page references the itch project URL | structural | `grep -c "cherokee-language.itch.io/cherokee-language-lessons-1" content/apps/cherokee-language-lessons-1.md` ≥ 1 | R-1 |
| SC-2 | The page carries no store links and no desktop-jar link (#30's end state, re-asserted) | structural | `grep -cE "play\.google\.com|apps\.apple\.com|amazon\.com" content/apps/cherokee-language-lessons-1.md` = 0; `grep -c "downloads/CLL1" content/apps/cherokee-language-lessons-1.md` = 0 | R-1 |
| SC-3 | The page edit does not break the build | behavioral | executed `./build.sh` over the edited working tree; exit 0 | R-1 |
| SC-4 | The page embeds the itch.io embed widget for this project (or the recorded fallback disposition) | structural | `grep -c "itch.io/embed/" content/apps/cherokee-language-lessons-1.md` ≥ 1; on fallback, the work record carries the disposition | R-1 |
| SC-5 | The page's descriptive prose accurately describes the app — no factual claim absent from the app's release notes and the page's prior prose; the developer's wording approval is recorded | content | side-by-side read against the release notes / prior page prose; approval in the work record | R-2(a) |
| SC-6 | The page embeds ≥ 1 in-app screenshot sourced from the cll1-gdx repo's screenshots | structural | `grep -niE "!\[.*\]\(.*\)" content/apps/cherokee-language-lessons-1.md` (image embed) present; the referenced file's origin recorded in the work record | R-2(c) |

Traceability note: R-2(b) (layout) is a presentational rewrite with no standalone grep instrument — its satisfaction is verified by the work's own record (the reviewer's before/after page read), per the R-7 procedural-note style.

## Impact

Consumers: the CLL-1 page's visitors (a live distribution target replaces three dead store links and a removed desktop jar); the itch-only migration (distribution complete for all five app pages). Rollback: revert the page edit (including its added image assets).

## Out of scope

- The itch.io project creation, the 1.82 build, and the butler publish — CherokeeLanguage/cll1-gdx#5 (its deliverable is this spec's dependency).
- Any other page or asset — CherokeeLessons#30.

## Approach

A one-line page edit following the itch-link pattern #30 establishes for the other four app pages, executed only after the dependency's deliverable is verifiably live (its SC-3/SC-5). Alternatives weighed: adding the itch link inside #30 — rejected (its execution would block on another repo's pipeline, coupling the specs); leaving the page link-less until a bulk rewrite — rejected (the page would ship with no distribution path indefinitely).

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)
