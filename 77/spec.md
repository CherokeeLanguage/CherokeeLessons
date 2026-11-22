# Spec — blog-posting workflow: Mastodon toots + modern inline embed back-fill for new blog entries

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/77

## Intent and executive summary

**Problem Statement** — CherokeeLessons has no standing procedure connecting its blog entries to Mastodon. New entries publish with no toot, and no mechanism anchors the resulting discussion thread back into the article. Both sibling sites in this workspace carry a workflow of record (brothertown-language-com: SOP rule 5 — placeholder, hand-post, signed-out probe, follow-up PR; michael-conrad.com: hand-edited ledger + OEmbed wrapper script), and their forms diverge; neither is adapted here.

**Root Cause / Motivation** — The sibling workflows postdate this repo's content era: the archive (2020–2023 entries under `content/content/`) predates the practice and carries no matching toots, so nothing brought the workflow over. The developer has directed the adaptation now, before the next blog-posting cycle, choosing the brothertown workflow as the source and the michael-conrad.com form as the requested comparison.

**Approach Chosen** — A standing repo doc, `docs/blog-posting-workflow.md` (creating `docs/`), defining the adapted workflow of record: agent-assisted and credentialless; paste-ready toot text prepared by the agent, posted by hand from `@michaelconrad@mastodon.social`; signed-out API discipline for tag verification, URL discovery, and dual-verification; a body-authored `### Comment on Mastodon` section carrying the **modern Mastodon inline embed generated fresh from the OEmbed endpoint**; giscus remaining the site's discussions system; the GAP mechanism for threadless flag-release; the CSP origins step riding the first embed-bearing change; and an `AGENTS.md` pointer fact.

**Alternatives Considered & Why Discarded** —
- *michael-conrad.com ledger + wrapper-script back-fill* — rejected: the developer's standing ruling is "no cargo culting old stuff" (2026-11-21/22); the agent-editor mechanics of record need no repo machinery, and brothertown's own standards-adoption record scoped this machinery out.
- *brothertown plain-anchor section (`[View on Mastodon](<thread-url>)`, embeds rejected there)* — rejected: the developer explicitly allowed embeds and directed the modern inline form.
- *Frontmatter `[extra]` field + template-rendered slot* — rejected: the developer chose the body-authored block (2026-11-21).
- *Full ending-block import (sign-off, hot-linked in-body hashtag line, AI byline), section placed dead last within it* — rejected: "no cargo culting" (2026-11-22); new entries keep the site's current article shape, toot hashtags derive from frontmatter tags at toot time.
- *Automated posting via Mastodon write API* — rejected: no credential surface exists or is wanted; both source workflows of record are hand-post, and the adapted workflow binds that mechanics.
- *Landing the `_headers` CSP edit in this deliverable* — rejected: the design of record (2026-11-21, presented and confirmed by proceeding to spec) defers the origins edit to the first embed-bearing change per the repo's evergreen iframe-embed gate; this deliverable stays documentation-scoped.

**Key Design Decisions** —
1. *Account of record carried unchanged:* toots posted by the developer from `@michaelconrad@mastodon.social`; its profile URL is the threadless stand-in. Tradeoff: the blog channel binds to the developer's personal handle — the only in-repo Mastodon decision of record (CherokeeLessons#26, 2026-11-10, "not applicable right now") deferred the account for app *announcements*, a distinct surface; the carried reading went uncorrected through the discussion.
2. *The of-record back-fill artifact is a method, not a snapshot:* fresh signed-out OEmbed GET output, verbatim, per entry. Live generator evidence (2026-11-21, mastodon.social v5.0.0-nightly.2026-11-20) is the `blockquote.mastodon-embed` + `data-embed-url` + `data-allowed-prefixes` script family; the docs.joinmastodon.org example (updated 2026-05-01) shows the earlier `iframe.mastodon-embed` family — both hydrated by the same `embed.js`, same CSP origins. Generator evolution between entries is not a defect; hand-rolled and legacy-authored markup are never the form of record. Tradeoff: the fallback styling is Mastodon's fixed light theme, accepted as the generator's own surface.
3. *Body-authored block:* the section ships inside the entry's markdown body, not frontmatter/template. Tradeoff: it appears in feeds and no-JS contexts (the generator's fallback anchor serves those) at the cost of per-post edits instead of uniform template rendering.
4. *Giscus coexistence by one word:* the intro line adapts the sibling-shared wording to "You can **also** comment on this post via Mastodon:" — giscus stays the discussions system of record; the block renders above the giscus footer widget (body content vs page-footer template fact).
5. *Credentialless agent-editor mechanics:* every API touch is a signed-out public read; no scripts, ledger, hooks, or CI encode the workflow. Tradeoff: each back-fill is manual agent work executed inside the entry's follow-up PR cycle.

