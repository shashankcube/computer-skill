# `contact_created` — Schema Reference

## Input Port: `input`

_No input fields (trigger or system-provided)._

## Output Port: `output`

- **`created_by`** (`composite`)
  - Composite: `_gen:created_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED** — Type of the contact.
    - Allowed: `dev_user`, `service_account`, `sys_user`
- **`created_date`** (`timestamp`) — Timestamp when the contact was created.
- **`description`** (`text`) — The description of the contact.
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
- **`email`** (`text`) — Email address of the user.
- **`full_name`** (`text`) — Full name of the user.
- **`is_verified`** (`bool`) — Whether the contact is verified.
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
- **`tags`** (`[]composite`) — Tags associated with the contact.
  - Composite: `_gen:tags`
  - **`tag`** (`composite`)
    - Composite: `_gen:tag`
- **`id`** (`id`) **REQUIRED** — Globally unique object ID.
  - ID type: `revu`
