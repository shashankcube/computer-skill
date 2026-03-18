# `question_answer_created` — Schema Reference

## Input Port: `input`

_No input fields (trigger or system-provided)._

## Output Port: `output`

- **`answer`** (`text`) — The Answer.
- **`created_by`** (`composite`)
  - Composite: `_gen:created_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `service_account`, `sys_user`
- **`created_date`** (`timestamp`) — Timestamp when the QA was created.
- **`modified_by`** (`composite`)
  - Composite: `_gen:modified_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`, `sysu`, `svcacc`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `service_account`, `sys_user`
- **`modified_date`** (`timestamp`) — Timestamp when the QA was modified.
- **`question`** (`text`) — The Question.
- **`status`** (`enum`) — Status of the question answer.
  - Allowed: `archived`, `discarded`, `draft`, `published`, `review_needed`
- **`suggested_answer`** (`text`) — An alternative answer suggested by the QA generation algorithm.
- **`suggested_for_deletion`** (`bool`) — Whether the QA was marked for deletion by the QA generation algorithm.
- **`topic`** (`text`) — The topic to which the QA belongs.
- **`verified`** (`bool`) — Whether the QA was verified.
- **`id`** (`id`) **REQUIRED** — The ID of the created QA.
  - ID type: `question_answer`
