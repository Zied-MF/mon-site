# ADR 005 — Frontend Architecture

## Status
Accepted — MVP

## Framework and Rendering
**Next.js 14 — Pages Router — Incremental Static Regeneration (ISR)**

- All pages statically generated and revalidated every 12 hours (`revalidate: 43200`)
- Matches 12-hour content cycle exactly with zero infrastructure orchestration
- Static HTML from CDN on first load — no client-side JS needed for content
- Pages Router chosen over App Router for lower complexity at MVP

| Page | Strategy | Revalidate |
|---|---|---|
| Homepage `/` | ISR getStaticProps | 43200s |
| Category `/categories/[slug]` | ISR getStaticProps + getStaticPaths | 43200s |
| News item `/news/[slug]` | ISR getStaticProps + getStaticPaths | 43200s |

## CSS
**Tailwind CSS** — utility-first, minimal bundle (PurgeCSS built in), no runtime CSS-in-JS. CSS custom properties for theme tokens in `styles/globals.css`.

## Component Structure

```
pages/
  index.tsx
  categories/[slug].tsx
  news/[slug].tsx

components/
  layout/
    Layout.tsx        — site shell
    Header.tsx        — logo + category nav
    Footer.tsx
  nav/
    CategoryNav.tsx   — horizontal scrollable category links
  news/
    NewsFeed.tsx      — ordered list of NewsCard, handles empty state
    NewsCard.tsx      — core reusable card component
    NewsCardBadges.tsx
    NewsDetail.tsx    — full item layout
    ImportanceSection.tsx — "Pourquoi c'est important" callout
  ui/
    Badge.tsx         — generic badge with variant prop
    ExternalLink.tsx  — with rel="noopener noreferrer"
    PriorityIndicator.tsx
    LanguageBadge.tsx
  seo/
    PageSEO.tsx       — wraps next/head
    NewsJsonLd.tsx    — NewsArticle JSON-LD

types/
  news.ts             — shared data contract

styles/
  globals.css
```

## Data Contract (types/news.ts)
```typescript
interface NewsItem {
  slug: string
  titre_fr: string
  resume_fr: string
  pourquoi_important: string
  categorie: "LLM" | "agents-ia" | "ia-generative" | "open-source" | "reglementation" | "startups" | "hardware" | "recherche"
  source_url: string
  source_name: string
  original_pub_date: string  // ISO 8601
  source_lang: "fr" | "en" | "de"
  priorite: "high" | "medium" | "low"
  is_visible: 1  // only visible items served
}
```

## Pagination
**Static pagination** (not infinite scroll). Generates discrete URLs (`/page/2`, `/categories/llm/page/2`) for SEO crawlability. Chunk size: 20 items per page.

## Performance Targets
| Metric | Target |
|---|---|
| LCP | < 1.5s mobile (3G throttled) |
| CLS | 0 |
| TTFB | < 200ms (CDN cache hit) |
| Total page weight | < 150 KB compressed |

Strategy: ISR + CDN, system font stack or next/font, no images in MVP data model, no third-party analytics scripts (use Plausible if needed with `strategy="afterInteractive"`).

## Accessibility
WCAG 2.1 Level AA. Key requirements:
- `<html lang="fr">` on all pages
- Skip navigation link ("Aller au contenu")
- `<nav aria-label="Catégories">` for category nav
- `<time datetime="ISO">` for all dates
- 4.5:1 contrast on all text
- Keyboard-navigable, visible focus rings
- Semantic HTML: nav, main, article, header, footer

## V2 Deferrals
Search, dark mode, newsletter subscription, bookmarks, PWA, RSS feed page, social sharing, infinite scroll, analytics dashboard.