**User Intent / Original Prompt** — (all 2026-11-21/22, chat)
- "create a spec to adapt the workflow from brothertown-language for posting new blog entries with the mastodon toots and the back fill into the blog article for the link."
- "also compare with ../michael-conrad.com"
- "let's adapt the brothertown workflow for this repo, but with embeds allowed. discuss."
- "do not cargo cult what is currently in use. re-evaluate and discuss."
- "think 'modern' replacement for 'legacy' approach"
- "let's use body-authored block"
- "we need to use modern Mastodon inline for this, no cargo culting old stuff. discuss."
- "widget is ok, just keep in mind that giscus is the discussions system currently in use."
- "existing blog entries do not have matching mastodon entries"
- "no cargo culting"
- "proceed to spec"

## Not Included

- Existing entries and any retroactive toot or back-fill work — ground: developer statement 2026-11-21 ("existing blog entries do not have matching mastodon entries"); the archive has no Mastodon surface to attach.
- Automated posting, storing, or any Mastodon credential surface; any wrapper script or pairing ledger (michael-conrad.com machinery explicitly not carried).
- Ending-block conventions (sign-off, in-body hashtag line, AI byline) — not imported into new entries' article shape.
- The CherokeeLessons#26 app-announcement workflow — a separate standing surface, filed independently.
- Theme/template changes and any `content/` file change in this deliverable.
- The `static/_headers` CSP edit itself — deferred to the first embed-bearing change (the doc binds the step).
- The Zola scaffold (`content/new_page.py`) — reused as-is.

## Requirements

Notation: "the doc" is `docs/blog-posting-workflow.md`, the deliverable of record.

- **R-1 — Standing workflow doc.** The repo gains `docs/blog-posting-workflow.md`, defining the standing workflow for publishing new blog entries (under `content/content/`) with Mastodon toots and back-filling the toot link into the article. The doc states its applicability to new entries going forward and records that existing entries carry no matching toots and are not retrofitted.
- **R-2 — Account of record.** The doc binds the Mastodon account and instance of record: toots are posted by the developer from `@michaelconrad@mastodon.social`; that account's profile URL (`https://mastodon.social/@michaelconrad`) is the threadless stand-in target.
- **R-3 — Toot-text preparation.** The doc binds the preparation step: the agent supplies paste-ready toot text carrying the article URL and a short lede, with hashtags derived from the entry's frontmatter tags in Mastodon concatenated form (`#TagText`), each tag verified via signed-out public read (`GET https://<instance>/api/v1/tags/<tag>`) before entering the toot; the paste-ready info also carries the posting visibility, public of record. The developer posts the toot by hand — no script, no API write anywhere in this step.
- **R-4 — Probe discipline.** The doc binds the URL discovery and verification step: after posting, the agent finds the toot via signed-out public reads (account statuses timeline and/or the account RSS feed and/or single-status reads) and confirms **both** the toot's own canonical `url` **and** its link back to the article (status body link or preview-card `card.url`) before any repo change lands; the developer never supplies the thread URL.
- **R-5 — Back-fill block form.** The doc binds the body-authored back-fill block, appended at the end of the entry's `index.md`:
  1. heading `### Comment on Mastodon`, intro line `You can also comment on this post via Mastodon:` (the "also" adaptation carries the giscus coexistence);
  2. `<!-- mastodon-embed-start -->` … `<!-- mastodon-embed-end -->` sentinels wrapping the embed, enabling idempotent completion/replacement;
  3. inside the sentinels: output of the OEmbed endpoint — `GET https://<instance>/api/oembed?url=<verified toot url>` — inserted verbatim, generated fresh per entry; the generator family of record (verified live 2026-11-21) is `blockquote.mastodon-embed` carrying `data-embed-url`, with fallback anchor to the verified toot URL, plus `<script data-allowed-prefixes="https://mastodon.social/" async src="https://mastodon.social/embed.js"></script>`;
  4. the doc records that generator evolution is not a defect (the same `embed.js` hydrates both the of-record family and the older `iframe.mastodon-embed` family shown in the joinmastodon docs example), that embed origins stay mastodon.social either way, and that legacy `iframe.mastodon-embed` markup or any hand-rolled substitute is never authored as the form of record.
