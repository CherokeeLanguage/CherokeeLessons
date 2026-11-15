# CherokeeLessons.com

The new and improved CherokeeLessons.com using the Zola static site generator.

![Screenshot](screenshot.png)

## Building and deploying

The zola version is pinned in `ZOLA_VERSION` (currently 0.22.1 — the latest
release compatible with the vendored Zulma theme).

- `./build.sh` downloads the pinned zola release binary on first use (cached
  as `./zola`, gitignored), reports its version, and builds the site into
  `public/`. The branch decides the base URL: `master` builds with
  `https://cherokeelessons.org/`; any other branch builds domain-neutral
  (`--base-url /`) so a preview hostname serves; `SITE_BASE_URL` overrides
  both.

## Deployment (Cloudflare Workers Static Assets)

The site deploys through **Workers Builds**: a merge to `master` triggers a
build + `npx wrangler deploy` automatically; PR branches get preview builds
(inspect one with `npx wrangler preview`). The deploy unit is the assets-only
`wrangler.jsonc` manifest — no Worker script, no bindings — and wrangler is
pinned in `package.json`.

### Prerequisites

Before running the setup, confirm you have:

- **A Cloudflare account with the `cherokeelessons.org` zone.** Custom-domain
  attachment only works for a zone that lives on the same account as the
  Worker. Verify at the dashboard: the zone appears under
  *Websites → <account>* and shows its nameservers as Cloudflare-assigned.
- **An account identity with the rights the procedure touches:** Workers
  (create + configure the Worker) and DNS records on the zone. Nothing
  below provisions storage; the deploy unit is the assets-only manifest
  plus the site build.
- **GitHub authorization for Workers Builds:** the *Workers Builds GitHub
  App* must be installed and granted access to this repository
  (`CherokeeLanguage/CherokeeLessons`). Verify at the dashboard:
  *Workers & Pages → Settings → Builds* (or the Worker's *Settings → Build*)
  shows the connected repository. If the App is connected to the org,
  confirm which repositories it can see before assuming the build will
  start.
- **A local clone of this repository** (for the repo-side facts the
  dashboard asks for: the worker name, the build command, the production
  branch — all recorded here).

### The setup procedure

The steps are ordered: each names what it depends on and how to confirm it
worked. Do not proceed past a step whose verification fails — the failure
modes subsection below lists the common build failures.

1. **Connect the GitHub App to this repository.**
   *Depends on:* prerequisites (App installed with this repo in scope).
   *Do:* dashboard → *Workers & Pages* → create/connect the build source,
   authorizing the App for `CherokeeLanguage/CherokeeLessons`.
   *Verify:* the dashboard's build source shows the repo; the initial
   build attempt appears in *Deployments/Build history* (it may fail here —
   that is fine, the next steps fix the configuration).
2. **Create the Worker with the exact name `cherokeelessons-org`.**
   *Depends on:* step 1 (the build source must point at this repo).
   *Do:* name the Worker exactly `cherokeelessons-org` — the name is the
   link between the builds integration and the tracked `wrangler.jsonc`
   (`name: "cherokeelessons-org"`); a mismatch is a builds-fail condition.
   *Verify:* the Worker exists in the dashboard with that name, and its
   settings page exposes a *Builds* section (no build configured yet).
3. **Configure the build: command `./build.sh`, production branch `master`.**
   *Depends on:* step 2.
   *Do:* set the build command to `./build.sh` (unset means no build runs
   and the Wrangler config is not consulted for it) and the production
   branch to `master` — **not** the builds default `main`; this repo's
   branch of record is `master`.
   *Verify:* the settings read back exactly: build command `./build.sh`,
   production branch `master`.
4. **Trigger and watch a production build.**
   *Depends on:* step 3.
   *Do:* push to `master` (or use the dashboard's retry) and open
   *Deployments → Build history*. The build log should show the pinned
   Zola download (`ZOLA_VERSION`), the `./build.sh` run (branch-aware
   base-url: `master` builds `https://cherokeelessons.org/`), and the
   `npx wrangler deploy` upload.
   *Verify:* the deployment shows success and `npx wrangler deployments`
   (or the dashboard) lists the active deployment with the expected commit.
5. **Attach the custom domains `cherokeelessons.org` and
   `www.cherokeelessons.org`.**
   *Depends on:* step 4 (the Worker must exist with a successful
   deployment before domains attach).
   *Do:* dashboard → the Worker's *Settings → Domains & Routes* → add both
   custom domains; Cloudflare creates the zone records for you.
   *Verify:* DNS: `https://cherokeelessons.org/` serves the site (HTTP
   200, Zola content). `www.cherokeelessons.org` served a 301 to the apex
   when last working (2026-11-11) but was NXDOMAIN on 2026-11-12 — confirm
   the record state after attachment rather than assuming either; attachment
   state may change (the stand-up ops sequence includes apex and www).
6. **Confirm the end state.**
   *Depends on:* step 5.
   *Verify:* the deployment is active for the production branch; merging a
   PR to `master` produces a new build + deploy without manual dashboard
   action; PR branches produce preview deployments (inspect one with
   `npx wrangler preview`); the site serves at the apex.

### When a build fails

The three common causes, by their symptom in *Deployments → Build history*:

- **Worker-name mismatch** — the builds integration cannot find a Worker
  matching the `wrangler.jsonc` `name`. Symptom: the build fails at/deploy
  stage complaining the Worker or project does not exist. Fix: make the
  dashboard Worker name exactly `cherokeelessons-org` (step 2).
- **No build command configured** — with the build command unset, no build
  runs and the Wrangler config is not consulted for one; deploys then ship
  nothing. Symptom: an empty/failed deploy with no Zola output in the log.
  Fix: set the build command to `./build.sh` (step 3).
- **Wrong production branch** — the builds default is `main`; pushes to
  `master` produce nothing until the production branch is set to `master`
  (step 3). Symptom: merges to `master` produce no deployment at all.

### Verifying locally (before pushing)

- `./build.sh` downloads the pinned zola release binary on first use
  (cached as `./zola`, gitignored), reports its version, and builds into
  `public/`. Branch decides the base URL: `master` builds
  `https://cherokeelessons.org/`; any other branch builds domain-neutral
  (`--base-url /`) so a preview hostname serves; `SITE_BASE_URL` overrides
  both. Serve `public/` with any static server (or `npx wrangler preview`,
  which exercises the actual deploy unit, the assets-only `wrangler.jsonc`)
  and check the rendered site before pushing.

## Notes

postWeight=$(("$(date +%s)" / 10 * 10))
postDate="$(TZ=UTC date -u --iso-8601=seconds|cut -f 1 -d '+')Z"
echo "post weight: $postWeight"
echo "post date: $postDate"
