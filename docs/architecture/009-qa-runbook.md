# ADR 009 — QA Runbook

**Status:** Accepted — MVP
**Date:** 2026-03-08
**Context:** French AI news website, 12-hour ingestion cycle, Node.js backend, Next.js 14 frontend, SQLite, no user accounts.

---

## Purpose and How to Use This Runbook

This runbook defines the quality validation process for the French AI news site across four phases. It is the authoritative reference for every validation cycle from initial pipeline setup through post-launch operation.

### Two modes of use

**(a) Pre-launch full validation**
Run all four phases in order before the first public launch. Each phase produces a gate decision. Do not proceed to the next phase if a BLOCKER is unresolved.

**(b) Post-ingestion lightweight check**
Run the abbreviated checklist in Phase 6 after each 12-hour production ingestion cycle. This is a regression scan, not a full validation pass.

### Who runs each phase

| Phase | Owner | Trigger |
|---|---|---|
| Phase 1 — Pipeline validation | Backend engineer or tech lead | After first ingestion cycle; after any pipeline code change |
| Phase 2 — Frontend structural validation | Frontend engineer or tech lead | Once the frontend is deployed to staging |
| Phase 3 — Editorial quality review | Editorial reviewer | Before launch (30-item minimum sample); monthly thereafter |
| Phase 4 — Pre-launch checklist | Tech lead with frontend and editorial input | Once, before first public launch |
| Phase 6 — Post-ingestion lightweight check | Backend engineer or automated monitor | After each 12-hour production ingestion cycle |

### What BLOCKER and NON-BLOCKING mean in this project

**BLOCKER:** The issue prevents launch or corrupts the site's core function. Do not ship until this is resolved. Examples: broken ingestion, invisible items, missing canonical tags, 500 errors on public pages.

**NON-BLOCKING:** The issue degrades quality, SEO, or accessibility but does not prevent the site from functioning. Must be addressed before or shortly after launch, but does not gate the release. Examples: a single miscategorized item, a slightly-too-short title.

---

## Phase 1 — Pipeline Validation

Run this phase after the first ingestion cycle and after any pipeline code change.

### Ingestion run health

- [ ] **[BLOCKER]** Full ingestion run completes without unhandled errors (no uncaught exceptions in logs, process exits cleanly)
- [ ] **[BLOCKER]** A row is written to `ingestion_runs` with `started_at`, `ended_at`, and `items_stored` all non-null
- [ ] **[BLOCKER]** `ended_at` is later than `started_at` (run did not stall or exit mid-cycle)

### Category coverage

- [ ] **[BLOCKER]** All 8 categories have at least 3 visible items (`is_visible = 1`) after the cycle. Query: `SELECT categorie, COUNT(*) FROM news_items WHERE is_visible = 1 GROUP BY categorie`

### Deduplication

- [ ] **[BLOCKER]** Running ingestion twice in succession produces zero new rows on the second run (`items_new = 0` in the second `ingestion_runs` entry)
- [ ] **[BLOCKER]** Items older than 48 hours relative to ingestion time are not stored (verify `original_pub_date` on ingested items)

### Field completeness

- [ ] **[BLOCKER]** No stored item has a null or empty value for: `titre_fr`, `resume_fr`, `pourquoi_important`, `categorie`, `priorite`, `source_url`, `source_name`, `original_pub_date`, `source_lang`, `published_at`. Query: `SELECT id FROM news_items WHERE titre_fr IS NULL OR resume_fr IS NULL OR pourquoi_important IS NULL OR categorie IS NULL OR priorite IS NULL OR source_url IS NULL OR source_name IS NULL OR original_pub_date IS NULL OR source_lang IS NULL OR published_at IS NULL`

### Enum validation

- [ ] **[BLOCKER]** All `categorie` values are one of the 8 allowed slugs. Query: `SELECT DISTINCT categorie FROM news_items` — compare against the allowed list.
- [ ] **[BLOCKER]** All `priorite` values are one of: `high`, `medium`, `low`. Query: `SELECT DISTINCT priorite FROM news_items`

### Source URL health