- **R-6 — Giscus coexistence.** The doc binds that giscus is the site's discussions system in use (template fact: `templates/page.html` loads it in the page footer); the Mastodon block is an additional venue pointer rendered above the giscus widget; nothing in the workflow repositions, replaces, or out-frames giscus.
- **R-7 — Threadless publication (GAP mechanism).** The doc binds, adapted from the brothertown SOP rule 5 text of record (2026-11-15): an entry may publish threadless only on the developer's explicit flag-release instruction. Its threadless state of record is the section shell plus, inside the sentinels, a plain placeholder anchor `[View on Mastodon](https://mastodon.social/@michaelconrad)` — never presented as a generated embed, which a profile URL cannot produce — and an HTML `<!-- GAP: … -->` comment above the section naming the blocker; no silent placeholder. The completion sequence of record: toot posted → probe (R-4) → the placeholder replaced by fresh verified OEmbed output per R-5 → GAP comment stripped → `./build.sh` → follow-up PR → human merge.
- **R-8 — Credentialless, no automation.** The doc binds that no Mastodon credentials, tokens, or write-API calls exist anywhere in the workflow; all Mastodon API contact is signed-out public reads; posting is the developer's manual act; no wrapper scripts, pairing ledger, CI step, or hook encodes the workflow.
- **R-9 — CSP step of record.** The doc binds the CSP/iframe step: `https://mastodon.social` enters `static/_headers`'s `script-src` and `frame-src` in the same change as the first embed-bearing back-fill, and the served page is verified by browser render with a console check per the repo's evergreen iframe-embed gate (`./build.sh` proves nothing about the frame).
- **R-10 — AGENTS.md pointer.** The repo's `AGENTS.md` gains a pointer fact naming `docs/blog-posting-workflow.md` as the blog-posting + back-fill workflow of record for new entries.
- **R-11 — No-invention boundary.** The doc carries no factual claim beyond this spec's design record and the documented sources: no boost or reach claims, no invented repair obligations, no account identities beyond the bound handle.

## Success criteria

| ID | Criterion | Class | Instrument |
|---|---|---|---|
| SC-1 | The doc exists, states applicability to new `content/content/` entries going forward, and records the existing-entries exclusion | structural | read the doc; check the applicability and existing-entries statements |
| SC-2 | The doc binds the account values exactly: handle, instance/home base, profile-URL placeholder | structural | read the doc against R-2's bound values |
| SC-3 | The doc binds toot-text preparation: frontmatter-derived hashtags, signed-out `api/v1/tags` verification, developer hand-post | structural | read the doc against R-3; execute the documented tag GET form once, signed out |
| SC-4 | The doc binds the probe discipline: signed-out discovery, dual `url`+link-back verification, agent-discovered URL | structural | read the doc against R-4 |
| SC-5 | The doc binds the block form of record: heading, "also" intro, sentinels, fresh-OEmbed-verbatim generation, the recorded generator family with its fallback anchor and allowed-prefixes script, and the legacy/hand-rolled exclusions | structural | read the doc against R-5; compare the doc's recorded family against one fresh live OEmbed GET |
| SC-6 | The doc binds giscus coexistence exactly as R-6 states | structural | read the doc against R-6 |
| SC-7 | The doc binds the GAP mechanism with the explicit-instruction gate and the completion sequence | structural | read the doc against R-7 and the brothertown SOP rule-5 text of record |
| SC-8 | The doc binds credentialless/no-automation/no-scripts/no-ledger | structural | read the doc against R-8 |
| SC-9 | The doc binds the CSP step: origins, same-change timing, browser-render console instrument | structural | read the doc against R-9 against the current `static/_headers` content |
| SC-10 | `AGENTS.md` carries the pointer fact naming the doc path | structural | grep `AGENTS.md` for the doc path |
| SC-11 | The doc carries no factual claim beyond the design record and cited sources | structural | side-by-side read of the doc against this spec's design record, sources table, and R-11 |
| SC-12 | The doc's documented OEmbed GET, executed signed-out against a public toot URL, returns JSON whose `html` carries embed-generating markup sourced from mastodon.social — of-record family: `blockquote.mastodon-embed` + `data-embed-url` + `data-allowed-prefixes` script + fallback anchor to the status URL | behavioral | execute the documented GET against `https://mastodon.social/@michaelconrad/117624227198231767` and assert the four of-record family markers |

