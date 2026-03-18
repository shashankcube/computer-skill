# `workspace_created` — Schema Reference

## Input Port: `input`

_No input fields (trigger or system-provided)._

## Output Port: `output`

- **`account`** (`composite`) — The account associated with the workspace.
  - Composite: `_gen:account`
  - **`display_name`** (`text`) — Name of the Organization.
  - **`id`** (`id`) **REQUIRED** — The id of the account.
    - ID type: `account`
- **`description`** (`text`) — The description of the workspace.
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`display_name`** (`text`) — Name of the Organization.
- **`domain`** (`text`) — The domain of the workspace.
- **`external_ref`** (`text`) — The external reference of the workspace.
- **`id`** (`id`) **REQUIRED** — The id of the workspace.
  - ID type: `revo`
