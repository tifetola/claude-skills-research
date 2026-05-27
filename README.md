# Claude Code Skills — Topic Research Pipeline

8 Claude Code custom skills for a complete SEO topic research pipeline. Client domain in, prioritized content roadmap out.

**Output:** A tiered, sequenced build queue of content topics — each validated against real keyword data, SERP competition, and commercial fit — ready to brief.

---

## Pipeline

```
/topic-research <client-domain>
       │
       ├── Phase 1: Business Context
       │       ├── Website crawl → product, ICP, pricing, features, differentiators
       │       └── Ahrefs site metrics → DR, top pages, organic keywords, competitors
       │                   ↓ [Approval checkpoint — confirm account picture]
       ├── Phase 2: Opportunity Extraction
       │       └── /opportunity-extraction → 7 types: competitor, objection, feature,
       │                                     use-case, industry, pain, winner expansion
       │                   ↓ [Approval checkpoint — confirm opportunity angles]
       ├── Phase 3: Keyword Validation
       │       └── /keyword-validation → Ahrefs matching + related terms, volume + KD
       │                                 per opportunity cluster
       ├── Phase 4: SERP Qualification
       │       └── /serp-qualification → live SERP check per candidate
       │                                 QUALIFY / CONDITIONAL / KILL verdicts
       │                                 (many topics die here — that's the point)
       ├── Phase 5: Business Fit Filter
       │       └── /business-fit-filter → 5 gates: pipeline alignment, ICP match,
       │                                  product fit, client alignment, cannibalization
       ├── Phase 6: Topic Shaping
       │       └── /topic-shaping → format per topic: alternatives page, comparison,
       │                            LP, pricing page, guide, use case, etc.
       └── Phase 7: Prioritization
               └── /topic-prioritization → 6-dimension scoring, 3-tier output,
                                           sequenced build queue
```

---

## Skills

| Phase | Skill | Command | What it does |
|---|---|---|---|
| **Master** | Topic Research | `/topic-research <domain>` | Runs all 7 phases. Account intelligence → prioritized roadmap. Two approval checkpoints built in. |
| Phase 1 | Business Context | `/business-context <domain>` | Account Intelligence Document: product, ICP, competitors, objections, commercial priorities, organic footprint. |
| Phase 2 | Opportunity Extraction | `/opportunity-extraction` | Extracts content opportunities from account intelligence. 7 types. Business angles — not keywords yet. |
| Phase 3 | Keyword Validation | `/keyword-validation` | Ahrefs validation: matching terms, related terms, volume + KD per opportunity. Ahrefs validates here — it doesn't set strategy. |
| Phase 4 | SERP Qualification | `/serp-qualification` | Checks what actually ranks. QUALIFY / CONDITIONAL / KILL per keyword. Many topics die here. That's the point. |
| Phase 5 | Business Fit Filter | `/business-fit-filter` | 5 commercial gates per keyword: pipeline alignment, ICP match, product fit, client alignment, cannibalization check. |
| Phase 6 | Topic Shaping | `/topic-shaping` | Assigns content format to every surviving topic. Flags architecture implications (URL patterns, pillar structures). |
| Phase 7 | Topic Prioritization | `/topic-prioritization` | 6-dimension scoring (1–3 each). Tier 1 ≥14, Tier 2 10–13, Tier 3 ≤9. Sequenced build queue. |

### Scoring dimensions (Phase 7)

| Dimension | 3 | 2 | 1 |
|---|---|---|---|
| Commercial intent | Alternatives / comparison / pricing | Use case / pain-solution | Editorial / top-of-funnel |
| ICP alignment | Exact priority segment | Broad category match | Adjacency requiring assumptions |
| Ranking feasibility | KD ≤ 40 + beatable gap | KD 41–65 or angle advantage | KD 66+ or locked SERP |
| Content effort (inverted) | Short LP < 1,200w | New page 1,200–2,500w | Long-form 2,500w+ |
| Existing authority | Already ranks in this cluster | Some related content | Completely new territory |
| Adjacency to wins | Direct expansion of winning page | Same category, different cluster | Standalone |

---

## Installation

```bash
# Clone alongside the content writing skills repo
git clone https://github.com/tifetola/claude-skills-research.git ~/claude-skills-research

# Symlink each skill into your Claude skills directory
cd ~/claude-skills-research
for skill in */; do
  ln -sf "$(pwd)/${skill%/}" ~/.claude/skills/
done
```

Skills are automatically detected by Claude Code from `~/.claude/skills/`. Invoke any skill by typing `/skill-name` in a Claude Code session.

> **Note:** Claude Code scans `~/.claude/skills/` one level deep. Symlinks are used so these repos can live in their own directories while still being discovered by Claude Code.

---

## Quick Start

```
/topic-research elacity.ai
/topic-research risevision.com
```

**Run a single phase:**
```
/business-context saas-company.com
/keyword-validation
/topic-prioritization
```

---

## Part of a two-repo set

This repo covers topic research. For the content writing pipeline (search intent → draft → QC → publication), see:

→ [claude-skills-content](https://github.com/tifetola/claude-skills-content)
