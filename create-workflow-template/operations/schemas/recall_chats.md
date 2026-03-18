# `recall_chats` — Schema Reference

## Input Port: `input`

- **`query`** (`text`) — Extract only specific topic keywords. Drop time words ("yesterday", "last week", "last fiscal year") and generic fillers ("find", "what was"). If the prompt has no clear topic (e.g., "what were the topics of conversations last week?"), leave this empty so only the date filters are used.
- **`start_date`** (`timestamp`) — Start of date range (ISO 8601 timestamp). If any time reference exists, always set both start_date and end_date. Examples: "last fiscal year" -> first day of that fiscal year; "last week" -> first day of last week; "last month" -> first day of last month. Leave empty only when no time reference.
- **`end_date`** (`timestamp`) — End of date range (ISO 8601 timestamp). If any time reference exists, always set both start_date and end_date. Examples: "last fiscal year" -> last day of that fiscal year; "last week" -> last day of last week; "last month" -> last day of last month. Leave empty only when no time reference.
- **`limit`** (`int`) — Maximum number of results to return. Defaults to 10 if not specified. Valid range: 1-100.

## Output Port: `output`

- **`results_count`** (`int`) **REQUIRED** — Number of conversations/messages found
- **`results`** (`text`) **REQUIRED** — Array of search results with id, title, snippet, and comments (if any)
