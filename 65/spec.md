# Spec — CSP style-src origin for the giscus widget's injected stylesheet

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/65 · filed 2026-11-20 under the developer's directive "add an sc to the spec to implement and do so" (following the reported widget-rendering defect); sibling records: CherokeeLessons#62 (the runbook — its Diagnosis (d) names frame-src as the CSP differential), CherokeeLessons#63/#64 (the data-repo fix this branches on top of, whose spec pins its diff away from CSP)
<!-- artifacts: .issues/65/ -->

## Intent

**Problem Statement.** The site's giscus `client.js` injects a wrapper stylesheet (`https://giscus.app/default.css`) into the document that sizes the comment frame — but `static/_headers`'s `style-src` ("'self' 'unsafe-inline' https://fonts.googleapis.com") carries no `https://giscus.app` origin, so the injection is refused (console, verbatim: `Refused to load the stylesheet 'https://giscus.app/default.css' because it violates the following Content Security Policy directive: "style-src 'self' 'unsafe-inline' https://fonts.googleapis.com". Note that 'style-src-elem' was not explicitly set, so 'style-src' is used as a fallback. @ https://giscus.app/client.js:5`). Measured on the CherokeeLessons#63 preview (2026-11-21): the widget iframe computes to its intrinsic **300px** width inside the 955px article column — the widget functions but renders narrow, the defect the developer reported ("the giscus widget is not displaying very well").