- [ ] **[NON-BLOCKING]** Spot-check 5 `source_url` values return HTTP 200 (see Phase 5 — Tools for curl command)

### Date field correctness

- [ ] **[BLOCKER]** `original_pub_date` reflects the publication date from the source RSS feed, not the ingestion timestamp. Verify by comparing `original_pub_date` to `published_at` on 3 items — they should differ and `original_pub_date` should predate or equal `published_at`

### Slug stability

- [ ] **[BLOCKER]** Re-ingesting the same item (same `source_url`) does not change its slug. Test by noting a slug before a second ingestion run and confirming it is unchanged after.

### Resilience

- [ ] **[BLOCKER]** Simulating a feed failure (point a feed URL at an unreachable host) does not corrupt existing items — previously stored rows remain intact and `is_visible` values are unchanged
- [ ] **[NON-BLOCKING]** Feed failure is logged in the `errors` JSON column of `ingestion_runs`

### Visibility default

- [ ] **[BLOCKER]** Newly stored items have `is_visible = 1` by default. Verify on the most recent ingestion batch: `SELECT id, is_visible FROM news_items ORDER BY published_at DESC LIMIT 10`

---

## Phase 2 — Frontend Structural Validation

Run this phase once the frontend is deployed to staging.

### Page availability

- [ ] **[BLOCKER]** Homepage (`/`) returns HTTP 200 and renders at least 1 news item
- [ ] **[BLOCKER]** All 8 category pages return HTTP 200: `/categories/llm/`, `/categories/agents-ia/`, `/categories/robotique/`, `/categories/recherche/`, `/categories/industrie/`, `/categories/politique-regulation/`, `/categories/outils-developpeurs/`, `/categories/securite-ia/` (adjust slugs to match the actual `CATEGORIE_SLUG_MAP`)
- [ ] **[BLOCKER]** A news item detail page loads fully with `titre_fr`, `resume_fr`, `pourquoi_important`, `source_name`, `original_pub_date`, and `source_lang` all rendered

### Category correctness

- [ ] **[BLOCKER]** Each category page shows only items belonging to that category. Verify by loading a category page and confirming no item from a different category appears. Spot-check 2 categories.

### Error states

- [ ] **[BLOCKER]** A request to an unknown slug (e.g., `/news/slug-inexistant-12345/`) returns HTTP 404 — not HTTP 500 and not a blank page
- [ ] **[NON-BLOCKING]** An empty category page (0 visible items) renders a graceful empty state — no JavaScript error, no crash, no stack trace exposed

### Link behavior

- [ ] **[BLOCKER]** Source links (`source_url`) open in a new tab with `rel="noopener noreferrer"` present in the rendered HTML anchor

### Navigation

- [ ] **[BLOCKER]** All 8 category links are present in the rendered HTML `<nav>` element (not injected by JavaScript only — verify in page source)
- [ ] **[NON-BLOCKING]** Breadcrumb is rendered on category pages and news item pages

### Internationalisation

- [ ] **[BLOCKER]** `<html lang="fr">` is set on all pages (homepage, category, item)

### Document structure

- [ ] **[BLOCKER]** Each page contains exactly one `<h1>` element
- [ ] **[BLOCKER]** The `<title>` tag is unique per page — item pages must not share the same title
- [ ] **[BLOCKER]** The `<meta name="description">` tag is unique per item page and is derived from the first 1–2 sentences of `resume_fr`, not a generic fallback

### SEO tags

- [ ] **[BLOCKER]** A self-referencing `<link rel="canonical">` is present on all pages (homepage, category pages, item pages)
- [ ] **[BLOCKER]** `NewsArticle` structured data (JSON-LD) is present on all item pages. Validate one item page using the schema.org validator (see Phase 5 — Tools).

### Pagination

- [ ] **[NON-BLOCKING]** If at least 21 visible items exist, `/page/2` returns HTTP 200 and renders items 21–40

### Incremental Static Regeneration (ISR) fallback

- [ ] **[BLOCKER]** Navigating to a slug for a new item that was not included in `getStaticPaths` returns the rendered page (HTTP 200), not HTTP 404. This confirms `fallback: "blocking"` is active. Test by ingesting a new item, noting its slug, and requesting the page before the next ISR revalidation cycle.

