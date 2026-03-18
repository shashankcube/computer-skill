# `update_brand` — Schema Reference

## Input Port: `input`

- **`description`** (`text`) — The updated description to set for the brand.
  - Validation: max_len=65536
- **`id`** (`id`) **REQUIRED** — The ID of the brand to be updated.
  - ID type: `brand`
- **`name`** (`text`) — The updated name to set for the brand.
  - Validation: min_len=1, max_len=255

## Output Port: `output`

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
    - Allowed: `dev_user`, `sys_user`, `service_account`
- **`created_date`** (`timestamp`) — Timestamp when the object was created.
- **`description`** (`text`) — The description of the brand.
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`modified_by`** (`composite`)
  - Composite: `_gen:modified_by`
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
- **`modified_date`** (`timestamp`) — Timestamp when the object was last modified.
- **`name`** (`text`) — The name of the brand.
- **`id`** (`id`) **REQUIRED** — Globally unique Brand ID.
  - ID type: `brand`
