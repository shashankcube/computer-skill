# `update_question_answer` — Schema Reference

## Input Port: `input`

- **`access_level`** (`enum`)
  - Allowed: `external`, `internal`, `private`, `public`, `restricted`
- **`answer`** (`text`) — Updates answer of the question-answer object, or unchanged if not provided.
- **`applies_to_parts`** (`composite`)
  - Composite: `_gen:applies_to_parts`
  - **`set`** (`[]id`) — Updates the parts that the question-answer applies to.
    - ID type: `capability`, `component`, `custom_part`, `enhancement`, `feature`, `linkable`, `microservice`, `product`, `runnable`
- **`id`** (`id`) **REQUIRED** — The question-answer's ID.
  - ID type: `question_answer`
- **`owned_by`** (`composite`)
  - Composite: `_gen:owned_by`
  - **`set`** (`[]id`) — Sets the owner IDs to the provided user IDs. This must not be empty.
    - ID type: `devu`, `sysu`, `svcacc`
- **`question`** (`text`) — Updates question of the question-answer object, or unchanged if not provided.
- **`sources`** (`composite`)
  - Composite: `_gen:sources`
  - **`set`** (`[]id`) — Sets the sources that generated the question-answer.
    - ID type: `conversation`
- **`status`** (`enum`) — Status of the question answer.
  - Allowed: `archived`, `discarded`, `draft`, `published`, `review_needed`

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
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
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
- **`id`** (`id`) **REQUIRED** — The ID of the created QA.
  - ID type: `question_answer`
