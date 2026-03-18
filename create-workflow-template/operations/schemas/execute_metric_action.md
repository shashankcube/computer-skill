# `execute_metric_action` — Schema Reference

## Input Port: `input`

- **`action`** (`enum`) **REQUIRED**
  - Allowed: `complete`, `pause`, `restart`, `resume`, `start`
- **`event_date`** (`timestamp`) **REQUIRED** — Timestamp of the event.
- **`metric`** (`id`) **REQUIRED** — The metric's ID for which the metric action is to be executed.
  - ID type: `metric_definition`
- **`object`** (`id`) **REQUIRED** — The underlying object's ID on which the metric action is to be executed.
  - ID type: `conversation`, `incident`, `issue`, `ticket`
