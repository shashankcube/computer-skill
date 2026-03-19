---
skill-name: trace-diagnosis
user-invocable: true
description: Diagnose issues in Computer agent execution traces. Fetches MELTS traces, analyzes tool calls, errors, latency and response quality, creates an engineering ticket. Use when debugging agent failures or investigating why the Computer agent gave wrong answers.
arguments:
  - name: context
    description: Description of the issue or the conversation context where the agent failed
    required: false
---

# TraceDiagnosis

Diagnoses issues in Computer agent execution traces. Fetches MELTS traces for recent user messages, analyzes tool calls, errors, latency and response quality, and provides a triage summary.

## Tools

1. **FetchTraces** — Fetches MELTS execution traces for the current conversation. Returns the skill call chain, inputs, outputs, errors, timing, and token usage.
2. **FetchKnowledgeBase** — Returns content from the **Computer Agent Common Issues** reference article only. Use it for known root causes and fixes. Do not use the customer org's knowledge base or any other org articles for this analysis.
3. **CreateTicket** — Creates a ticket with the full technical analysis as the description.

## Your Output to the User

Your text output should be two parts: (1) one line stating the root cause only, in plain language; (2) one line stating that we have used their data to report this bug to the DevRev team, and the ticket ID. Keep it brief — 2 to 3 sentences total.

Example output:
"I analyzed the traces and found the issue: the agent used a search tool instead of a database query for your request. I've created ticket TKT-181 to track this for the engineering team."

Do not include in your output:
- Headings, sections, or markdown formatting
- "What should have happened" or correct execution paths
- Skill or tool names (KnowledgeSearch, NLToSQL, GetKGSchema, HybridSearch, ExecuteSQL, FetchObjectContext, etc.)
- Technical terms (SQL, namespace, DON ID, aggregation, semantic search, DQL, etc.)
- Step-by-step tables or bullet point breakdowns
- Code blocks or SQL examples
- Recommendations or workarounds
- Apologies or follow-up questions

All technical details go exclusively into the CreateTicket description where only engineers see them.

## Task Sequence

1. Call **FetchTraces** to get execution traces for this conversation. Check the result carefully — if the output contains "timeout", "error", or is empty, treat it as a failure even if the tool call itself reports success. If traces are unavailable, use the SHORT ticket template (Reference B2) instead of the full template.
2. Call **FetchKnowledgeBase** with the error or failure pattern to find known root causes.
3. Call **CreateTicket**:
   - If you have trace data: use the FULL ticket template (Reference B1) with actual data from the traces.
   - If traces are unavailable: use the SHORT ticket template (Reference B2). Do NOT use the full template and fill it with "not available".
4. Output (1) one line with the root cause only, and (2) one line that we've reported this with the ticket ID.

## Reference A: Failure Patterns

Use these to classify the failure when writing the CreateTicket description:

- **A: SQL Silent Fallback** — GetNodeSchema fails/wrong DON ID → HybridSearch used for analytical query → wrong counts
- **B: SQL Generation Error** — NLToSQL/ExecuteSQL → table/column not found, forbidden expression
- **C: Code Counting Error** — ExecuteSQL returns N rows → ExecuteCode → different count
- **D: Invalid Search Params** — HybridSearch → namespace not permitted, bad params
- **E: Object Fetch Failure** — FetchObjectContext → mongo no documents, invalid ID, 400/404 error
- **F: Permission Error** — GetKGSchema → "not authorised" → downstream blocked
- **G: Timeout/503** — Any skill → 503, timeout, deadline exceeded
- **H: Context Overflow** — Multiple calls succeed → sudden "unable to process"
- **I: No Skills Called** — Agent responded without calling any tools
- **J: Wrong Skill Selected** — Wrong tool for the job (e.g. HybridSearch for counts)

## Reference B1: Full Ticket Template (when trace data IS available)

**Title**: [TraceDiagnosis] — [what failed in ≤10 words]

**Description**:

```
## Summary
[2-3 sentences: what the user asked, what went wrong, what the impact was]

## Failure Pattern
[Pattern letter]: [1 sentence explaining why this pattern was identified]

## Turn 1 Analysis: "[user's exact query for turn 1]"

**Agent Response**: "[first 200 chars of agent's response]..."

### Skill Calls

#### Skill Call 1: [SkillName]
- **Query**: [the search query, SQL query, or natural language input — verbatim from trace]
- **Namespace** (if applicable): [the namespace value passed]
- **Full Input**: [complete input JSON or all key fields]
- **Output**: [full output or key fields, truncate if >500 chars with "[truncated]"]
- **Duration**: [X]ms
- **Status**: Success / Failed ([HTTP status code])
- **Error**: [exact error message if failed, "none" if success]

[Continue for ALL skill calls in this turn.]

### Turn 1 Errors
1. **[SkillName] — [error type]**: [exact error message]
   - Triggering input: [what caused it]
   - Downstream impact: [what broke because of this]

### Turn 1 Assessment
[What went wrong — 2-3 sentences]

**Token Usage**: [prompt_tokens] prompt / [completion_tokens] completion
**Total Duration**: [X]ms

---

[Add more turns as needed]

---

## Root Cause Analysis

### What Failed
[Name the specific skill, error, input that caused it. Include DON IDs, error codes.]

### Why It Failed
[Technical root cause. Reference KB match if found.]

### Silent Fallbacks
[List cases where skill failed and agent silently switched approach.]

### Knowledge Base Match
[Quote relevant root cause and fix from KB. If no match: "No match found."]

## Impact
[How this affected the user]

## Severity
[High / Medium / Low]

## Recommendation
*Only if FetchKnowledgeBase found a match.*
- **Workaround**: [1 sentence]
- **Engineering Fix**: [1 sentence]
- **Known Issue**: [Yes/No]

## Reproduction
- **DM ID**: [conversation ID]
- **Timestamp**: [when failing query was sent]
- **User**: [user name and DON ID]
- **Org**: [org slug / DON]
- **Agent**: [agent ID]
- **Repro query**: "[exact user query]"
```

## Reference B2: Short Ticket Template (when trace data is NOT available)

**Title**: [TraceDiagnosis] Trace Unavailable — [brief description]

**Description**:

```
## Summary
[2-3 sentences: what the user asked and what appeared to go wrong]

## Trace Status
FetchTraces returned: [exact error message or "timeout"].

## What We Know
- **User query**: "[exact user query]"
- **Agent response summary**: [brief summary]
- **Suspected pattern**: [Pattern letter or "Unknown"]
- **Knowledge base match**: [if found]

## What Engineers Need To Do
1. Pull MELTS traces manually
2. Identify full skill call chain and error cascade
3. Confirm suspected failure pattern

## Reproduction
- **DM ID**: [if available]
- **User**: [user name and DON ID]
- **Org**: [org slug / DON]
- **Repro query**: "[exact user query]"
```

## Rules

1. Your text output to the user is two parts only: root cause + ticket ID. No technical details.
2. Always call FetchTraces first. Inspect result for "timeout", "error", or empty — treat as failure.
3. Always create a ticket. Every diagnosis gets a ticket.
4. Choose the right template: B1 when you have traces, B2 when you don't. Never fill B1 with "not available".
5. When using B1: be exhaustive — engineers should never need to re-pull traces.
6. When using B2: keep it short and actionable.
7. Never fabricate trace data. Only include fields where you have actual data.
8. For known root causes, use only FetchKnowledgeBase output. Do not search customer org's data.
