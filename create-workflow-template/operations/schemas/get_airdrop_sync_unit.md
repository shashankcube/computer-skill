# `get_airdrop_sync_unit` — Schema Reference

## Input Port: `input`

- **`id`** (`id`) **REQUIRED** — The Sync Unit ID
  - ID type: `sync_unit`

## Output Port: `output`

- **`created_date`** (`timestamp`) — Timestamp when the Airdrop Sync was created.
- **`external_sync_unit_name`** (`text`) — The name of the sync unit in the external system.
- **`external_system_id`** (`text`) — The ID of the external system.
- **`external_system_name`** (`text`) — The name of the external system.
- **`external_system_type`** (`enum`)
  - Allowed: `adaas`, `confluence`, `github`, `hubspot`, `jira`, `jira_data_center`, `linear`, `rocketlane`, `s3`, `salesforce_sales`... (13 total)
- **`is_archived`** (`bool`) — The flag signaling if sync unit is archived.
- **`modified_date`** (`timestamp`) — Timestamp when the Airdrop Sync was last modified.
- **`name`** (`text`) — The name of the sync unit.
- **`sync_run`** (`composite`) — Object for holding run-specific data.
  - Composite: `_gen:sync_run`
  - **`ended_at`** (`timestamp`) — The time when a sync was ended.
  - **`item_analytics_artifact`** (`composite`)
    - Composite: `_gen:item_analytics_artifact`
  - **`mode`** (`enum`) — The direction/mode of a sync run.
    - Allowed: `initial`, `sync_from_devrev`, `sync_to_devrev`
  - **`progress`** (`composite`) — Progress.
    - Composite: `_gen:progress`
  - **`started_at`** (`timestamp`) — The time when a sync was started.
- **`sync_type`** (`enum`) — Type of sync preferences.
  - Allowed: `manual`, `periodic`
- **`id`** (`id`) **REQUIRED** — Globally unique object ID.
  - ID type: `sync_unit`
