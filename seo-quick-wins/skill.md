# SEO Quick Wins

Find fast ranking-improvement opportunities from existing pages and real search data. This skill takes the client's Google Search Console data, identifies keywords in positions 4–15 that their pages rank for but aren't directly targeting, triangulates demand with Ahrefs, crawls competitor pages to benchmark content depth and structure, and produces a holistic action plan per page. Every claim is grounded in real data — GSC CSV, Ahrefs API, live page crawls, and competitor benchmarks. No invented figures.

## Invocation

`/seo-quick-wins <domain>`

Example: `/seo-quick-wins proposify.com`

---

## Step 0 — Setup

Ask the user the following questions all at once:

1. **GSC data** — Ask them to paste or attach their Google Search Console CSV export. Then explain how to get the most useful format:

   > **Best format:** In GSC → Search Results → set date range to last 3 months → Queries tab → Export CSV. This gives: query, clicks, impressions, CTR, position. If you can also export from the Pages tab, provide that too — two CSVs is fine.
   >
   > **Even better:** If you have a query+page breakdown (from GSC API, Looker Studio, or a third-party tool like Ahrefs/Screaming Frog), that unlocks deeper analysis. Paste it in any format — I'll handle the columns.
   >
   > **Minimum needed:** Any GSC export with position and impressions data.

2. **Any context?** Known priorities, pages they want to grow, verticals or competitor pages that matter most, anything to avoid.

3. **Output file path** — where should the report be saved? Default: `./quick-wins-[domain]-[date].md`

Wait for all answers before proceeding.

---

## Phase 1 — Parse and Classify the GSC Data

### 1A. Identify the CSV format

Inspect the columns provided:

| Format | Columns present | What it unlocks |
|---|---|---|
| **Queries** (standard export) | query, clicks, impressions, CTR, position | Keywords the site ranks for — need Ahrefs to identify which page ranks for each |
| **Pages** (standard export) | page, clicks, impressions, CTR, position | Pages that get traffic — need Ahrefs organic keywords per page to find ranking queries |
| **Query + Page** (API / Looker Studio) | query, page, clicks, impressions, CTR, position | Direct mapping — most powerful, no Ahrefs gap-fill needed for this step |

Note the format. If queries-only, flag that Ahrefs will be used to identify ranking pages in Phase 2. If pages-only, flag that Ahrefs will be used to retrieve keyword sets per page.

### 1B. Filter for the quick-win window

Apply these filters to the raw CSV:

- **Position: 4.0–15.0** — the sweet spot. Position ≤ 3 are already winning. Position > 15 need more authority, not optimization.
- **Impressions: ≥ 10/month** — removes noise from near-zero impression queries. Adjust to ≥ 5 if the site is small.
- **Exclude branded queries** — remove any query containing the client's brand name or product name. These are navigational, not opportunities.
- **Exclude navigational** — queries that are clearly a URL (contain `.com`, `login`, `sign in`, `app.`, etc.)

After filtering, count: total qualifying query rows, unique pages involved.

### 1C. Group by page

Group the filtered rows by page URL (or, if queries-only format, proceed to Phase 2A first to identify pages, then group).

For each page:
- List all qualifying keywords (position 4–15, impressions ≥ 10)
- Note the position range and total impressions for that page's keyword cluster
- Sort pages by **total impressions across all qualifying keywords** (descending) — this is a proxy for total opportunity on that page

Print a brief data summary before proceeding:

```
## GSC Data Summary

**CSV format detected:** [Queries / Pages / Query+Page]
**Date range:** [from CSV, if readable]
**Total qualifying keyword-page rows (pos 4–15, impressions ≥ 10):** N
**Unique pages with qualifying keywords:** N
**Top 5 pages by total qualifying impressions:**
1. [URL] — N qualifying keywords, N total impressions
2. ...
```

---

## Phase 2 — Identify Ranking Pages (if queries-only CSV)

If the CSV format is **queries-only** (no page column), run the following to identify which page ranks for each qualifying keyword:

**A. Ahrefs organic keyword lookup**
For each unique qualifying keyword (or in batches using `keywords-explorer-overview`), use `site-explorer-organic-keywords` filtered by domain to find the ranking page URL. Run in parallel for efficiency.

**B. Fallback: live SERP check**
For keywords where Ahrefs doesn't return a ranking page, use `WebSearch` with `site:<domain> [keyword]` to find the ranking page.

**Do not guess ranking pages.** If neither Ahrefs nor a live SERP check confirms a ranking page for a keyword, flag it as "page unconfirmed" and exclude it from per-page analysis. Count and report how many were excluded this way.

