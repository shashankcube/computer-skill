# `timeline_comment_created` — Schema Reference

## Input Port: `input`

- **`object_type`** (`[]uenum`) **REQUIRED** — The type of object that the Timeline entry belongs to.
  - Default: `[14]`
- **`panels`** (`[]uenum`) **REQUIRED** — The type of panel that the Timeline entry belongs to.
  - Default: `[2]`
- **`visibility`** (`[]uenum`) **REQUIRED** — The visibility of the entry. 'Internal' is visible with the Dev organization, and 'external' is visible to the Dev organization and Customers both.
  - Default: `[1, 2]`
- **`user_type`** (`[]uenum`) **REQUIRED** — The type of user that the Timeline entry belongs to.
  - Default: `[1]`

## Output Port: `output`

- **`artifacts`** (`[]composite`)
  - Composite: `_gen:artifact-summary`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`file`** (`composite`) — Defines a file object.
    - Composite: `_gen:file`
  - **`id`** (`id`) **REQUIRED** — The id of the artifact.
    - ID type: `artifact`
- **`body`** (`text`) — The comment's body. If the comment has been deleted, then no body will appear in the response.
- **`body_type`** (`enum`) — The type of the body to use for the comment.
  - Allowed: `data`, `snap_kit`, `snap_widget`, `text`
- **`created_by`** (`composite`)
  - Composite: `_gen:created_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`display_picture`** (`composite`)
    - Composite: `_gen:display_picture`
  - **`email`** (`text`) — Email address of the user.
  - **`external_ref`** (`text`) — External ref is a mutable unique identifier for a user within the Rev organization from your primary customer record. If none is available, a good alternative is the email address/phone number which could uniquely identify the user. If none is specified, a system-generated identifier will be assigned to the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `rev_user`, `service_account`, `sys_user`
  - **`id`** (`id`) **REQUIRED** — The id of the user.
    - ID type: `devu`, `sysu`, `svcacc`, `revu`
  - **`rev_org`** (`composite`)
    - Composite: `_gen:rev_org`
- **`created_date`** (`timestamp`) — Timestamp when the object was created.
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`modified_by`** (`composite`)
  - Composite: `_gen:modified_by`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`display_picture`** (`composite`)
    - Composite: `_gen:display_picture`
  - **`email`** (`text`) — Email address of the user.
  - **`external_ref`** (`text`) — External ref is a mutable unique identifier for a user within the Rev organization from your primary customer record. If none is available, a good alternative is the email address/phone number which could uniquely identify the user. If none is specified, a system-generated identifier will be assigned to the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`, `rev_user`, `service_account`, `sys_user`
  - **`id`** (`id`) **REQUIRED** — The id of the user.
    - ID type: `devu`, `sysu`, `svcacc`, `revu`
  - **`rev_org`** (`composite`)
    - Composite: `_gen:rev_org`
- **`modified_date`** (`timestamp`) — Timestamp when the object was last modified.
- **`object_display_id`** (`text`) **REQUIRED** — The display ID of the object that the Timeline entry belongs to.
- **`id`** (`id`) **REQUIRED** — The id of the comment.
  - ID type: `comment`
- **`object`** (`id`) **REQUIRED** — The object that the Timeline entry belongs to.
  - ID type: `account`, `capability`, `conversation`, `enhancement`, `feature`, `incident`, `issue`, `linkable`, `opportunity`, `product`, `revo`, `revu`, `runnable`, `ticket`
- **`object_type`** (`uenum`) — The type of object that the Timeline entry belongs to.
- **`panels`** (`[]uenum`) — The type of panel that the Timeline entry belongs to.
- **`visibility`** (`uenum`) — The visibility of the entry. 'Internal' is visible with the Dev organization, and 'external' is visible to the Dev organization and Customers both.
