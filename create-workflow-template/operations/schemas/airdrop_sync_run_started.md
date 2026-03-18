# `airdrop_sync_run_started` — Schema Reference

## Input Port: `input`

_No input fields (trigger or system-provided)._

## Output Port: `output`

- **`created_by`** (`composite`)
  - Composite: `_gen:created_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`display_picture`** (`composite`)
    - Composite: `_gen:display_picture`
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `sys_user`, `service_account`
- **`sync_run`** (`composite`) — Object for holding run-specific data.
  - Composite: `_gen:sync_run`
  - **`item_analytics_artifact`** (`composite`)
    - Composite: `_gen:item_analytics_artifact`
  - **`mode`** (`enum`) — The direction/mode of a sync run.
    - Allowed: `initial`, `sync_from_devrev`, `sync_to_devrev`
  - **`started_at`** (`timestamp`) — The time when a sync was started.
- **`sync_unit`** (`id`) **REQUIRED** — Globally unique object ID of a Sync Unit
  - ID type: `sync_unit`
- **`id`** (`id`) **REQUIRED** — Globally unique object ID.
  - ID type: `sync_history`
