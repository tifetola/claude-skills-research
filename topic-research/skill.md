# Topic Research (Master Pipeline)

Full SEO topic research pipeline — from account understanding to a prioritized content roadmap ready for briefing. Runs seven phases in sequence: business context ingestion, opportunity extraction, keyword validation, SERP qualification, business fit filtering, topic shaping, and prioritization.

Every phase can also be run independently as its own skill.

## Invocation

`/topic-research <client-domain>`

Example: `/topic-research risevision.com`

---

## Step 0 — Setup

Ask the user these questions all at once before doing anything else:

1. **What is the primary commercial goal for this engagement?** (e.g., "rank for competitor alternatives in enterprise manufacturing", "drive more trials from schools", "own the digital signage content space before a competitor does")
2. **What materials do you have?** Tell me what's available — I'll guide you through sharing each one:
   - SEO onboarding form
   - Sales or discovery call transcripts
   - Client Slack or email threads
   - Competitor list the client has named
   - Existing content inventory or sitemap URL
   - Product or positioning docs
   - Any ICP preferences or exclusions the client stated explicitly
3. **Output file path:** Where should the final topic plan be saved? Default: `./topic-plan-[domain]-[date].md`

Wait for all answers before proceeding.

---

## Phase 1 — Business Context Ingestion

Build a complete Account Intelligence Document. The rest of the pipeline depends on this. Proceeding without it produces the wrong opportunities, wrong keywords, and wrong priorities.

### 1A. Website Intelligence (run all in parallel)

Use WebFetch:

**Homepage** — fetch `https://<domain>`. Extract: what the product is in plain English, core value proposition, who it's for (roles, company sizes, industries named), core capabilities, navigation structure, any pricing signals, customer logos or case study references.

**Key internal pages** — pick from navigation and run in parallel:
- `/features` or `/product` — exact feature names as the product calls them
- `/pricing` — plan tiers, pricing model, what's in each tier
- `/solutions` or `/use-cases` — named ICP segments and use cases
- `/customers` or `/case-studies` — who buys, what outcomes they achieve
- `/about` — company stage, size, positioning context
- `/integrations` — which tools they connect with
- `/blog` or `/resources` — what content themes they've already published

From all pages, extract specifically: exact product and feature names (not paraphrases), exact customer segments named on the site, the language the product uses for its own outcomes and categories, any differentiation claims.

### 1B. Ahrefs Site Intelligence (run in parallel with 1A)

- `site-explorer-metrics` — DR, organic traffic, organic keyword count
- `site-explorer-top-pages` (limit: 20) — top pages by traffic to understand what already works
- `site-explorer-organic-keywords` (limit: 50) — current rankings, especially positions 1–20
- `site-explorer-organic-competitors` (limit: 10) — top organic competitors

### 1C. GSC Data (if available)
- `gsc-keywords` (limit: 50) — what queries are already sending traffic
- `gsc-pages` (limit: 20) — which pages are performing

### 1D. Process User-Provided Materials

Process whatever was shared in Step 0.

**From call transcripts and onboarding forms, extract:**
- What the client said they want to rank for
- What the client said they want to avoid
- Which competitors came up by name — in what context
- Which product features were emphasized
- Which customer segments were called out explicitly
- Objections that came up in sales
- Any conversions or content wins mentioned
- The client's actual business goal beyond "more traffic"

**From Slack and comms:** any stated priorities, frustrations, or preferences that reveal what the client values.

**From positioning docs:** how they position against competitors, which features they lead with, what customer outcomes they promise, any segments explicitly pursued or avoided.

### Checkpoint 1

Print the full Account Intelligence Document:

```
## Account Intelligence — [client domain]
Date: [today's date]

### What They Sell
[Plain-English description. What it does. Who uses it.]

### Commercial Priorities
[What the client wants to sell more of — specific products, plans, ICPs, or verticals. Distinct from what the website says.]

### ICP (Ideal Customer Profile)
[Specific roles, company sizes, industries. Distinguish who they currently sell to from who they want to sell to.]

### Features and Differentiators That Matter Commercially
[Commercially significant product capabilities. Use exact product names.]

### Competitor Landscape
[Which competitors come up most. In what context — calls, Ahrefs data, or both.]

### Objections and Friction Points
[Sales hesitations. These are content opportunities.]

### What Already Works
[Top pages, ranking strengths, content the client flagged as performing.]

### What They Want to Avoid
[Segments, topics, or positioning they've explicitly ruled out.]

### Current Organic Authority
- Domain Rating: [X]
- Monthly organic traffic: [X]
- Organic keywords: [X]
- Top organic competitors: [list]
- Ranking strengths: [topic clusters or pages working]

### Gaps and Flags
[What's missing. What would change recommendations if known.]
```

