# Spec — external ebook-store links removed from book pages (site-only ebook channel)

Remote issue: https://github.com/CherokeeLanguage/CherokeeLessons/issues/55 · part of the CherokeeLessons#30 recomposition (2026-11-14)

## Problem

The developer's standing distribution directive of record (2026-11-12 chat: "only itch.io is the platform for distribution going forwards. the ebook is now only available via cherokeelessons.org.") names the ebook surface, but 15 `content/books/` pages still carry external ebook-store links — 26 link lines (`smashwords.com` ePub and/or `amazon.com/dp` Kindle) — and the tracked scaffold `content/books/new-page.sh` carries 2 further template store-link lines that re-create external ebook-store links on every scaffolded page (a standing regression vector).

## Scope

This repository — `content/books/*.md` (15 pages) and `content/books/new-page.sh` only. Third-party-published books' retail Amazon links in two `content/content/` posts (Cherokee Narratives, Cherokee Reference Grammar) are correctly outside: retail links to others' books, not the developer's own distribution.

## Requirements

- **R-1 — Store links removed.** The external ebook-store links SHALL be removed from the book pages — every `smashwords.com` ePub link and every `amazon.com` Kindle link across the 15 `content/books/` pages (26 link lines, measured) — leaving the on-site free-PDF direct downloads as the only ebook channel (they are the site-only ebook channel's existing form).
- **R-2 — Scaffold regression vector closed.** The tracked scaffold `content/books/new-page.sh` SHALL lose its two template store-link lines.
- **R-3 — No invention.** No new claims, links, or dispositions beyond link removal.

## Success criteria

| ID | Criterion | Class | Instrument |
|----|-----------|-------|------------|
| SC-1 | No external ebook-store links remain on the book pages or the scaffold | structural | `grep -rnE 'smashwords\.com|amazon\.com/dp' content/books` is empty |

(Paperback Lulu links and `content/app-ads.txt` reseller entries are out of scope — recorded open decisions on the umbrella CherokeeLessons#30.)

## Out of scope

- The Lulu paperback links on the 15 book pages — not ebooks; disposition an open developer decision (umbrella-recorded).
- The `content/app-ads.txt` reseller entries — store-attribution infrastructure; disposition an open developer decision (umbrella-recorded).
- The two book PDFs' delivery disposition (CherokeeLessons#31's R-8; the R2 plan was ruled invalid 2026-11-12 — the disposition decision is the developer's).
- Book content, front matter, or page structure beyond the link lines.

## Approach

Link-line removal across 15 pages plus the scaffold's two template lines under the developer's "only … via cherokeelessons.org" directive; the on-site free-PDF direct downloads remain. Rollback: revert the commit set (history-recoverable).

## Impact

Consumers: book-page visitors — the external-store buy paths disappear; the on-site free-PDF download remains as the (now sole) ebook channel per the developer's directive. Risk: low — link removal only.

> Full spec and plan artifacts: https://github.com/CherokeeLanguage/CherokeeLessons/tree/issues-data/55
<!-- artifacts: .issues/55/ -->
