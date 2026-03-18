# `sprint_updated` — Schema Reference

## Input Port: `input`

- **`fields_to_watch`** (`[]enum`) **REQUIRED** — Fields to watch for changes. The trigger will only be fired if any of these fields are updated.
  - Allowed: `name`, `state`, `start_date`, `end_date`, `parent`

## Output Port: `output`

- **`end_date`** (`timestamp`) — Timestamp when the vista ends.
- **`name`** (`text`) **REQUIRED** — Name of the group.
- **`start_date`** (`timestamp`) — Timestamp when the vista starts.
- **`state`** (`enum`) — Defines the state of the group item.
  - Allowed: `active`, `completed`, `planned`
- **`type`** (`enum`) **REQUIRED** — Type of the group object.
  - Allowed: `curated`, `dynamic`
- **`old_vista_group_item`** (`composite`) — Old sprint object
  - Composite: `_gen:implicit:vista_group_item`
  - **`end_date`** (`timestamp`) — Timestamp when the vista ends.
  - **`name`** (`text`) **REQUIRED** — Name of the group.
  - **`start_date`** (`timestamp`) — Timestamp when the vista starts.
  - **`state`** (`enum`) — Defines the state of the group item.
    - Allowed: `active`, `completed`, `planned`
  - **`type`** (`enum`) **REQUIRED** — Type of the group object.
    - Allowed: `curated`, `dynamic`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `vista_group_item`
  - **`parent`** (`composite`) — The sprint board this sprint belongs to
    - Composite: `_gen:parent`
- **`id`** (`id`) **REQUIRED** — Globally unique object ID.
  - ID type: `vista_group_item`
- **`parent`** (`composite`) — The sprint board this sprint belongs to
  - Composite: `_gen:parent`
  - **`id`** (`id`) **REQUIRED** — Parent vista ID.
    - ID type: `vista`
  - **`name`** (`text`) **REQUIRED** — Name of the parent vista.
  - **`display_id`** (`text`) **REQUIRED** — Human-readable object ID.
