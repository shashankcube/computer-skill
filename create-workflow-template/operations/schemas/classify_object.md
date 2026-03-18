# `classify_object` — Schema Reference

Classifies an object into a set of customizable categories.

## Input Port: `input`

- **`object_id`** (`id`) **REQUIRED** — ID of the object.
  - ID type: `issue`, `ticket`, `conversation`, `enhancement`
- **`object_type`** (`enum`) **REQUIRED** — Type of the object.
  - Allowed: `issue`, `ticket`, `conversation`, `enhancement`
  - Default: `issue`
- **`categories`** (`[]composite`) **REQUIRED** — Specify the categories into which the object should be classified.
  - Composite: `object_category`
  - **`name`** (`tokens`) **REQUIRED** — Name of the category into which the object can be classified.
  - **`description`** (`text`) — Describe which type of objects should be categorized into this category.
- **`guidelines`** (`text`) — Specify any special instructions or examples that should be used by the classifier.

## Output Port: `output`

- **`category`** (`text`) — Output category from the object classification action node.
- **`justification`** (`text`) — Output category justification from the object classification action node.
- **`session_id`** (`tokens`) **REQUIRED** — Session ID generated for this AI operation.
