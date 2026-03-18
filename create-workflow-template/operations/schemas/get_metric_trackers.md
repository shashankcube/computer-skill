# `get_metric_trackers` — Schema Reference

## Input Port: `input`

- **`metric`** (`id`) **REQUIRED** — The ID of the metric that is being tracked.
  - ID type: `metric_definition`
- **`object`** (`id`) **REQUIRED** — The ID of the underlying object on which the metric is being tracked.
  - ID type: `conversation`, `incident`, `issue`, `ticket`

## Output Port: `output`

- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`last_started_at`** (`timestamp`) — The time the metric was last started if it's in a running state.
- **`status`** (`enum`) **REQUIRED** — The current status of the metric.
  - Allowed: `completed`, `paused`, `running`
- **`time_consumed`** (`int`) — The amount of time already consumed when the metric was last stopped. Units are same as present in metric definition.
- **`type`** (`enum`) **REQUIRED**
  - Allowed: `time_metric_tracker`
- **`id`** (`id`) **REQUIRED** — Globally unique object ID.
  - ID type: `metric_tracker`