## Dependencies

| Reference | Relationship | Status |
|---|---|---|
| Design discussion record (this session, 2026-11-21/22; quoted in Intent) | Source of every disposition the requirements bind | satisfied |
| Repo evergreen iframe-embed gate (AGENTS.md, Build & verification) | Standing repo rule R-9's CSP step references and defers to | satisfied |
| CherokeeLessons#26 (app-announcement standing format, account unbound there) | Same-repo Mastodon-workflow precedent; establishes the account topic was deferred for announcements only — no blocking relationship to this spec | satisfied |
| `./build.sh` verification gate (AGENTS.md, Build & verification) | Verification command the workflow's build steps and this deliverable's tree use | satisfied |

No prerequisite is pending.

## Traceability

| Requirement | Success criteria |
|---|---|
| R-1 | SC-1 |
| R-2 | SC-2 |
| R-3 | SC-3 |
| R-4 | SC-4, SC-12 |
| R-5 | SC-5, SC-12 |
| R-6 | SC-6 |
| R-7 | SC-7 |
| R-8 | SC-8 |
| R-9 | SC-9 |
| R-10 | SC-10 |
| R-11 | SC-11 |

Every SC traces to at least one requirement; every requirement is verified.

## Documentation sources

| Source | Type | Location | Verification |
|---|---|---|---|
| brothertown-language-com SOP rule 5 (Mastodon discussion back-link; publication sequence when the thread does not exist yet; of record 2026-11-15) + rule 6 adjacency | workflow of record (adapted source) | `/home/muksihs/git/brothertown-language-com/docs/sop-site-setup.md` lines 193–232 | read directly this session 2026-11-21; GAP form and probe sequence quoted from the text |
| michael-conrad.com `local_util/backfill_mastodon_embeds.py` | comparison implementation (ledger+script variant, rejected) | `/home/muksihs/git/michael-conrad.com/local_util/backfill_mastodon_embeds.py` | read directly 2026-11-21: OEmbed GET, section shell `### Comment on Mastodon` + "You can comment…via Mastodon:" + `<!-- mastodon-embed-start/end -->` sentinels |
| michael-conrad.com shipped embed block and post markup | comparison evidence | `/home/muksihs/git/michael-conrad.com/content/blog/2026-10-27_the-brothertown-language-website-is-live/index.md` lines 141–149 | read directly 2026-11-21 |
| michael-conrad.com static dir (no `_headers`) | platform fact (its CSP comparison never applied) | `/home/muksihs/git/michael-conrad.com/static/` | `ls` check 2026-11-22: file absent |
| mastodon.social OEmbed endpoint, live output for status 117624227198231767 | live API evidence (generator family of record) | `GET https://mastodon.social/api/oembed?url=https%3A%2F%2Fmastodon.social%2F%40michaelconrad%2F117624227198231767` | live call 2026-11-21: `blockquote.mastodon-embed` + `data-embed-url` + `data-allowed-prefixes` script + fallback anchor |
| mastodon.social `embed.js` (3821 bytes) | live script evidence (hydration mechanics; "Legacy embeds" vs "New generation of embeds") | `https://mastodon.social/embed.js` (fetched copy at session temp) | downloaded and read 2026-11-21: blockquote hydration via `data-embed-url`, sandbox + postMessage height, zero fetch/XHR; handles both markup families |
| joinmastodon OEmbed method docs | API documentation | `https://docs.joinmastodon.org/methods/oembed/` | fetched 2026-11-22 (page updated 2026-05-01): `GET /api/oembed`, public, parameters `url`/`maxwidth`/`maxheight`; example shows the earlier iframe family — divergence from live output recorded in R-5/decision 2 |
| mastodon.social signed-out account lookup for the bound handle | live API evidence (account of record) | `GET https://mastodon.social/api/v1/accounts/lookup?acct=michaelconrad@mastodon.social` | live call 2026-11-22: id `112101023658888147`, url `https://mastodon.social/@michaelconrad` |
| CherokeeLessons `static/_headers` | repo fact (CSP of record; origins R-9 binds into) | `static/_headers` | read 2026-11-21: frame-src = giscus.app, youtube.com, itch.io; script-src = self, unsafe-inline, giscus.app |
| CherokeeLessons `templates/page.html`, `templates/post_macros.html`, `giscus.json` | repo facts (giscus in page footer; the coexistence R-6 binds) | repo tree | read 2026-11-21 |
| CherokeeLessons `content/new_page.py`, `config.toml`, `content/content/_index.md` | repo facts (scaffold; blog section conventions) | repo tree | read 2026-11-21 |
| CherokeeLessons `.issues/26/spec.md` | repo issue-store record (announcement workflow; account deferred there 2026-11-10) | `.issues/26/spec.md` | read 2026-11-21 |
| CherokeeLessons content grep for mastodon surfaces | repo fact (zero existing mastodon references) | `grep -ri mastodon content static templates` | run 2026-11-21: no matches |

