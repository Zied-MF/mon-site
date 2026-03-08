# ADR 008 — Source List

**Status:** Accepted — MVP  
**Date:** 2026-03-08  
**Relates to:** ADR 002 (Ingestion Pipeline), ADR 007 (Data Contract)

---

## 1. Purpose and scope

This document is the canonical reference for all RSS/Atom feed sources monitored by the ingestion pipeline. It governs:

- which sources are included at MVP and why
- the criteria for adding or removing a source
- the operational rules for managing source state in the `feed_sources` table
- a list of candidates deferred to v2

**Who maintains this document:** the tech lead, in coordination with the editorial team. Any change to the active source list must be reflected here with a reason and a date.

**Relation to the ingestion pipeline (ADR 002):** the `feed_sources` table in the SQLite database is the runtime representation of this document. Each row in that table corresponds to one entry in this document. The fields `name`, `url`, `lang`, and `is_active` in `feed_sources` must match what is documented here. This document is the source of truth; the database row is the operational copy. Discrepancies between the two must be resolved in favor of this document after editorial review.

The pipeline reads only rows where `is_active = 1`. Setting `is_active = 0` is the safe, non-destructive way to pause ingestion from a source without removing any already-ingested items.

---

## 2. Inclusion criteria

A source must meet ALL of the following criteria to be eligible for inclusion.

1. Content is primarily or substantially about artificial intelligence, or the publication operates a dedicated AI vertical with consistent AI coverage and a separate RSS feed or filtered feed for that vertical.
2. The publication language is French (fr), English (en), or German (de), as required by the `source_lang` constraint defined in ADR 007. Content in other languages is not processed.
3. The publication exposes a working RSS 2.0 or Atom feed that includes, at minimum, article titles and publication dates. Feeds that return only headlines with no descriptive content are not eligible unless full-body extraction via `@mozilla/readability` reliably succeeds.
4. The publication produces at least one article per week on average over the preceding three months. Sources with irregular or seasonal publication patterns below this threshold are not eligible at MVP.
5. The publication is editorially credible and independent. This excludes spam sites, SEO content farms, pure content aggregators that republish others without editorial value, and press release distribution services.
6. Automated feed reading is not explicitly prohibited by the publication Terms of Service in a way that creates meaningful legal risk. Passive RSS/Atom feed reading is generally permissible; sources that actively block feed access or explicitly restrict automated consumption in their ToS should be escalated to legal review before inclusion.
7. The feed contains enough structured content — title plus at minimum a short description or summary — to allow the OpenAI `gpt-4o-mini` summarization step to produce a usable output even when full-body HTML extraction fails. A feed that returns only a title and no other content is not sufficient for the MVP summarization pipeline.