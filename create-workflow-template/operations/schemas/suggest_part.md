# `suggest_part` — Schema Reference

Suggest a part that can be attributed to a given object.

## Input Port: `input`

- **`id`** (`id`) **REQUIRED** — The ID of the ticket, conversation or incident to suggest parts for.
  - ID type: `ticket`, `conversation`, `incident`

## Output Port: `output`

- **`id`** (`id`) **REQUIRED** — The ID of the suggested part.
  - ID type: `capability`, `enhancement`, `feature`, `product`, `linkable`, `runnable`
- **`found`** (`bool`) **REQUIRED** — If suggestion is present, returns true.