**Root Cause / Motivation.** The CSP of record (#54 era, 2026-11-20) derived its style-src from the theme-font origins and carried giscus.app in script-src, frame-src, and connect-src — but not style-src. The giscus client's document-stylesheet injection was not part of that derivation's observed origin set, and the refusal went to the console as ambient noise until the developer observed the visual consequence. A live mutation on the preview (setting the frame's `width: 100%` → 955px) proved the refused stylesheet is the missing sizing source. It must be solved now because the developer directed it in this cycle and the fix is one tracked header token.

**Approach Chosen.** One-origin addition to `static/_headers`'s `style-src`: `https://giscus.app`. Shipped as tracked site content through the site's pipeline: feature branch (stacked on `feature/63-giscus-data-repo-pointer` so the preview carries the data-repo fix and the widget addresses the right repository) → PR preview → human merge → Workers Builds auto-deploy on master.

**Alternatives Considered & Why Discarded.**
- Riding CherokeeLessons#64's PR with a second commit: rejected — #63's spec pins its diff to exactly the one `data-repo` attribute (SC-2), its scope is complete and verify-passed awaiting human merge, and the git-workflow scope rules keep these two records separate; the developer's directive lands the requirement in its own spec record.
- Inline-styling the frame from `templates/page.html` (avoid the stylesheet inject): rejected — fights the giscus client's own mechanism, duplicates what the origin allows, and couples our template to giscus internals that may change.
- Broadening style-src with `'unsafe-inline'` extensions or a splat: rejected — the minimal origin-specific grant is the narrowest change consistent with the CSP's derived form.
- Doing nothing (the noise is console-only): rejected — the rendering defect is a stated developer requirement, not a preference.

**Key Design Decisions.**
- Origin-specific grant: `https://giscus.app` joins `style-src` only; no other directive changes.
- The behavioral verification runs on the *served* preview (not localhost — the CherokeeLessons#59 record rules out bare-localhost widget diagnosis) and measures geometry, not just console silence: computed frame width ≈ wrapper width.
- Stacked branch: `feature/65-giscus-style-src` off `feature/63-giscus-data-repo-pointer`'s tip so the preview includes #63's fix; the PR targets master and becomes a clean one-commit diff after #64 merges.
- Draft PR as preview infrastructure; the PR-ready transition waits on the developer's word (the directive carried implementation, and the filing boundary stays theirs).

**User Intent / Original Prompt.**

- Developer, 2026-11-20: "the giscus widget is not displaying very well." + "playwright check https://feature-63-giscus-data-repo-pointer-cherokeelessons-org.m-conrad-202.workers.dev/content/how-to-use-public-domain-comics-for-cherokee-language-teaching/" (the check executed; findings reported in-session)
- Developer, 2026-11-20: "add an sc to the spec to implement and do so"
- Session-observed failures: the console refusal line (above); computed frame width 300px vs 955px wrapper (playwright `getBoundingClientRect`, 2026-11-21); live mutation restoring 955px proving the causal link.

## Not Included

- Any other CSP directive — script-src/frame-src/connect-src already carry giscus.app correctly.
- The transient "Loading comments…" backend behavior (both pages stalled 40+s during diagnosis; the same page rendered fully an hour earlier) — environmental, not this change's subject.
- CherokeeLessons#64's scope and merge state — separate record and branch.
- Local-serve widget behavior (#59's record) — carried by #62's serve-context branch.

## Requirements

- **R-1** — The `style-src` directive in `static/_headers` SHALL carry `https://giscus.app` (the origin giscus's client injects document stylesheets from).
- **R-2** — No other CSP directive SHALL change; the working-tree diff against the branch base SHALL be the single `style-src` token addition.
- **R-3** — The rendered-page behavior SHALL be verified on a served preview of the branch: the giscus iframe computes to its wrapper column's width (no intrinsic-300px render) and the console logs no `style-src` refusal for `giscus.app/default.css`.

## Success criteria

| ID | Criterion | Class | Instrument |
|---|---|---|---|
| SC-1 | The style-src carries the giscus origin | structural | `grep -cE 'style-src.*(unsafe-inline ).*https://giscus\.app' static/_headers` ≥ 1 |
| SC-2 | The diff touches only the style-src directive | structural | `git diff <base-tip> -- static/_headers` shows exactly one removed/added line pair; the removed line's style-src lacks giscus.app; the added line's style-src carries it; every other directive identical between the two lines |
| SC-3 | The rendered widget computes full-column width with no refusal | behavioral | playwright render of the branch preview (headless msedge, scrolled to the widget): `document.querySelector("iframe[src*=giscus]").getBoundingClientRect().width` ≈ the `.giscus` wrapper's width (955-class), and the console contains no `Refused to load the stylesheet 'https://giscus.app/default.css'` line |

## Dependencies

| Reference | Relationship | Status |
|---|---|---|
| CherokeeLessons#63/#64 | stacked underneath (preview carries the data-repo fix; widget addresses the right repo) | satisfied (branch tip 2d0cad6) |
| `static/_headers` (tracked CSP of record) | the change's surface | satisfied (read 2026-11-20) |
| `gh` authentication / `./build.sh` | PR + build-gate machinery | satisfied |
| The giscus injection behavior | grounds R-1 (client.js injects document-level stylesheet links) | satisfied (live: `<link rel="stylesheet" href="https://giscus.app/default.css">` present in the DOM; refusal logged; mutation proof) |

## Traceability

| Requirement | Success criteria |
|---|---|
| R-1 | SC-1 |
| R-2 | SC-2 |
| R-3 | SC-3 |

No orphan rows in either direction (SC-1→R-1; SC-2→R-2; SC-3→R-3).

## Documentation Sources

| Source | Type | Location | Verification |
|---|---|---|---|
| `static/_headers` CSP line (style-src lacking giscus.app) | tracked repo file | `static/_headers:13` | read 2026-11-20 |
| Console refusal line (verbatim) | live observation | #63-preview render, console log `.playwright-cli/console-2026-11-21T04-40-55-914Z.log` | captured 2026-11-21 |
| Injected `<link rel="stylesheet" href="https://giscus.app/default.css">` in the DOM (`sheet` property present; frame computed 300px vs wrapper 955px) | live DOM observation | preview render eval, 2026-11-21 | executed live |
| Live mutation (`f.style.width="100%"` → 955px) | causal proof | preview render eval, 2026-11-21 | executed live |
| giscus assets reachability (`client.js`, `default.css` → HTTP 200) | network observation | curl from this host, 2026-11-21 | executed live |
| CherokeeLessons#59 record (localhost-serve diagnosis exclusion) | local issue store | `.issues/59/research/download-column.md:24` | read 2026-11-20 |
| CherokeeLessons#62 runbook (branch (d): CSP blank-frame differential; served-page verification pattern) | published wiki page | `giscus-not-installed-on-this-repository.md` (wiki HEAD fe0ac66) | read/published 2026-11-21 |
| AGENTS.md (repo) — served-page verification pattern; PR preview builds; merge-to-master auto-deploy | repo doc | `AGENTS.md` Build & verification; Deployment | read (session context) 2026-11-20 |

## Cost Frame

- SC-1/SC-2 (greps + diff inspection): seconds at the branch; skipping — an unverified CSP change reaches a site-wide deploy.
- SC-3 (preview render): one browser session, minutes; skipping — the criterion the change exists for (the rendering defect) goes unobserved, and a wrong token (or a refused one) ships.
- Build gate: one `./build.sh` run; skipping — a broken deploy reaches the merge candidate.

## Edge Cases

- The added origin must not loosen other directives: SC-2's diff-shape instrument pins single-directive change.
- CSP caching: preview deployments carry the updated `_headers`; if a stale render shows the refusal, hard-reload/redeploy before judging (the instrument re-runs on a fresh session).
- The loading-stall transient may mask a healthy widget state at render time: SC-3 hinges on computed geometry + absence of the REFUSAL line (both load-state-independent), not on the widget reaching its comments UI — a stuck-at-loading frame with full width and no refusal still passes this change's criteria (its cause is #59-adjacent, out of scope).
- Merge cadence: human merge only; after #64 merges, this PR rebases to a clean single-commit diff against master.
- Slug drift: not applicable (no repository-name input in this change).

## Completion gate

Every success criterion SC-1 through SC-3 passes before the change is complete; partial implementation is not permitted.

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)
