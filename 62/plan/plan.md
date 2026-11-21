# Plan — CherokeeLessons#62: wiki runbook for the giscus "not installed on this repository" error

## Goal

Implement the CherokeeLessons#62 spec: author, in the CherokeeLessons wiki, one runbook page for the giscus "not installed on this repository" error following the sre-runbook structure, with exact commands, stated expected outputs, five decision branches, provenance labels, and the unresolved-record note. The page is identified as **`giscus-not-installed-on-this-repository`** — wiki filename (and URL slug) `giscus-not-installed-on-this-repository.md`, page H1/title `Runbook: giscus "is not installed on this repository" error`. Five items: page skeleton with symptom fidelity, the diagnosis decision tree, command-output fidelity against executed checks, the remediation/verification/provenance sections, and publish with the full SC gate.

## Execution surface

Two git surfaces. (1) The CherokeeLessons wiki, provisioned per the wiki-operations card (`https://github.com/CherokeeLanguage/CherokeeLessons.wiki.git` — direct-publish; verify before push, no PR gate); the runbook page is authored in the wiki checkout. (2) The site repository (this project root, `master`, read-only) where the runbook's commands execute for fidelity checks. Evidence surfaces: `.issues/62/spec.md` (the SC set), `.issues/27/`, `.issues/59/research/download-column.md`, `templates/page.html`, `static/_headers`, and the live `gh` checks.

## Items

- **Item 1 — page skeleton: symptom + flow order.**
  - **Deliverable:** the new wiki file `giscus-not-installed-on-this-repository.md` in the wiki checkout (filename above is the page's permanent identity), carrying the full sre-runbook section flow in order (symptom → prerequisites/blast radius → diagnosis → rollback → remediation → verification) with the symptom section quoting both observed forms verbatim and naming `templates/page.html` as the embed surface and both recorded serve contexts.
  - **RED:** a fresh wiki clone contains no `.md` page matching "giscus is not installed" (`grep -ril` → empty) — the page does not exist.
  - **GREEN:** author the skeleton per spec R-1/R-2/R-3 in the wiki checkout.
  - **Tier marks:** no tests authored; instrument class: fact-check greps in the wiki checkout.
  - **Instrument:** spec SC-1, SC-2, SC-3 greps in the checkout (SC-1's final wiki-resident form re-verifies at Item 5).

- **Item 2 — diagnosis decision tree (branches 0–d).**
  - **Deliverable:** the diagnosis section with five explicit condition→branch decision points, each naming its deciding evidence and exact command: serve-context (127.0.0.1/deployed), stale `data-repo` vs `nameWithOwner` (with the rename-redirect trap named, giscus#1238), giscus App install, repository visibility, CSP differential (`static/_headers` `frame-src`).
  - **RED:** the checkout's page lacks the deciding-evidence markers (`grep -c "data-repo"` → 0; `grep -c "nameWithOwner"` → 0).
  - **GREEN:** author the five branches per spec R-4 with exact commands and stated expected outputs.
  - **Tier marks:** no tests authored; instrument class: fact-check greps.
  - **Instrument:** spec SC-4 grep set.

- **Item 3 — command-output fidelity (executed checks).**
  - **Deliverable:** the page's documented expected outputs for branch (a)'s two deciding commands aligned with the outputs they actually produce in the repo's live state.
  - **RED:** the page is written before this alignment — its branch-(a) expected-output statements are unverified against the live repo (`grep -n "data-repo=" templates/page.html` and `gh repo view CherokeeLanguage/CherokeeLessons.com --json nameWithOwner` executed at the project root; the unaligned page's statements are not yet compared).
  - **GREEN:** execute both commands at the project root; align the page's documented outputs with the executed outputs (2026-11-20 live state documented in the spec's SC-5 evidence; state-relative per the spec).
  - **Tier marks:** two live command executions; ephemeral observation; no test files.
  - **Instrument:** spec SC-5 — executed outputs match the page's documented expectations for the repo's state at verification time.

- **Item 4 — remediation, verification, provenance.**
  - **Deliverable:** the remediation section (per-branch exact steps; the stale-pointer branch names `templates/page.html`, the PR/merge ship path, auto-deploy on master, and CherokeeLessons#63 as the fix's change record; no automatic-fix promise), the verification section (served-page console check — deploy preview/deployed domain, not bare localhost; error-string absence assertion), and provenance labels (`observed`, `record-backed`, `upstream-documented`) on the branches plus the explicit #27/#59 unresolved-disagreement note.
  - **RED:** the checkout's page lacks the surface/ship-path markers (`grep -cE "pull request|PR"` → 0 on the pre-edit page) and the provenance markers (`grep -ci "observed"` → 0).
  - **GREEN:** author the sections per spec R-5/R-6/R-7.
  - **Tier marks:** no tests authored; instrument class: fact-check greps.
  - **Instrument:** spec SC-6, SC-7, SC-8 grep sets.

- **Item 5 — publish + full SC gate.**
  - **Deliverable:** the wiki page pushed (direct-publish per the wiki-operations card; verify-before-push) and every SC instrument re-run against a fresh clone of the pushed wiki.
  - **RED:** the pushed wiki still lacks the page (pre-publish state: the checkout holds unpublished edits; `git ls-remote` … the page's commit absent from the wiki remote).
  - **GREEN:** push the wiki; clone fresh; run the complete instrument set SC-1…SC-8.
  - **Tier marks:** no tests authored; instrument class: fact-check greps on a fresh clone.
  - **Instrument:** the spec's completion gate — all of SC-1…SC-8 passing on the pushed wiki.

## Dependency order

Item 1 precedes 2–4 (sections author onto the existing skeleton); Item 3 runs after Item 2 (it aligns branch (a)'s documented outputs, which Item 2 authored); Item 4 completes the content; Item 5 last (push publishes everything the checkout holds; the gate verifies the final artifact). No cycles: every item's RED describes the checkout state before its own GREEN.

## Traceability

| Item | Success criteria |
|---|---|
| Item 1 | SC-1, SC-2, SC-3 |
| Item 2 | SC-4 |
| Item 3 | SC-5 |
| Item 4 | SC-6, SC-7, SC-8 |
| Item 5 | SC-1, SC-2, SC-3, SC-4, SC-5, SC-6, SC-7, SC-8 |

Every SC covered; no orphan items.

## Verification summary

All instruments are fact-check greps run in the wiki checkout at cycle time, two live command executions at the site-repo root (Item 3), and one fresh-clone gate after publish (Item 5); no authored test files exist anywhere in this plan; no recorded browser dispatch is required (the runbook's own verification instructions are page content, not executed here — the page's post-fix served observation belongs to CherokeeLessons#63's Item 2).

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)
