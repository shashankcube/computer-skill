# `sleep_for` — Schema Reference

Pause the workflow for a specified duration.

## Input Port: `input`

- **`duration`** (`composite`) **REQUIRED** — Duration to sleep for.
  - Composite: `devrev:duration`

## Output Port: `output`

No output fields (empty schema). The workflow simply resumes after the duration.
