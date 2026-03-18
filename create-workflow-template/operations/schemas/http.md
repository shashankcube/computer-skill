# `http` — Schema Reference

Make HTTP requests.

## Input Port: `input`

- **`method`** (`enum`) — Method of the HTTP request.
  - Allowed: `GET`, `POST`, `PUT`, `DELETE`
  - Default: `GET`
- **`url`** (`text`) **REQUIRED** — URL of the HTTP request.
- **`query_params`** (`[]composite`) — Query parameters of the HTTP request.
  - Composite: `pair`
  - **`key`** (`text`) **REQUIRED** — Key.
  - **`value`** (`text`) — Value.
- **`headers`** (`[]composite`) — Headers of the HTTP request.
  - Composite: `pair`
  - **`key`** (`text`) **REQUIRED** — Key.
  - **`value`** (`text`) — Value.
- **`body`** (`text`) — Body of the HTTP request.
- **`auth_type`** (`enum`) — Authentication type.
  - Allowed: `Bearer`, `Basic`, `None`
  - Default: `None`

### Keyring / Connection

- **`auth_token`** (optional) — Authentication token. Accepts any keyring type.

## Output Port: `output`

- **`status_code`** (`int`) — Status code of the HTTP response.
- **`body`** (`text`) — Body of the HTTP response.
- **`headers`** (`[]composite`) — Headers of the HTTP response.
  - Composite: `pair`
  - **`key`** (`text`) **REQUIRED** — Key.
  - **`value`** (`text`) — Value.
- **`content_type`** (`text`) — Content type of the HTTP response.

> **Note:** Execution timeout is configurable.
