# `get_feature` — Schema Reference

## Input Port: `input`

- **`id`** (`id`) **REQUIRED** — The id of the feature.
  - ID type: `feature`

## Output Port: `output`

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
- **`created_date`** (`timestamp`) — Timestamp when the Feature was created.
- **`description`** (`text`) — Description of the Feature.
- **`display_id`** (`text`) — Human-readable Feature ID unique to the Dev organization.
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
- **`modified_date`** (`timestamp`) — Timestamp when the Feature was modified.
- **`name`** (`text`) **REQUIRED** — Name of the Feature.
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
- **`id`** (`id`) **REQUIRED** — The ID of the Feature
  - ID type: `feature`
