# ADR 007 — Shared Data Contract: Backend Pipeline to Frontend

## Status
Accepted — MVP

## Context

The ingestion pipeline (Node.js, SQLite, ADR 002) and the frontend (Next.js 14 ISR, ADR 005) are two separate runtime environments. This document is the authoritative source of truth for all types, field constraints, API shapes, and data flow rules that connect them. It supersedes the informal type definition in ADR 005 on any point of conflict.

Both engineers must implement against this document. Any future change to the contract requires updating this document first.

---

## 1. Purpose and Scope

This document defines:

- All TypeScript types shared between backend and frontend
- The full field specification for every field in `NewsItem`
- Slug generation rules (stability requirement)
- The canonical categorie mapping (DB display value → URL slug → French label)
- API response shapes exposed by the backend
- Frontend `getStaticProps` data fetching contracts per page type
- Validation rules enforced at ingestion time
- Constraints and invariants both sides must honor
- What is explicitly excluded from this contract

This document is for: backend engineer, frontend engineer, QA engineer.

---

## 2. TypeScript Types

The following types are the source of truth. The file `types/news.ts` in the frontend must match these definitions exactly. The backend uses the same definitions for its query output layer.

```typescript
// ─── Enums ──────────────────────────────────────────────────────────────────

/**
 * Categorie — display values stored verbatim in the DB `categorie` column.
 * These are the canonical French display names.
 */
export type Categorie =
  | "LLM"
  | "Agents IA"
  | "IA générative"
  | "Open source"
  | "Réglementation"
  | "Startups"
  | "Hardware"
  | "Recherche";

/**
 * CategorieSlug — URL-safe slug values used in routing and API query params.
 * Derived from Categorie via CATEGORIE_SLUG_MAP.
 * Used in: /categories/{slug}/, API ?categorie={slug}
 */
export type CategorieSlug =
  | "llm"
  | "agents-ia"
  | "ia-generative"
  | "open-source"
  | "reglementation"
  | "startups"
  | "hardware"
  | "recherche";

export type SourceLang = "fr" | "en" | "de";

export type Priorite = "high" | "medium" | "low";

// ─── Categorie Mapping ───────────────────────────────────────────────────────

/**
 * CATEGORIE_SLUG_MAP — maps DB display values to URL slugs.
 *
 * Both backend and frontend must use this map. The backend uses it to derive
 * the slug field at query time. The frontend uses it for routing and display.
 *
 * This object is the single source of truth for the display ↔ slug mapping.
 * It can be imported into both environments (shared module or duplicated with
 * a comment referencing this ADR).
 */
export const CATEGORIE_SLUG_MAP: Record<Categorie, CategorieSlug> = {
  "LLM": "llm",
  "Agents IA": "agents-ia",
  "IA générative": "ia-generative",
  "Open source": "open-source",
  "Réglementation": "reglementation",
  "Startups": "startups",
  "Hardware": "hardware",
  "Recherche": "recherche",
} as const;

/**
 * SLUG_CATEGORIE_MAP — reverse map, for lookup by slug.
 * Used by the frontend to resolve a URL slug back to a display label.
 */
export const SLUG_CATEGORIE_MAP: Record<CategorieSlug, Categorie> =
  Object.fromEntries(
    Object.entries(CATEGORIE_SLUG_MAP).map(([k, v]) => [v, k])
  ) as Record<CategorieSlug, Categorie>;

// ─── Core News Types ─────────────────────────────────────────────────────────

/**
 * NewsItem — the primary frontend-consumed type.
 * Represents a single news item as returned by the backend API.
 *
 * This is the type returned by getStaticProps for the news detail page,
 * and the base type for NewsItemSummary and NewsItemDetail.
 *
 * Key design decisions:
 * - `categorie` carries the URL-safe slug (CategorieSlug), not the DB display
 *   value. The backend derives this at query time. The frontend never stores
 *   the DB display value.
 * - `is_visible` is always `true` in this type. The backend only serves
 *   visible items. The literal boolean replaces the DB INTEGER 0/1.
 * - `slug` is stable across ingestion runs (see Section 4).
 */
export interface NewsItem {
  /** Stable URL slug. Format: {keywords}-{month}-{year}-{id} */
  slug: string;

  /** French editorial headline. 60–100 characters. */
  titre_fr: string;

  /** French summary. 60–120 words. */
  resume_fr: string;

  /** Why it matters section. 40–80 words. */
  pourquoi_important: string;

  /** URL-safe category slug derived from DB display value at query time. */
  categorie: CategorieSlug;

  /** Direct URL to the original source article. */
  source_url: string;

  /** Human-readable name of the source publication. */
  source_name: string;

  /** Publication date from the original source. ISO 8601. */
  original_pub_date: string;

  /** Language of the original source article. */
  source_lang: SourceLang;

  /** Editorial priority level. */
  priorite: Priorite;

  /** Always true — backend filters out non-visible items before serving. */
  is_visible: true;

  /** Ingestion timestamp. ISO 8601. Used for feed ordering. */
  published_at: string;
}

/**
 * NewsItemSummary — card view subset.
 * Used on: homepage feed, category pages, "Actualités liées" blocks.
 * Excludes pourquoi_important to reduce payload on list endpoints.
 */
export type NewsItemSummary = Omit<NewsItem, "pourquoi_important">;

/**
 * NewsItemDetail — full item for the detail page.
 * Includes all NewsItem fields. Adds original_title for structured data
 * and internal tracing.
 */
export interface NewsItemDetail extends NewsItem {
  /** Original article title in the source language. Used for schema.org NewsArticle. */
  original_title: string;
}

// ─── Pagination ──────────────────────────────────────────────────────────────

/**
 * PaginatedFeed — used by the homepage and category page list endpoints.
 * Page size is fixed at 20 items (see ADR 005).
 */
export interface PaginatedFeed {
  items: NewsItemSummary[];
  /** Total number of visible items matching the query (for pagination UI). */
  total: number;
  /** Current page index, 1-based. */
  page: number;
  /** Number of items per page. Fixed at 20 for MVP. */
  pageSize: number;
  /** Total number of pages. Math.ceil(total / pageSize). */
  totalPages: number;
  /** Null if this is the last page. */
  nextPage: number | null;
  /** Null if this is the first page. */
  prevPage: number | null;
}

// ─── Backend-Only Types (documented for completeness) ────────────────────────

/**
 * IngestionRun — maps to the ingestion_runs DB table.
 * Backend-only. Not exposed via API. Documented here for operational clarity.
 */
export interface IngestionRun {
  id: number;
  started_at: string;      // ISO 8601
  ended_at: string | null; // ISO 8601, null if still running
  items_fetched: number;
  items_new: number;
  items_stored: number;
  /** Serialized JSON array of error objects: { feed: string, error: string }[] */
  errors: string;
}

/**
 * FeedSource — maps to the feed_sources DB table.
 * Backend-only. Not exposed via API in MVP.
 */
export interface FeedSource {
  id: number;
  name: string;
  url: string;
  lang: SourceLang;
  /** 1 = active, 0 = disabled. SQLite INTEGER. */
  is_active: 0 | 1;
}
```

