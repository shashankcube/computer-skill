# `create_enhancement` — Schema Reference

## Input Port: `input`

- **`name`** (`text`) **REQUIRED** — The name of the enhancement.
- **`description`** (`text`) — Detailed description of what this enhancement will accomplish.
- **`owned_by`** (`[]id`) **REQUIRED** — The users that own the enhancement.
  - ID type: `devu`, `sysu`, `svcacc`
- **`parent_part`** (`id`) — The parent part that this enhancement belongs to (optional).
  - ID type: `capability`, `component`, `custom_part`, `enhancement`, `feature`, `linkable`, `microservice`, `product`, `runnable`
- **`target_start_date`** (`timestamp`) — When development of this enhancement is expected to begin.
- **`target_close_date`** (`timestamp`) — When this enhancement is expected to be completed and delivered.
- **`stage_v2`** (`id`) — The initial stage of the enhancement.
  - ID type: `custom_stage`

## Output Port: `output`

- **`created_by`** (`composite`)
  - Composite: `_gen:created_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — Name of the user who created the enhancement.
  - **`email`** (`text`) — Email of the user who created the enhancement.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — ID of the user who created the enhancement.
    - ID type: `devu`, `sysu`, `svcacc`
- **`created_date`** (`timestamp`) — When the enhancement was created.
- **`description`** (`text`) — Description of the enhancement.
- **`modified_by`** (`composite`)
  - Composite: `_gen:modified_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — Name of the user who last modified the enhancement.
  - **`email`** (`text`) — Email of the user who last modified the enhancement.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — ID of the user who last modified the enhancement.
    - ID type: `devu`, `sysu`, `svcacc`
- **`modified_date`** (`timestamp`) — When the enhancement was last modified.
- **`name`** (`text`) **REQUIRED** — The name of the created enhancement.
- **`owned_by`** (`[]composite`) **REQUIRED**
  - Composite: `_gen:user-summary`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — Name of the enhancement owner.
  - **`email`** (`text`) — Email of the enhancement owner.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — ID of the user who owns the enhancement.
    - ID type: `devu`, `sysu`, `svcacc`
- **`tags`** (`[]composite`) — Tags associated with the object.
  - Composite: `_gen:tags`
  - **`tag`** (`composite`)
    - Composite: `_gen:tag`
- **`target_close_date`** (`timestamp`) — Planned completion date for the enhancement.
- **`target_start_date`** (`timestamp`) — Planned start date for the enhancement.
- **`id`** (`id`) **REQUIRED** — The enhancement that was created.
  - ID type: `enhancement`
- **`stage_v2`** (`composite`) — The current stage of the enhancement.
  - Composite: `_gen:stage_v2`
  - **`stage`** (`composite`) — The current stage of the enhancement.
    - Composite: `_gen:stage`