Ask: "Does this account picture look right? Anything wrong, missing, or to add before I extract opportunities?"

Wait for confirmation or edits before proceeding to Phase 2.

---

## Phase 2 — Opportunity Extraction

Turn account intelligence into structured content opportunity angles. Not keywords yet — business angles. Seven types to check.

### 2A. Scan for Signals

Review the Account Intelligence Document and identify signals across all seven opportunity types:

**Competitor signals:** Which competitors were named in calls or comms? Which appear in Ahrefs competitors? Which does the client specifically fear or want to displace?

**Objection signals:** What hesitations or friction came up in sales? Implementation, cost, switching, migration concerns?

**Feature and differentiator signals:** Which features are commercially significant — gating higher plans, named in calls, emphasized in positioning?

**Use-case signals:** What jobs do customers actually hire this product for? Which use cases came up repeatedly?

**Industry and vertical signals:** Which verticals were explicitly called out? Which do they want to enter?

**Pain and problem signals:** What language did customers use to describe their situation before finding the product? What would they search before knowing the product exists?

**Existing winner signals:** Which pages already drive traffic and pipeline? What adjacent opportunities do they create?

### 2B. Form Opportunities

For each signal cluster, form one or more content opportunities. Format each as:

```
### [Opportunity Name]
Type: [Competitor-led | Objection-led | Feature-led | Use-case-led | Industry-led | Pain-led | Winner expansion]
Signal: [The specific evidence — a named competitor, a quoted objection, a product feature, a named ICP segment]
Opportunity: [One sentence — what is this content for?]
Commercial value: [Why does this help pipeline, not traffic?]
Likely formats: [What kind of page(s) this probably becomes]
```

### 2C. Check Coverage

After forming all opportunities: are stated commercial priorities represented? Is anything over-represented? Does anything contradict the client's exclusions? Remove it.

### 2D. Pre-Validation Priority

Assign High / Medium / Low to each opportunity before keyword validation:
- **High:** Maps directly to a stated commercial priority AND is supported by a named signal from calls or comms
- **Medium:** Maps to a plausible commercial priority but inferred rather than explicitly stated
- **Low:** Plausible but weak signal — speculative or requires ICP assumptions

### Checkpoint 2

Print the full Opportunity Inventory. Ask: "Here are the opportunity angles I'm seeing. Any you want to add, remove, or reprioritize before I validate against search demand?"

Wait for confirmation. Apply changes. Then proceed to Phase 3.

---

## Phase 3 — Keyword Validation

Validate every opportunity against real search demand using Ahrefs. This is where Ahrefs comes in — not to dictate strategy, but to confirm opportunities map to actual search behavior.

Work through opportunities in priority order (High → Medium → Low). Run Ahrefs calls in parallel wherever possible.

**For each opportunity, run in parallel:**

- `keywords-explorer-matching-terms` — 2–3 seeds representing the core angle
- `keywords-explorer-related-terms` — surface adjacent keywords from the top seed
- `keywords-explorer-overview` — direct validation for specific keywords you're confident about
- `site-explorer-organic-keywords` for named competitor domains (competitor-led opportunities only, limit: 30) — what are they ranking for that the client isn't?

**Seed patterns by type:**

| Type | Seed Patterns |
|---|---|
| Competitor-led | `[competitor] alternatives`, `alternatives to [competitor]`, `[client] vs [competitor]`, `best [competitor] alternatives` |
| Objection-led | `[product category] migration`, `switching from [competitor]`, `[product category] implementation`, `[pain] solution` |
| Feature-led | `[feature name] software`, `software with [feature]`, `[feature] for [ICP]` |
| Use-case-led | `[product] for [use case]`, `[use case] software`, `best [product category] for [use case]` |
| Industry-led | `[product category] for [industry]`, `[industry] [product category] software` |
| Pain-led | `how to [solve the pain]`, `[pain noun] solution`, `[problem description] tool` |
| Winner expansion | `[existing top keyword] [adjacent modifier]`, `[topic cluster] [related subtopic]` |