---

## 3. Field-by-Field Specification

Every field present in `NewsItem` is specified below.

| Field | TypeScript type | DB column | Constraints (ADR 003) | SEO usage | Notes |
|---|---|---|---|---|---|
| `slug` | `string` | Derived at query time | Stable, unique, URL-safe | Used in `/news/{slug}/` URL and sitemap | Format: `{keywords}-{month}-{year}-{id}`. See Section 4. |
| `titre_fr` | `string` | `titre_fr` | 60–100 chars, no clickbait | H1 and `<title>` on item page | AI-generated, editorial review required before publish |
| `resume_fr` | `string` | `resume_fr` | 60–120 words | Meta description source (first 1–2 sentences) | Must be unique per item to avoid duplicate meta risk (ADR 004) |
| `pourquoi_important` | `string` | `pourquoi_important` | 40–80 words, 1–3 sentences | Not used in meta | Omitted from `NewsItemSummary` |
| `categorie` | `CategorieSlug` | `categorie` (display value) | Must be one of 8 values | Category page slug in breadcrumb and nav | Backend converts display → slug at query time |
| `source_url` | `string` | `source_url` | Must be a valid, reachable URL | Outbound link, `rel="noopener"`, no `nofollow` | Deduplication key in DB |
| `source_name` | `string` | `source_name` | Non-empty string | Displayed in source attribution | Human-readable publication name |
| `original_pub_date` | `string` | `original_pub_date` | ISO 8601 | `<time datetime="">` and `datePublished` in schema.org | The source's publication date, not the ingestion date |
| `source_lang` | `SourceLang` | `source_lang` | `fr` \| `en` \| `de` | Displayed as FR/EN/DE badge | |
| `priorite` | `Priorite` | `priorite` | `high` \| `medium` \| `low` | Not used in meta | Drives visual priority indicator on cards |
| `is_visible` | `true` | `is_visible` | DB: INTEGER 1 only | Not used in meta | Backend WHERE clause filters `is_visible = 1` before serving. Type reflects post-filter guarantee. |
| `published_at` | `string` | `published_at` | ISO 8601 | Not exposed in SEO markup | Ingestion timestamp. Used for feed ordering (DESC). Not the source publication date. |
| `original_title` | `string` (detail only) | `original_title` | Non-empty string | `schema.org NewsArticle` headline fallback | Only present in `NewsItemDetail`. Used internally for tracing. |

