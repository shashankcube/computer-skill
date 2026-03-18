# `test_example` — Schema Reference

## Input Port: `input`

- **`new_field_required`** (`text`) **REQUIRED** — This is a new required field
- **`new_field_required`** (`text`) **REQUIRED** — This is a new required field
- **`new_field`** (`text`) — This is a new field
- **`message`** (`text`) **REQUIRED** — A new description
  - Default: `Hello, World!`
  - Validation: min_len=1, max_len=1000

## Output Port: `output`

- **`message`** (`text`) — The echoed message.
- **`nested_level_field_1`** (`text`) — This is a nested level field that should be promoted to the top level.
- **`nested_level_field_2`** (`text`) — This is a nested level field that should be promoted to the top level.
