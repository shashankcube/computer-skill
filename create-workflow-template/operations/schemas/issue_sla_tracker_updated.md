# `issue_sla_tracker_updated` — Schema Reference

## Input Port: `input`

- **`fields_to_watch`** (`[]enum`) **REQUIRED** — Fields to watch for changes. The trigger will only be fired if any of these fields are updated.
  - Allowed: `stage`

## Output Port: `output`

- **`metric_target_summaries`** (`[]composite`) **REQUIRED** — Summary of the metrics target being tracked in the SLA tracker.
  - Composite: `_gen:metric_target_summaries`
  - **`breached_at`** (`timestamp`) — For breached metrics the time they entered into breach. This is the same as what the target date was, unless the breach happened due to a different policy starting to apply.
  - **`completed_in`** (`int`) — For completed metrics the time (in minutes) it took to complete them. (Taking into account the schedule if any).
  - **`completed_in_seconds`** (`int`) — For completed metrics the time (in seconds) it took to complete them. (Taking into account the schedule if any).
  - **`is_out_of_schedule`** (`bool`) — If true, the schedule attached to this metric is out of schedule at the time of the query. It is not set for metrics in *completed* stage.
  - **`metric_definition`** (`composite`)
    - Composite: `_gen:metric_definition`
  - **`next_schedule_transition`** (`timestamp`) — The next time the schedule will change its state, if such is known.
  - **`org_schedule`** (`composite`)
    - Composite: `_gen:org_schedule`
  - **`org_schedule_version`** (`int`) — The version of the org schedule at the time this tracker was created.
  - **`remaining_time`** (`int`) — Time in minutes that remains on a paused metric.
  - **`target_time`** (`timestamp`) — Time at which the metric would breach SLA if no action taken.
  - **`warning_target_time`** (`timestamp`) — Time at which the metric would reach the SLA warning limit if no action taken.
  - **`stage`** (`enum`) **REQUIRED** — SLA stage of the object being tracked.
    - Allowed: `active`, `breached`, `completed`, `paused`, `warning`
  - **`status`** (`enum`) **REQUIRED** — Status of the object being tracked.
    - Allowed: `hit`, `in_progress`, `miss`
- **`modified_date`** (`timestamp`) — Timestamp when the object was last modified.
- **`sla`** (`composite`)
  - Composite: `_gen:sla`
  - **`name`** (`text`) **REQUIRED** — Human-readable name.
  - **`status`** (`enum`) **REQUIRED** — Status determines how an item can be used. In 'draft' status an item can be edited but can't be used. When 'published' the item can longer be edited but can be used. 'Archived' is read-only.
    - Allowed: `archived`, `draft`, `published`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `sla`
  - **`sla_type`** (`enum`) **REQUIRED** — Type of the SLA. Internal - for internal objects (issues, etc) external - for customer facing objects (tickets, conversations etc)
    - Allowed: `external`, `internal`
- **`id`** (`id`) **REQUIRED** — Globally unique object ID.
  - ID type: `sla_tracker`
- **`applies_to_id`** (`id`) **REQUIRED** — The id of the issue.
  - ID type: `issue`
- **`stage`** (`enum`) **REQUIRED** — SLA stage of the object being tracked.
  - Allowed: `active`, `breached`, `completed`, `paused`, `warning`
- **`status`** (`enum`) **REQUIRED** — Status of the object being tracked.
  - Allowed: `hit`, `in_progress`, `miss`
