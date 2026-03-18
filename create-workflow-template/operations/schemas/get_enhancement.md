# `get_enhancement` — Schema Reference

## Input Port: `input`

- **`id`** (`id`) **REQUIRED** — The id of the enhancement.
  - ID type: `enhancement`

## Output Port: `output`

- **`actual_close_date`** (`timestamp`) — Actual close date for the object.
- **`actual_start_date`** (`timestamp`) — Actual start date for the object.
- **`created_by`** (`composite`)
  - Composite: `_gen:created_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — ID of the user
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `service_account`, `sys_user`
- **`created_date`** (`timestamp`) — Timestamp when the Enhancement was created.
- **`description`** (`text`) — Description of the Enhancement.
- **`display_id`** (`text`) — Human-readable Enhancement ID unique to the Dev organization.
- **`modified_by`** (`composite`)
  - Composite: `_gen:modified_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — The id of the user.
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `service_account`, `sys_user`
- **`modified_date`** (`timestamp`) — Timestamp when the Enhancement was modified.
- **`name`** (`text`) **REQUIRED** — Name of the Enhancement.
- **`owned_by`** (`[]composite`) **REQUIRED**
  - Composite: `_gen:user-summary`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — ID of the user.
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `service_account`, `sys_user`
- **`tags`** (`[]composite`) — Tags associated with the object.
  - Composite: `_gen:tags`
  - **`tag`** (`composite`)
    - Composite: `_gen:tag`
- **`target_close_date`** (`timestamp`) — Target close date for the object.
- **`target_start_date`** (`timestamp`) — Target start date for the object.
- **`id`** (`id`) **REQUIRED** — The ID of the Enhancement
  - ID type: `enhancement`
- **`stage_v2`** (`composite`) — The stage of the enhancement.
  - Composite: `_gen:stage_v2`
  - **`stage`** (`composite`)
    - Composite: `_gen:stage`
  - **`state`** (`composite`)
    - Composite: `_gen:state`
