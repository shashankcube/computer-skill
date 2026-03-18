# `get_customer` — Schema Reference

## Input Port: `input`

- **`id`** (`id`) **REQUIRED** — The id of the customer.
  - ID type: `revu`

## Output Port: `output`

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