---

## Phase 3 — Editorial Quality Review

Run this phase before launch (minimum 30-item sample drawn across all 8 categories) and monthly thereafter.

### Sample selection

- [ ] **[NON-BLOCKING]** Sample of 30 items is drawn with at least 2 items per category. If a category has fewer than 2 items, sample all available items from that category.

### Language correctness

- [ ] **[BLOCKER]** All `resume_fr` values are written in grammatical French. No full sentences in English, German, or other languages are present. Spot-check every item in the sample.
- [ ] **[BLOCKER]** All `titre_fr` values are in French. No untranslated English titles.

### Originality

- [ ] **[BLOCKER]** No `resume_fr` value is a verbatim copy of the source article text. Spot-check 5 items by opening the `source_url` and comparing the résumé to the original text.
- [ ] **[BLOCKER]** `pourquoi_important` adds perspective not already stated in `resume_fr`. It must not be a rephrasing of the résumé. Evaluate all 30 sampled items.

### Field length compliance

- [ ] **[BLOCKER]** All `titre_fr` values are between 60 and 100 characters (inclusive). Query: `SELECT id, titre_fr, LENGTH(titre_fr) FROM news_items WHERE is_visible = 1 AND (LENGTH(titre_fr) < 60 OR LENGTH(titre_fr) > 100)`
- [ ] **[BLOCKER]** All `resume_fr` values are between 60 and 120 words. Script or manual count required. Any out-of-range résumé is a blocker.
- [ ] **[NON-BLOCKING]** All `pourquoi_important` values are between 40 and 80 words. Values outside this range are flagged for editorial correction but do not block launch if fewer than 3 items are affected.

### Categorisation accuracy

- [ ] **[BLOCKER]** At least 85% of sampled items are assigned to the correct category according to the editorial categorisation rules defined in ADR 003. Items below this threshold trigger a review of the AI categorisation prompt.

### Priority integrity

- [ ] **[BLOCKER]** No item rated `high` priority fails to meet at least 2 of the high-priority criteria defined in ADR 003. Priority inflation (excessive use of `high`) is the primary quality risk for this field. If more than 20% of sampled items are rated `high`, escalate to editorial review.

### LLM artifact detection

- [ ] **[BLOCKER]** No item contains raw prompt fragments, template placeholders, or garbled output (e.g., `{{title}}`, `As an AI language model`, incomplete sentences that trail off).

### Deduplication of content

- [ ] **[NON-BLOCKING]** No two visible items covering the same story have near-identical `resume_fr` content. If found, the lower-priority item should be set to `is_visible = 0`.

### Source URL quality

- [ ] **[BLOCKER]** All `source_url` values in the sample are direct links to individual articles — not homepages, RSS feeds, or category pages. Spot-check all 30 sampled items.

### Source language label

- [ ] **[NON-BLOCKING]** The `source_lang` value matches the actual language of the source article. Spot-check 5 items.

### Style compliance

- [ ] **[NON-BLOCKING]** No item contains exclamation marks, rhetorical questions, or filler phrases (e.g., "Il est important de noter que", "Comme vous le savez"). Review all 30 sampled items.

---

## Phase 4 — Pre-Launch Checklist

Run once, before first public launch.

### Performance

- [ ] **[BLOCKER]** Lighthouse mobile audit on homepage: LCP < 2.5s and CLS < 0.1. Run via Lighthouse CLI or PageSpeed Insights (see Phase 5 — Tools).
- [ ] **[NON-BLOCKING]** Lighthouse mobile audit on one category page: LCP < 2.5s and CLS < 0.1.
- [ ] **[NON-BLOCKING]** Lighthouse mobile audit on one item page: LCP < 2.5s and CLS < 0.1.
- [ ] **[NON-BLOCKING]** Total uncompressed page weight of homepage is under 500 KB (check in Lighthouse or browser DevTools Network tab).
- [ ] **[NON-BLOCKING]** No render-blocking scripts present in `<head>` (no synchronous `<script>` tags without `async` or `defer`).

### Crawlability

