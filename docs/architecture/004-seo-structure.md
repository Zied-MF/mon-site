# ADR 004 — SEO Structure

## Status
Accepted — MVP

## URL Structure

| Page type | Pattern |
|---|---|
| Homepage | `/` |
| Category | `/categories/{slug}/` |
| News item | `/news/{slug}/` |

### Category slugs
| Display name | URL slug |
|---|---|
| LLM | llm |
| Agents IA | agents-ia |
| IA générative | ia-generative |
| Open source | open-source |
| Réglementation | reglementation |
| Startups | startups |
| Hardware | hardware |
| Recherche | recherche |

### News item slug format
`{keywords-from-headline}-{month}-{year}`
Example: `/news/google-gemini-2-ultra-annonce-mars-2026/`

## Metadata Templates

### Homepage
- Title: `Actu IA en français - Résumés et veille intelligence artificielle | [Site]`
- Meta: `Veille IA en français : résumés éditorialisés des actualités IA publiées toutes les 12 heures. LLM, agents, startups, réglementation et plus.`
- H1: `L'actualité intelligence artificielle en français`

### Category pages
- Title: `[Category name] - Actualités IA en français | [Site]`
- Meta: Unique per category, ~145 chars, includes 1–2 specific topic signals
- H1: `[Category name] : actualités IA en français`

### News item pages
- Title: `[French editorial headline] | [Site]` (max 65 chars)
- Meta: Generated from first 1–2 sentences of résumé_fr (must be unique per item)
- H1: French editorial headline (same as titre_fr)

## Structured Data (schema.org)
- Homepage: `WebSite` with name, url, description, inLanguage: "fr"
- Category pages: `CollectionPage`
- News item pages: `NewsArticle` with headline, datePublished, inLanguage: "fr", author as Organization (the editorial site), publisher with logo, canonical URL

## Internal Linking
- All 8 category links in HTML navigation (not JS-only)
- Breadcrumb on category and item pages: Accueil > [Categorie] > [Titre]
- "Actualités liées" block: 2–4 links to same-category items
- Outbound source links: standard anchor, no rel="nofollow" (natural citation), rel="noopener"

## Canonical Strategy
Each news item page is self-canonical. Never set canonical to the source URL.

## Sitemap and Robots
- `/sitemap.xml`: homepage (1.0), categories (0.8), news items (0.6)
- `/robots.txt`: Allow all, declare sitemap
- Update sitemap after each ingestion run or rebuild
- Submit to Google Search Console at launch

## Critical Risk
**Duplicate meta descriptions.** If news item meta is generated from a template rather than from résumé_fr, Google will reduce crawl priority site-wide. Every news item must have a unique meta description drawn from its résumé_fr content.
