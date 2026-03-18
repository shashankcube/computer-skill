# `get_session_details` — Schema Reference

## Input Port: `input`

- **`platform`** (`enum`) **REQUIRED** — Indicates the environment where the session was recorded — android, ios, or web.
  - Allowed: `android`, `ios`, `web`
- **`session_id`** (`text`) **REQUIRED** — The unique ID of the session to retrieve.
  - Validation: min_len=1
- **`version_key`** (`text`) **REQUIRED** — The unique app ID to which the session belongs, also referred to as integration key
  - Validation: min_len=1

## Output Port: `output`

- **`session`** (`composite`) — The session metadata including device information, duration, and recording identifiers.
  - Composite: `_gen:session`
  - **`console_metadata`** (`composite`) — Summary of console log information captured during a web session.
    - Composite: `_gen:console_metadata`
  - **`created_at`** (`timestamp`) **REQUIRED** — The session creation date.
  - **`dead_click_count`** (`int`) **REQUIRED** — Number of clicks on unresponsive elements during the web session.
  - **`device_metadata`** (`composite`) — Metadata describing the device and environment where the session was recorded.
    - Composite: `_gen:device_metadata`
  - **`first_page_title`** (`text`) — The title of the first page visited.
  - **`first_page_url`** (`text`) — The URL of the first page visited.
  - **`has_exception_occurred`** (`bool`) **REQUIRED** — Whether an exception occurred.
  - **`is_anr`** (`bool`) **REQUIRED** — Indicates whether the mobile app became unresponsive / UI freeze (ANR) during the session.
  - **`is_crash`** (`bool`) **REQUIRED** — Whether crash occurred in the session.
  - **`network_metadata`** (`composite`) — Summary of network response status codes recorded during a web session.
    - Composite: `_gen:network_metadata`
  - **`page_load_time_ms`** (`int`) — The page load time in milliseconds.
  - **`platform`** (`enum`) — The platform type for the device.
    - Allowed: `android`, `ios`, `web`
  - **`rage_click_count`** (`int`) **REQUIRED** — Number of rapid repeated clicks indicating user frustration during the web session.
  - **`rage_tap`** (`bool`) **REQUIRED** — Indicates whether the user performed rapid repeated taps (rage taps) in the mobile app during the session.
  - **`recording_ids`** (`[]text`) — The list of recording IDs in this session.
  - **`rev_org_id`** (`id`) — The workspace ID associated with this session.
    - ID type: `revo`
  - **`rev_user_id`** (`id`) — The contact ID associated with this session.
    - ID type: `revu`
  - **`session_attributes`** (`composite`) — Custom key-value metadata associated with the session.
    - Composite: `_gen:session_attributes`
  - **`session_duration_ms`** (`int`) **REQUIRED** — The session duration in milliseconds.
  - **`session_id`** (`text`) **REQUIRED** — The unique session identifier.
  - **`tab_ids`** (`[]text`) — The list of tab IDs in this session.
  - **`type`** (`enum`) **REQUIRED** — Specifies the category of the client where the session occurred — Mobile or Web.
    - Allowed: `mobile`, `web`
