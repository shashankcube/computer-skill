# `update_account` — Schema Reference

## Input Port: `input`

- **`description`** (`text`) — Updated description of the account.
- **`display_name`** (`text`) — Updated display name for the account.
- **`domains`** (`[]text`) — Updated list of company's domain names. Example - ['devrev.ai'].
- **`external_refs`** (`[]text`) — Updated External Refs of account.
- **`id`** (`id`) **REQUIRED** — The ID of account to update.
  - ID type: `account`
- **`tier`** (`enum`) — Updated tier of the account.
  - Allowed: `tier_1`, `tier_2`, `tier_3`
- **`owned_by`** (`[]id`) — Updated list of the users owning this account.
  - ID type: `devu`, `sysu`

## Output Port: `output`

- **`created_by`** (`composite`)
  - Composite: `_gen:created_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `rev_user`, `service_account`, `sys_user`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
- **`created_date`** (`timestamp`) — Timestamp when the account was created.
- **`description`** (`text`) — Description of the corresponding Account.
- **`display_id`** (`text`) — Human-readable account ID unique to the Dev organization.
- **`display_name`** (`text`) — Name of the Organization.
- **`domains`** (`[]text`) — Company's domain names. Example - 'devrev.ai'.
- **`external_refs`** (`[]text`) — External refs are unique identifiers from your customer system of records, stored as a list.
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
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
- **`modified_date`** (`timestamp`) — Timestamp when the account was modified.
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
- **`tier`** (`enum`) — The Tier of the corresponding Account.
  - Allowed: `tier_1`, `tier_2`, `tier_3`
- **`id`** (`id`) **REQUIRED** — The ID of the account updated.
  - ID type: `account`
