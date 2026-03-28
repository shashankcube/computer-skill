# Tool Patterns for Platform-Integrated Skills

This reference covers patterns for writing skills that leverage platform tools effectively. The key principle: **describe capabilities, not tool names**. Tools change; capabilities persist.

## Table of Contents

1. [Tool Discovery Pattern](#tool-discovery-pattern)
2. [Data Querying Skills (SQL/KG)](#data-querying-skills)
3. [DevRev Workflow Skills](#devrev-workflow-skills)
4. [Communication Skills (Slack/GitHub)](#communication-skills)
5. [Observability Skills (Datadog)](#observability-skills)
6. [Multi-Tool Orchestration](#multi-tool-orchestration)
7. [Common Skill Archetypes](#common-skill-archetypes)

---

## Tool Discovery Pattern

Every skill that depends on external tools should include a discovery step. Here's the pattern:

```markdown
## Prerequisites

Before starting, verify the tools you need are available:

1. Scan the available tool list for [category] tools (look for tools with names matching "[relevant prefix/pattern]"), or use ToolSearch with keywords like "[relevant keywords]" if ToolSearch is available
2. If the required tools aren't available, inform the user what's needed and stop
3. Note the exact tool names for use in subsequent steps
```

Why this matters: A skill installed in one environment might be used in another where different MCP servers are configured. Tool discovery makes skills portable. The discovery step should work regardless of whether `ToolSearch` is available — scanning the tool list directly is always an option.

---

## Data Querying Skills

Skills that query organizational data (reports, dashboards, analytics) typically follow this pattern:

### The Schema-First Pattern

```markdown
## Workflow

### Step 1: Understand the data model
Before writing any queries, discover what data is available:
- Use the knowledge graph schema tools to get an overview of all objects and relationships
- For specific objects you'll query, get their detailed schema (fields, types, relationships)
- Note which objects are global vs organization-specific

### Step 2: Construct queries
Based on the schema and the user's request:
- Build SQL queries that use the correct field names from the schema
- Use the natural-language-to-SQL tools if available — they have context on query syntax
- Always validate field names against the schema before executing

### Step 3: Execute and validate
- Execute queries using the available SQL execution tools
- Check that results make sense given the schema
- Handle empty results gracefully — explain what was queried and why it might be empty

### Step 4: Present results
- Format results according to the user's needs (table, summary, chart data)
- Include the query used so the user can modify it
```

### Key principles for data skills

- **Schema before query**: The model should always understand the data model before writing SQL. This prevents hallucinated column names and wrong joins.
- **Iterative refinement**: SQL queries often need adjustment. The skill should encourage trying a query, examining results, and refining.
- **Explain the data**: Don't just dump results. Summarize what the data shows and highlight interesting patterns.

### Common data skill types

| Skill Type | Typical Queries | Key Considerations |
|------------|----------------|-------------------|
| Dashboard | Aggregations, GROUP BY, time series | Handle date ranges, support filtering |
| Report | JOINs across objects, computed fields | May need multiple queries stitched together |
| Analytics | Statistical queries, trend analysis | Consider data freshness and completeness |
| Search | WHERE with multiple conditions, LIKE | Handle fuzzy matching, suggest alternatives |

---

## DevRev Workflow Skills

Skills that interact with DevRev's work management system (issues, tickets, enhancements, sprints).

### Object Lifecycle Awareness

DevRev objects have specific lifecycles with valid stage transitions. A skill that creates or updates work items should:

```markdown
## Working with DevRev objects

### Before creating or updating work items
1. Check available subtypes for the work type you're creating (use list subtypes tools)
2. For updates, verify the stage transition is valid (use stage transition tools)
3. When linking objects, understand the relationship types available

### Creating work items
- Set the appropriate subtype based on the nature of the work
- Assign to the right part (product area) if known
- Add to the current sprint if the work is immediate
- Include a clear title and description with enough context for anyone to pick it up

### Updating work items
- Always check current state before updating
- Use valid stage transitions — don't skip stages
- Add timeline entries to explain why changes were made
```

### Common DevRev skill patterns

- **Triage skills**: Read incoming items, categorize, assign, set priority
- **Sprint management**: Plan sprints, track progress, identify blockers
- **Reporting skills**: Aggregate work item data for status reports
- **Escalation skills**: Detect stale items, notify owners, update status

---

## Communication Skills

Skills that post to Slack, create GitHub issues/PRs, or otherwise communicate externally.

### The Confirm-Before-Send Pattern

```markdown
## Posting messages

External communication is hard to undo. Before posting:
1. Draft the message and show it to the user
2. Wait for explicit confirmation before sending
3. After sending, confirm what was sent and where

Exception: If the user has explicitly said "just post it" or the skill is designed for automated posting, skip the confirmation. But default to confirming.
```

### Slack-specific patterns

- **Channel discovery**: Use list channels / search to find the right channel
- **Thread awareness**: When responding to a discussion, post in the thread, not the channel
- **Formatting**: Use Slack's markdown (mrkdwn) — it's slightly different from standard markdown
- **User mentions**: Search for users by name to get their IDs for @mentions

### GitHub-specific patterns

- **PR creation**: Include a clear title, description with context, and link to relevant issues
- **Code review**: Use inline comments on specific files/lines, not just top-level comments
- **Issue management**: Check for duplicates before creating, link related issues

---

## Observability Skills

Skills that query Datadog or similar monitoring platforms.

### The Context-First Pattern

```markdown
## Investigating issues

### Step 1: Establish context
- What service or component is involved?
- What time range are we looking at?
- Are there known incidents or deployments in that window?

### Step 2: Gather signals
- Search logs with relevant filters (service, severity, time range)
- Check metrics for the affected service
- Look for related traces if available
- Check for active monitors or incidents

### Step 3: Correlate
- Cross-reference logs, metrics, and traces
- Look for patterns (error spikes, latency increases, deployment markers)
- Check upstream/downstream dependencies

### Step 4: Summarize findings
- Present a timeline of events
- Highlight the most likely root cause
- Suggest next steps
```

---

## Multi-Tool Orchestration

Many valuable skills combine multiple tool categories. Here's how to structure them:

### The Pipeline Pattern

```markdown
## Workflow

This skill uses multiple platform capabilities in sequence:

### Phase 1: Gather (data tools)
- Query the relevant data using SQL/KG tools
- Pull context from DevRev work items

### Phase 2: Analyze (compute)
- Process the gathered data (scripts, inline analysis)
- Identify patterns, anomalies, or action items

### Phase 3: Act (communication/workflow tools)
- Create work items for action items found
- Post summaries to relevant Slack channels
- Update dashboards or reports

### Phase 4: Verify
- Confirm all actions were taken
- Summarize what was done for the user
```

### Tool dependency chains

When skills chain tools, make the dependencies explicit:

```markdown
## Tool chain
This skill needs tools from these categories (check availability before starting):
1. **Data querying** — to pull organizational data
2. **Work management** — to create/update work items based on findings
3. **Messaging** — to notify stakeholders (optional, gracefully degrade if unavailable)
```

---

## Common Skill Archetypes

These are the most common types of skills people build in DevRev-like environments. Use these as starting points:

### 1. Report Generator
**Purpose**: Pull data, analyze it, produce a formatted report
**Tools needed**: SQL/KG (required), filesystem (for output)
**Key pattern**: Schema-first, iterative query refinement, structured output template

### 2. Workflow Automator
**Purpose**: Automate a multi-step process (triage, sprint planning, release tracking)
**Tools needed**: DevRev (required), Slack (optional for notifications)
**Key pattern**: Object lifecycle awareness, confirm-before-act, audit trail via timeline entries

### 3. Dashboard Builder
**Purpose**: Create visual or data-driven dashboards from organizational data
**Tools needed**: SQL/KG (required), filesystem (for HTML/charts)
**Key pattern**: Schema-first, aggregation queries, HTML/chart generation scripts

### 4. Investigation Assistant
**Purpose**: Help debug production issues by correlating signals
**Tools needed**: Datadog (required), DevRev (optional for linked incidents)
**Key pattern**: Context-first, multi-signal correlation, timeline reconstruction

### 5. Communication Drafter
**Purpose**: Draft and send messages (status updates, incident comms, release notes)
**Tools needed**: Slack or GitHub (required), DevRev (for data)
**Key pattern**: Confirm-before-send, audience-aware tone, structured templates

### 6. Data Explorer
**Purpose**: Help users explore and understand their organizational data
**Tools needed**: SQL/KG (required)
**Key pattern**: Interactive schema exploration, progressive query building, explain-as-you-go

### 7. Sprint/Project Tracker
**Purpose**: Track sprint progress, identify risks, generate status reports
**Tools needed**: DevRev (required), Slack (optional)
**Key pattern**: Aggregate work item states, compare against goals, highlight blockers

### 8. Deck/Presentation Builder
**Purpose**: Create slide decks or presentations from organizational data
**Tools needed**: SQL/KG (for data), filesystem (for output), templates (in assets/)
**Key pattern**: Data gathering -> narrative construction -> template population -> output generation

---

## Anti-Patterns to Avoid

1. **Hardcoding tool names**: Don't write `Use mcp__plugin_devrev_devrev__create_work`. Write "Create a work item using the available DevRev tools."

2. **Assuming tool availability**: Don't write a skill that silently fails if a tool is missing. Include discovery and graceful degradation.

3. **Ignoring schema discovery**: Don't guess field names or object structures. Always discover the schema first for data-querying skills.

4. **Skipping confirmation for external actions**: Creating issues, posting messages, and updating work items should be confirmed with the user unless the skill is explicitly designed for automation.

5. **Monolithic tool chains**: Don't write a skill that requires 10 different MCP servers. Keep tool dependencies minimal and make optional tools truly optional.

6. **Tool-specific error messages**: Don't say "ExecuteSQL returned error 422". Say "The query failed — the error suggests [interpretation]. Try [alternative approach]."