**Filter candidates:**
- Include: global volume ≥ 50/mo (≥ 20/mo for named competitor alternatives — high-intent at any volume)
- Include: intent is clearly commercial or transactional, or informational but on the buyer journey
- Exclude: volume is 0, navigational intent, too broad for the ICP, client already ranks positions 1–5
- Flag: 0 volume but strategically important (new competitor, emerging feature term); KD 70+ but commercially critical

Print a summary (total candidates by opportunity type, top 10 by volume, low-volume high-intent calls, any opportunities with no searchable demand). Then print the full candidate pool grouped by opportunity.

---

## Phase 4 — SERP Qualification

Check what actually ranks. Many topics die here. That's the point.

For each keyword in the candidate pool, run in parallel:

- `serp-overview` — top 10 ranking pages, their DR, URL type, traffic
- `WebSearch` for the exact keyword — SERP feature type, result types, top 3 organic titles
- `WebFetch` the #1 and #2 results (selective — skip if intent and format are obvious from titles alone) — extract H1, H2 structure, content format, approximate depth

**For each keyword, answer:**

1. Is intent clear and consistent, or mixed?
2. What content format does Google reward for this keyword?
3. Does the client's product category fit the SERP?
4. How competitive is it, realistically — average DR of top 5, any beatable positions, any DR gaps?
5. Does the rewarded format match what the client can produce?
6. Are there freshness signals — stable rankings with no recent updates, or an actively contested SERP?

**Assign a verdict:**
- **QUALIFY** — clear intent, beatable competition, format fits, product category fits
- **CONDITIONAL** — good opportunity but one specific concern — state it explicitly
- **KILL** — mixed intent, wrong category dominates, ultra-high-DR lock with no gaps

Print a summary (counts per verdict, top 10 qualifying keywords, most common kill reasons). Then print the full list with verdicts and SERP format notes.

---

## Phase 5 — Business Fit Filter

Apply the commercial filter. Every surviving keyword passes five gates.

For each QUALIFY and CONDITIONAL keyword:

**Gate 1 — Pipeline, not traffic:** Will any of these readers plausibly convert? Does the product solve their problem? Is there a purchase path?

**Gate 2 — ICP match:** Does the implied searcher match the priority ICP on at least two dimensions: role, industry, company size, or problem type?

**Gate 3 — Product fit:** Can the client's product genuinely solve the problem implied by this keyword — without stretching or misrepresenting what it does?

**Gate 4 — Client alignment:** Would the client recognize this as a sound recommendation? Cross-reference the "what to avoid" section from Phase 1. Any conflict with stated positioning gets cut or flagged.

**Gate 5 — Net new vs. update vs. cannibalization:** Check against existing content inventory and current Ahrefs rankings. Assign: `NEW` / `UPDATE` / `CANNIBALIZES — RESOLVE`

Print filter results (keywords entering, passing, fail breakdown by gate, net new vs. update vs. cannibalization split). Then the full passing list with gate verdict, new/update/cannibalizes designation, and notes on any conditional passes. Plus the full cut list with one-line fail reasons.

---

## Phase 6 — Topic Shaping

Assign the correct content format to every surviving topic. Format drives everything downstream — structure, CTA, ranking mechanics, design requirements.

Reference SERP format notes from Phase 4 for each keyword.

**Format options:**
- Alternatives page, comparison page, category landing page, vertical landing page, feature page, use case page, pricing page, integration page, pain-solution article, implementation or migration guide, comparison hub, blog post or guide, update existing page

For each topic:
- **Chosen format** and one-sentence reason
- **Alternative rejected** — most tempting wrong choice and why it fails
- **Hybrid element** if needed
- **Cluster grouping** — if multiple keywords should target the same page, name the primary and secondaries
- **Architecture flag** if the format requires a new URL pattern, page type, or pillar structure

