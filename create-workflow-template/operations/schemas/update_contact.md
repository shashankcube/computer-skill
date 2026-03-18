# `update_contact` — Schema Reference

## Input Port: `input`

- **`description`** (`text`) — The description of the contact.
- **`display_name`** (`text`) — The display name of the contact.
- **`email`** (`text`) — The email of the contact.
- **`external_refs`** (`[]text`) — External references associated with the contact.
- **`phone_numbers`** (`[]text`) — The phone numbers of the contact.
- **`tags`** (`[]composite`) — The tags of the contact.
  - Composite: `_gen:tags`
  - **`id`** (`id`) **REQUIRED** — The ID of the tag.
    - ID type: `tag`
- **`id`** (`id`) **REQUIRED** — ID of contact to be updated.
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
