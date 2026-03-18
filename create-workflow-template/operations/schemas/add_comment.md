# `add_comment` — Schema Reference

Adds a comment to an object.

## Input Port: `input`

- **`object`** (`id`) **REQUIRED** — ID of the object.
  - ID type: `account`, `capability`, `conversation`, `enhancement`, `feature`, `incident`, `issue`, `linkable`, `opportunity`, `product`, `meeting`, `revo`, `revu`, `runnable`, `ticket`, `comment`, `dm`, `article`
- **`body`** (`text`) **REQUIRED** — Comment body.
- **`visibility`** (`enum`) — Specifies the entry's visibility. 'Internal' limits visibility to the Dev organization, 'External' allows visibility to both the Dev organization and its customers, and 'Private' limits visibility to specified users only. Defaults to 'External' if unspecified.
  - Allowed: `external`, `internal`, `private`
  - Default: `external`

## Output Port: `output`

- **`id`** (`id`) **REQUIRED** — ID of the comment.
  - ID type: `comment`
- **`created_by`** (`composite`) **REQUIRED** — ID of the user who created the comment.
  - Composite: `created_by`
  - **`type`** (`enum`) **REQUIRED** — Type of the user.
    - Allowed: `dev_user`, `sys_user`, `rev_user`, `service_account`
  - **`id`** (`id`) **REQUIRED** — ID of the user.
    - ID type: `devu`, `sysu`, `revu`, `svcacc`
  - **`display_id`** (`text`) **REQUIRED** — Display ID of the user.
- **`created_by_agent`** (`composite`) — ID of the service account who created the comment on behalf of the user.
  - Composite: `created_by_agent`
  - **`id`** (`id`) — ID of the service account.
    - ID type: `svcacc`
  - **`display_id`** (`text`) **REQUIRED** — Display ID of the agent.
