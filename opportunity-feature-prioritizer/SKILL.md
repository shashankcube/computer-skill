---
name: opportunity-feature-prioritizer
description: >
  Analyze open opportunities to identify and prioritize product features that prospects
  and customers need. Use this skill when the user wants to understand what features
  to build next based on sales pipeline data, when they ask about opportunity-driven
  feature prioritization, what prospects are asking for, which features would unblock
  the most revenue, pipeline-to-product alignment, or any variation of "what should
  we build to win more deals". Also trigger when users mention opportunity analysis,
  deal-feature mapping, revenue-weighted feature requests, or sales-informed roadmap.
---

# Opportunity Feature Prioritizer

Analyzes open opportunities to surface which product features are most needed across the pipeline, helping you decide what to prioritize building.

## Why this skill exists

Product teams often build features disconnected from what sales is hearing. Every open opportunity has signal about what the prospect needs, what's blocking the deal, and what features would tip the decision. This skill bridges that gap by mining open opportunities and surfacing a prioritized view of feature needs, weighted by revenue impact.

## Prerequisites — Tool Discovery

This skill requires data querying tools (SQL execution, knowledge graph schema) because opportunity objects (OPP-*) are not directly accessible through standard DevRev work management tools — there is no "opportunity" type in list_works and no "opportunity" namespace in search.

Before starting, check what tools are available:

1. **Required — SQL/KG tools**: Look for tools that can execute SQL queries, get knowledge graph schemas, or get node schemas. Without these, you cannot query opportunity objects at all.
2. **Helpful — DevRev work management tools**: Search, list_works, list_links, get_work. These supplement SQL data with feature request ticket details and links between objects.
3. **Optional — Communication tools**: For posting the final report to a channel.

If SQL/KG tools are not available, tell the user: "I can't directly query opportunity objects without SQL tools. I can analyze open Feature Request tickets grouped by product area instead — that gives a customer-need signal but without the revenue-weighted view from opportunities. Want me to do that, or can you help me get SQL access?"

## Workflow

### Step 1: Discover the opportunity data model

Use the knowledge graph schema tools to understand what's available:

1. Get the full KG schema to find the opportunity object
2. Get the detailed node schema for the opportunity object — understand its fields:
   - Key fields to look for: ACV/ARR (deal value), stage/pipeline stage, account, owner, target close date, forecast category, type of opportunity, linked features/products
3. Also get the schema for feature request or ticket objects to understand how they link to opportunities
4. Understand the join paths: opportunity -> account -> feature requests, or opportunity -> linked tickets/parts

### Step 2: Query open opportunities

Write and execute a SQL query to pull all open opportunities:

```sql
SELECT
  opp_id,
  account_name,
  acv,                    -- or whatever the deal value field is called
  stage_name,
  target_close_date,
  owner,
  forecast_category
FROM opportunity_dataset   -- discover actual dataset name from schema
WHERE stage_name NOT IN ('Closed Won', 'Closed Lost', 'Closed-Won', 'Closed-Lost')
ORDER BY acv DESC
```

Adapt field names based on what you discovered in Step 1. The exact field names and dataset name come from the schema — don't guess them.

### Step 3: Find feature signals linked to each opportunity

This is the core analysis. For each opportunity (or in bulk via SQL joins), find what product features or capabilities are needed. Multiple signal sources:

**Signal 1 — Linked feature request tickets (strongest signal)**:
Query feature request tickets that are linked to opportunities, either directly or through the same account:
```sql
-- Join opportunities with feature request tickets through account or direct links
SELECT
  opp.opp_id,
  opp.account_name,
  opp.acv,
  fr.display_id,
  fr.title,
  fr.applies_to_part_name    -- the feature/product area
FROM opportunity_dataset opp
JOIN ticket_dataset fr ON fr.account_id = opp.account_id
WHERE opp.stage_name NOT IN ('Closed Won', 'Closed Lost')
  AND fr.subtype = 'Feature Request'
  AND fr.state IN ('open', 'in_progress')
```

