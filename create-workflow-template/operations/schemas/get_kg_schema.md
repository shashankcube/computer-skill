# `get_kg_schema` — Schema Reference

## Input Port: `input`

- **`include_fields`** (`bool`) — Whether to include detailed field information in the response.
- **`kg_allowlist`** (`[]tokens`) — The knowledge graph nodes to allowlist. Use regex patterns to match dataset names here. Example: ['prod_*', 'users_*']
- **`kg_blocklist`** (`[]tokens`) — The knowledge graph nodes to blocklist. Use regex patterns to match dataset names here. Blocking is done on top of the allowlist filtered results i.e. blocking has higher precedence than allowing.

## Output Port: `output`

- **`table_schemas`** (`[]composite`) **REQUIRED** — The list of table schemas.
  - Composite: `table_schema`
  - **`name`** (`tokens`) **REQUIRED** — The name of the table.
  - **`id`** (`id`) **REQUIRED** — The ID of the table.
    - ID type: `dataset`
  - **`status`** (`enum`) **REQUIRED** — The status of the table.
    - Allowed: `draft`, `published`
  - **`description`** (`text`) **REQUIRED** — Human-readable description of the table.
  - **`fields`** (`[]composite`) — Array of fields defining the table structure.
    - Composite: `table_field`
  - **`parent_entity_type`** (`tokens`) — Type of entity this table extends if any.
  - **`primary_entity_types`** (`[]tokens`) — Types of entities this table masters.
  - **`related_entity_types`** (`[]tokens`) — Types of entities this table relates to.
