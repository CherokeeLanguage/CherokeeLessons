# Spec — repo fix: giscus embed data-repo pointer (templates/page.html) — post-rename slug

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/63 · filed 2026-11-20 under the developer's directive "implement the spec to apply the repo fix and file a PR"; sibling record: the wiki runbook spec CherokeeLessons#62 (which documents this fix as its remediation and excludes performing it)
<!-- artifacts: .issues/63/ -->

## Intent

**Problem Statement.** `templates/page.html:32` embeds the site's giscus comment widget with `data-repo="CherokeeLanguage/CherokeeLessons.com"` — a slug that ceased to exist when CherokeeLessons#27 renamed the repository to `CherokeeLanguage/CherokeeLessons` on 2026-11-10. The rename shipped without the pointer update (#27's audit explicitly named updating it a repo-content change and deferred it), and nothing has landed since (verified by read 2026-11-20). Giscus's backend does not follow rename redirects (giscus#1238, live-verified 2026-11-10 in #27's audit), so the widget queries a repository name that no longer exists. The comments widget consequently addresses a stale target on every content page rendered through the site's content template.

**Root Cause / Motivation.** The 2026-11-10 rename propagated identically to all repo-identity surfaces (#27) but deliberately excluded the tracked template pointer, which the #27 audit flagged as an urgent follow-up. Records conflict about the deployed domain's present widget state: #27 (mechanism-backed, giscus#1238) predicts the deployed widget errors until this fix lands; CherokeeLessons#59 (2026-11-21 research record) claims the error "does not fire this way on the deployed domain's build". The embedded stale slug is wrong under both records — it names a nonexistent repository — and the deciding post-fix observation belongs to this change's PR preview, executed and recorded rather than assumed. It must be solved now because the developer directed the fix in this cycle ("apply the repo fix and file a PR") and the fix is the recorded pending follow-up of #27.

**Approach Chosen.** A one-attribute change to `templates/page.html` — `data-repo="CherokeeLanguage/CherokeeLessons.com"` → `data-repo="CherokeeLanguage/CherokeeLessons"` (the repo's current `nameWithOwner`, live-verified 2026-11-20 via `gh repo view`) — shipped as tracked site content through the repo's pipeline: feature branch → PR with an executed PR-preview render observation recorded in the body → human merge → Workers Builds auto-deploy on merge to master.

**Alternatives Considered & Why Discarded.**
- Regenerating the entire giscus script block via the giscus.app generator (fresh `data-repo-id`/`data-category-id`): rejected — the node-ID attributes are rename-stable (giscus node IDs do not change on rename; labeled an expectation in CherokeeLessons#62's spec), a wider diff adds churn with no evidence of need, and a post-fix failing observation routes to that re-derivation as the named follow-up, not to this change.
- Bundling with the wiki runbook work (CherokeeLessons#62): rejected — different deliverable class (repo-tree change with a PR boundary vs wiki direct-publish) and #62's spec excludes performing this fix explicitly; separate specs under the split discipline.
- Retargeting `data-repo` to a different repository for comments: rejected — the developer's disposition is that this site's comments live on this repository's Discussions; #62's runbook documents retargeting as remediation branch (c) for other cases only.
- Reopening #27 instead of a new issue: rejected — #27 is closed and verified-delivered; this fix is its named follow-up requiring its own pipeline pass and change record.

**Key Design Decisions.**
- Minimal diff: exactly one attribute changes; the script block's other eight attributes stay byte-identical so the change's blast radius is inspectable at a glance (SC-2).
- The deciding ambiguity-resolution surface is the PR preview render (served, not localhost — per the #59 local-serve record): its console/widget state is recorded verbatim and dated in the PR body, whatever it shows, because the two dated records disagree pre-fix (R-3, SC-3).
- Human-only merge; the agent files the PR and halts at that boundary.
- `./build.sh` clean-build gate before push — the repo's verification gate for content changes (AGENTS.md), a process gate rather than a criterion subject.

**User Intent / Original Prompt.**

- Developer, 2026-11-20: "is this a simple change in the giscus setup in the repo?" (answered in-session: yes — the recorded pending fix is a one-line `data-repo` update)
- Developer, 2026-11-20: "implement the spec to apply the repo fix and file a PR" — implementation authorization for this fix and the PR boundary.
- CherokeeLessons#27 audit (2026-11-10), cited: "giscus resolves the repo via its backend through the GitHub API, which does not follow rename redirects (giscus#1238, verified live 2026-11-10) — the widget is expected to error until the deferred content fix lands and the site redeploys."
- CherokeeLessons#59 research (2026-11-21), cited: giscus logs "not installed on this repository" on 127.0.0.1 serve; "does not fire this way on the deployed domain's build".

## Not Included

- Regenerating `data-repo-id`/`data-category-id` via the giscus.app generator — expected rename-stable; named as the follow-up if the post-fix observation demands it.
- The CherokeeLessons#62 wiki runbook (its own deliverable, own record) and any wiki navigation changes.
- Investigating the localhost-serve widget behavior (#59's record) — #62's runbook serve-context branch carries that.
- CSP changes — `static/_headers` already carries `https://giscus.app` in `frame-src`; no new origin arises.

## Requirements

- **R-1** — The giscus script block's `data-repo` attribute in `templates/page.html` SHALL read `CherokeeLanguage/CherokeeLessons` (the repository's current `nameWithOwner`).
- **R-2** — No other attribute of that script block SHALL change; the working-tree diff against trunk for `templates/page.html` SHALL be exactly that one attribute line.
- **R-3** — The change SHALL ship as tracked site content: feature branch → PR; the PR body SHALL record the executed PR-preview render observation — the giscus widget's visible state and the browser console's giscus lines, verbatim and dated, from the preview deployment (a served surface, not a localhost serve).

## Success criteria

| ID | Criterion | Class | Instrument |
|---|---|---|---|
| SC-1 | The pointer reads the post-rename slug and no longer carries the pre-rename value | structural | at the branch: `grep -cE 'data-repo="CherokeeLanguage/CherokeeLessons"' templates/page.html` ≥ 1 AND `grep -cE 'data-repo="CherokeeLanguage/CherokeeLessons\.com"' templates/page.html` = 0 |
| SC-2 | The diff against trunk touches exactly that one attribute | structural | `git diff <base-tip> -- templates/page.html` contains exactly one removed/added line pair; the removed line contains `data-repo="CherokeeLanguage/CherokeeLessons.com"`; the added line contains `data-repo="CherokeeLanguage/CherokeeLessons"` with every other script-block attribute identical between the two lines |
| SC-3 | The filed PR records the executed preview observation | structural | `gh pr view <PR> --json body` contains a dated line recording the giscus widget's observed visible state and the console giscus output from the preview render |

## Dependencies

| Reference | Relationship | Status |
|---|---|---|
| CherokeeLessons#27 record (`.issues/27/`) | the rename and this fix's provenance as its deferred follow-up | satisfied (read 2026-11-20) |
| CherokeeLessons#62 spec (`.issues/62/spec.md`) | sibling spec: the runbook documents this fix as remediation branch (a); this change is that fix's record | satisfied (this cycle) |
| `gh` CLI authentication | PR creation and issue operations | satisfied (session-init: logged in) |
| `./build.sh` | the repo's content-change verification gate before push | satisfied (documented, AGENTS.md) |
| The 2026-11-10 rename fact + current slug | grounds R-1's target value | satisfied (live `gh repo view` 2026-11-20: `CherokeeLanguage/CherokeeLessons`) |

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
| `templates/page.html` giscus script block (lines 31–44; stale value at line 32) | tracked repo file | `templates/page.html` | read 2026-11-20 |
| CherokeeLessons#27 spec + audit | local issue store | `.issues/27/spec.md`, `.issues/27/audit/validation.yaml` | read 2026-11-20 (rename 2026-11-10; deferred pointer fix; giscus#1238 live-verified) |
| giscus#1238 — giscus backend does not follow rename redirects | upstream issue | https://github.com/giscus/giscus/issues/1238 | live-verified 2026-11-10 (#27 audit); corroborated 2026-11-20 (GitHub API resolves the redirect giscus does not) |
| Current repository slug (`nameWithOwner`) | live API observation | `gh repo view CherokeeLanguage/CherokeeLessons --json nameWithOwner` → `CherokeeLanguage/CherokeeLessons` | executed live 2026-11-20 |
| CherokeeLessons#59 research note (local-serve record; deployed-domain claim) | local issue store | `.issues/59/research/download-column.md:24` | read 2026-11-20 |
| AGENTS.md (repo) — PR preview builds; merge-to-master auto-deploy; `./build.sh` gate | repo doc | `AGENTS.md` Build & verification; Deployment | read (session context) 2026-11-20 |
| `static/_headers` CSP (no change needed; `https://giscus.app` already in `frame-src`) | tracked repo file | `static/_headers:13` | read 2026-11-20 |

## Cost Frame

- SC-1/SC-2 (greps + diff inspection): acting — seconds at the branch; skipping — the change's blast radius and correctness go unverified before a site-wide deploy.
- SC-3 (preview render): one browser session against the PR preview, minutes; skipping — the deciding post-fix observation the two conflicted records could not resolve pre-fix is lost, and the merge lands with an unobserved comments surface.
- Clean-build gate: one `./build.sh` run; skipping — a broken build reaches the PR and the merge candidate.

## Edge Cases

- Preview observation shows the error still: the record is carried verbatim (the SC requires recording, not a passing outcome); the named follow-up is attribute re-derivation via the giscus.app generator — triaged after the observation, not fixed silently in this change.
- Preview observation shows a blank frame with a CSP violation: that is the differential failure mode #62's runbook branch (d) names; recorded verbatim; no CSP change is in this change's scope.
- Pre-fix deployed state already error-free (#59's side of the conflict): the fix remains correct (the stale slug is wrong under both records) — correctness does not require a reproducing symptom; the recorded observation documents the state change or its absence either way.
- Merge cadence: human merge only; the agent's work ends at the filed, reviewed PR.
- Slug drift: if the repository is renamed again before merge, the branch's `data-repo` value is re-ground against a fresh `gh repo view` observation before push.

## Completion gate

Every success criterion SC-1 through SC-3 passes before the change is PR-complete; partial implementation is not permitted.

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)
