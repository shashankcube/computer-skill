# `get_part` — Schema Reference

## Input Port: `input`

- **`id`** (`id`) **REQUIRED** — The id of the part.
  - ID type: `capability`, `feature`, `product`, `runnable`, `linkable`, `enhancement`

## Output Port: `output`

- **`created_by`** (`composite`)
  - Composite: `_gen:created_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — ID of the user.
    - ID type: `revu`, `devu`, `sysu`, `svcacc`
- **`created_date`** (`timestamp`) — Timestamp when the part was created.
- **`description`** (`text`) — Description of the part.
- **`display_id`** (`text`) — Human-readable part ID unique to the Dev organization.
- **`modified_by`** (`composite`)
  - Composite: `_gen:modified_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `rev_user`, `service_account`, `sys_user`
  - **`id`** (`id`) **REQUIRED** — The id of the user.
    - ID type: `devu`, `revu`, `sysu`, `svcacc`
- **`modified_date`** (`timestamp`) — Timestamp when the part was modified.
- **`name`** (`text`) **REQUIRED** — Name of the part.
- **`owned_by`** (`[]composite`) **REQUIRED**
  - Composite: `_gen:user-summary`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `rev_user`, `service_account`, `sys_user`
  - **`id`** (`id`) **REQUIRED** — ID of the user.
    - ID type: `revu`, `devu`, `sysu`, `svcacc`
- **`tags`** (`[]composite`) — Tags associated with the object.
  - Composite: `_gen:tags`
  - **`tag`** (`composite`)
    - Composite: `_gen:tag`
  - **`value`** (`text`) — The value for the object's association with the tag.
- **`id`** (`id`) **REQUIRED** — The ID of the part
  - ID type: `capability`, `feature`, `product`, `runnable`, `linkable`, `enhancement`
