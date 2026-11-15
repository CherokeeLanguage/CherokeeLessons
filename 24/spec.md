# Spec — itch.io page for Cherokee Language Lessons 1: create the page and deploy the published artifact (itch migration, phase 1)

> **Ownership correction (2026-11-14, developer direction):** the itch page creation, the artifact deployment, and the push tooling are **app-repo work** under the developer's cross-repo filing rule (2026-11-11) — owned by the paired spec CherokeeLanguage/cll1-gdx#4 and its publish successor CherokeeLanguage/cll1-gdx#5, which already carry the build/publish deliverables (the 1.82 jar + APK and the page at slug `cherokee-language-lessons-1`). This spec's channel table, R-4/R-5/R-7, and SC-5/SC-6 are superseded to that app-repo ownership. This repo's ticket owns only the site surface — the CLL-1 page's link removals (CherokeeLessons#30) and itch-link addition (CherokeeLessons#32) — and its SC-1–SC-4 remain as acceptance criteria for the app-repo-delivered page.
Paired spec: **CherokeeLanguage/cll1-gdx#4** — the cll1-gdx repo's side of this app's migration (its deploy-target conversion). The paired app-repo spec owns the app repo side (the itch page, the artifact deployment, and the push tooling); this repo's ticket owns the site acceptance record for the delivered page (SC-1-SC-4) and the site page's executable update (its site-scope section below).


## Problem

The Cherokee Language Lessons 1 app is downloadable only through a site-hosted binary and store links of unknown validity. The migration makes itch.io the app download channel; this app has no itch presence, so its site page cannot point at an itch URL yet. (Disposition of record, remediated 2026-11-14: the site page's conversion is owned by the filed distribution-policy spec CherokeeLessons#30 — this spec owns the itch page and the initial deployment.)

## Scope

Phase 1 of the two-phase migration, for this app: create its itch.io project page and deploy this repo's currently-published Cherokee Language Lessons 1 artifact to it. Site-repo changes are limited to the repeatable push tooling; the site's app page is not modified by this spec (the site-page conversion is owned by CherokeeLessons#30's link removals, with the itch link added by CherokeeLessons#32 — see Out of scope).

## Requirements

- **R-1 — itch project page.** A itch.io project is created under the `cherokee-language` account, titled "Cherokee Language Lessons 1" (slug `cherokee-language-lessons-1`, page `https://cherokee-language.itch.io/cherokee-language-lessons-1`), published, classified Educational, with the site page's featured image (`static/images/cll1-gdx.png`) as cover art. Project creation is a developer dashboard step — butler has no project-creation path (it pushes to existing projects), matching the Bound Pronouns precedent.
- **R-2 — Pricing mirrors Bound Pronouns.** The page's payment setting is "Name your own price" (free download, donations enabled) — the developer's "download/donate page" direction and the live Bound Pronouns page's configuration.
- **R-3 — Description derives from the site page.** The page's description text is adapted from `content/apps/cherokee-language-lessons-1.md`'s existing prose, with no factual claim absent from that prose (BoundPronouns#59 is the form precedent). Cover image comes from `static/images/cll1-gdx.png`, the file the site page's front matter references.
- **R-4 — Butler deployment of the existing artifact.** The page's download channel carries this repo's currently-published Cherokee Language Lessons 1 artifact per the channel table, pushed with butler. Pushes carry no version claims (no `--userversion`; butler's auto-numbering stands). The artifact deploys as an opaque file under its existing filename — metadata (tags, filename versions, page dates) is not trusted, so no version label is verified or asserted as truth.
- **R-5 — Repeatable push tooling.** The repo-local push script plus gitignored `itch.conf` (mirroring the BoundPronouns pattern: butler resolution PATH→vendored `.tools/butler/`, `REPLACE_ME` config guard, per-channel pushes) encode this app's channel-table row. No secrets in the repo — butler credentials live in butler's own store.
- **R-7 — Ordering.** The dashboard project creation precedes the butler pushes; platform tags follow butler's channel-naming rules (`win-linux-mac` auto-tags all three desktop platforms).

### Channel table

| App | itch project (slug) | Channel | Source artifact (this repo) |
|---|---|---|---|
| CLL1 | `cherokee-language-lessons-1` | `win-linux-mac` | `static/downloads/CLL1-1.72.jar` |

Desktop-channel-only launch: the site never hosted a CLL1 APK (its Android builds went through the stores), and no store-era APK artifact is in evidence in this repo.

## Success criteria

| ID | Criterion | Class | Instrument | Traces to |
|---|---|---|---|---|
| SC-1 | The itch URL serves the published game page | behavioral | HTTP GET of the URL returns the game page (HTTP 200 + expected game title) | R-1 |
| SC-2 | The page's download section lists its channel's file with filename matching the channel table's source artifact, and its displayed size consistent with the source artifact's size at the page's displayed granularity | behavioral | Page fetch; compare the listed filename against the source file's name, and the displayed size against the source file's byte size rounded the same way the page displays it | R-4 |
| SC-3 | The page's payment setting reads "Name your own price" | behavioral | Page fetch text | R-2 |
| SC-4 | The page's description derives from the site page's prose with no factual claim absent from that prose | content | Side-by-side read of the itch description against the site page source | R-3 |
| SC-5 | The repo-local push script + gitignored `itch.conf` exist, carry the BoundPronouns-pattern guards (REPLACE_ME, butler resolution), and encode this app's channel-table row | structural | Read the script and config | R-5 |
| SC-6 | The pushes carry no version claims | structural | Script read (`--userversion` absent); butler push output shows auto-numbered builds only | R-4 |

Traceability note: R-7 is a procedural requirement whose satisfaction is verified by the work's own record (ordering of steps taken); it carries no standalone SC. The site-pages boundary is carried by this spec's Scope section.

## Approach

Mirror the proven BoundPronouns pattern: the developer creates and configures the itch project dashboard-side (one-time prerequisites: `butler` install + `butler login`, project creation with the R-1/R-2 settings), the agent supplies description text adapted from the site page and executes the butler pushes through the repo-local tooling, then verifies the live page against the SCs.

Alternatives weighed (2026-11-09): CI-driven pushes — rejected, this is a one-time deployment of an existing artifact, and CI enters later at the binary-repo deploy-pipeline conversion; API-driven project creation — rejected, no documented butler/API path creates projects; an Android channel — rejected for now, no APK artifact is in evidence to deploy.

## Site page update — executable site scope (2026-11-14, developer direction)

- **R-S1 — CLL-1 page update.** `content/apps/cherokee-language-lessons-1.md` SHALL drop its self-hosted desktop jar link (`/downloads/CLL1-1.72.jar`) and ALL its store links (`play.google.com`, `apps.apple.com`, `amazon.com` — all probe-dead). The page's itch link is NOT added by this ticket: it arrives via CherokeeLessons#32 once the cll1-gdx publish (CherokeeLanguage/cll1-gdx#5) delivers the project — until then the page carries no distribution links by design (no dead links shipped).
- **R-S2 — Page presentation refresh — layout, wording, screenshots (added 2026-11-14, developer direction: "update the layout and text wording — the app pages don't describe the apps very well", "adding and including 1 or more screenshots is acceptable; each app repo should have screen shots").** The page's rewrite SHALL go beyond the link removals:
  - (a) **Wording.** The page's descriptive text SHALL be expanded into prose that accurately describes what the app does and its notable capabilities — drafted by the agent from the app's release notes and the page's existing prose, carrying no factual claim absent from those sources, and reviewed/approved by the developer before the edit lands (content decisions are the developer's).
  - (b) **Layout.** The page's presentation SHALL be reworked to the itch-first model's interim state: prominent, accurate app description, no dead link lists.
  - (c) **Screenshots.** The page SHALL embed one or more in-app screenshots (developer confirmed acceptance 2026-11-14). Screenshot files come from the cll1-gdx repo, which carries its own screenshots; if a needed screenshot does not yet exist, its production is app-repo work and this ticket declares the dependency — never fabricates or substitutes site-side images.
  - (d) **Itch widget and itch presentation are #32's scope** — the widget needs a live project to embed; this ticket's page carries only wording/layout/screenshots, and CherokeeLessons#32 adds the itch link + widget when the project is live.

### Site-update criteria

| ID | Criterion | Class | Instrument | Traces to |
|---|---|---|---|---|
| SC-S1 | The page carries no desktop-jar link and no store links | structural | `grep -c "downloads/CLL1" content/apps/cherokee-language-lessons-1.md` = 0; `grep -cE "play\.google\.com|apps\.apple\.com|amazon\.com" content/apps/cherokee-language-lessons-1.md` = 0 | R-S1 |
| SC-S2 | The page carries no dead/fabricated links (no itch link until #32 adds it) | structural | `grep -c "cherokee-language.itch.io" content/apps/cherokee-language-lessons-1.md` = 0 at this ticket's completion | R-S1 |
| SC-S3 | The page edit does not break the build | behavioral | executed `./build.sh` over the edited working tree; exit 0 | R-S1 |
| SC-S4 | The page's descriptive prose accurately describes the app — no factual claim absent from the app's release notes and the page's prior prose; the developer's wording approval is recorded | content | side-by-side read against the release notes / prior page prose; approval in the work record | R-S2(a) |
| SC-S5 | The page embeds ≥ 1 in-app screenshot sourced from the cll1-gdx repo's screenshots | structural | `grep -niE "!\[.*\]\(.*\)" content/apps/cherokee-language-lessons-1.md` (image embed) present; the referenced file's origin recorded in the work record | R-S2(c) |

Traceability note: R-S2(b) (layout) is a presentational rewrite with no standalone grep instrument — its satisfaction is verified by the work's own record (the reviewer's before/after page read), per the R-7 procedural-note style. R-S2(d)'s widget scope is CherokeeLessons#32's (its R-1 carries the widget; SC-4 traces it).

## Out of scope

- The site-page landing conversion of `content/apps/cherokee-language-lessons-1.md` — owned by CherokeeLessons#30 (removes the store links and the desktop-jar link) and CherokeeLessons#32 (adds the itch link once the cll1-gdx publish delivers it). Originally filed as "phase 2 — separate spec" with no spec named; remediated 2026-11-14 to point at the filed owners so the deferred work is not dangling. [Superseded same day: the page's removal edits are now this ticket's R-S1 above per the developer's site-update direction; #30 retains the end-state re-assertion (its SC-12) and #32 retains the itch-link addition.]
- The `CherokeeLanguage/cll1-gdx` repo's deploy-target conversion ticket — filed in that repo.
- Any artifact rebuild — only the existing site-repo copy deploys.

## Impact

One public itch.io page appears under the `cherokee-language` account. The site's CLL-1 page content changes per this spec's site-scope section (R-S1/R-S2); the cll1-gdx repo's tracked content is untouched by this ticket (screenshot production is an app-repo dependency declared in R-S2(c)); other apps unaffected. Risk is low: new external state only, no destructive operation (a mis-configured page can be edited or deleted dashboard-side; butler pushes to a channel are idempotent and re-pushable).

🤖 Co-authored with AI: OpenCode (huggingface/zai-org/GLM-5.3-Flash)

> Full spec and plan artifacts: https://github.com/CherokeeLanguage/CherokeeLessons.com/tree/issues-data/24/
>
> AI-facing artifacts pointer: `.issues/24/`

