# Plan — CherokeeLessons#65: giscus-app style-src origin

## Goal

Implement the CherokeeLessons#65 spec: add `https://giscus.app` to `static/_headers`'s `style-src` so the giscus client's injected wrapper stylesheet applies and the comment frame renders at its wrapper column's width. Two items: the single-token edit, the PR with the executed preview-render observation. Authorized through the PR boundary ("approved for stacked PR"); the PR stacks on `feature/63-giscus-data-repo-pointer`.

## Execution surface

Repository: CherokeeLessons (this project root). Branch: `feature/65-giscus-style-src`, created off the stacked base tip `2d0cad6` (the #63 branch's tip; trunk `master` verified fresh at `129a1916` both in origin and local). Build gate: `./build.sh`. Remote PR operations: `gh`. The preview deployment arrives with the pushed PR branch (Workers preview builds).

## Items

- **Item 1 — the single-token edit.**
  - **Deliverable:** `static/_headers`'s `style-src` gains `https://giscus.app`; every other directive byte-identical.
  - **RED:** on the base tip, `grep -cE 'style-src[^;]*giscus\.app' static/_headers` returns 0 — the style-src lacks the origin the giscus client injects stylesheets from (the failing state the developer observed). (Directive-bounded: unanchored `.*` false-positives on script-src's giscus.app token earlier on the same CSP line — caught at RED confirmation 2026-11-21.)
  - **GREEN:** append ` https://giscus.app` inside the `style-src` directive's token list; no other directive touched.
  - **Tier marks:** no tests authored; instrument class: fact checks (directive greps + `git diff` inspection) plus the repo's build gate at cycle time.
  - **Instrument:** spec SC-1 (grep set) and SC-2 (`git diff 2d0cad6 -- static/_headers`: exactly one removed/added pair, all other directives identical), plus a clean `./build.sh` before push.

- **Item 2 — PR with executed preview-render observation.**
  - **Deliverable:** pushed branch and filed PR (draft for preview, ready on verify PASS); PR body carries summary, provenance, SC-1/SC-2 instrument outputs, build-gate result, and the dated preview observation: computed giscus frame width ≈ `.giscus` wrapper width AND no `style-src` refusal for `giscus.app/default.css` in the console.
  - **RED:** `gh pr list --head feature/65-giscus-style-src --state all` returns empty — no PR exists.
  - **GREEN:** push the branch, open the PR, wait for the Workers preview, execute the playwright render (headless msedge, scrolled to the widget), record its outputs verbatim in the PR body.
  - **Tier marks:** one recorded browser dispatch (playwright-cli session; ephemeral observation — recorded in the PR body, no test files).
  - **Instrument:** spec SC-3 — the render's measured geometry + console scan, recorded verbatim and dated.

## Dependency order

Item 1 precedes Item 2: the PR carries Item 1's diff, and the preview renders the CSP only once the branch pushes. No cycles: Item 1's RED runs on the untouched stacked base; Item 2's RED runs after Item 1's commit and push.

## Traceability

| Item | Success criteria |
|---|---|
| Item 1 | SC-1, SC-2 |
| Item 2 | SC-3 |

Every SC covered (SC-1→1, SC-2→1, SC-3→2); no orphan items.

## Verification summary

Fact checks (greps, diff inspection) at cycle time on the branch; `./build.sh` clean-build at cycle time before push; the preview render is one recorded browser session whose outputs are quoted into the PR body (ephemeral observation, PR-recorded); no authored test files exist anywhere in this plan.

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)