**Signal 2 — Links between objects**:
Use the DevRev links tool to find direct connections:
- For key opportunities, call list_links to find linked tickets, parts, or enhancements
- Links like `is_related_to`, `depends_on`, `is_blocked_by` are especially valuable

**Signal 3 — Text mining** (if structured links are sparse):
Look at opportunity descriptions, notes, or timeline entries for mentions of product capabilities or feature names.

### Step 4: Aggregate and score features

Flip the mapping: for each unique feature/product area, aggregate all opportunities that need it.

**Scoring**:

```
Feature Priority Score = SUM across linked opportunities of:
  deal_value * stage_weight * time_urgency
```

Where:
- **deal_value**: ACV/ARR of the opportunity
- **stage_weight**: Later pipeline stages get higher weight because the signal is more validated
  - Early stages (Discovery, Qualification): 0.3
  - Mid stages (Evaluation, Demo, PoC): 0.5
  - Late stages (Negotiation, Proposal): 0.8
  - Closing: 1.0
  - Adapt these to the actual stage names found in the data
- **time_urgency**: Deals closing sooner matter more
  - Closing in <30 days: weight 1.5
  - 30-90 days: weight 1.0
  - 90+ days: weight 0.5

If deal value data isn't available, fall back to opportunity count per feature.

Try to do as much of this aggregation in SQL as possible rather than pulling all data and computing in-memory.

### Step 5: Cross-reference with existing roadmap

Check if the top prioritized features already have enhancements or issues in progress:

1. Use the DevRev parts/enhancements tools or SQL to find existing work items for each feature
2. Flag:
   - **Already in progress**: Feature X is being built (link to enhancement)
   - **Planned but not started**: Enhancement exists but not started
   - **Gap**: No existing work item — this is where new investment is needed

### Step 6: Present the prioritization report

```markdown
# Feature Prioritization Report
_Based on [N] open opportunities | Generated [date]_

## Executive Summary
- Total pipeline analyzed: $X ARR across N open opportunities
- Top feature gap: [feature name] — needed by N deals worth $X
- Urgent: [features blocking deals closing in <30 days]

## Top Prioritized Features

### 1. [Feature/Product Area Name]
- **Priority Score**: X (weighted by revenue + stage + urgency)
- **Pipeline Impact**: N opportunities, $X total ACV
- **Key deals**:
  | Account | ACV | Stage | Close Date |
  |---------|-----|-------|------------|
  | Acme Corp | $200K | Negotiation | Apr 15 |
  | BigCo | $150K | Evaluation | May 30 |
- **What they need**: [summary from feature request descriptions]
- **Existing roadmap**: [In progress / Planned / Gap]

### 2. [Next Feature]
...

## Insights
- [Patterns — e.g., "Enterprise deals consistently need SSO and audit logs"]
- [Risk — e.g., "$500K in pipeline closing in 30 days blocked on Feature X"]
- [Clusters — e.g., "Integration features dominate top 5"]

## Raw Data
[Full table of opportunities -> features mapping for reference]
```

### Step 7: Offer next actions

- "Want me to create issues for the top feature gaps?"
- "Want me to post this to a channel?"
- "Want me to dig deeper into a specific feature or set of opportunities?"
- "Want me to save this as a file?"
- "Want me to re-run this with different filters (specific stage, time range, deal size threshold)?"

## Handling edge cases

- **No SQL/KG tools**: Can't query opportunities directly. Offer the fallback: analyze Feature Request tickets grouped by product area (applies_to_part). This gives customer-need signal without revenue weighting.
- **Opportunities don't link to features**: Join through accounts — find feature request tickets from the same accounts that have open opportunities.
- **No deal value fields**: Use opportunity count as the metric. Note the limitation.
- **Inconsistent feature naming**: Normalize — "SSO", "single sign-on", "SAML auth" are likely the same. Group them and flag for user verification.
- **Very large pipeline**: Add filters — focus on top N by ACV, or a specific pipeline stage, or closing within a time range. Ask the user what slice they care about.
