# `create_account` — Schema Reference

## Input Port: `input`

- **`description`** (`text`) — Description of the account.
- **`display_name`** (`text`) **REQUIRED** — Name of the account.
- **`domains`** (`[]text`) — List of company's domain names. Example - ['devrev.ai'].
- **`tags`** (`[]composite`) — Tags associated with the account.
  - Composite: `_gen:tags`
  - **`id`** (`id`) **REQUIRED** — The ID of the tag.
    - ID type: `tag`

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
- **`description`** (`text`) — Description of the corresponding Account.
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`display_name`** (`text`) — Name of the Organization.
- **`domains`** (`[]text`) — Company's domain names. Example - 'devrev.ai'.
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
- **`owned_by`** (`[]composite`) **REQUIRED**
  - Composite: `_gen:user-summary`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `service_account`, `sys_user`
- **`tags`** (`[]composite`) — Tags associated with an object.
  - Composite: `_gen:tags`
  - **`tag`** (`composite`)
    - Composite: `_gen:tag`
- **`id`** (`id`) **REQUIRED** — Globally unique Account ID.
  - ID type: `account`
