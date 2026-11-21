# Plan — CherokeeLessons#63: giscus embed data-repo pointer fix

## Goal

Implement the CherokeeLessons#63 spec: change the giscus script block's `data-repo` attribute in `templates/page.html` from the pre-rename slug to `CherokeeLanguage/CherokeeLessons`, ship it as tracked site content on a feature branch, and file the PR whose body records the executed PR-preview render observation. Two items: the minimal edit, the PR filing with the recorded preview observation.

## Execution surface

Repository: CherokeeLessons (this project root). Branch: `fix/giscus-data-repo-pointer`, created off the verified fresh trunk tip (`master`) per the git-workflow-branch card — the branch exists before the first file modification. Build gate: `./build.sh` (the repo's only sanctioned build path; vendored pinned zola fetched on first use — needs network once). Remote PR operations: `gh` (authenticated). The preview deployment for the observation arrives with the pushed PR branch (Workers preview builds).

## Items

- **Item 1 — the one-attribute edit.**
  - **Deliverable:** `templates/page.html`'s giscus script block: `data-repo="CherokeeLanguage/CherokeeLessons.com"` → `data-repo="CherokeeLanguage/CherokeeLessons"`; every other byte of the block identical.
  - **RED:** on the base tip, `grep -cE 'data-repo="CherokeeLanguage/CherokeeLessons"' templates/page.html` returns 0 and `grep -cE 'data-repo="CherokeeLanguage/CherokeeLessons\.com"' templates/page.html` returns 1 — the branch-0 assertion fails against current behavior.
  - **GREEN:** the single-attribute change named above; no speculative edits elsewhere in the file.
  - **Tier marks:** no tests authored; instrument class: fact checks (attribute greps + `git diff` inspection) plus the repo's build gate at cycle time.
  - **Instrument:** spec SC-1 (both greps) and spec SC-2 (`git diff <base-tip> -- templates/page.html` shows exactly one removed/added line pair with all other block attributes identical), plus a clean `./build.sh` (exit 0) as the process gate before push.

- **Item 2 — PR with recorded preview observation.**
  - **Deliverable:** pushed branch and filed PR (human merge gate); PR body carries summary, provenance, executed-test evidence (build gate output, SC-1/SC-2 instrument outputs), and the dated preview observation: giscus widget visible state + console giscus lines from a content page rendered on the preview deployment.
  - **RED:** `gh pr list --head fix/giscus-data-repo-pointer` returns empty — no PR exists for the branch.
  - **GREEN:** execute the preview render (playwright-cli card path: real browser, console capture), record its output verbatim in the PR body, file the PR.
  - **Tier marks:** one recorded browser dispatch (playwright-cli session; ephemeral observation — recorded in the PR body, no test files).
  - **Instrument:** spec SC-3 — `gh pr view <N> --json body` contains the dated observation line.

## Dependency order

Item 1 precedes Item 2: the PR carries item 1's diff, and the preview build only exists once the branch with the edit pushes. No cycles: item 1's RED runs on the untouched base tip; item 2's RED runs after item 1's commit and push.

## Traceability

| Item | Success criteria |
|---|---|
| Item 1 | SC-1, SC-2 |
| Item 2 | SC-3 |

Every SC covered (SC-1→1, SC-2→1, SC-3→2); no orphan items.

## Verification summary

Fact checks (greps, diff inspection) run at cycle time on the branch; `./build.sh` clean-build runs at cycle time before push; the preview render is one recorded browser session whose output is quoted into the PR body (ephemeral observation, PR-recorded); no authored test files exist anywhere in this plan.

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)
