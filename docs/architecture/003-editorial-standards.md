# ADR 003 — Editorial Standards

## Status
Accepted — MVP

## Field Standards

### Titre
- 60–90 characters (max 100)
- Declarative French, present tense for recent events
- Identifies actor + action + consequence when space allows
- No clickbait, no superlatives ("révolutionnaire", "game-changer")
- Proper nouns and model names preserve original casing (GPT-4o, EU AI Act)

### Résumé
- 60–120 words (target 80–100)
- Three beats: what happened / who is involved / one concrete detail
- No copy-paste from source, no editorial opinion, no marketing language

### Pourquoi c'est important
- 40–80 words, 1–3 sentences
- Must answer at least one: what changes for practitioners / how it connects to a trend / implication for FR audience / what to watch next
- Editorial voice — not a second summary, not filler

### Priority levels
| Level | Criteria |
|---|---|
| high | ≥2 of: major model release, EU/FR regulatory decision, verified security incident, funding >100M EUR/USD, research with immediate practical consequence |
| medium | Default. Product updates, notable OSS, smaller funding, non-binding regulatory drafts, interesting research |
| low | Blog/marketing content, event announcements, repackaged news, commentary without news hook |

Priority inflation is the most common and most damaging editorial error. Default to medium when uncertain.

### Category assignment
| Category | Assign when |
|---|---|
| LLM | Model itself is the subject: release, eval, architecture, benchmark |
| Agents IA | Autonomous agents, tool use, multi-agent, agentic workflows |
| IA générative | Generation applied to image/audio/video/multimodal |
| Open source | Openly licensed model, dataset, or framework |
| Réglementation | Laws, regulations, enforcement, official policy on AI |
| Startups | Funding rounds, acquisitions, launches |
| Hardware | Chips, GPUs, TPUs, AI compute infrastructure |
| Recherche | Academic papers, institutional research |

One category per item. When in conflict: Reglementation beats LLM, Open source beats Startups when openness is the story.

### Source attribution
- Required: source name, direct URL, publication date (source's date), source language label (FR/EN/DE)
- Link to primary source, not secondary summary
- Note paywalled articles with "(article payant)"

## AI Generation vs. Editorial Review
AI drafts all fields. Editorial review is mandatory before publication for:
- Title (accuracy, not misleading)
- Résumé (no verbatim copy, no unsupported claims)
- "Pourquoi c'est important" (highest filler risk)
- Priority (most common error)
- Category (boundary cases)
- Source URL (direct, active, correct article)

## Publication Checklist
- [ ] Title in French, 60–100 characters
- [ ] Résumé 60–120 words, no copy-paste
- [ ] "Pourquoi" adds context not in résumé
- [ ] Category matches rules
- [ ] Priority meets threshold
- [ ] Source URL direct and active
- [ ] Source language label present
- [ ] Publication date is source's date
- [ ] No exclamation marks, rhetorical questions, filler phrases

## V2 Deferrals
Author bylines, sub-categories/tags, "breaking" priority tier, multiple source links per item, editorial queue/approval workflow, versioned correction log.
