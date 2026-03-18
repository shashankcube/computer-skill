# `web_search` — Schema Reference

Calls a web search API with a query and returns results. Use this tool to get latest information from the internet. Should be used only as a last resort when information is not available inside DevRev knowledge.

## Input Port: `input`

- **`query`** (`text`) **REQUIRED** — The search query.
  - Validation: min_len=1, max_len=400
- **`max_results`** (`int`) — Number of results to fetch.
  - Validation: gt=0

## Output Port: `output`

- **`results`** (`[]composite`) — Search results containing URLs and snippets.
  - Composite: `web_search_result`
  - **`url`** (`text`) **REQUIRED** — URL of the search result.
  - **`snippet`** (`text`) — Snippet of the result content.
