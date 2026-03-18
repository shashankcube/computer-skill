# `update_enhancement` — Schema Reference

## Input Port: `input`

- **`description`** (`text`) — The updated description of the enhancement.
- **`name`** (`text`) — The updated name of the enhancement.
- **`owned_by`** (`composite`)
  - Composite: `_gen:owned_by`
  - **`set`** (`[]id`) — The user that owns the enhancement.
    - ID type: `devu`, `sysu`, `svcacc`
- **`release_notes`** (`text`) — Updates the release notes of the enhancement.
  - Validation: max_len=32768
- **`target_close_date`** (`timestamp`) — Updates the target close date of the enhancement.
- **`target_start_date`** (`timestamp`) — Updates the target start date of the enhancement.
- **`id`** (`id`) **REQUIRED** — The ID of the enhancement
  - ID type: `enhancement`
- **`stage_v2`** (`id`) — The stage of the enhancement.
  - ID type: `custom_stage`

## Output Port: `output`

- **`actual_close_date`** (`timestamp`) — Actual close date for the object.
- **`actual_start_date`** (`timestamp`) — Actual start date for the object.
- **`created_by`** (`composite`)
  - Composite: `_gen:created_by`
  - **`display_id`** (`text`) — Display ID of the user who created the enhancement.
  - **`display_name`** (`text`) — The display name of the user who created the enhancement.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) — ID of the user.
    - ID type: `devu`, `sysu`, `svcacc`
- **`created_date`** (`timestamp`) — The date the enhancement was created.
- **`description`** (`text`) — The description of the enhancement.
- **`display_id`** (`text`) — The display ID of the enhancement.
- **`modified_by`** (`composite`)
  - Composite: `_gen:modified_by`
  - **`display_id`** (`text`) — Display ID of the user who modified the enhancement.
  - **`display_name`** (`text`) — The display name of the user who modified the enhancement.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) — ID of the user.
    - ID type: `devu`, `sysu`, `svcacc`
- **`modified_date`** (`timestamp`) — The date the enhancement was last modified.
- **`name`** (`text`) **REQUIRED** — The name of the enhancement.
- **`owned_by`** (`[]composite`) **REQUIRED**
  - Composite: `_gen:user-summary`
- **`tags`** (`[]composite`) — The tags associated with the enhancement.
  - Composite: `_gen:tags`
  - **`tag`** (`composite`)
    - Composite: `_gen:tag`
- **`target_close_date`** (`timestamp`) — Target close date for the object.
- **`target_start_date`** (`timestamp`) — Target start date for the object.
- **`id`** (`id`) **REQUIRED** — ID of the enhancement.
  - ID type: `enhancement`
- **`stage_v2`** (`composite`) — The stage of the enhancement.
  - Composite: `_gen:stage_v2`
  - **`stage`** (`composite`)
    - Composite: `_gen:stage`
  - **`state`** (`composite`)
    - Composite: `_gen:state`
