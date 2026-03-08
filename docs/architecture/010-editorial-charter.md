# ADR 010 — Editorial Charter

## Status
Accepted — MVP

## Context

This charter is the editorial contract governing all content published on ce site. It applies equally to human editors and to AI-assisted generation. It is the reference document when a question arises about what to publish, how to write it, and why a particular standard exists.

It is also the source document for the public-facing "Méthode éditoriale" page. Sections marked **[public-adaptable]** are written to be lifted, lightly adapted, and published directly for readers.

The charter does not duplicate the field-level specifications in ADR 003. It provides the reasoning and principles behind those specifications.

---

## 1. What this charter is

Ce site publishes editorial content about artificial intelligence. Every item it publishes — a title, a summary, a "Pourquoi c'est important" — is a claim made to a reader. This charter defines what standards back that claim.

A reader who encounters an item on ce site should be able to answer three questions with confidence:

- Where does this information come from?
- Has someone applied judgment to it, or was it just aggregated?
- Is the summary accurate to the source?

This charter exists to make the answer to all three questions unambiguously yes.

---

## 2. Editorial mission **[public-adaptable]**

**English (internal reference):**

Ce site exists to make AI news readable and useful for a French-speaking audience. It monitors publications in French, English, and German, selects what matters, and publishes original French-language summaries — with clear attribution to every source. The goal is not to replace the sources. The goal is to help readers decide what is worth reading, then send them there.

**French (for direct use in public copy):**

Ce site existe pour rendre l'actualité de l'intelligence artificielle lisible et utile pour un public francophone. La rédaction surveille des publications en français, en anglais et en allemand, sélectionne ce qui compte, et publie des résumés originaux en français — avec attribution claire vers chaque source. L'objectif n'est pas de remplacer les sources. L'objectif est d'aider les lecteurs à identifier ce qui mérite d'être lu, puis de les y envoyer.

---

## 3. How ce site selects news

**Source coverage**

Ce site monitors publications in French, English, and German. Sources are refreshed every 12 hours. The source list is maintained in ADR 008. Sources are chosen for editorial quality, relevance to the AI field, and reliability of information — not for output volume.

**Selection criteria**

An item is selected if it is:
- directly relevant to the AI field (technology, policy, research, infrastructure, or business)
- significant enough that a French-speaking AI practitioner or informed observer would want to know about it
- reported by a reliable source, or verifiable against one

**What is not selected**

- Press releases published without independent corroboration
- Social media posts, regardless of who posted them
- Content that is off-topic or only tangentially related to AI
- Duplicate coverage of the same story from a lower-quality source when a better source has already been selected
- Marketing content repackaged as news

**Priority logic**

| Level | When it applies |
|---|---|
| high | At least two of: major model release, EU or French regulatory decision, verified security incident, funding above 100M EUR/USD, research with immediate practical consequence |
| medium | Default. Product updates, notable open-source releases, smaller funding rounds, non-binding regulatory drafts, significant research |
| low | Blog or marketing content, event announcements, repackaged news, commentary without a news hook |

Priority inflation is the most common editorial error. When uncertain, assign medium.

**Why 12 hours**

The 12-hour cadence is deliberate. Ce site reads widely so the reader does not have to. Each cycle is an editorial selection, not a real-time feed. Publishing less, more carefully, is the quality signal.

---

## 4. How summaries are written

**AI-assisted, editorially reviewed**

AI generates first drafts of all content fields. Editorial review is mandatory before publication. AI output is a starting point, not a finished product. The review covers accuracy, tone, verbatim copy detection, priority assignment, category accuracy, and source URL validity. See ADR 003 for the full publication checklist.

**The résumé is original content**

The résumé_fr is not a translation of the source article. It is an original synthesis written in French. The editor reads the source, identifies the essential information, and writes a summary that stands on its own — accurate to the source but not derived word-for-word from it. This is both a legal and an editorial requirement.

**Structure of a résumé**