---

## 4. Slug Generation Rules

### Format

```
{keywords}-{month-fr}-{year}-{id}
```

Example: `google-gemini-2-ultra-annonce-mars-2026-42`

### Rules

1. **Keywords** are extracted from `titre_fr`. Take the first 5–7 significant words (stop words excluded). Normalize: lowercase, remove accents, replace spaces and punctuation with hyphens, strip leading/trailing hyphens.
2. **Month** is the French month name of `original_pub_date`, lowercased: `janvier`, `fevrier`, `mars`, `avril`, `mai`, `juin`, `juillet`, `aout`, `septembre`, `octobre`, `novembre`, `decembre`. No accents (URL-safe).
3. **Year** is the 4-digit year from `original_pub_date`.
4. **ID** is the DB `id` (INTEGER PK). This is the stability anchor.

### Stability requirement

The `id` suffix makes slugs stable regardless of titre_fr changes after initial ingestion. If `titre_fr` is corrected post-publication, the slug does not change. The backend must always re-derive the slug from the same formula at query time using the stored `id` and `original_pub_date`.

Do not persist the slug to the DB. Generate it on read. This avoids slug drift and migration complexity.

### Collision guarantee

Because `id` is a unique PK, two items with identical keyword+month+year prefixes will always produce distinct slugs. No additional collision check is required.

### Reference implementation (backend)

```typescript
function generateSlug(titre_fr: string, original_pub_date: string, id: number): string {
  const STOP_WORDS = new Set([
    "le", "la", "les", "un", "une", "des", "de", "du", "et", "en",
    "au", "aux", "ce", "que", "qui", "sur", "par", "pour", "avec",
    "dans", "est", "son", "sa", "ses", "il", "elle", "ils", "elles"
  ]);

  const FR_MONTHS: Record<number, string> = {
    1: "janvier", 2: "fevrier", 3: "mars", 4: "avril", 5: "mai", 6: "juin",
    7: "juillet", 8: "aout", 9: "septembre", 10: "octobre", 11: "novembre", 12: "decembre"
  };

  const keywords = titre_fr
    .toLowerCase()
    .normalize("NFD")
    .replace(/[\u0300-\u036f]/g, "")   // remove accents
    .replace(/[^a-z0-9\s]/g, " ")      // remove non-alphanumeric
    .split(/\s+/)
    .filter(w => w.length > 1 && !STOP_WORDS.has(w))
    .slice(0, 6)
    .join("-");

  const date = new Date(original_pub_date);
  const month = FR_MONTHS[date.getUTCMonth() + 1];
  const year = date.getUTCFullYear();

  return `${keywords}-${month}-${year}-${id}`;
}
```

---

## 5. Categorie Mapping

The DB stores the French display name. The API and frontend use the URL-safe slug. This table is the canonical mapping.

| DB value (`categorie`) | URL slug (`CategorieSlug`) | French display label (UI) |
|---|---|---|
| `LLM` | `llm` | LLM |
| `Agents IA` | `agents-ia` | Agents IA |
| `IA générative` | `ia-generative` | IA générative |
| `Open source` | `open-source` | Open source |
| `Réglementation` | `reglementation` | Réglementation |
| `Startups` | `startups` | Startups |
| `Hardware` | `hardware` | Hardware |
| `Recherche` | `recherche` | Recherche |

The backend applies the mapping at query time before serializing the response. The frontend never receives the DB display value. The frontend uses the slug for routing and the French display label for UI rendering (resolved via `SLUG_CATEGORIE_MAP`).

---

## 6. API Response Shapes

The backend exposes a local HTTP API consumed by Next.js `getStaticProps` at build and revalidation time. All responses return `Content-Type: application/json`. All list endpoints return only `is_visible = 1` items.

### 6.1 Feed list — homepage and category pages

```
GET /api/news?page={n}&pageSize=20
GET /api/news?categorie={CategorieSlug}&page={n}&pageSize=20
```

