# `loop_over_customers` — Schema Reference

## Input Port: `input`

- **`associations`** (`[]id`) — Filters for customers with specified associations (account/workspace).
  - ID type: `account`, `revo`
- **`created_by`** (`[]id`) — Filters for customers that were created by the specified user(s).
  - ID type: `devu`, `svcacc`, `sysu`
- **`created_date`** (`composite`)
  - Composite: `_gen:created_date`
  - **`after`** (`timestamp`) — Filters for objects created after the provided timestamp (inclusive).
  - **`before`** (`timestamp`) — Filters for objects created before the provided timestamp (inclusive).
  - **`type`** (`enum`) **REQUIRED** — Type of date filter.
    - Allowed: `range`
- **`email`** (`[]text`) — List of emails of customers to be filtered.
- **`external_ref`** (`[]text`) — List of external refs to filter customers for.
- **`external_refs`** (`[]text`) — Filters for customers with the provided external_refs.
- **`is_verified`** (`bool`) — Value of is_verified field to filter the customers.
- **`modified_date`** (`composite`)
  - Composite: `_gen:modified_date`
  - **`after`** (`timestamp`) — Filters for objects created after the provided timestamp (inclusive).
  - **`before`** (`timestamp`) — Filters for objects created before the provided timestamp (inclusive).
  - **`type`** (`enum`) **REQUIRED** — Type of date filter.
    - Allowed: `range`
- **`phone_numbers`** (`[]text`) — List of phone numbers, in E.164 format, to filter customers on.
- **`rev_org`** (`[]id`) — List of IDs of customer organizations to be filtered.
  - ID type: `revo`
- **`limit`** (`int`) — The maximum number of customers to return. Maximum is 1000.
- **`tags`** (`[]id`) — List of tags to be filtered.
  - ID type: `tag`

## Input Port: `block_callback`

_No input fields (trigger or system-provided)._

## Output Port: `block_start`

- **`created_by`** (`composite`)
  - Composite: `_gen:created_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED** — The type of the user.
    - Allowed: `devu`, `sysu`, `svcacc`
- **`created_date`** (`timestamp`) — Timestamp when the contact was created.
- **`description`** (`text`) — The description of the contact.
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
- **`display_picture`** (`composite`)
  - Composite: `_gen:user-base-properties.display_picture`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `artifact`
- **`email`** (`text`) — Email address of the user.
- **`full_name`** (`text`) — Full name of the user.
- **`is_verified`** (`bool`) — Whether the customer is verified or not.
- **`modified_by`** (`composite`)
  - Composite: `_gen:modified_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `rev_user`, `service_account`, `sys_user`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
- **`modified_date`** (`timestamp`) — Timestamp when the contact was last modified.
- **`phone_numbers`** (`[]text`) — Phone numbers of the user.
- **`rev_org`** (`composite`)
  - Composite: `_gen:rev_org`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — Name of the Organization.
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `account`, `rev_org`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `revo`
- **`state`** (`enum`) — State of the user.
  - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
- **`tags`** (`[]composite`) — Tags associated with the contact.
  - Composite: `_gen:tags`
  - **`tag`** (`composite`)
    - Composite: `_gen:tag`
- **`id`** (`id`) **REQUIRED** — Globally unique object ID.
  - ID type: `revu`
- **`external_ref`** (`text`) — External ref is a mutable unique identifier for a user within the Customer organization from your primary customer record. If none is available, a good alternative is the email address/phone number which could uniquely identify the user. If none is specified, a system-generated identifier will be assigned to the user.