After this phase, every keyword in the candidate pool must have a confirmed page URL.

---

## Phase 3 — Segment Pages by Commercial Priority

Classify every unique ranking page URL into a commercial tier. This determines analysis order — bottom funnel first.

### Tier 1 — Revenue-driving pages (analyze first)

These pages are closest to the money. Quick wins here have the highest potential pipeline impact.

| Sub-type | URL signals | Content signals |
|---|---|---|
| **Competitor alternatives page** | `/alternatives`, `/vs-`, `/compare`, `/competitors` | H1 contains competitor names or "alternatives" |
| **Competitor comparison page** | `/vs/`, `/compare/[competitor]` | Head-to-head format |
| **Competitor review page** | `/reviews/`, `/[competitor]-review` | Single-competitor review format |
| **Product listicle / best-of page** | `/best-`, `/top-`, `/[category]-software` | Numbered list format, multiple products mentioned |
| **Pricing page** | `/pricing`, `/plans`, `/cost` | Pricing tiers visible |
| **Product or feature landing page** | `/features/`, `/product/`, `/solutions/`, `/platform/` | Single feature or capability focused |
| **Use case or industry landing page** | `/for-[use-case]`, `/[industry]`, `/use-cases/` | Specific ICP or vertical focused |

### Tier 2 — Commercial-intent blog or resource content

Blog posts or guide content that target buyers, not just readers.

- Topics: software comparisons, how-to guides for a job the product solves, buyer's guides, "what is X" posts where X is the product category, pain-point articles
- Signals: CTA to trial/demo present, product mentioned as solution, competitor names appear

### Tier 3 — Informational content

Top-of-funnel content: industry news, general how-tos, educational explainers. Still worth optimizing, but lowest priority — wins here bring traffic that may not convert.

---

## Phase 4 — Crawl and Analyze Each Page

Work through pages in Tier 1 order first, then Tier 2, then Tier 3. Run WebFetch calls in parallel across pages within the same tier.

For each page, fetch `https://[full page URL]` and extract:

**On-page targeting signals:**
- **Title tag** — exact text
- **H1** — exact text
- **Meta description** — exact text if findable
- **H2 headings** — full list (all of them, in order)
- **H3 headings** — list if present
- **First 150 words of body content** — exact
- **Primary keyword this page is targeting** — infer from title + H1 combination

**Content structure and depth:**
- **Word count** — estimate from the HTML content (rough: count visible text tokens, multiply by ~1.3 for average word length approximation). Report as a range (e.g. "~600–800 words") not a precise figure unless the page is very short.
- **Content format** — listicle / comparison table / landing page copy / long-form blog / FAQ block / pillar page
- **Sections present** — list every meaningful H2/H3 section topic (not just the heading text — translate it to what the section is actually about)
- **Content elements present** — note which of these exist: comparison table, pricing table, FAQ/accordion, customer quotes or case studies, product screenshots, video embeds, schema markup (look for JSON-LD blocks), internal links to related pages, CTA type (demo / trial / contact / none)
- **Content elements missing** — flag obvious gaps: no FAQ, no comparison table, no social proof, no structured data, no internal links to relevant hub pages

**Do not fabricate page content.** If a WebFetch fails or returns an error, note it and exclude that page from further analysis. Never infer what a page says without reading it.

---

## Phase 5 — Identify Keyword Gaps

This is the core of the skill. For each page, compare:
- **What the page targets** (title tag + H1 + meta description)
- **What the page ranks for** (the qualifying GSC keywords for that page)

For each keyword in that page's GSC qualifying pool, classify the gap type:

| Gap Type | Definition | Fix effort |
|---|---|---|
| **TYPE A — Title miss** | Keyword or its primary terms are not in the title tag. Page has some relevance (it ranks), but the title doesn't reinforce it. | Very low — edit title tag |
| **TYPE B — H1 miss** | Keyword not in H1 but may appear in title or body | Low — edit H1 |
| **TYPE C — Meta miss** | Keyword in title/H1 but not in meta description | Very low — edit meta |
| **TYPE D — Content gap** | Keyword appears superficially in the page (1–2 mentions, no dedicated section) — page has authority but content doesn't serve the intent | Medium — add a section |
| **TYPE E — Page type mismatch** | The keyword's SERP rewards a different page format than what's ranking (e.g., a blog post ranks for a query Google rewards with landing pages) | Higher — consider new page or restructure |
| **TYPE F — Cannibalization** | Two or more pages on the site rank for the same keyword in positions 4–15 — splitting authority | Medium — consolidate or redirect |
| **TYPE G — Depth gap** | Keyword is in the title/H1 but page is too thin vs. what the SERP rewards — top-ranking pages are significantly deeper, more structured, or more comprehensive | Medium-High — expand content based on competitor benchmark |