**Response:**

```typescript
// HTTP 200
PaginatedFeed

// HTTP 400 — invalid categorie slug or invalid page param
{
  error: "INVALID_PARAM",
  field: "categorie" | "page",
  message: string
}

// HTTP 500
{
  error: "INTERNAL_ERROR",
  message: string
}
```

Items are ordered by `published_at DESC` (most recent ingestion first), then by `priorite` (high > medium > low) as secondary sort within the same ingestion window.

### 6.2 Single item — news detail page

```
GET /api/news/{slug}
```

Slug is parsed: the backend extracts the numeric `id` from the trailing `-{id}` segment, queries by `id`, verifies `is_visible = 1`, and returns `NewsItemDetail`.

**Response:**

```typescript
// HTTP 200
NewsItemDetail

// HTTP 404 — item not found or not visible
{
  error: "NOT_FOUND"
}

// HTTP 400 — slug malformed (no trailing numeric id)
{
  error: "INVALID_SLUG"
}
```

### 6.3 Static paths — for getStaticPaths

```
GET /api/news/paths
```

Returns only the slugs needed for `getStaticPaths`. Lightweight.

**Response:**

```typescript
// HTTP 200
{
  slugs: string[]  // all slugs for is_visible = 1 items
}
```

### 6.4 Category paths — for getStaticPaths on category pages

No dedicated endpoint. The frontend derives all valid category slugs statically from `CATEGORIE_SLUG_MAP`. No API call needed.

---

## 7. Frontend Data Fetching Contract

### 7.1 Homepage (`pages/index.tsx`)

```typescript
export const getStaticProps: GetStaticProps = async () => {
  const data: PaginatedFeed = await fetchJson(`/api/news?page=1&pageSize=20`);
  return {
    props: { feed: data },
    revalidate: 43200,
  };
};
```

Props type: `{ feed: PaginatedFeed }`.

For paginated pages (`/page/[n]`), pass `?page={n}` and generate paths via `getStaticPaths` using `totalPages` from the page 1 response.

### 7.2 Category page (`pages/categories/[slug].tsx`)

```typescript
export const getStaticPaths: GetStaticPaths = async () => {
  const slugs = Object.values(CATEGORIE_SLUG_MAP);
  return {
    paths: slugs.map(slug => ({ params: { slug } })),
    fallback: false,
  };
};

export const getStaticProps: GetStaticProps = async ({ params }) => {
  const slug = params!.slug as CategorieSlug;
  const data: PaginatedFeed = await fetchJson(`/api/news?categorie=${slug}&page=1&pageSize=20`);
  return {
    props: { feed: data, categorieSlug: slug },
    revalidate: 43200,
  };
};
```

Props type: `{ feed: PaginatedFeed; categorieSlug: CategorieSlug }`.

The component resolves the French display label via `SLUG_CATEGORIE_MAP[categorieSlug]`.

### 7.3 News detail page (`pages/news/[slug].tsx`)

```typescript
export const getStaticPaths: GetStaticPaths = async () => {
  const { slugs } = await fetchJson<{ slugs: string[] }>(`/api/news/paths`);
  return {
    paths: slugs.map(slug => ({ params: { slug } })),
    fallback: "blocking",  // new items published between builds are revalidated on first hit
  };
};

export const getStaticProps: GetStaticProps = async ({ params }) => {
  const slug = params!.slug as string;
  const item: NewsItemDetail = await fetchJson(`/api/news/${slug}`);
  return {
    props: { item },
    revalidate: 43200,
  };
};
```

Props type: `{ item: NewsItemDetail }`.

`fallback: "blocking"` is required so that items ingested after the last build are served via ISR on first request, not a 404.

---

## 8. Constraints and Invariants

Both backend and frontend must enforce these rules unconditionally.

| # | Constraint | Owner |
|---|---|---|
| C1 | Only items where `is_visible = 1` are ever returned by any API endpoint. | Backend |
| C2 | `categorie` in API responses is always a `CategorieSlug`, never a DB display value. | Backend |
| C3 | `is_visible` in the `NewsItem` type is always the literal `true`. The backend enforces this by filtering before serving. | Backend |
| C4 | Slugs are generated deterministically from `titre_fr`, `original_pub_date`, and `id`. They are never persisted to the DB. | Backend |
| C5 | The `id` suffix in the slug is the authoritative lookup key. The backend ignores the keyword prefix when resolving a slug to a DB row. | Backend |
| C6 | `published_at` (ingestion time) is used for feed ordering. `original_pub_date` (source date) is used for display and schema.org markup. These two fields must never be swapped. | Both |
| C7 | `resume_fr` must be unique across all visible items. The frontend generates meta descriptions from it. If two items have identical résumé content, one must be hidden (`is_visible = 0`). | Backend (at validation) |
| C8 | All date fields (`original_pub_date`, `published_at`) are ISO 8601 strings. The frontend renders them with `<time datetime="{value}">`. No timestamps are formatted by the backend. | Both |
| C9 | The `pageSize` is fixed at 20. Neither side changes this without a contract update. | Both |
| C10 | `fallback: "blocking"` on the news detail page is required. Do not change to `false`. | Frontend |

