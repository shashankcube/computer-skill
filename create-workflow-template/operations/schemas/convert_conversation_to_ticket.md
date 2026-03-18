# `convert_conversation_to_ticket` — Schema Reference

Converts a conversation to a ticket. Only conversations from PLuG and Slack can be converted to tickets.

## Input Port: `input`

- **`conversation_id`** (`id`) **REQUIRED** — ID of the conversation to be converted to a ticket.
  - ID type: `conversation`

## Output Port: `output`

- **`ticket_id`** (`id`) **REQUIRED** — ID of the ticket created from the conversation.
  - ID type: `ticket`