For each opportunity record:
- Page URL
- Keyword
- Current position (from GSC or Ahrefs)
- GSC impressions
- Gap type (A / B / C / D / E / F / G)
- What the page currently has in title / H1 that conflicts or lacks
- Recommended fix — specific and holistic (see output format rules in Phase 9)

---

## Phase 6 — Demand Triangulation

Run all Ahrefs volume lookups **in parallel** in batches. Never call Ahrefs one keyword at a time for this phase.

### 6A. Ahrefs volume and KD

Use `keywords-explorer-overview` in batches of up to 25 keywords per call for all opportunity keywords identified in Phase 5.

For each keyword record, add:
- **Global monthly search volume** (from Ahrefs)
- **Keyword difficulty (KD)** (from Ahrefs)
- **Traffic potential (TP)** — the total traffic the #1 ranking page earns from all keywords it ranks for. This is the true upside of owning that position, not just the target keyword's volume.
- **Top-ranking URL** (from Ahrefs — used in Phase 7 for competitor crawl)

If Ahrefs returns zero volume or no data for a keyword, do not substitute an estimate. Record it as "Ahrefs: no data" and keep it in the list if GSC impressions are strong (≥ 50/mo) — GSC impressions are a real demand signal.

### 6B. Triangulate demand score

For each keyword, calculate a **Demand Score** using both signals:

| Signal | Score |
|---|---|
| Ahrefs volume ≥ 1,000/mo | 3 |
| Ahrefs volume 200–999/mo | 2 |
| Ahrefs volume 50–199/mo | 1 |
| Ahrefs no data BUT GSC impressions ≥ 200/mo | 2 |
| Ahrefs no data BUT GSC impressions 50–199/mo | 1 |
| Ahrefs no data AND GSC impressions < 50/mo | 0 — flag for review |

**Do not conflate impressions with volume.** Impressions at position 8 represent a fraction of total search volume. Treat them as a floor signal, not an equivalent.

---

## Phase 7 — Competitor Benchmarking (Top Opportunities)

For the **top 20 opportunities by combined score** (scored in Phase 8), benchmark what the highest-ranking competitor pages look like. This is the foundation of the holistic content recommendations.

For each opportunity:

### 7A. Identify the top 3 competitors for this keyword

Use the Ahrefs `serp-overview` to get the top 10 results for the keyword. Pick the top 3 ranking URLs that are **not** the client's own domain and **not** aggregator/directory sites (like G2, Capterra, Trustpilot) unless the client is competing in that format. Note their domains and URLs.

### 7B. Crawl each competitor page

Use WebFetch on each of the 3 competitor URLs. Extract (same fields as Phase 4):
- Title tag
- H1
- H2 headings (full list)
- H3 headings (abbreviated)
- Estimated word count (same estimation method as Phase 4)
- Content elements present: comparison table / FAQ / pricing table / case studies / screenshots / video / schema / CTA type
- Number of products/tools reviewed (for listicles)
- Notable structural features the client page lacks

Run these WebFetch calls in parallel across the 3 competitors.

### 7C. Build a competitor benchmark table

For each target keyword, produce a comparison table:

```
| Page | Word Count (est.) | H2 Sections | Has FAQ | Has Table | Has Schema | CTA Type | Notable |
|---|---|---|---|---|---|---|---|
| [Client page] | ~X words | N sections | Y/N | Y/N | Y/N | [type] | — |
| [Competitor 1] | ~X words | N sections | Y/N | Y/N | Y/N | [type] | [what stands out] |
| [Competitor 2] | ~X words | N sections | Y/N | Y/N | Y/N | [type] | [what stands out] |
| [Competitor 3] | ~X words | N sections | Y/N | Y/N | Y/N | [type] | [what stands out] |
```

### 7D. Identify content gaps vs. the field

From the competitor crawls, identify the **specific sections or content elements** that rank well but the client's page is missing. These become the "Add these sections" list in the action card.

Examples of what to look for:
- Sections the top 3 all have that the client doesn't (strong signal — Google probably rewards this)
- FAQ blocks that likely earn "People also ask" placements
- Comparison tables with feature checkmarks
- Customer use-case sections (industry-specific)
- Pricing/plan transparency (even ballpark)
- Internal links to supporting pages
- Schema markup types (FAQ schema, HowTo schema, Product schema, Review schema)

