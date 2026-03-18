# `create_contact` — Schema Reference

## Input Port: `input`

- **`account`** (`id`) — The account that the contact is associated with.
  - ID type: `account`
- **`description`** (`text`) — The description of the contact.
- **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
- **`email`** (`text`) — The email address of the contact.
- **`external_refs`** (`[]text`) — External references associated with the contact.
- **`phone_numbers`** (`[]text`) — The phone numbers of the contact.
- **`rev_org`** (`id`) — The workspace that the contact is associated with.
  - ID type: `revo`
- **`tags`** (`[]composite`) — The tags associated with the contact.
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
- **`description`** (`text`) — The description of the contact.
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
- **`email`** (`text`) — Email address of the user.
- **`full_name`** (`text`) — Full name of the user.
- **`is_verified`** (`bool`) — Whether the user is verified or not
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
- **`phone_numbers`** (`[]text`) — Phone numbers of the user.
- **`rev_org`** (`composite`) — The workspace that the contact is associated with.
  - Composite: `_gen:rev_org`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — Name of the Organization.
  - **`id`** (`id`) **REQUIRED** — The id of the workspace.
    - ID type: `revo`
- **`tags`** (`[]composite`) — Tags associated with the object.
  - Composite: `_gen:tags`
  - **`tag`** (`composite`)
    - Composite: `_gen:tag`
- **`id`** (`id`) **REQUIRED** — Globally unique object ID.
  - ID type: `revu`
