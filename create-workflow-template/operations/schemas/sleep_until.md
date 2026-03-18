# `sleep_until` — Schema Reference

Pause the workflow until a specific date and time. You can also adjust this time by adding or subtracting some duration. Use this to delay actions, like sending reminders, until the right moment. If the time is in the past, the workflow will resume immediately.

## Input Port: `input`

- **`sleep_until`** (`timestamp`) **REQUIRED** — Select the time when the workflow should resume.
- **`offset_duration`** (`composite`) — Choose if you want to add or subtract extra time from the date and time you picked.
  - Composite: `offset`
  - **`add_or_subtract`** (`enum`) **REQUIRED** — Choose whether to add or subtract the duration from the specified timestamp.
    - Allowed: `add`, `subtract`
  - **`duration`** (`composite`) **REQUIRED** — Duration of time to add or subtract.
    - Composite: `devrev:duration`

## Output Port: `output`

No output fields (empty schema). The workflow simply resumes at the specified time.