**Do not crawl competitors that block bots.** If WebFetch returns a 403 / paywall / login screen, note it and skip. Fall back to WebSearch `site:[competitor] [keyword]` to see their title/description at minimum.

---

## Phase 8 — Score and Prioritize Every Opportunity

Score each keyword-page opportunity across four dimensions:

### Scoring dimensions

**1. Commercial tier of the page (1–3)**
- Tier 1 page (alternatives / listicle / landing page / pricing): **3**
- Tier 2 page (commercial-intent blog post): **2**
- Tier 3 page (informational): **1**

**2. Position proximity — how close to the prize (1–3)**
- Current position 4–6: **3** (one push from top 3)
- Current position 7–10: **2**
- Current position 11–15: **1**

**3. Demand score (0–3)** — from Phase 6B

**4. Fix effort — inverted (1–3)**
- TYPE A, B, or C (title / H1 / meta edit only, no content changes needed): **3**
- TYPE D or G (content section additions, word count expansion): **2**
- TYPE E or F (page restructure / consolidation / new page needed): **1**

**Total score = sum of four dimensions. Max: 12. Min: 3.**

### Priority tiers

| Tier | Score | Label |
|---|---|---|
| **Tier 1** | 10–12 | Do this week |
| **Tier 2** | 7–9 | Do this month |
| **Tier 3** | 4–6 | Backlog — do when above is done |
| **Deprioritize** | ≤ 3 | Low demand + high effort — only if nothing better |

Within each tier: sort by score descending. Where scores are equal, rank by GSC impressions descending.

---

## Phase 9 — Generate Output

### Print inline (in this conversation)

Print a **Quick Win Action Card** for every confirmed opportunity. This is the core deliverable — not a table of metadata changes, but a specific, prioritized brief for each page.

Do not abbreviate the list. The user should see every action card without opening the file.

---

**Action Card format:**

```
---

### [Page URL] — [Page type] — Score: [N/12]

**Primary keyword cluster:** [keyword 1] ([vol], KD [N]), [keyword 2] ([vol], KD [N]), ...
**Traffic potential at #1:** [Ahrefs TP for primary keyword]
**Current avg. position:** [X] | **GSC impressions/mo:** [N] | **GSC clicks/mo:** [N]

#### What the page has now
- **Title:** "[exact title]"
- **H1:** "[exact H1]"
- **Word count (est.):** ~[N] words
- **Content elements present:** [list]
- **Notable gaps:** [list what's missing]

#### What top-ranking competitors look like
| Page | Words | Key sections they have | Notable |
|---|---|---|---|
| [Competitor 1] | ~N | [list H2 topics] | [e.g. "has FAQ schema, comparison table"] |
| [Competitor 2] | ~N | [list H2 topics] | [e.g. "listicle with 12 tools vs client's 8"] |
| [Competitor 3] | ~N | [list H2 topics] | [e.g. "dedicated school use-case section"] |

#### Gap types identified
[List each gap type — A through G — for this page with a one-line note]

#### Action plan

**1. Title tag** (TYPE A — if applicable)
- Change from: "[current title]"
- Change to: "[recommended title]"
- Why: [specific reason — which keyword it targets, what's wrong with the current one]

**2. H1** (TYPE B — if applicable)
- Change from: "[current H1]"
- Change to: "[recommended H1]"

**3. Meta description** (TYPE C — if applicable)
- Change to: "[recommended meta — 150-160 chars, includes primary keyword, has a CTA]"

**4. Word count target**
- Current: ~[N] words
- Competitor average: ~[N] words (range: [low]–[high])
- Recommended target: ~[N] words
- Why: [specific — e.g. "top 3 results average 1,800 words; the client page at 400 words cannot compete for KD [N] — expand or lose ground"]

**5. Sections to add** (TYPE D / G — if applicable)
List each recommended section with:
- **Section heading (recommended H2):** "[exact text]"
- **What to cover:** [2–4 bullet points of what content should be in this section]
- **Why:** [which competitor has it / what SERP signal suggests it / which keywords it captures]

For example:
- **Add H2: "Digital Signage for Schools: What Districts Actually Need"**
  - Cover: enrollment size tiers, emergency alert integration (a common district requirement), FERPA/CIPA considerations, deployment at scale (100+ screens)
  - Why: all 3 top-ranking pages have an education-specific section; the client page has only one passing paragraph mentioning schools

**6. Structural additions** (if applicable — based on what competitors have that client lacks)
- [ ] Add FAQ block (if top competitors have FAQ schema — common "People also ask" driver)
  - Suggested questions: [list 3–5 specific questions based on "People also ask" from SERP and competitor FAQ headings]
- [ ] Add comparison table (if keyword SERP rewards comparison format)
- [ ] Add customer use-case or case study snippet (if competitors lead with social proof)
- [ ] Add schema markup type: [FAQ / HowTo / Product / Review / BreadcrumbList] — explain why
- [ ] Add internal links to: [specific pages on the client site that are topically related]

**7. Cannibalization note** (TYPE F — if applicable)
- [Which other page is splitting this keyword]
- [Recommended resolution: consolidate / redirect / differentiate scope]

**Estimated effort:** [Very low (title/H1/meta only) / Low (+ small content additions) / Medium (full section additions, 500+ new words) / High (restructure or new page needed)]
**Expected impact:** [Specific — e.g. "High — vol 2,200, KD 3, currently pos 7. A title + H1 edit alone should push to top 5."]

---
```

