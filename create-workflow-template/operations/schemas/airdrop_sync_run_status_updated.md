# `airdrop_sync_run_status_updated` — Schema Reference

## Input Port: `input`

- **`status`** (`[]enum`) **REQUIRED** — The status that the sync run transitions to. The trigger will only be fired if the sync run status changes to one of these values. NOTE: The Sync Run ID will be available only if the sync run status is in final state(completed or failed).
  - Allowed: `waiting_for_user_input`, `completed`, `failed`

## Output Port: `output`

- **`created_by`** (`composite`) — User who created the sync run
  - Composite: `_gen:implicit:created-by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `sys_user`, `service_account`
- **`sync_unit`** (`id`) — Globally unique object ID of a Sync Unit
  - ID type: `sync_unit`
- **`sync_run`** (`composite`) — Object for holding sync run data
  - Composite: `_gen:implicit:sync-run`
  - **`ended_at`** (`timestamp`) — The time when a sync was ended.
  - **`item_analytics_artifact`** (`composite`)
    - Composite: `_gen:item_analytics_artifact`
  - **`mode`** (`enum`) — The direction/mode of a sync run.
    - Allowed: `initial`, `sync_from_devrev`, `sync_to_devrev`
  - **`progress`** (`composite`) — Progress.
    - Composite: `_gen:progress`
  - **`started_at`** (`timestamp`) — The time when a sync was started.
- **`id`** (`id`) — Globally unique object ID of a Sync Run
  - ID type: `sync_history`
