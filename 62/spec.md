# Spec — wiki runbook: giscus "not installed on this repository" error (diagnosis → remediation)

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/62 · filed 2026-11-20 under the developer's runbook-spec directive; implementation + repo fix (own spec/PR) directed 2026-11-20
<!-- artifacts: .issues/62/ -->

## Intent

**Problem Statement.** The CherokeeLessons comment widget (giscus, embedded via `templates/page.html`) fails with "An error occurred: giscus is not installed on this repository" — observed 2026-11-20/21 (console: `[giscus] An error occurred. Error message: "giscus is not installed on this repository"` from `https://giscus.app/client.js`; the heading renders inside the comment iframe on the content page). No runbook exists for this failure mode anywhere in the CherokeeLessons wiki, and the diagnosis is trap-prone twice over: the error text implicates the giscus App installation while the tracked embed in fact still carries the pre-rename pointer (`data-repo="CherokeeLanguage/CherokeeLessons.com"` at `templates/page.html:32`, after CherokeeLessons#27 renamed the repo on 2026-11-10; giscus's backend does not follow rename redirects — giscus#1238), and a naive repo-existence check falsely clears that cause (the GitHub REST API follows rename redirects — `gh repo view CherokeeLanguage/CherokeeLessons.com --json nameWithOwner` returned `CherokeeLanguage/CherokeeLessons` live 2026-11-20 — so "the repo exists" is not deciding evidence).

**Root Cause / Motivation.** Cause attribution across surfaces is record-conflicted, and the runbook must not paper over it. Certain: (i) the tracked embed's `data-repo` names a repo slug that ceased to exist at the 2026-11-10 rename, and the #27-deferred one-line fix has not landed (verified by read 2026-11-20); (ii) the error is observed firing on 127.0.0.1-served pages (CherokeeLessons#59's dated research record — the widget "does not fire this way on the deployed domain's build", predating #59's change); (iii) CherokeeLessons#27's mechanism-backed record predicts the deployed widget errors until the pointer fix lands (giscus#1238, verified live 2026-11-10). Records (ii) and (iii) disagree about the deployed domain's present state. What must be solved now: an operator facing this error has no documented procedure; the runbook captures the evidenced decision tree (serve context → repo-config checks → CSP differential) with every check executable as written, and labels each branch's evidence provenance instead of asserting an unresolved mechanism.

**Approach Chosen.** One runbook page in the CherokeeLessons wiki following the sre-runbook card's structure (symptom → diagnosis with exact commands and stated expected outputs → rollback → remediation → verification), each decision point carrying its deciding evidence, branches labeled by provenance. The runbook documents the site fix (updating `templates/page.html`'s `data-repo`) as the stale-pointer branch's remediation; it performs no tracked-content change.

