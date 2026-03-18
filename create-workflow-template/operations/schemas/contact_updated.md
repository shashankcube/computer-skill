# `contact_updated` — Schema Reference

## Input Port: `input`

- **`fields_to_watch`** (`[]enum`) **REQUIRED** — Fields to watch for changes. The trigger will only be fired if any of these fields are updated.
  - Allowed: `description`, `display_name`, `email`, `phone_numbers`, `external_refs`, `tags`

## Output Port: `output`

- **`created_by`** (`composite`)
  - Composite: `_gen:created_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED** — User type
    - Allowed: `dev_user`, `service_account`, `sys_user`
- **`created_date`** (`timestamp`) — Timestamp when the contact was created.
- **`description`** (`text`) — Description of the contact.
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
- **`email`** (`text`) — Email address of the user.
- **`full_name`** (`text`) — Full name of the user.
- **`is_verified`** (`bool`) — Whether the contact is verified or not.
- **`modified_by`** (`composite`)
  - Composite: `_gen:modified_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED** — User type
    - Allowed: `dev_user`, `service_account`, `sys_user`
- **`modified_date`** (`timestamp`) — Timestamp when the contact was last modified.
- **`phone_numbers`** (`[]text`) — Phone numbers of the user.
- **`rev_org`** (`composite`)
  - Composite: `_gen:rev_org`
- **`tags`** (`[]composite`) — Tags associated with the contact.
  - Composite: `_gen:tags`
  - **`tag`** (`composite`)
    - Composite: `_gen:tag`
- **`old_rev_user`** (`composite`) — Old contact object
  - Composite: `_gen:implicit:rev-user`
  - **`created_by`** (`composite`)
    - Composite: `_gen:created_by`
  - **`created_date`** (`timestamp`) — Timestamp when the object was created.
  - **`description`** (`text`) — Description of the Rev user.
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`is_verified`** (`bool`) — Whether the Rev user is verified or not.
  - **`modified_by`** (`composite`)
    - Composite: `_gen:modified_by`
  - **`modified_date`** (`timestamp`) — Timestamp when the object was last modified.
  - **`phone_numbers`** (`[]text`) — Phone numbers of the user.
  - **`tags`** (`[]composite`) — Tags associated with the object.
    - Composite: `_gen:tags`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `revu`
- **`id`** (`id`) **REQUIRED** — Globally unique object ID.
  - ID type: `revu`
