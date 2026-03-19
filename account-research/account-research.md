---
skill-name: account-research
description: Research a company or person — combines web search, DevRev internal data, and optional enrichment/CRM. Use when user says "research [company]", "look up [person]", "intel on [prospect]", "who is [name] at [company]", "tell me about [company]", or "pull up signals for [account]".
arguments:
  - name: target
    description: Company name, domain, person name, or DevRev account ID to research
    required: true
---

# Account Research

Get a complete picture of any company or person before outreach. Combines three data layers — each adds depth but none is required.

```
┌──────────────────────────────────────────────────────┐
│               ACCOUNT RESEARCH                       │
├──────────────────────────────────────────────────────┤
│  LAYER 1 — WEB (always available)                    │
│  ✓ Company overview, size, industry                  │
│  ✓ Recent news, funding, leadership changes          │
│  ✓ Hiring signals and open roles                     │
│  ✓ Key people from LinkedIn                          │
│  ✓ Product/service and customer base                 │
├──────────────────────────────────────────────────────┤
│  LAYER 2 — DEVREV (when account exists in DevRev)    │
│  + Opportunities: stage, ACV, TCV, forecast          │
│  + Tickets: open issues, severity, themes            │
│  + Engagement: meetings, emails, discussions          │
│  + Account metadata: tier, owner, contacts, ARR      │
├──────────────────────────────────────────────────────┤
│  LAYER 3 — ENRICHMENT (when tools connected)         │
│  + Verified emails, phone numbers, org chart         │
│  + Tech stack, funding history, investors             │
│  + Intent signals, hiring velocity                    │
└──────────────────────────────────────────────────────┘
```

---

## Step 1: Parse the Request

Determine what the user needs and which layers to activate.

| Input | Type | Layers |
|-------|------|--------|
| "Research Stripe" | Company research | Web + DevRev (if exists) |
| "Look up John Smith at Acme" | Person + company | Web + DevRev |
| "Intel on acme.com" | Domain-based | Web + DevRev |
| "Who is the CTO at Notion" | Role-based search | Web |
| "Research Stripe — check DevRev too" | Explicit DevRev | Web + DevRev |
| "Tell me about ACC-42" | DevRev account ID | DevRev + Web |

**Default behavior**: Always run web search. Check DevRev if the user mentions DevRev data, provides a DevRev ID, or asks about internal relationship/pipeline/tickets.

---

## Step 2: Web Search (Always)

Run these searches in parallel:

1. `"[Company name]"` — Homepage, about page
2. `"[Company name] news"` — Recent announcements (last 90 days)
3. `"[Company name] funding"` — Investment history
4. `"[Company name] careers"` — Hiring signals
5. `"[Person name] [Company] LinkedIn"` — Profile info (if person research)
6. `"[Company name] product"` — What they sell
7. `"[Company name] customers"` — Who they serve

**Extract**: company description, positioning, recent news, leadership team, open roles, technology mentions, customer base.

---

## Step 3: DevRev Data (When Applicable)

Activate when: user asks for internal data, provides a DevRev ID, or says "check DevRev", "our data", "what do we have on them", "pipeline", "tickets", "relationship history".

### Tools

1. **HybridSearch** — Find account by name, get DON ID.
2. **FetchObjectContext** — Full object details by DON or display ID.
3. **GetKGSchema** — Full knowledge graph schema. Call before any SQL path.
4. **GetNodeSchema** — Schema details for a specific node.
5. **NLToSQL** — Convert natural language to SQL. Never write SQL yourself.
6. **ExecuteSQL** — Run SQL from NLToSQL. Always add LIMIT 500.
7. **MeetingSummaryOpp** — Meeting summaries linked to an opportunity.
8. **EmailSummaryOpp** — Email summaries linked to an opportunity.
9. **OpportunityDiscussions** — Discussion threads linked to an opportunity.
10. **WebSearch** — External web intelligence.

### DevRev Data Gathering Sequence

1. **HybridSearch** to find account by name → get DON ID
2. **FetchObjectContext** on account DON → owner, tier, industry, contacts, ARR
3. **SQL path** (GetKGSchema → GetNodeSchema → NLToSQL → ExecuteSQL) for:
   - Linked opportunities: display ID, title, stage, ACV, TCV, owner, close date, forecast category
   - Linked tickets: display ID, title, severity, stage, created date
   - LIMIT 500. Exclude duplicates silently.
4. **FetchObjectContext** on top opportunities (stage 3+) for deeper context
5. **MeetingSummaryOpp** + **EmailSummaryOpp** on active opportunities for engagement history
6. **OpportunityDiscussions** for internal notes and themes

### Account Disambiguation

Multiple accounts can share the same name. If one match found — state assumption and proceed. If multiple — list them and ask user to select.

---

## Step 4: Enrichment (When Tools Connected)

If enrichment tools are available:
1. Enrich company → firmographics, funding, tech stack
2. Search people → org chart, contact list
3. Enrich person → verified email, phone, background
4. Get signals → intent data, hiring velocity

---

## Step 5: Synthesize

1. Combine all layers — prioritize enrichment > DevRev > web for accuracy
2. Identify qualification signals (positive, concerns, unknowns)
3. Generate talking points tailored to the person/account
4. Recommend approach and next steps
5. Be explicit about what data came from which source

