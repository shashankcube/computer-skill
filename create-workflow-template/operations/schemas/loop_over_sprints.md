# `loop_over_sprints` — Schema Reference

## Input Port: `input`

- **`applies_to_part`** (`composite`) — The filter for applies to part.
  - Composite: `_gen:applies_to_part`
  - **`include_child_parts`** (`bool`) — Whether to include items belonging to children of any of the provided parts.
  - **`parts`** (`[]id`) **REQUIRED** — Part IDs to filter on.
    - ID type: `product`, `feature`, `capability`, `enhancement`, `runnable`, `linkable`
- **`created_date`** (`composite`) — Provides ways to specify date ranges on objects.
  - Composite: `_gen:created_date`
  - **`after`** (`timestamp`) — Filters for objects created after the provided timestamp (inclusive).
  - **`before`** (`timestamp`) — Filters for objects created before the provided timestamp (inclusive).
  - **`type`** (`enum`) **REQUIRED** — Type of date filter.
    - Allowed: `range`
- **`parent_id`** (`[]id`) — Parent Sprint Board ID
  - ID type: `vista`
- **`sort_by`** (`[]uenum`) — Fields to sort the sprints by.
- **`state`** (`[]enum`) — Denotes the state of the sprint
  - Allowed: `active`, `completed`, `planned`
- **`limit`** (`int`) — The maximum number of sprints to return.Maximum is 1000.
- **`created_by`** (`[]id`) — Filters for sprints created by any of these users.
  - ID type: `devu`, `svcacc`, `sysu`

## Input Port: `block_callback`

_No input fields (trigger or system-provided)._

## Output Port: `block_start`

- **`end_date`** (`timestamp`) — Timestamp when the vista group item ends.
- **`name`** (`text`) **REQUIRED** — Name of the group.
- **`object_type`** (`enum`) **REQUIRED** — Type of DevRev object for which the grouped vista is applicable.
  - Allowed: `conversations`, `parts`, `works`
- **`parent`** (`composite`)
  - Composite: `_gen:parent`
  - **`display_id`** (`text`) **REQUIRED** — Human-readable object ID unique to the Dev organization.
  - **`flavor`** (`enum`) — Denotes the use case of the vista.
    - Allowed: `nnl`, `sprint_board`, `support_inbox`
  - **`id`** (`id`) **REQUIRED** — Parent vista ID.
    - ID type: `vista`
  - **`name`** (`text`) **REQUIRED** — Name of the parent vista.
  - **`type`** (`text`) **REQUIRED** — Type of the parent vista.
- **`start_date`** (`timestamp`) — Timestamp when the vista group item starts.
- **`state`** (`enum`) — Defines the state of the group item.
  - Allowed: `active`, `completed`, `planned`
- **`type`** (`enum`) **REQUIRED** — Type of conversations vista group item.
  - Allowed: `curated`, `dynamic`
- **`id`** (`id`) **REQUIRED** — The ID of the Sprint
  - ID type: `vista_group_item`