- [ ] **[BLOCKER]** `GET /robots.txt` returns HTTP 200 and allows all crawlers (`User-agent: *`, no `Disallow` on content paths).
- [ ] **[BLOCKER]** `GET /sitemap.xml` returns HTTP 200 and lists: the homepage, all 8 category pages, and all visible item pages.
- [ ] **[NON-BLOCKING]** `sitemap.xml` has been submitted to Google Search Console.

### Link integrity

- [ ] **[BLOCKER]** All internal navigation links resolve without HTTP 404 or 500 errors. Use a link checker tool (see Phase 5 — Tools).
- [ ] **[BLOCKER]** Source links use `rel="noopener"` without `rel="nofollow"` — these are citation links and must pass link equity.

### Accessibility

- [ ] **[BLOCKER]** Body text color contrast ratio meets 4.5:1 minimum (WCAG 2.1 AA). Verify using axe DevTools or browser accessibility inspector.
- [ ] **[BLOCKER]** A "Aller au contenu" skip navigation link is present and functional at the top of every page.
- [ ] **[BLOCKER]** All interactive elements (navigation links, source links, pagination) are keyboard-navigable using Tab and Enter.
- [ ] **[BLOCKER]** Focus indicators are visible on all interactive elements (no `outline: none` without a custom visible replacement).
- [ ] **[BLOCKER]** All date values use `<time datetime="...">` elements with a machine-readable ISO 8601 date in the `datetime` attribute.
- [ ] **[NON-BLOCKING]** Screen reader test completed on homepage and one item page using NVDA + Firefox (Windows) or VoiceOver + Safari (macOS/iOS). All visible content is announced correctly. Headings, links, and dates are readable in logical order.

### Cross-browser and cross-device

- [ ] **[BLOCKER]** Renders correctly on Chrome latest (P0).
- [ ] **[BLOCKER]** Renders correctly on Firefox latest (P0).
- [ ] **[NON-BLOCKING]** Renders correctly on Safari latest (P1).
- [ ] **[BLOCKER]** Renders correctly on iPhone + Safari (P0).
- [ ] **[BLOCKER]** Renders correctly on Android + Chrome (P0).

### Content gate

- [ ] **[BLOCKER]** At least 3 items are visible (`is_visible = 1`) per category — not just 1. Query: `SELECT categorie, COUNT(*) FROM news_items WHERE is_visible = 1 GROUP BY categorie HAVING COUNT(*) < 3`
- [ ] **[BLOCKER]** No item with `is_visible = 0` appears on any public page (homepage, category pages, pagination). Verify by querying `is_visible = 0` items, noting their slugs, and confirming those slugs return HTTP 404.

---

## Phase 6 — Post-Ingestion Lightweight Check (Recurring)

Run after each 12-hour production ingestion cycle. This is a regression scan, not a full validation pass.

- [ ] **[BLOCKER]** `ingestion_runs` shows a completed run: the latest row has `ended_at` not null.
- [ ] **[BLOCKER]** `items_stored > 0` — the pipeline produced results. If 0, investigate feed availability and OpenAI connectivity before next cycle.
- [ ] **[NON-BLOCKING]** The `errors` JSON column is empty or contains only known, non-critical warnings. Any new error type must be investigated before the next cycle.
- [ ] **[BLOCKER]** The homepage shows new items: compare `MAX(published_at)` before and after the ingestion run to confirm new content was published.
- [ ] **[BLOCKER]** No category page is empty. Quick check: `SELECT categorie, COUNT(*) FROM news_items WHERE is_visible = 1 GROUP BY categorie` — all 8 categories must return at least 1 row.
- [ ] **[NON-BLOCKING]** If an ISR rebuild was triggered after ingestion, `sitemap.xml` reflects the new items.
- [ ] **[BLOCKER]** No new items have null or empty required fields. Query: `SELECT id FROM news_items WHERE published_at > (SELECT started_at FROM ingestion_runs ORDER BY started_at DESC LIMIT 1) AND (titre_fr IS NULL OR resume_fr IS NULL OR categorie IS NULL OR priorite IS NULL OR source_url IS NULL)`
- [ ] **[BLOCKER]** Slug integrity: request the detail page for 1–2 newly ingested items and confirm HTTP 200 is returned.

