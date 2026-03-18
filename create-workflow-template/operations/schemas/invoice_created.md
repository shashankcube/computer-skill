# `invoice_created` — Schema Reference

## Input Port: `input`

_No input fields (trigger or system-provided)._

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
