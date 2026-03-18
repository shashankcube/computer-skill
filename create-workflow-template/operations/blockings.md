# Blocking Operations (2)

> Pause/wait operations that suspend a workflow until a condition or duration is met.


## 1. Sleep For

- **Slug:** `sleep_for`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.sleep_for`
- **Tags:** HIPAA

Pause the workflow for a specified duration

### Input (`input`)

- `duration` (composite) **(required)** — Duration to sleep for

---

## 2. Sleep Until

- **Slug:** `sleep_until`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.sleep_until`
- **Tags:** HIPAA

Pause the workflow until a specific date and time. You can also adjust this time by adding or subtracting some duration. Use this to delay actions, like sending reminders, until the right moment. If the time is in the past, the workflow will resume immediately.

### Input (`input`)

- `sleep_until` (timestamp) **(required)** — Select the time when the workflow should resume
- `offset_duration` (composite) — Choose if you want to add or subtract extra time from the date and time you picked
  - `add_or_subtract` (enum) **(required)** — Choose whether to add or subtract the duration from the specified timestamp
    - Allowed: `add`, `subtract`
  - `duration` (composite) **(required)** — Duration of time to add or subtract

---
