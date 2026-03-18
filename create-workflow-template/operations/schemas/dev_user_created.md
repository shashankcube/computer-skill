# `dev_user_created` — Schema Reference

## Input Port: `input`

_No input fields (trigger or system-provided)._

## Output Port: `output`

- **`availability_modes`** (`[]composite`) — The availability modes of the user. Stock allowed values: ```   {     "id": 1,     "label": "Ticket",     "ordinal": 1,     "overridable": false   },   {     "id": 2,     "label": "Conversation",     "ordinal": 2,     "overridable": false   },   {     "id": 3,     "label": "Issue",     "ordinal": 3,     "overridable": false   },   {     "id": 4,     "label": "Incident",     "ordinal": 4,     "overridable": false   },   {     "id": 5,     "label": "Away",     "ordinal": 5,     "overridable": false   },   {     "id": 6,     "label": "Sick",     "ordinal": 6,     "overridable": false   },   {     "id": 7,     "label": "Lunch",     "ordinal": 7,     "overridable": false   },   {     "id": 8,     "label": "Travel",     "ordinal": 8,     "overridable": false   },   {     "id": 9,     "label": "Vacation",     "ordinal": 9,     "overridable": false   } ```
  - Composite: `_gen:availability_modes`
  - **`id`** (`int`) **REQUIRED** — The unique ID of the enum value.
  - **`label`** (`text`) **REQUIRED** — The display label of the enum value.
  - **`ordinal`** (`int`) **REQUIRED** — Used for determining the relative order of the enum value.
  - **`value`** (`json_value`) — The actual value of the enum value.
- **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
- **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
- **`display_picture`** (`composite`)
  - Composite: `_gen:user-base-properties.display_picture`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `artifact`
- **`email`** (`text`) — Email address of the user.
- **`experience_start_date`** (`timestamp`) — Start date of the user's employment.
- **`full_name`** (`text`) — Full name of the user.
- **`job_history`** (`[]composite`) — Job history of the user.
  - Composite: `_gen:job_history`
  - **`employment_status`** (`composite`) — The properties of an enum value.
    - Composite: `_gen:employment_status`
  - **`end_date`** (`timestamp`) — The end date of the job, or not specified if current.
  - **`is_current`** (`bool`) — Is this the current active job for the user.
  - **`location`** (`text`) — The job location for the user.
  - **`start_date`** (`timestamp`) — The start date of the job.
  - **`title`** (`text`) — The job title for the user.
- **`phone_numbers`** (`[]text`) — Phone numbers of the user.
- **`reports_to`** (`composite`)
  - Composite: `_gen:reports_to`
  - **`display_id`** (`text`) — Human-readable object ID unique to the Dev organization.
  - **`display_name`** (`text`) — The user's display name. The name is non-unique and mutable.
  - **`email`** (`text`) — Email address of the user.
  - **`full_name`** (`text`) — Full name of the user.
  - **`state`** (`enum`) — State of the user.
    - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
  - **`id`** (`id`) **REQUIRED** — Globally unique object ID.
    - ID type: `devu`
  - **`type`** (`enum`) **REQUIRED**
    - Allowed: `dev_user`
- **`skills`** (`[]composite`) — Array of skills of the user.
  - Composite: `_gen:skills`
  - **`name`** (`text`) — Name of the skill.
- **`state`** (`enum`) — State of the user.
  - Allowed: `active`, `deactivated`, `deleted`, `locked`, `shadow`, `unassigned`
- **`timezone`** (`text`) — The user's timezone in IANA Time Zone format (e.g., 'Asia/Kolkata', 'America/New_York').
- **`id`** (`id`) **REQUIRED** — Globally unique object ID.
  - ID type: `devu`
