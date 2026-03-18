# `get_complete_enhancement_details` — Schema Reference

## Input Port: `input`

- **`object_id`** (`id`) **REQUIRED** — Object to fetch context for.
  - ID type: `enhancement`, `issue`
- **`duration`** (`enum`) **REQUIRED** — Number of days to look back for timeline entries, comments, and linked work items
  - Allowed: `7`, `14`, `21`, `30`
  - Default: `7`

## Output Port: `output`

- **`context`** (`text`) — Fetches context on this object.
