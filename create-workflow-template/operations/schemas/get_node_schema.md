# `get_node_schema` — Schema Reference

## Input Port: `input`

- **`id`** (`id`) **REQUIRED** — The ID of the table for which to get the schema.
  - ID type: `dataset`

## Output Port: `output`

- **`name`** (`tokens`) **REQUIRED** — The name of the table.
- **`id`** (`id`) **REQUIRED** — The ID of the table.
  - ID type: `dataset`
- **`status`** (`enum`) **REQUIRED** — The status of the table.
  - Allowed: `draft`, `published`
- **`description`** (`text`) **REQUIRED** — Human-readable description of the table.
- **`fields`** (`[]composite`) — Array of fields defining the table structure.
  - Composite: `table_field`
  - **`name`** (`tokens`) **REQUIRED** — The name of the field.
  - **`type`** (`tokens`) **REQUIRED** — SQL-compatible data type, e.g., VARCHAR, INTEGER, STRUCT, ARRAY.
  - **`description`** (`text`) **REQUIRED** — A human-readable description of the field.
  - **`enum`** (`composite`) — Allowed values for this field, either static (embedded) or dynamic (external reference).
    - Composite: `table_enum`
  - **`fields`** (`[]composite`) — For STRUCT types, defines nested field names; for ARRAY of STRUCT, defines element structure.
    - Composite: `table_field`
  - **`is_primary_key`** (`bool`) — Whether this column is part of the primary key.
  - **`id_types`** (`[]tokens`) — Types of IDs this field can contain.
- **`parent_entity_type`** (`tokens`) — Type of entity this table extends if any.
- **`primary_entity_types`** (`[]tokens`) — Types of entities this table masters.
- **`related_entity_types`** (`[]tokens`) — Types of entities this table relates to.