---

## 9. Validation Rules

The backend enforces these rules at ingestion time, before writing to the DB. Items that fail validation are skipped and logged to `ingestion_runs.errors`.

| Field | Rule | On failure |
|---|---|---|
| `titre_fr` | Length between 60 and 100 characters (inclusive) | Log warning, skip item |
| `resume_fr` | Word count between 60 and 120 | Log warning, skip item |
| `pourquoi_important` | Word count between 40 and 80 | Log warning, skip item |
| `categorie` | Must be one of the 8 valid `Categorie` display values | Apply fallback `"Recherche"`, log warning |
| `priorite` | Must be `"high"`, `"medium"`, or `"low"` | Apply fallback `"low"`, log warning |
| `source_lang` | Must be `"fr"`, `"en"`, or `"de"` | Skip item, log error |
| `source_url` | Must be a non-empty, syntactically valid URL | Skip item, log error |
| `original_pub_date` | Must be a parseable ISO 8601 date string | Skip item, log error |
| `original_title` | Must be non-empty | Skip item, log error |
| Duplicate `source_url` | Must not already exist in `news_items` | Skip silently (expected deduplication behavior) |
| `original_pub_date` age | Must not be older than 48 hours at ingestion time | Skip item, log info |

Word count is computed by splitting on whitespace and counting non-empty tokens.

---

## 10. What Is Not in This Contract

The following are explicitly out of scope for this document and this contract.

- **Authentication and authorization.** The API is internal, called only by Next.js at build/revalidation time, not by end users. No auth layer in MVP.
- **Admin or CMS API.** Editorial review and the `is_visible` toggle are out of scope for the data contract. They operate directly on the DB.
- **Search API.** Full-text search is a V2 deferral (ADR 005).
- **RSS feed endpoint.** V2 deferral.
- **User state.** No bookmarks, preferences, or session state in MVP.
- **Image fields.** No image assets in the MVP data model (ADR 005).
- **Author or byline fields.** V2 deferral (ADR 003).
- **Tags or sub-categories.** V2 deferral (ADR 003).
- **Site name and branding constants.** ADR 006 is unresolved. Site name does not appear in this contract.
- **Error page shapes.** HTTP error responses from the API are backend-internal during ISR. Next.js handles 404 and 500 pages independently.
- **Cache headers and CDN configuration.** Managed by the deployment environment, not this contract.

---

## Decisions and Rationale

### D1 — `categorie` is a `CategorieSlug` in API output, not the DB display value

The DB stores `"IA générative"`. The frontend needs `"ia-generative"` for routing and `"IA générative"` for display. If the API returned the display value, the frontend would need to apply the mapping at render time on every component that constructs a URL, creating duplication risk and potential inconsistency. The backend applies the mapping once at query time. The frontend receives the URL-safe value and resolves the display label via `SLUG_CATEGORIE_MAP` where needed.

### D2 — Slug is not persisted to the DB

Persisting slugs would require a migration whenever the generation formula changes. The `id` suffix guarantees uniqueness and stability. Generating at query time costs negligible compute and avoids a permanent schema dependency.

### D3 — `is_visible` is typed as the literal `true` in `NewsItem`

A backend filter guarantees that `is_visible = 1` before serving. The type reflects the post-filter state. This removes the need for frontend guards against `is_visible === 0`, which should never occur in production data.

### D4 — `pourquoi_important` is excluded from `NewsItemSummary`

List pages (homepage, category pages) do not render the importance section. Excluding it reduces the payload of list API responses. Detail pages use `NewsItemDetail` which extends `NewsItem` and therefore includes it.

### D5 — `original_title` is only in `NewsItemDetail`

The original title is needed for `schema.org NewsArticle` structured data on the detail page. It is not needed on card or list views. Limiting it to `NewsItemDetail` keeps `NewsItemSummary` lean.
