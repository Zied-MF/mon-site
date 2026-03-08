# ADR 002 — Backend Ingestion Pipeline

## Status
Accepted — MVP

## Context
The site ingests AI news from French, English, and German sources every 12 hours, generates French summaries via AI, classifies items by category and priority, and stores them for frontend consumption.

## Stack

| Layer | Tool |
|---|---|
| Runtime | Node.js 20+ (TypeScript) |
| Scheduler | node-cron (in-process) |
| RSS parsing | rss-parser |
| Content extraction | @mozilla/readability |
| AI (all tasks) | OpenAI gpt-4o-mini |
| Database | SQLite via better-sqlite3 |
| Query layer | Kysely |

## MVP Sources (10 feeds)

**French:** Le Monde Informatique IA, Journal du Net IA, 01net
**English:** The Verge AI, MIT Technology Review, Hugging Face Blog, VentureBeat AI, ArXiv cs.AI
**German:** Heise Online KI, t3n KI

## Ingestion Flow

```
[CRON — every 12 hours]
  → Fetch all RSS feeds in parallel (Promise.allSettled)
  → Deduplicate against stored source_url + discard items older than 48h
  → Extract article body with @mozilla/readability (fallback: RSS description)
  → Single AI call per item (gpt-4o-mini) → JSON: titre_fr, resume_fr, pourquoi_important, categorie, priorite
  → Validate enum fields
  → Persist to news_items
  → Write ingestion_runs log entry
```

## Data Schema

### news_items
| Column | Type | Notes |
|---|---|---|
| id | INTEGER PK | |
| source_url | TEXT UNIQUE | Deduplication key |
| source_name | TEXT | |
| source_lang | TEXT | fr/en/de |
| original_title | TEXT | |
| original_pub_date | TEXT | ISO 8601 |
| titre_fr | TEXT | AI-generated |
| resume_fr | TEXT | AI-generated |
| pourquoi_important | TEXT | AI-generated |
| categorie | TEXT | Enum |
| priorite | TEXT | high/medium/low |
| published_at | TEXT | ISO 8601, ingestion time |
| is_visible | INTEGER | Default 1, editorial toggle |

### ingestion_runs
| Column | Type |
|---|---|
| id | INTEGER PK |
| started_at | TEXT |
| ended_at | TEXT |
| items_fetched | INTEGER |
| items_new | INTEGER |
| items_stored | INTEGER |
| errors | TEXT (JSON) |

### feed_sources
| Column | Type |
|---|---|
| id | INTEGER PK |
| name | TEXT |
| url | TEXT UNIQUE |
| lang | TEXT |
| is_active | INTEGER |

## AI Prompt Strategy

Single call per article returns structured JSON. System prompt establishes French editorial voice. User prompt provides: source_lang, original_title, truncated content (max 3000 tokens). Prompt explicitly constrains enum values for category and priority.

## Error Handling
- Feed fetch failure → log, skip feed, continue
- Article page fetch failure → fall back to RSS description
- OpenAI error → retry once after 2s, then skip and log
- JSON parse failure → skip and log raw response
- Invalid enum → apply fallback (categorie: "recherche", priorite: "low"), log warning

## V2 Deferrals
PostgreSQL, Redis/job queue, fuzzy deduplication, editorial CMS, admin dashboard, multi-provider LLM fallback, full-text search, webhook alerting.
