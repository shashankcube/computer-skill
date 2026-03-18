# `create_dm` — Schema Reference

## Input Port: `input`

- **`is_default`** (`bool`) — Whether this is the default DM for messaging the constituent users. If true, then this DM is always returned when opening a DM for the users. Note only one DM may be the default for a given set of users. By Default, this is true.
- **`title`** (`text`) — The title for the chat.
- **`users`** (`[]id`) **REQUIRED** — The users to send direct messages to. The authenticated user must be included in this list.
  - ID type: `devu`, `sysu`, `svcacc`

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
  - **`type`** (`enum`) **REQUIRED** — The type of the user.
    - Allowed: `devu`, `sysu`, `svcacc`
- **`created_date`** (`timestamp`) — Timestamp when the object was created.
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
  - **`type`** (`enum`) **REQUIRED** — The type of the user.
    - Allowed: `devu`, `sysu`, `svcacc`
- **`modified_date`** (`timestamp`) — Timestamp when the object was last modified.
- **`title`** (`text`) — The title given to the chat.
- **`users`** (`[]composite`) **REQUIRED**
  - Composite: `_gen:user-summary`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED** — The type of the user.
    - Allowed: `devu`, `sysu`, `svcacc`
- **`id`** (`id`) **REQUIRED** — Globally unique object ID.
  - ID type: `dm`
