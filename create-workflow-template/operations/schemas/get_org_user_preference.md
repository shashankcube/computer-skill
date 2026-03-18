# `get_org_user_preference` — Schema Reference

## Input Port: `input`

- **`id`** (`id`) **REQUIRED** — The id of the organization user.
  - ID type: `devu`

## Output Port: `output`

- **`created_by`** (`composite`)
  - Composite: `_gen:created_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — ID of the user.
    - ID type: `devu`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`
- **`created_date`** (`timestamp`) — Timestamp when the object was created.
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`general_preferences`** (`composite`) — Preferences group for General settings.
  - Composite: `_gen:general_preferences`
  - **`availability`** (`composite`) — Preferences group for Availability.
    - Composite: `_gen:availability`
- **`modified_by`** (`composite`)
  - Composite: `_gen:modified_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — The id of the user.
    - ID type: `devu`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`
- **`modified_date`** (`timestamp`) — Timestamp when the object was last modified.
