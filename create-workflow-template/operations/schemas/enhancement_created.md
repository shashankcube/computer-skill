# `enhancement_created` — Schema Reference

## Input Port: `input`

_No input fields (trigger or system-provided)._

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
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `sys_user`
- **`created_date`** (`timestamp`) — Timestamp when the enhancement was created.
- **`description`** (`text`) — Description of the enhancement.
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`name`** (`text`) **REQUIRED** — Name of the enhancement.
- **`owned_by`** (`[]composite`) **REQUIRED**
  - Composite: `_gen:user-summary`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — ID of the user
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `sys_user`
- **`tags`** (`[]composite`) — Tags associated with the object.
  - Composite: `_gen:tags`
  - **`tag`** (`composite`)
    - Composite: `_gen:tag`
- **`target_close_date`** (`timestamp`) — Target close date for the object.
- **`target_start_date`** (`timestamp`) — Target start date for the object.
- **`id`** (`id`) **REQUIRED** — ID of the enhancement.
  - ID type: `enhancement`
