# `invoice_updated` — Schema Reference

## Input Port: `input`

- **`fields_to_watch`** (`[]enum`) **REQUIRED** — Fields to watch for changes. The trigger will only be fired if any of these fields are updated. Only applicable in case of update events.
  - Allowed: `pdf_url`, `due_date`, `total`, `items`, `status`

## Output Port: `output`

- **`status`** (`enum`) **REQUIRED** — Status of the invoice
  - Allowed: `draft`, `finalized_pending`, `open`, `paid`, `posted`, `uncollectible`, `void`
- **`due_date`** (`timestamp`) — Due date of the invoice
- **`billing_email`** (`text`) **REQUIRED** — Email address for billing
- **`total`** (`double`) **REQUIRED** — Total amount of the invoice
- **`pdf_url`** (`text`) — URL to the invoice PDF
- **`account`** (`composite`) — The account of the customer
  - Composite: `account`
  - **`display_name`** (`text`) — Display name of the account
  - **`id`** (`id`) **REQUIRED** — ID of the account
    - ID type: `account`
- **`id`** (`id`) **REQUIRED** — The ID of the invoice
  - ID type: `invoice`
- **`items`** (`composite`) — Line items in the invoice
  - Composite: `invoice_line_item`
  - **`id`** (`id`) — ID of the line item
    - ID type: `invoice_line_item`
  - **`total`** (`double`) — Total amount for this line item
- **`old_invoice`** (`composite`) — The previous state of the invoice
  - Composite: `invoice`
  - **`status`** (`enum`) **REQUIRED** — Status of the invoice
    - Allowed: `draft`, `finalized_pending`, `open`, `paid`, `posted`, `uncollectible`, `void`
  - **`due_date`** (`timestamp`) — Due date of the invoice
  - **`billing_email`** (`text`) **REQUIRED** — Email address for billing
  - **`total`** (`double`) **REQUIRED** — Total amount of the invoice
  - **`pdf_url`** (`text`) — URL to the invoice PDF
  - **`account`** (`composite`) — The account of the customer
    - Composite: `account`
  - **`id`** (`id`) **REQUIRED** — The ID of the invoice
    - ID type: `invoice`
  - **`items`** (`composite`) — Line items in the invoice
    - Composite: `invoice_line_item`