## Cost frame

The content-criteria instruments (SC-1 through SC-11) are reads of one doc plus one `AGENTS.md` line — minutes each; skipping them risks a workflow of record that contradicts this repo's own facts (CSP origins, giscus position, account binding) and misfires at first execution inside a live publishing cycle. SC-3's tag GET and SC-12's OEmbed GET are one signed-out HTTP call each — seconds; skipping them risks enshrining a dead or drifted API method whose bitrot surfaces mid-cycle, exactly where a stalled back-fill blocks a publication. Acting on any SC costs less than discovering its failure during the first entry's publish.

## Edge cases

| Condition | Expected behavior | Resolution |
|---|---|---|
| Toot missing, private, or deleted when the probe runs | Probe finds no public toot linking back; back-fill does not proceed; the (GAP) state stands | Report blocker; the follow-up PR waits until a resolvable public toot exists |
| Toot deleted after back-fill shipped | The hydrated embed has nothing to render; the block degrades to its no-JS fallback anchor, now a dead link | None of record — the design defines no repair cycle; the doc may describe the degrade in one line, obligation-free |
| Entry folder renamed between publication and back-fill | The link-back check (`card.url` or body link) no longer matches the entry's URL; dual verification fails | Correctly no back-fill: halt and report — the failure mode the michael-conrad.com script silently swallowed |
| OEmbed endpoint unreachable or its output unusable at back-fill time | No fallback substitute markup is authored (the anti-cargo rule holds) | Block the completion step; report; the GAP/placeholder state persists until the endpoint answers |
| Carrier tag dead or unverified (`api/v1/tags` empty/failed) | The entry's frontmatter tags remain the hashtag source of record; the verification result is recorded, not fatal | Toot proceeds; the doc records the check's findings state |
| Probe returns multiple candidate toots (reposts, quotes) | Verification constrains candidates to `url`s published by the bound handle (`@michaelconrad`) that link back to the article | Bind the canonical `url` of the author's own toot; ignore derivatives |

## Completion gate

Every success criterion passes before the deliverable is complete; partial implementation is not permitted. SC-1 through SC-11 pass by the stated reads; SC-12 passes by executing the documented GET form live. The workflow's first real execution (a new entry's toot + back-fill) is future work outside this deliverable, governed by the doc it defines.

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)