---

### Save to file

Save the complete report to the agreed path. The file contains everything printed inline, plus a methodology header.

```markdown
# SEO Quick Wins — [domain]
Date: [date]

## Data Sources
- GSC CSV: [date range, number of rows, format detected]
- Ahrefs: keywords-explorer-overview, site-explorer-organic-keywords, serp-overview
- Page crawls: [N pages crawled — client] + [N pages crawled — competitors]
- SERP checks: [N keywords spot-checked]

## Methodology
- Position filter: 4.0–15.0
- Impressions filter: ≥ 10/month
- Branded/navigational queries excluded: yes
- Demand triangulation: Ahrefs global volume + traffic potential + GSC impressions (floor signal only — not equivalent to volume)
- Competitor benchmarks: top 3 non-directory ranking pages crawled per top-20 opportunity
- Word count estimates: approximate from HTML text content — treat as directional, not precise
- Scoring: commercial tier (1–3) + position proximity (1–3) + demand (0–3) + fix effort inverted (1–3) = max 12

## Full Action Cards
[All action cards from inline output, in priority order]

## Pages Excluded
[List of pages where crawl failed, with reason]

## Keywords Excluded
[Branded/navigational list with brief note]
```

---

## Rules

- **No invented data.** Every volume figure, KD score, position, and word count must come from Ahrefs API or a live page crawl. If Ahrefs returns no data, say "no data." Never substitute an estimate.
- **No invented page content.** Every title tag, H1, section list, and gap observation must come from a live WebFetch of the page. Never infer what a page says without crawling it. This applies to competitor pages too.
- **GSC impressions ≠ search volume.** Impressions at position 8 are a fraction of total monthly searches. They are a floor signal — real demand exists, but it is larger than impressions suggest. Never state impressions as if they equal total volume.
- **Word count targets must be benchmarked, not guessed.** Never recommend "expand to 2,000 words" without showing competitor pages actually average ~2,000 words. The target should be the competitor average ± 10–20%, not an arbitrary number.
- **Sections to add must be specific.** Not "add more content about schools." Say: "Add H2 'Digital Signage for K-12 Classrooms' covering emergency alert requirements, FERPA considerations, and multi-screen district rollouts — [Competitor X] ranks #2 with exactly this section and it captures the 'digital signage for schools' query the client misses."
- **FAQ questions must come from real data.** Pull them from "People also ask" boxes (via WebSearch), competitor FAQ headings (from their crawled pages), or Ahrefs keyword suggestions. Do not invent them.
- **Print inline AND save.** The user reads the full list in this conversation. The file is a save for their records.
- **Bottom funnel first, always.** Tier 1 commercial pages come before blog posts regardless of score.
- **Flag crawl failures honestly.** If WebFetch fails for a page (client or competitor), note it and work with what you have. Never guess at content.
- **Cannibalization is structural.** TYPE F issues should always be escalated — two pages splitting the same keyword cluster are a ceiling on both.
- **SERP checks are spot-checks, not a kill filter.** A difficult SERP at position 8 can still move to position 5 with on-page changes. Flag difficulty but keep the opportunity.
- **Stay in scope.** This skill finds optimization opportunities for existing pages. It does not create new content briefs or recommend entirely new topic areas — that is the `/topic-research` pipeline.
- **Traffic potential is the real number to highlight.** A keyword with vol 800 might have a TP of 23,000 if the #1 ranking page ranks for 200+ related terms. Always surface TP alongside raw volume so the client understands the upside.
