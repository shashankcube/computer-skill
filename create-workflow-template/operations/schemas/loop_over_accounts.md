# `loop_over_accounts` — Schema Reference

## Input Port: `input`

- **`created_by`** (`[]id`) — Filters for accounts created by the specified user(s).
  - ID type: `devu`, `svcacc`, `sysu`
- **`created_date`** (`composite`)
  - Composite: `_gen:created_date`
  - **`after`** (`timestamp`) — Filters for objects created after the provided timestamp (inclusive).
  - **`before`** (`timestamp`) — Filters for objects created before the provided timestamp (inclusive).
- **`display_name`** (`[]text`) — Array of display names of accounts to be filtered.
- **`domains`** (`[]text`) — Domains for accounts to be filtered.
- **`external_refs`** (`[]text`) — Array of references of accounts to be filtered.
- **`modified_date`** (`composite`)
  - Composite: `_gen:modified_date`
  - **`after`** (`timestamp`) — Filters for objects created after the provided timestamp (inclusive).
  - **`before`** (`timestamp`) — Filters for objects created before the provided timestamp (inclusive).
- **`owned_by`** (`[]id`) — Filters for accounts owned by the specified user(s).
  - ID type: `devu`, `sysu`
- **`stage`** (`[]id`) — Filters for accounts on specified stages.
  - ID type: `custom_stage`
- **`tier`** (`[]text`) — Tier of the accounts to be filtered.
- **`websites`** (`[]text`) — Array of websites of accounts to be filtered.
- **`tags`** (`[]id`) — Filters for accounts with any of the provided tags.
  - ID type: `tag`
- **`limit`** (`int`) — The maximum number of accounts to return. Maximum is 1000.

## Input Port: `block_callback`

_No input fields (trigger or system-provided)._

## Output Port: `block_start`

- **`artifacts`** (`[]composite`)
  - Composite: `_gen:artifact-summary`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`file`** (`composite`) — Defines a file object.
    - Composite: `_gen:file`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `artifact`
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
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `rev_user`, `service_account`, `sys_user`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `svcacc`, `sysu`
- **`created_date`** (`timestamp`) — Timestamp when the object was created.
- **`description`** (`text`) — Description of the corresponding Account.
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`display_name`** (`text`) — Name of the Organization.
- **`domains`** (`[]text`) — Company's domain names. Example - 'devrev.ai'.
- **`external_refs`** (`[]text`) — External refs are unique identifiers from your customer system of records, stored as a list.
- **`modified_by`** (`composite`)
  - Composite: `_gen:modified_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`display_picture`** (`composite`)
    - Composite: `_gen:display_picture`
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `rev_user`, `service_account`, `sys_user`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `svcacc`, `sysu`
- **`modified_date`** (`timestamp`) — Timestamp when the object was last modified.
- **`owned_by`** (`[]composite`) **REQUIRED**
  - Composite: `_gen:user-summary`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`display_picture`** (`composite`)
    - Composite: `_gen:display_picture`
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `rev_user`, `service_account`, `sys_user`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `svcacc`, `sysu`
- **`primary_account`** (`composite`)
  - Composite: `_gen:primary_account`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — Name of the Organization.
  - **`id`** (`id`) **REQUIRED** — Globally unique Account ID.
    - ID type: `account`
- **`stock_schema_fragment`** (`id`) — Stock schema fragment.
  - ID type: `app_fragment`, `custom_type_fragment`, `stock_sf`, `tenant_fragment`
- **`tags`** (`[]composite`) — Tags associated with an object.
  - Composite: `_gen:tags`
  - **`tag`** (`composite`)
    - Composite: `_gen:tag`
  - **`value`** (`text`) — The value for the object's association with the tag.
- **`tier`** (`text`) — The Tier of the corresponding Account.
- **`websites`** (`[]text`) — Company's website links. Filling in this property will also fill in domain. Example - 'www.devrev.ai'.
- **`id`** (`id`) **REQUIRED** — Globally unique Account ID.
  - ID type: `account`