Print format breakdown summary, architecture flags, and the full shaped topic list.

---

## Phase 7 — Prioritization

Score every topic and produce a tiered, sequenced content roadmap.

**Six scoring dimensions (1–3 each):**

1. **Commercial intent** — 3: evaluation/decision mode (alternatives, comparison, pricing, feature); 2: mid-funnel (use case, pain-solution, implementation); 1: top-of-funnel/editorial
2. **ICP alignment** — 3: exact match to priority segment; 2: broad category match; 1: adjacency requiring assumptions
3. **Ranking feasibility** — 3: KD ≤ 40 + visible beatable gap; 2: KD 41–65 or competitive but angle advantage; 1: KD 66+ or locked SERP
4. **Content effort (inverted)** — 3: update or short LP under 1,200w; 2: new page 1,200–2,500w; 1: new long-form 2,500w+
5. **Existing authority leverage** — 3: already ranks in this cluster; 2: some related content, no strong signals; 1: completely new territory
6. **Adjacency to current wins** — 3: direct expansion of an existing winning page; 2: same broad category, different cluster; 1: standalone

**Priority Score = sum of all six dimensions.** Maximum: 18. Minimum: 6.

**Tiers:**
- Tier 1 (build first): ≥ 14
- Tier 2 (build next): 10–13
- Tier 3 (backlog): ≤ 9

**Sequencing within Tier 1:**
1. Quick wins first — low KD + existing authority
2. Highest commercial intent second — alternatives pages, comparisons for named competitors
3. Cluster batches third — build related pages together for compound authority
4. Long bets last within tier

Override: if the Account Intelligence Document records specific client sequencing preferences, honor them above the score order — note the trade-off explicitly.

---

## Phase 8 — Final Output

Compile and save the complete Topic Research Report to the path confirmed in Step 0.

```markdown
# Topic Research Report — [client domain]
Date: [date]

## Account Intelligence
[Full Phase 1 output]

## Opportunity Inventory
[Full Phase 2 output]

## Keyword Validation Summary
[Phase 3 summary + full candidate pool]

## SERP Qualification Summary
[Phase 4 summary + verdict list]

## Business Fit Filter Summary
[Phase 5 summary + passing list + cut list]

## Topic Shaping Summary
[Phase 6 format breakdown + shaped topic list]

## Prioritized Topic Plan
[Full Phase 7 output — this is the primary deliverable]

---

## Methodology Notes
- Keyword data: Ahrefs MCP (global volume unless otherwise noted)
- SERP data: Ahrefs serp-overview + live WebSearch
- Account intelligence sources: client website, [list materials provided]
- Date of data: [date]
```

After saving, print:
```
Topic plan saved to [path] — [N] topics — Tier 1: [N] | Tier 2: [N] | Tier 3: [N]
```

---

## Pipeline Rules

- **Never skip Phase 1.** Keyword research without account context produces the wrong keywords. Every phase downstream uses the Account Intelligence Document as its foundation.
- **Phase 1 checkpoint is required.** The account picture must be confirmed before opportunity extraction begins. Wrong account understanding compounds across all seven phases.
- **Phase 2 checkpoint is required.** The Opportunity Inventory shapes all keyword validation. Proceeding without user confirmation means Ahrefs calls are pointed at the wrong targets.
- **Ahrefs validates — it doesn't dictate strategy.** Opportunities come from account intelligence in Phase 2. Phase 3 confirms whether those opportunities have search demand. Volume doesn't create the strategy; the account context does.
- **Kill freely in Phase 4.** A smaller, well-qualified list entering Phase 5 produces a better roadmap than a bloated list that passes everything. SERP Qualification exists to eliminate bad targets.
- **Phase 5 is the commercial gate.** Traffic is not the goal. Pipeline from the right ICP is the goal. Every filter exists to enforce this. A keyword that fails the ICP test doesn't belong in the plan regardless of volume.
- **Format shapes everything downstream.** A wrong format decision in Phase 6 sends a well-researched topic into the wrong brief, the wrong page structure, and the wrong CTA. Take the time to get it right.
- **Never invent data.** All volume and KD figures must come from Ahrefs API calls. All product facts must come from the client's live website or user-provided materials. No fabrication.
