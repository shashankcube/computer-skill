# `if_else` — Schema Reference

Branch the workflow based on a condition.

## Input Port: `input`

- **`condition`** (`struct`) **REQUIRED** — Condition to evaluate.
  - This is a structured condition object (e.g., comparison expressions).

## Output Port: `true`

No fields. This port is followed when the condition evaluates to true.

## Output Port: `false`

No fields. This port is followed when the condition evaluates to false.