---

## Output Format

Only include sections where actual data exists. Omit sections entirely rather than guessing.

### Full Overview (Company Research)

```markdown
# Research: [Company Name]

**Generated:** [Date]
**Sources:** Web Search [+ DevRev] [+ Enrichment]

---

## Quick Take

[2-3 sentences: who they are, why they might need you, best angle for outreach]

---

## Company Profile

| Field | Value |
|-------|-------|
| **Company** | [Name] |
| **Website** | [URL] |
| **Industry** | [Industry] |
| **Size** | [Employee count] |
| **Headquarters** | [Location] |
| **Founded** | [Year] |
| **Funding** | [Stage + amount if known] |
| **Revenue** | [Estimate if available] |

### What They Do
[1-2 sentence description of business, product, customers]

### Recent News
- **[Headline]** — [Date] — [Why it matters for outreach]
- **[Headline]** — [Date] — [Why it matters]

### Hiring Signals
- [X] open roles in [Department]
- Notable: [Relevant roles — Engineering, Sales, AI/ML]
- Growth indicator: [Interpretation]

---

## Key People

### [Name] — [Title]
| Field | Detail |
|-------|--------|
| **LinkedIn** | [URL] |
| **Background** | [Prior companies, education] |
| **Tenure** | [Time at company] |
| **Email** | [If enrichment available] |

**Talking Points:**
- [Personal hook based on background]
- [Professional hook based on role]

---

## DevRev Relationship [If DevRev data available]

| Field | Detail |
|-------|--------|
| **Account Tier** | [Tier] |
| **Account Owner** | [Name] |
| **ARR** | [Value] |
| **Status** | [New / Prospect / Customer / Churned] |
| **Last Engagement** | [Date and type] |

### Pipeline

| Opp ID | Title | Stage | ACV | TCV | Owner | Close Date |
|--------|-------|-------|-----|-----|-------|------------|
| [ID] | [Title] | [Stage] | [ACV] | [TCV] | [Owner] | [Date] |

**Pipeline Summary:** [X] active opps, $[Y] total ACV, [Z] closed won

### Open Tickets

| Ticket | Title | Severity | Stage | Created |
|--------|-------|----------|-------|---------|
| [ID] | [Title] | [Severity] | [Stage] | [Date] |

### Engagement History
- [Date] — [Meeting/Email] — [Key points]
- [Date] — [Meeting/Email] — [Key points]

### Internal Themes
[Recurring topics from discussions, tickets, and internal notes]

---

## Tech Stack [If Enrichment Available]

| Category | Tools |
|----------|-------|
| **Cloud** | [AWS, GCP, Azure] |
| **Data** | [Snowflake, Databricks] |
| **CRM** | [Salesforce, HubSpot] |
| **Other** | [Relevant tools] |

**Integration Opportunity:** [How your product fits their stack]

---

## Qualification Signals

### Positive
- ✅ [Signal and evidence]

### Concerns
- ⚠️ [Concern and what to watch]

### Unknown (Ask in Discovery)
- ❓ [Gap in understanding]

---

## Recommended Approach

**Best Entry Point:** [Person and why]

**Opening Hook:** [What to lead with based on research]

**Discovery Questions:**
1. [Question about their situation]
2. [Question about pain points]
3. [Question about decision process]

**Recommended Next Steps:**
1. [Action with owner and timeline]
2. [Action with owner and timeline]

---

## Sources
- [Source 1](URL)
- [DevRev objects referenced: DON IDs]
```

### Person Research

Focus on: background, role, LinkedIn activity, talking points, DevRev contact history if available.

### Targeted Question

For simple questions ("Who owns the Stripe account?", "What's their ARR?") — return a 1-3 sentence direct answer. No full brief needed.

---

## Signal Interpretation Guide

### Product Signals (1st-Party)
| Signal | Meaning |
|--------|---------|
| Active seats increasing | Expansion opportunity |
| Feature adoption spike | Deepening engagement — good for expansion talk |
| Login frequency drop | Churn risk — proactive outreach needed |
| Trial started | Active evaluation — time-sensitive |
| Trial nearing expiry | Decision point — immediate action |

### Intent Signals (3rd-Party)
| Signal | Meaning |
|--------|---------|
| Hiring for relevant roles | Growing team, budget exists |
| Funding announced | New budget likely — congratulate and reach out |
| Tech stack change | May need new integrations |
| Competitive product removed | Window of opportunity — very high priority |

### Relationship Signals
| Signal | Meaning |
|--------|---------|
| Recent meeting logged | Active relationship |
| No meeting in 90+ days | Relationship cooling — check in |
| Open opportunity | Active deal — coordinate with AE |
| Champion job change | Risk or new opportunity |

**Principles:**
1. Recency matters most — yesterday's signal outweighs last quarter's
2. Signal clusters > single signals
3. Absence of signals is a signal — minimal activity at a paid account = risk
4. Contextualize against account stage — trial: conversion signals, mature: expansion/churn

---

## Quality Standards

- Every fact must trace to a data source — never speculate
- Be explicit when data is missing or stale
- Prioritize enrichment data > DevRev data > web data for accuracy
- Keep full briefings readable in 2-3 minutes
- Show the SQL query used at the bottom when DevRev data is queried