Every résumé follows three beats:
1. What happened
2. Who is involved
3. One concrete detail that makes the item specific and credible

Target length is 60–120 words. Enough to inform. Not enough to substitute for reading the source.

**The "Pourquoi c'est important" section**

This section is the editorial layer that distinguishes a curated briefing from a link list. It adds perspective that is not already in the résumé: what this development changes for practitioners, how it connects to a larger trend, what it means for the French-speaking AI community, or what to watch next. It is 40–80 words. It must say something. Filler is not acceptable.

---

## 5. Tone and what is avoided

**The target register**

Direct, professional, and accessible to an informed non-specialist. A reader who knows what a large language model is but does not follow every benchmark paper. Technical terms are used correctly and without apology — LLM, RAG, fine-tuning, inference — but jargon is not layered on for effect.

**What is never acceptable**

- Sensationalist language: "révolutionnaire", "va tout changer", "incroyable", "game-changer"
- Exclamation marks in editorial copy
- Rhetorical questions used as substitutes for declarative statements
- Filler phrases that carry no information
- Press release language: if an item reads like a product announcement, it is rewritten or not published
- Vague claims: every sentence must carry information

**The test**

If a sentence can be removed without losing any meaning, it should be removed.

---

## 6. Source attribution **[public-adaptable]**

Every item published on ce site links directly to the original source article. The source name, publication date, and source language (FR, EN, or DE) are always displayed alongside the item.

Paywalled articles are labeled "(article payant)" so the reader knows before clicking.

Ce site's summaries are original content. They do not replace the source article. They help the reader decide whether the source article is worth their time — then the outbound link sends them there. The source link is a feature of every item, not a footnote.

No secondary summaries or aggregators are linked in place of the primary source. If the primary source is paywalled and no reliable free alternative exists, the item is still published with the paywall label.

---

## 7. What makes ce site editorially useful **[public-adaptable]**

Ce site offers three things a general news feed does not:

**Language coverage.** It monitors publications in French, English, and German. Most French-speaking readers do not have time to monitor international sources individually. Ce site does that work and publishes findings in French.

**Editorial judgment.** Items are assigned a priority level and a category. The priority level reflects structural significance, not novelty or traffic potential. A story that matters is labeled high priority even if it is less immediately exciting than a product announcement.

**Context, not just links.** Every item includes a "Pourquoi c'est important" section. This is the editorial layer: the reason this development matters, the trend it connects to, the implication worth tracking. A briefing without that layer is just a list of links.

The 12-hour cadence reinforces this. Each update is a deliberate editorial selection. Ce site does not optimize for volume.

---

## 8. Why ce site does not republish full articles

Two reasons, stated plainly.

**Legal.** Reproducing a full article without a license from the rights holder is copyright infringement. Attribution does not change this. Linking to the source does not change this. The law is clear, and ce site does not operate in a grey area on this point.

**Editorial.** The value ce site provides is synthesis and prioritization. A reader who wants the full article can follow the source link — it is always there, always direct. Republishing the full article would not add value. It would substitute for the source rather than directing the reader to it, which is the opposite of the editorial mission.

These two reasons reinforce each other. Ce site publishes summaries because that is what it is for, and because publishing more than that would be both legally indefensible and editorially pointless.

---

## 9. What this charter does not cover

The following are deferred to v2 or to a separate policy document:

- Monetization and advertising policy
- Comment moderation (no comments in MVP)
- Correction and retraction policy
- Author and contributor identity (no bylines in MVP)

These omissions are intentional. Introducing them prematurely would create obligations ce site cannot yet fulfill consistently.

---

## References

- ADR 001 — Product Brief
- ADR 003 — Editorial Standards (field-level specifications, publication checklist)
- ADR 004 — SEO Structure (canonical strategy, original content classification)
- ADR 006 — Site Name Decision (name TBD; use "ce site" and "la rédaction" until resolved)
- ADR 008 — Source List
