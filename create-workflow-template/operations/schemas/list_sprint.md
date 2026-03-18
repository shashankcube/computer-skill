# `list_sprint` — Schema Reference

## Input Port: `input`

- **`cursor`** (`text`) — The cursor to resume iteration from. If not provided, then iteration starts from the beginning.
- **`limit`** (`int`) — Maximum number of sprints to return. If not provided, fetches upto 50 items.
- **`parent_id`** (`[]id`) — Parent Sprint Board ID
  - ID type: `vista`
- **`sort_by`** (`[]uenum`) — Fields to sort the sprints by.
- **`state`** (`[]enum`) — Denotes the state of the sprint
  - Allowed: `active`, `completed`, `planned`

## Output Port: `output`

- **`next_cursor`** (`text`) — The cursor used to iterate subsequent results in accordance to the sort order. If not set, then no later elements exist.
- **`prev_cursor`** (`text`) — The cursor used to iterate preceding results in accordance to the sort order. If not set, then no prior elements exist.
- **`vista_group`** (`[]composite`)
  - Composite: `_gen:vista-group`
  - **`end_date`** (`timestamp`) — Timestamp when the vista group item ends.
  - **`name`** (`text`) **REQUIRED** — Name of the group.
  - **`object_type`** (`enum`) **REQUIRED** — Type of DevRev object for which the grouped vista is applicable.
    - Allowed: `conversations`, `parts`, `works`
  - **`parent`** (`composite`)
    - Composite: `_gen:parent`
  - **`start_date`** (`timestamp`) — Timestamp when the vista group item starts.
  - **`state`** (`enum`) — Defines the state of the group item.
    - Allowed: `active`, `completed`, `planned`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `vista_group_item`