**Alternatives Considered & Why Discarded.**
- Fixing the widget inside this change: excluded by the runbook's nature — tracked site content needs its own pipeline pass; the developer directed that fix separately (own spec, own PR) in the same cycle.
- A repo-tracked doc or issue comment instead of the wiki: the developer named the wiki as the deliverable surface; incident-response material belongs where the operator looks during an incident.
- One runbook per cause branch: one incident, one symptom, one decision tree — sre-runbook structures one runbook around the symptom; branches live inside it as decision points.
- A generic giscus-troubleshooting page covering every widget failure mode: scope is this error; the CSP blank-frame failure mode enters only as the differential decision point (the repo's closest confusable per the evergreen iframe-CSP gate).

**Key Design Decisions.**
- Trigger keyed to the observed full string "giscus is not installed on this repository" (widget heading prefixed "An error occurred:"); the developer's shorthand "giscus is not installed" maps to it.
- Diagnosis anchored on deciding evidence, not cause narratives: serve-context check first (CherokeeLessons#59's record), then the repo-name comparison with the redirect trap named (embedded value vs returned `nameWithOwner` mismatch — never mere existence), then App-install and repo-visibility branches, then the CSP differential (blank frame + CSP violation is a different failure mode).
- Branch provenance labels — `observed` (dated incident record on this site), `record-backed` (dated mechanism record), `upstream-documented` (giscus-documented cause, hypothesis for this site) — per the sre-runbook rule against presenting hypotheses as observed incidents; the #27/#59 disagreement about the deployed domain is carried explicitly as unresolved-by-these-records.
- The runbook is wiki-only, direct-publish; the deliverable performs no git-tracked change to the site repository.

**User Intent / Original Prompt.**
- Developer, 2026-11-20: "file a runbook spec to create a runbook in the wiki for: An error occurred: giscus is not installed"
- Developer, 2026-11-20: "is this a simple change in the giscus setup in the repo?" (answered yes — the recorded pending fix is a one-line `data-repo` update); then: "implement the spec to apply the repo fix and file a PR" — implementation authorization for this spec plus the repo fix as its own tracked change.
- Observed failures: console capture from the CherokeeLessons#59 verification session (2026-11-21T03:54Z playwright artifacts, since removed by session cleanup; verbatim contents recorded in the filing session's transcript): `[giscus] An error occurred. Error message: "giscus is not installed on this repository"`; widget heading "An error occurred: giscus is not installed on this repository" (snapshot heading, same session). CherokeeLessons#59 research (`.issues/59/research/download-column.md` line 24): giscus logs "not installed on this repository" when served from 127.0.0.1, "does not fire this way on the deployed domain's build", predating #59's change. CherokeeLessons#27 (2026-11-10): rename deferred the `templates/page.html` pointer fix; giscus#1238 mechanism verified live.

## Not Included

- Executing the `templates/page.html` `data-repo` update (the repo fix): directed separately by the developer; it files under its own spec and PR in this cycle.
- Runbooks for other embed failure modes (CSP blank-frame as its own incident, YouTube/itch embeds): the CSP contrast appears solely as a differential decision point.
- Wiki `_Sidebar`/`_Footer` navigation beyond what the plan names.
- giscus.app-side configuration research (Discussions setup, category regeneration) beyond what the decision points require.
- Resolving the #27/#59 deployed-domain record conflict by authoring fiat: the runbook carries both dated records plus the checks that decide the live state; post-fix observation contributes the deciding evidence (the fix's own verification observes it).

## Requirements

- **R-1** — A standalone runbook page for this error SHALL exist in the CherokeeLessons wiki (`github.com/CherokeeLanguage/CherokeeLessons.wiki`).
- **R-2** — The runbook SHALL follow the sre-runbook card's structure: symptom → diagnosis (real commands with real flags and stated expected output) → remediation → verification → rollback; prerequisites and blast radius SHALL be stated up front; rollback SHALL be stated before remediation; every step SHALL be executable as written — "adjust as appropriate" placeholders are prohibited.
- **R-3** — The symptom section SHALL quote the observed string "giscus is not installed on this repository" in both observed forms (widget heading "An error occurred: giscus is not installed on this repository"; console line prefixed `[giscus] An error occurred. Error message:`), SHALL name the affected surface (the giscus iframe on content pages rendered through `templates/page.html`), and SHALL state both recorded serve contexts (127.0.0.1 local serve — observed, CherokeeLessons#59; deployed domain — recorded prediction, CherokeeLessons#27).
- **R-4** — The diagnosis section SHALL present explicit condition→branch decision points, each branch naming the evidence that decides it:
  - (0) serve context — the observed page URL: an error on a 127.0.0.1/localhost serve is a recorded artifact of this site's local serve (CherokeeLessons#59, labeled `observed`); the branch directs confirming against the deployed domain or a deploy preview before any repo-config diagnosis.
  - (a) stale `data-repo` — compare the `data-repo` value at `templates/page.html:32` against `gh repo view <data-repo-value> --json nameWithOwner`; a mismatch between the embedded value and the returned `nameWithOwner` decides the branch. The runbook SHALL document the trap that the GitHub REST API follows rename redirects (an "exists" answer is not deciding evidence) while giscus's backend does not (giscus#1238).
  - (b) giscus App not installed on the resolved repository.
  - (c) the resolved repository is not public (giscus serves public repositories only — giscus discussions #1161/#1653).
  - (d) CSP differential — a blank widget frame with a CSP console violation indicates the iframe-CSP failure mode (`static/_headers` `frame-src` must carry `https://giscus.app`), not this error.
- **R-5** — The remediation section SHALL provide, per branch, exact steps with stated outcomes, executable as written, and SHALL NOT perform tracked-content changes itself: (a) for the stale pointer, update the `data-repo` value in `templates/page.html` to the resolved current name, stating the change ships as tracked site content through the site's pipeline (branch → PR → human merge; merge to master auto-deploys via Workers Builds); (b) for the App branch, install/configure the giscus GitHub App (`github.com/apps/giscus`) on the resolved repository; (c) for the visibility branch, make the repository public or retarget the embed to another giscus-configured public repository — each followed by executing the verification section's checks.
- **R-6** — The verification section SHALL instruct the rendered-page observation: browser-render the affected page **as served** (deploy preview or deployed domain — not a bare localhost serve, per the CherokeeLessons#59 record) and assert the widget shows no "giscus is not installed on this repository" heading and the browser console records no giscus error.
- **R-7** — Each diagnostic and remediation branch SHALL carry an evidence-provenance label chosen from `observed`, `record-backed`, `upstream-documented`; and the runbook SHALL state that records #27 and #59 disagree about the deployed domain's present state and that these two records do not resolve that disagreement.

## Success criteria

| ID | Criterion | Class | Instrument |
|---|---|---|---|
| SC-1 | A runbook page for this error exists as a standalone wiki page | structural | `git clone https://github.com/CherokeeLanguage/CherokeeLessons.wiki.git <tmpdir>`; the set `grep -ril "giscus is not installed" <tmpdir> --include='*.md'` contains ≥1 file that also matches `grep -qi remediat <f>` and `grep -qi rollback <f>` |
| SC-2 | The page carries the full sre-runbook flow, required section order, and up-front prerequisites/blast radius | structural | on the page: `grep -icE` ≥1 for each family `symptom`, `prerequisit\|blast`, `diagnos`, `rollback`, `remediat`, `verif`; and line(Symptom) < line(Prerequisites) < line(Diagnosis) < line(Rollback) < line(Remediation) < line(Verification) by heading line numbers |
| SC-3 | Symptom fidelity: both observed forms quoted verbatim; embed surface named | structural | on the page: `grep -c "giscus is not installed on this repository"` ≥1; `grep -c "An error occurred"` ≥1; `grep -c "templates/page.html"` ≥1 |
| SC-4 | Decision points present for branches 0–d with deciding evidence | structural | on the page: `grep -c "data-repo"` ≥1; `grep -c "nameWithOwner"` ≥1 (trap command); `grep -ciE "install"` ≥1 with `giscus.*app\|apps/giscus` ≥1; `grep -ci "public"` ≥1; `grep -ciE "frame-src\|CSP"` ≥1; `grep -cE "127\.0\.0\.1\|localhost"` ≥1 |
| SC-5 | The page's documented deciding checks for branch (a) reproduce their documented outputs against the repo's live state when executed at the project root | behavioral | execute at project root: `grep -n "data-repo=" templates/page.html` (2026-11-20 live: the `data-repo="CherokeeLanguage/CherokeeLessons.com"` script line) and `gh repo view CherokeeLanguage/CherokeeLessons.com --json nameWithOwner` (2026-11-20 live: `{"nameWithOwner":"CherokeeLanguage/CherokeeLessons"}` — redirect-resolved, ≠ embedded value); pass when the page's documented expected outputs match the executed outputs for the repo's state at verification time (state-relative) |
| SC-6 | The stale-pointer remediation names the tracked surface and the site's ship path without performing it | structural | on the page: `grep -c "templates/page.html"` ≥1; `grep -icE "pull request\|PR"` ≥1; `grep -ci "deploy"` ≥1 |
| SC-7 | The verification section instructs the served-page console check for the error's absence | structural | slice the verification section (from its heading to the next heading): `grep -ci "console"` ≥1; `grep -ci "giscus is not installed"` ≥1 (the string asserted absent); `grep -ciE "preview\|deployed"` ≥1 |
| SC-8 | Provenance labels present; the #27/#59 deployed-domain disagreement marked unresolved | structural | on the page: `grep -ci "observed"` ≥1; `grep -ci "record-backed"` ≥1; `grep -ciE "upstream"` ≥1; `grep -ciE "unresolved\|disagree"` ≥1 |

## Dependencies

| Reference | Relationship | Status |
|---|---|---|
| CherokeeLessons wiki (live: `has_wiki=true`; `.wiki.git` HEAD resolves to ebedeefa, 2026-11-20) | the deliverable's host surface, written through the wiki checkout | satisfied |
| sre-runbook card (`.opencode/skills/sre-runbook`) | defines the runbook form R-2 requires | satisfied (loaded) |
| wiki-operations skill (`.opencode/skills/wiki-operations`) | governs wiki provisioning/checkout/edit/verify/publish at implementation | pending (loads at plan/implement) |
| CherokeeLessons#27 record (`.issues/27/`) | rename history, deferred-pointer record, giscus#1238 verification | satisfied (read 2026-11-20) |
| CherokeeLessons#59 research (`.issues/59/research/download-column.md:24`) | dated local-serve observation | satisfied (read 2026-11-20) |
| Filing-session captures of the 2026-11-21T03:54Z console/heading artifacts | verbatim symptom strings (files removed by playwright-cli cleanup; contents in transcript) | satisfied (captured 2026-11-20) |
| The repo fix's own spec record (files in this cycle under the developer's directive) | the runbook's branch-(a) remediation tracks it as the site fix's change record | pending |

## Traceability

| Requirement | Success criteria |
|---|---|
| R-1 | SC-1 |
| R-2 | SC-2, SC-5 |
| R-3 | SC-3 |
| R-4 | SC-4, SC-5 |
| R-5 | SC-6 |
| R-6 | SC-7 |
| R-7 | SC-8 |

Every SC maps to ≥1 requirement (SC-1→R-1; SC-2→R-2; SC-3→R-3; SC-4→R-4; SC-5→R-2, R-4; SC-6→R-5; SC-7→R-6; SC-8→R-7); no orphans in either direction.

## Documentation Sources

| Source | Type | Location | Verification |
|---|---|---|---|
| `templates/page.html` giscus script block (lines 31–44; `data-repo` at line 32) | tracked repo file | `templates/page.html` | read 2026-11-20 |
| `static/_headers` CSP (`frame-src` carries `https://giscus.app`) | tracked repo file | `static/_headers:13` | read 2026-11-20 |
| CherokeeLessons#27 spec + audit | local issue store | `.issues/27/spec.md`, `.issues/27/audit/validation.yaml` | read 2026-11-20 (rename 2026-11-10; deferred pointer fix; giscus#1238 live-verified 2026-11-10) |
| CherokeeLessons#59 research note | local issue store | `.issues/59/research/download-column.md:24` | read 2026-11-20 |
| giscus#1238 — backend does not follow rename redirects | upstream issue | https://github.com/giscus/giscus/issues/1238 | live-verified 2026-11-10 (#27 audit); corroborated 2026-11-20 (the GitHub API resolves the redirect giscus does not) |
| GitHub REST rename-redirect behavior | live API observation | `gh repo view CherokeeLanguage/CherokeeLessons.com --json nameWithOwner` → `CherokeeLanguage/CherokeeLessons` | executed live 2026-11-20 |
| giscus discussions #1161, #1653 (App-installed-state and private-repo causes of this error string) | upstream discussions | https://github.com/orgs/giscus/discussions/1161, https://github.com/orgs/giscus/discussions/1653 | live web search 2026-11-20 |
| zer0-mistakes giscus-comments doc (error string ⇒ App missing) | external doc | https://zer0-mistakes.com/docs/features/giscus-comments/ | live web search 2026-11-20 |
| #59 verification session console/heading captures | observed-failure artifacts | `.playwright-cli/…` (removed by session cleanup; verbatim contents recorded in the 2026-11-20 filing session transcript) | captured 2026-11-21T03:54Z |
| Repo wiki state | live repo state | `gh api repos/CherokeeLanguage/CherokeeLessons` (`has_wiki:true`); `git ls-remote …/.wiki.git HEAD` → ebedeefa | executed live 2026-11-20 |
| AGENTS.md (repo) — served-page verification pattern, PR preview builds, merge-to-master auto-deploy | repo doc | `AGENTS.md` Build & verification; Deployment | read (session context) 2026-11-20 |

## Cost Frame

- SC-1/2/3/4/6/7/8 (wiki-content greps): acting — one wiki clone plus greps, minutes, at authoring/verification time. Skipping — an unverified runbook page whose wrong command surfaces mid-incident costs incident time and risks a misdirected remedy.
- SC-5 (executed checks): two live commands at the project root, seconds. Skipping — the redirect trap makes a wrong-expectation runbook actively harmful: an operator following it would falsely clear the stale-pointer branch and misdiagnose.
- Fresh-context validation dispatch (spec card): one reviewer pass before the approval gate. Skipping — structural defects (untraceable requirements, mushy instruments, invented criteria) reach the developer gate and then the wiki.

## Edge Cases

- Widget heading vs console wording: both carry the full string with different prefixes; the runbook quotes both (R-3, SC-3).
- Redirect-resolved false negative: an existence-style probe of the stale slug returns success and wrongly clears branch (a); R-4(a) mandates the returned-`nameWithOwner` comparison (SC-4 checks `nameWithOwner` presence).
- Local-serve confound: per #59's dated record a 127.0.0.1-served page shows the error independent of repo config; branch (0) forbids repo-config diagnosis off a localhost observation.
- `data-repo-id`/`data-category-id` after a rename: node IDs are expected to be rename-stable (expectation, not a verified claim — labeled as such in the runbook prose); the branch-(a) remediation treats `data-repo` as the primary fix and defers attribute re-derivation to the giscus.app generator when more than the name needs refreshing.
- Wiki page identity: no fixed filename/title is required by this spec — any wiki page satisfying SC-1…SC-8 qualifies; the plan fixes title/slug (SC-1's instrument is content-keyed, not name-keyed).
- Direct-publish surface: wiki edits publish on push with no PR gate; a later `approved for pr` utterance covering this wiki-only deliverable triggers the N/A-surface disposition (no repo-tree change ⇒ no PR), recorded at that boundary — the PR in this cycle belongs to the repo fix's separate spec.
- Incident remediated before the runbook ships: the repo fix may merge first; the runbook's branch-(a) examples then describe the pre-fix recorded state, which stays valid as documented history; SC-5 is state-relative.

## Completion gate

Every success criterion SC-1 through SC-8 passes before the deliverable is complete; partial implementation is not permitted.

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)
