# Control Operations (8)

> Flow-control operations that route or branch workflow execution.


## 1. AI Agent Skill

- **Slug:** `ai_agent_skill`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.ai_agent_skill`
- **Tags:** HIPAA
- **Scopes:** `ai_agent:execute`

Executes custom workflow logic for AI agents

### Input (`input`)

- `agent_session_id` (id) **(required)** — The agent session identifier
  - ID types: `ai_agent_session`
- `skill_call_id` (text) **(required)** — The skill call identifier
- `skill_name` (text) **(required)** — The name of the skill being executed

---

## 2. For Each

- **Slug:** `for_each`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.for_each`
- **Tags:** HIPAA

Iterate over a list of items

---

## 3. Go Back

- **Slug:** `go_back`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.go_back`
- **Tags:** HIPAA

Go back to the previous step in the workflow

---

## 4. If Else

- **Slug:** `if_else`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.if_else`
- **Tags:** HIPAA

Branch the workflow based on a condition

### Input (`input`)

- `condition` (struct) **(required)** — Condition to evaluate

---

## 5. Initialize Variable

- **Slug:** `init_variable`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.init_variable`
- **Tags:** HIPAA

Initializes a variable of a type

---

## 6. Router

- **Slug:** `router`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.router`
- **Tags:** HIPAA, BETA

Route workflow into parallel paths, executing only the routes whose conditions are met

---

## 7. Set Variable

- **Slug:** `set_variable`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.set_variable`
- **Tags:** HIPAA

Sets a variable with the given value

---

## 8. While

- **Slug:** `while`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.while`
- **Tags:** HIPAA

Executes a block of steps while a condition is true. Maximum iterations of 1000.

### Output (`block_start`)

- `iteration_number` (int) — The current iteration number of the loop starting from 1

---