---

## Regression Risk Reference

The following table lists the highest-risk regression areas. After any code change touching the listed trigger area, run the corresponding verification before merge.

| Risk | Trigger | What to verify |
|---|---|---|
| Slug stability | Change to `generateSlug()` | Re-ingest a known sample of items, confirm all slugs are byte-for-byte identical to pre-change values. Any slug change is a BLOCKER (causes 404 on indexed pages). |
| Category mapping | Change to `CATEGORIE_SLUG_MAP` | Load all 8 category pages and confirm each returns HTTP 200 and shows only items for that category. |
| `is_visible` filter | Change to the API query layer | Confirm no item with `is_visible = 0` appears on any public page (homepage, category pages, item pages). |
| Meta description uniqueness | Change to the `PageSEO` template | Spot-check 5 item pages: each must have a distinct `<meta name="description">` value derived from `resume_fr`. |
| `fallback: "blocking"` | Next.js config change | Request a page for a slug that does not exist in the static build; confirm HTTP 200 is returned, not HTTP 404. Any change here is a BLOCKER. |
| Deduplication | Change to ingestion fetch or hash logic | Run the ingestion cycle twice against the same feeds; confirm `items_new = 0` on the second run. |
| Date field swap | Change to the query layer or API serialiser | Confirm that `original_pub_date` is displayed in the article byline and `published_at` is used as the sort key. These must not be transposed (ADR 007, constraint C6). |

---

## Tools and Commands

### Ingestion log check

Query the SQLite database directly:

```bash
sqlite3 path/to/database.db "SELECT id, started_at, ended_at, items_fetched, items_new, items_stored, errors FROM ingestion_runs ORDER BY started_at DESC LIMIT 5;"
```

### HTTP status checks

Spot-check a URL with curl:

```bash
curl -o /dev/null -s -w "%{http_code}" https://example.com/news/slug/
```

To batch-check multiple URLs, write a short Node.js script using `node-fetch` or the built-in `fetch` (Node 18+) that reads a list of paths and reports any non-200 status.

### Lighthouse

Lighthouse CLI (install via npm):

```bash
npm install -g lighthouse
lighthouse https://staging.example.com/ --preset=perf --form-factor=mobile --output=html --output-path=./lighthouse-report.html
```

Alternatively use PageSpeed Insights without local installation: https://pagespeed.web.dev/

### Structured data validation

Use the schema.org Rich Results Test: https://search.google.com/test/rich-results

Paste a news item page URL or the raw HTML to validate `NewsArticle` JSON-LD.

### Accessibility

- Browser extension: axe DevTools (Chrome and Firefox) — run on each page type and review violations.
- Built-in: Chrome DevTools Accessibility panel for color contrast and ARIA tree inspection.

### Link checker

Use `broken-link-checker` (Node.js, install locally):

```bash
npx broken-link-checker https://staging.example.com/ --recursive --ordered
```

Alternatively use the W3C Link Checker online at https://validator.w3.org/checklink for smaller sites.

### Screen reader testing

- Windows: NVDA (free) + Firefox. Download at https://www.nvaccess.org/
- macOS / iOS: VoiceOver built into the OS. Enable with Command + F5 on macOS or triple-click the side button on iOS.
- Test sequence: navigate by headings (H key in NVDA), read a full news item, activate the source link, navigate pagination.

---

## What to Defer to v2

The following validation approaches are explicitly out of scope for the MVP and should be planned for a later iteration:

- **Automated end-to-end test suite** (Playwright or Cypress) — manual validation is sufficient for MVP volume.
- **Load testing** — the site is statically rendered with ISR; load risk is low at MVP scale.
- **Full accessibility audit by a specialist** — axe DevTools and the Phase 4 manual checks meet the MVP bar; a specialist audit is recommended before significant traffic growth.
- **Automated visual regression testing** (Percy, Chromatic, or similar) — not justified at MVP stage.
- **RSS feed output validation** — not applicable to MVP (the site consumes feeds; it does not produce one).
