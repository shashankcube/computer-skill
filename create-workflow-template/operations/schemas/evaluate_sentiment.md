# `evaluate_sentiment` — Schema Reference

Takes the ID of the object and evaluates the sentiment. Currently supports tickets and conversations.

## Input Port: `input`

- **`object`** (`id`) **REQUIRED** — The ID of the ticket or conversation to evaluate sentiment for.
  - ID type: `ticket`, `conversation`

## Output Port: `output`

- **`sentiment`** (`enum`) **REQUIRED** — The evaluated sentiment.
  - Allowed: `delighted`, `frustrated`, `happy`, `neutral`, `unhappy`, `unknown`
- **`justification`** (`text`) — The justification for the evaluated sentiment.
