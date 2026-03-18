---
skill-name: create-workflow-template
user-invocable: true
description: Create a DevRev workflow template JSON from a natural language description
arguments:
  - name: description
    description: Natural language description of the workflow to create
    required: true
---

# Create DevRev Workflow Template

Generate a valid DevRev workflow template JSON. Study the example templates in `.claude/skills/create-workflow-template/examples/` before generating to ensure the output matches the production format.

## Workflow Process

1. **Understand the user's intent** — Parse the natural language description to identify the trigger event, the sequence of actions, any conditions/branching, and any loops.
2. **Look up operations** — Consult the operations reference docs in `.claude/skills/create-workflow-template/operations/` to find the correct slugs, namespaces, and input/output schemas for each operation needed. For detailed field-level schemas (input/output ports, field types, required fields), check `.claude/skills/create-workflow-template/operations/schemas/<slug>.md` — the schema index is at `.claude/skills/create-workflow-template/operations/schema-index.md`.
3. **Study example templates** — Read relevant templates from `.claude/skills/create-workflow-template/examples/` to see real-world patterns for the operations you need. **Prioritize `working-*.json` examples** — these are validated, import-tested templates. Use python3 to pretty-print: `python3 -c "import json; d=json.load(open('.claude/skills/create-workflow-template/examples/FILE.json')); inner=json.loads(d['data']); print(json.dumps(inner, indent=2))"`.
4. **Build the template JSON** — Assemble the complete workflow JSON following the specification below, matching the format used in the example templates.
5. **Output the JSON** — Write the template to a file in `templates/<workflow_name>.json` and also display a summary to the user.
6. **Save as example (self-improvement)** — After a template is confirmed working (user reports successful import or no errors), copy it to `.claude/skills/create-workflow-template/examples/working-<descriptive-name>.json`. This grows the example library over time, making the skill better with every successful workflow. Also update the example templates table in this file with the new entry.

## Template Wrapper Format

Templates are wrapped in an outer envelope. The `data` field is a **stringified JSON** of the inner workflow:

```json
{
  "templateVersion": "2.0.0",
  "data": "<stringified inner workflow JSON>"
}
```

To produce this, build the inner workflow object first, then `JSON.stringify()` it into the `data` field.

## Inner Workflow Structure

```json
{
  "serialization_version": { "major": 2, "minor": 0, "patch": 0 },
  "title": "Workflow Title (1-256 chars)",
  "description": "What this workflow does (up to 65536 chars)",
  "labels": ["optional-label"],
  "steps": [ ...step objects... ]
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `serialization_version` | object | Yes | Always `{"major": 2, "minor": 0, "patch": 0}` |
| `title` | string | Yes | 1-256 chars |
| `description` | string | No | Up to 65536 chars |
| `labels` | string[] | No | Up to 16 labels, each up to 64 chars. Use `["agent_interaction"]` for agent workflows |
| `steps` | array | Yes | All workflow steps (triggers + actions + controls + blockings) |

## Step Structure

Each entry in `steps` is a node in the workflow graph:

```json
{
  "name": "Step Display Name",
  "description": "What this step does",
  "reference_key": "unique_step_key",
  "operation": {
    "namespace": "devrev",
    "slug": "operation_slug"
  },
  "input_values": [ ...input value mappings... ],
  "next_steps": [ ...edges to next steps... ],
  "ui_metadata": {
    "position": { "x": 400, "y": 200 }
  },
  "block_step_reference_key": "parent_loop_ref_key"
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | 1-256 chars |
| `description` | string | No | Up to 65536 chars |
| `reference_key` | string | Yes | Unique within workflow. Pattern: `^[a-zA-Z_][a-zA-Z0-9_]*$` |
| `operation` | object | Yes | `{namespace, slug}` — identifies the operation |
| `input_values` | array | No | Input data/expressions for this step |
| `next_steps` | array | No | Outgoing edges to other steps |
| `ui_metadata` | object | No | UI position `{position: {x, y}}` |
| `block_step_reference_key` | string | No | Ref key of parent loop/block step (for nested steps) |
| `system_options` | object | No | Execution options (e.g. `execute_as_user`) |
| `scopes` | object | No | Access scopes |

### Port Schema Rules (`input_ports` / `output_ports`)

The UI needs port schemas to render step configuration panels. Rules by operation type:

| Operation Type | `input_ports` | `output_ports` | Notes |
|---------------|:---:|:---:|-------|
| **Triggers** (`*_created`, `*_updated`) | No | No | System resolves schemas automatically |
| **Timer trigger** (`timer_trigger`) | **Yes** | **Yes** | Needs uenum type field + composite schemas for interval/cron. See Timer Trigger section |
| **Control ops** (`if_else`, `while`, `router`) | **Yes** | **Yes** | Need input schema + named output ports (true/false/error, block_start/output/error) |
| **Loop ops** (`loop_over_*`) | **Yes** | **Yes** | Need input schema (filter fields) + `block_callback` input port + `block_start`/`output`/`error` output ports. `block_start` uses `type: "block_start"` |
| **`for_each`** | **Yes** | **Yes** | Dynamic schema — avoid in templates, use `loop_over_*` instead |
| **`init_variable`** | **Yes** | **Yes** | Output port needs `scope_variables` array. See Variable Management section |
| **`set_variable`** | **Yes** | **Yes** | Input port needs `uenum` fields with `allowed_values`. See Variable Management section |
| **Action ops** (`update_*`, `create_*`, `get_*`, etc.) | **Yes** | No | Need input schema so UI renders field picker. Get schemas from `operations/schemas/<slug>.md` raw JSON files at `/tmp/op_schemas/<slug>.json` |
| **`invoke_code`** | **Yes** | **Yes** | Dynamic schema with `invalidate_on_field_update`. See `operations/schemas/invoke_code.md` for full patterns |
| **`manual_trigger`** | No | **Yes** | Has dynamic output schema |

**Port schema format** — uses the internal schema format with `field_descriptors`, `composite_schemas`, `data_name`, `db_name`, `oasis` metadata. Get the raw JSON from `/tmp/op_schemas/<slug>.json` (downloaded from `devrev/flow` repo `.gen.json` files) and include the relevant port's schema object.

**Error ports** — Control ops and `invoke_code` should include an `error` output port:
```json
{"name": "error", "schema": {"field_descriptors": [
  {"field_type": "text", "data_name": "message", "db_name": "message", "description": "Error message with more details about the error", "name": "message", "oasis": {"name": "message"}, "ui": {"display_name": "Error Message"}},
  {"field_type": "enum", "allowed_values": ["bad_request", "not_found", "internal"], "data_name": "type", "db_name": "type", "description": "Type of the error", "name": "type", "oasis": {"name": "type"}, "ui": {"display_name": "Error Type"}}
], "type": "field_descriptor"}, "type": "error"}
```

**`if_else` ports:**
```json
"input_ports": [{"name": "input", "schema": {"field_descriptors": [{"field_type": "struct", "data_name": "condition", "db_name": "condition", "description": "Condition to evaluate", "is_required": true, "name": "condition", "oasis": {"name": "condition"}}], "type": "field_descriptor"}, "type": "default"}],
"output_ports": [{"name": "true", "type": "default"}, {"name": "false", "type": "default"}, <error_port>]
```

**`invoke_code` ports** — include `input_ports` with variable composite + code editor schema, `output_ports` with empty output + error. Do NOT include `input_values` — the user configures code, input variables, and output schema in the UI after import. See `operations/schemas/invoke_code.md` for the full `input_ports` JSON.

## How Operations Are Referenced

Always use `namespace` + `slug`:

```json
"operation": {
  "namespace": "devrev",
  "slug": "ticket_created"
}
```

- First-party DevRev operations: `"devrev"` namespace
- Custom objects: `"custom_object"` namespace
- Third-party snap-in operations: use their snap-in namespace (e.g. `"send-emails"`)
- Look up the correct slug from `.claude/skills/create-workflow-template/operations/*.md` files

## Connecting Steps (next_steps)

Each step can connect to one or more next steps via port-based edges:

```json
"next_steps": [
  {
    "port_name": "output",
    "next_step_reference_key": "target_step_ref",
    "next_port_name": "input"
  }
]
```

**Port names by operation type:**

| Operation | Output Ports |
|-----------|-------------|
| Regular triggers/actions | `"output"` |
| `if_else` | `"true"` and `"false"` |
| `loop_over_*` / `for_each` / `while` | `"block_start"` (loop body) and `"output"` (after loop) |
| `router` | Named route ports |

The `next_port_name` on the target step is almost always `"input"`.

## Input Values (Data Mapping)

The `input_values` array defines how data flows into each step. **Use string type names** (not integer IDs):

```json
"input_values": [
  {
    "port_name": "input",
    "fields": [
      {
        "name": "field_name",
        "value": {
          "type": "<type_name>",
          "value": <value>
        }
      }
    ]
  }
]
```

### Value Types

| Type Name | Description | Example |
|-----------|-------------|---------|
| `"literal"` | Static/fixed value | `{"type": "literal", "value": "internal"}` |
| `"jsonata_expression"` | References other steps' outputs | `{"type": "jsonata_expression", "value": "$get('step_ref').field"}` |
| `"composite_value"` | Structured object with nested fields | See composite section below |
| `"text_template"` | Template string with embedded expressions | `{"type": "text_template", "value": "Hello {% expr $get('step_ref').name %}"}` |
| `"list_value"` | List of items | See list section below |

### Literal Values

Use for static values — strings, numbers, booleans, DON IDs, or structured objects:

```json
{ "type": "literal", "value": "internal" }
{ "type": "literal", "value": true }
{ "type": "literal", "value": "don:core:dvrv-us-1:devo/0:custom_stage/27" }
{ "type": "literal", "value": {"seconds": 30} }
```

### JSONata Expressions

Reference outputs from previous steps:

```json
{ "type": "jsonata_expression", "value": "$get('issue_created_1', 'output').id" }
{ "type": "jsonata_expression", "value": "$get('get_ticket_1', 'output').owned_by[0].id" }
{ "type": "jsonata_expression", "value": "$get('issue_updated_1', 'output').sprint.end_date" }
```

Syntax: `$get('<reference_key>', '<port_name>').<field_path>`
- Port name defaults to `'output'` if omitted: `$get('step_ref').field`

### Text Templates

Mix static text with embedded expressions:

```json
{ "type": "text_template", "value": "Hi {% expr $get('get_ticket_1', 'output').owned_by[0].id %}, the ticket has been escalated." }
```

### List Values

Wrap arrays — each item specifies its own type:

```json
{
  "type": "list_value",
  "value": {
    "items": [
      { "type": "literal", "value": ["stage"] },
      { "type": "jsonata_expression", "value": "$append($get('step_ref', 'output').owned_by, [])" }
    ]
  }
}
```

**Common pattern** — wrapping a single expression as a list (required for array-typed fields like `receivers`, `fields_to_watch`):
```json
{
  "type": "list_value",
  "value": {
    "items": [
      { "type": "jsonata_expression", "value": "$append($get('trigger_1', 'output').owned_by, [])" }
    ]
  }
}
```

### Composite Values

Structured objects with named sub-fields:

```json
{
  "type": "composite_value",
  "value": {
    "fields": [
      { "name": "set", "value": { "type": "literal", "value": ["don:identity:dvrv-us-1:devo/0:devu/1"] } }
    ]
  }
}
```

## Trigger Configuration

Trigger steps are regular steps that use a trigger-type operation. They support two special input fields:

### `fields_to_watch` — Filter which field changes fire the trigger

```json
{
  "name": "fields_to_watch",
  "value": {
    "type": "list_value",
    "value": {
      "items": [
        { "type": "literal", "value": ["stage"] }
      ]
    }
  }
}
```

### `_filter` — Conditional filter for when the trigger fires

```json
{
  "name": "_filter",
  "value": {
    "type": "literal",
    "value": {
      "conditions": [{
        "conditions": [{
          "operands": [
            { "type": "jsonata_expression", "value": "$get('opportunity_updated_1', 'output').stage.name" },
            { "type": "literal", "value": "3-evaluate" }
          ],
          "operator": "equals",
          "type": "rule"
        }],
        "logical_operator": "and",
        "negate": false,
        "type": "group"
      }],
      "logical_operator": "and",
      "negate": false,
      "type": "group"
    }
  }
}
```

## Timer Trigger

The `timer_trigger` operation requires full `input_ports` and `output_ports` (unlike event-based triggers which resolve schemas automatically).

**Input port** — includes `uenum` type field and composite schemas for interval/cron:
```json
"input_ports": [{"name": "input", "schema": {
  "composite_schemas": [
    {"description": "Timer interval configuration", "fields": [
      {"field_type": "enum", "allowed_values": ["days", "hours"], "data_name": "unit", "db_name": "unit",
       "default_value": "days", "description": "Unit of time for the interval", "is_required": true,
       "name": "unit", "oasis": {"name": "unit"}, "ui": {"display_name": "Unit of time"}},
      {"field_type": "int", "data_name": "number_of_units", "db_name": "number_of_units",
       "default_value": 1, "description": "Number of Units", "is_required": true,
       "name": "number_of_units", "oasis": {"name": "number_of_units"}, "ui": {"display_name": "Number of Units"}}
    ], "name": "timer_interval"},
    {"description": "Timer cron configuration", "fields": [
      {"field_type": "tokens", "data_name": "schedule", "db_name": "schedule",
       "description": "The cron expression for the timer trigger.", "is_required": true,
       "name": "schedule", "oasis": {"name": "schedule"}}
    ], "name": "timer_cron"}
  ],
  "field_descriptors": [
    {"field_type": "uenum", "allowed_values": [
      {"id": 1, "label": "Interval based", "ordinal": 1, "tooltip": "Trigger at regular intervals", "value": "interval"},
      {"id": 2, "label": "Cron based", "ordinal": 2, "tooltip": "Trigger at specific time using cron", "value": "cron"}
    ], "data_name": "type", "db_name": "type", "default_value": 1,
     "description": "The type of timer trigger", "name": "type", "oasis": {"name": "type"}, "ui": {"display_name": "Type"}},
    {"field_type": "composite", "composite_type": "timer_interval", "data_name": "interval", "db_name": "interval",
     "description": "The interval at which the workflow should trigger.", "is_required": false,
     "name": "interval", "oasis": {"name": "interval"}},
    {"field_type": "composite", "composite_type": "timer_cron", "data_name": "cron", "db_name": "cron",
     "description": "The cron configuration.", "is_required": false, "name": "cron", "oasis": {"name": "cron"}},
    {"field_type": "struct", "data_name": "_filter", "db_name": "_filter", "description": "Filter for the trigger event",
     "name": "_filter", "oasis": {"name": "_filter"}, "ui": {"display_name": "Filter"}}
  ], "type": "field_descriptor"}, "type": "default"}]
```

**Output port** — exposes the scheduled time:
```json
"output_ports": [{"name": "output", "schema": {"field_descriptors": [
  {"field_type": "timestamp", "data_name": "scheduled_time", "db_name": "scheduled_time",
   "description": "The time when the workflow was scheduled to trigger", "is_required": true,
   "name": "scheduled_time", "oasis": {"name": "scheduled_time"}}
], "type": "field_descriptor"}, "type": "default"}]
```

## Critical Field Type Requirements

### `uenum` Fields MUST Have `allowed_values`

Fields with `field_type: "uenum"` will cause import error `"Missing required field: allowed_values"` if `allowed_values` is missing. Each value must be an object with `{id, label, ordinal, value}`:

```json
{"field_type": "uenum", "allowed_values": [
  {"id": 1, "label": "Set Value", "ordinal": 1, "value": "set"},
  {"id": 2, "label": "Concatenate", "ordinal": 2, "value": "concat"}
], "data_name": "operation", ...}
```

The `value` can be a string or a complex object (e.g., for variable references):
```json
{"id": 1, "label": "init_variable_1 - summary", "ordinal": 1,
 "value": {"output_port_name": "output", "step_reference_key": "init_variable_1", "variable_name": "summary"}}
```

### `array` Fields MUST Have `base_type`

Fields with `field_type: "array"` will cause import error `"discriminator not set: base_type"` if `base_type` is missing:

```json
{"field_type": "array", "base_type": "id", "data_name": "owned_by", ...}
{"field_type": "array", "base_type": "text", "data_name": "external_ref", ...}
{"field_type": "array", "base_type": "composite", "composite_type": "pair", "data_name": "headers", ...}
{"field_type": "array", "base_type": "enum", "allowed_values": ["days", "hours"], "data_name": "units", ...}
```

Common `base_type` values: `id`, `text`, `composite`, `enum`

## If/Else Conditions

The `if_else` operation takes a structured condition as a literal:

```json
{
  "name": "condition",
  "value": {
    "type": "literal",
    "value": {
      "type": "group",
      "logical_operator": "and",
      "negate": false,
      "conditions": [{
        "conditions": [{
          "operands": [
            { "type": "jsonata_expression", "value": "$get('get_account_1', 'output').tnt__segment" }
          ],
          "operator": "exists",
          "type": "rule"
        }],
        "logical_operator": "and",
        "negate": false,
        "type": "group"
      }]
    }
  }
}
```

**Condition operators:** `equals`, `not_equals`, `contains_any`, `is_empty`, `is_not_empty`, `greater_than`, `less_than`, `greater_than_or_equal`, `less_than_or_equal`, `contains`, `not_contains`, `starts_with`, `ends_with`, `exists`, `not_exists`

**Logical operators:** `and`, `or`

**Note:** Conditions are typically nested as groups-within-groups (see the `_filter` example above). Each rule has `operands` (each with `type` and `value`), `operator`, and `type: "rule"`. Rules are wrapped in `type: "group"` with `logical_operator`.

## Loops and Iteration

### `loop_over_*` Operations (Preferred for Native Objects)

For iterating over DevRev native objects, use the dedicated `loop_over_*` operations. These have **static, well-defined schemas** that import reliably. Available operations:

| Operation | Object Type |
|-----------|------------|
| `loop_over_issues` | Issues |
| `loop_over_tickets` | Tickets |
| `loop_over_accounts` | Accounts |
| `loop_over_articles` | Articles |
| `loop_over_customers` | Customers |
| `loop_over_dev_users` | Dev Users |
| `loop_over_enhancements` | Enhancements |
| `loop_over_incidents` | Incidents |
| `loop_over_meetings` | Meetings |
| `loop_over_opportunity` | Opportunities |
| `loop_over_sprints` | Sprints |

**Key structural requirements for `loop_over_*`:**

1. **`block_callback` input port** — MUST be included:
```json
{"name": "block_callback", "schema": {"type": "field_descriptor"}, "type": "block_callback"}
```

2. **`block_start` output port** — uses `type: "block_start"` (NOT `"default"`):
```json
{"name": "block_start", "schema": {"type": "field_descriptor"}, "type": "block_start"}
```

3. **Two `next_steps` edges** — `block_start` to loop body, `output` to after-loop step:
```json
"next_steps": [
  {"port_name": "block_start", "next_step_reference_key": "first_body_step", "next_port_name": "input"},
  {"port_name": "output", "next_step_reference_key": "after_loop_step", "next_port_name": "input"}
]
```

4. **Block body steps** — MUST set `block_step_reference_key` pointing to the loop:
```json
"block_step_reference_key": "loop_over_issues_1"
```

5. **Accessing current item** — Fields are directly on `block_start` (NO `.item` prefix):
```json
"$get('loop_over_issues_1', 'block_start').id"
"$get('loop_over_issues_1', 'block_start').title"
"$get('loop_over_issues_1', 'block_start').stage.name"
```

6. **Input port schema** — Copy the full schema from the operation's schema file (`operations/schemas/loop_over_issues.md`). The `input_ports` include `input` (with filter fields like `owned_by`, `state`, `stages`, `limit`) and `block_callback`.

### `for_each` (Only When No Native Loop Exists)

**WARNING:** `for_each` has a **dynamic schema** (populated by a schema handler at runtime). Its `items_to_iterate` field has `field_type: "array"` but requires a `base_type` that is resolved dynamically — this makes templates using `for_each` prone to import errors like `"discriminator not set: base_type"`. **Only use `for_each` when no dedicated `loop_over_*` operation exists for the object type you need to iterate.**

The `for_each` operation still uses the same block body patterns:
- `block_step_reference_key` on body steps
- `block_start` / `output` port names for next_steps
- `block_callback` input port
- Access current item: `$get('for_each_1', 'block_start').item`

### Variable Management (init_variable + set_variable + $get_variable)

Variables allow accumulating data across loop iterations (e.g., building a summary string). The complete pattern is:

#### Step 1: `init_variable` — Create the variable before the loop

**Input port** with `variables` field (composite `devrev:schema`):
```json
"input_ports": [{"name": "input", "schema": {"field_descriptors": [
  {"field_type": "composite", "composite_type": "devrev:schema", "data_name": "variables", "db_name": "variables",
   "description": "The variables to initialize.", "name": "variables", "oasis": {"name": "variables"}}
], "invalidate_on_field_update": ["variables"], "type": "field_descriptor"}, "type": "default"}]
```

**Input values** — define the variable schema:
```json
"input_values": [{"port_name": "input", "fields": [{"name": "variables", "value": {"type": "literal", "value": {
  "fields": [{"description": "", "field_type": "text", "is_filterable": false, "name": "summary",
    "ui": {"create_view": {}, "detail_view": {"is_hidden": true}, "display_name": "summary", "summary_view": {"is_hidden": true}}
  }]
}}}]}]
```

**Output port** — MUST include `scope_variables` array matching the variable definition:
```json
"output_ports": [{"name": "output", "schema": {"type": "field_descriptor"},
  "scope_variables": [{"field_descriptor": {
    "field_type": "text", "data_name": "summary", "db_name": "summary", "description": "",
    "is_filterable": false, "name": "summary", "oasis": {"name": "summary"},
    "ui": {"create_view": {}, "detail_view": {"is_hidden": true}, "display_name": "summary", "summary_view": {"is_hidden": true}}
  }}],
  "type": "default"}, <error_port>]
```

#### Step 2: `set_variable` — Modify the variable inside the loop body

The `set_variable` step requires `uenum` fields with `allowed_values`. See the **uenum field type** section below.

**Input port** with three fields — `variable` (uenum), `operation` (uenum), `value` (text):
```json
"input_ports": [{"name": "input", "schema": {"field_descriptors": [
  {"field_type": "uenum", "allowed_values": [
    {"id": 1, "label": "init_variable_1 - summary", "ordinal": 1,
     "value": {"output_port_name": "output", "step_reference_key": "init_variable_1", "variable_name": "summary"}}
  ], "data_name": "variable", "db_name": "variable", "description": "Variable",
   "is_required": true, "name": "variable", "oasis": {"name": "variable"}, "ui": {"display_name": "Variable"}},
  {"field_type": "uenum", "allowed_values": [
    {"id": 1, "label": "Set Value", "ordinal": 1, "value": "set"},
    {"id": 2, "label": "Concatenate", "ordinal": 2, "value": "concat"}
  ], "data_name": "operation", "db_name": "operation", "description": "Operation to perform.",
   "is_required": false, "name": "operation", "oasis": {"name": "operation"}, "ui": {"display_name": "Operation"}},
  {"field_type": "text", "data_name": "value", "db_name": "value", "description": "Value",
   "is_filterable": false, "is_required": true, "name": "value", "oasis": {"name": "value"}, "ui": {"display_name": "Value"}}
], "invalidate_on_field_update": ["variable"], "type": "field_descriptor"}, "type": "default"}]
```

**Input values** — specify which variable, what operation, and what value:
```json
"input_values": [{"port_name": "input", "fields": [
  {"name": "variable", "value": {"type": "literal", "value": {
    "output_port_name": "output", "step_reference_key": "init_variable_1", "variable_name": "summary"}}},
  {"name": "operation", "value": {"type": "literal", "value": "concat"}},
  {"name": "value", "value": {"type": "text_template", "value": "{% expr $get('http_1', 'output').body %}"}}
]}]
```

#### Step 3: `$get_variable` — Read the accumulated variable after the loop

After the loop completes, access the variable using `$get_variable()`:
```
{% expr $get_variable('init_variable_1','summary',{'output_port_name' : 'output'}) %}
```

Syntax: `$get_variable('<init_step_ref>', '<variable_name>', {'output_port_name': 'output'})`

## Sleep/Wait Operations

**Sleep For** (pause for duration — value is a literal object with `seconds`):
```json
{
  "name": "Sleep For",
  "operation": { "namespace": "devrev", "slug": "sleep_for" },
  "reference_key": "sleep_for_1",
  "input_values": [{
    "port_name": "input",
    "fields": [{
      "name": "duration",
      "value": { "type": "literal", "value": {"seconds": 30} }
    }]
  }]
}
```

**Sleep Until** (pause until timestamp from another step):
```json
{
  "name": "Sleep Until",
  "operation": { "namespace": "devrev", "slug": "sleep_until" },
  "reference_key": "sleep_until_1",
  "input_values": [{
    "port_name": "input",
    "fields": [{
      "name": "sleep_until",
      "value": { "type": "jsonata_expression", "value": "$get('ticket_created_1', 'output').target_close_date" }
    }]
  }]
}
```

## Ask AI Operation

Use `ask_ai` for AI-powered text analysis, classification, or generation:

```json
{
  "name": "Ask AI",
  "operation": { "namespace": "devrev", "slug": "ask_ai" },
  "reference_key": "ask_ai_1",
  "input_values": [{
    "port_name": "input",
    "fields": [
      { "name": "model", "value": { "type": "literal", "value": "Normal (default)" } },
      { "name": "output_format", "value": { "type": "literal", "value": "text" } },
      {
        "name": "ai_input",
        "value": {
          "type": "text_template",
          "value": "Analyze the following comment and classify as Escalate or Ignore: {% expr $get('timeline_comment_created_1', 'output').body %}"
        }
      }
    ]
  }]
}
```

Access the AI response: `$get('ask_ai_1', 'output').ai_output`

## HTTP Operation

Use `http` for external API calls:

```json
{
  "name": "HTTP Request",
  "operation": { "namespace": "devrev", "slug": "http" },
  "reference_key": "http_1",
  "input_values": [{
    "port_name": "input",
    "fields": [
      { "name": "method", "value": { "type": "literal", "value": "POST" } },
      { "name": "auth_type", "value": { "type": "literal", "value": "Bearer" } },
      { "name": "url", "value": { "type": "text_template", "value": "https://api.devrev.ai/rev-users.get" } },
      { "name": "body", "value": { "type": "text_template", "value": "{\"id\":\"{% expr $get('prev_step', 'output').user_id %}\"}" } }
    ]
  }]
}
```

## Code Node (`invoke_code`)

The Code node executes custom Python code within a workflow. Use it when native nodes don't provide the flexibility needed — data transformation, complex calculations, text processing, custom business logic, or dynamic value generation.

### How It Works

1. The workflow engine collects `input_values` mapped from previous steps
2. Python code executes in a secure sandbox (the `run` function receives inputs as a dict)
3. The `run` function returns a dict — returned values become available to downstream nodes via `output_schema`

### Template Structure

The `invoke_code` step requires three input fields. **Critical type requirements:**
- `code` uses `text_template` type (NOT `literal`)
- `input_values` items use `composite_value_list` (NOT `composite_value`)
- `output_schema` uses `literal` with full UI metadata on each field

```json
{
  "name": "Execute Code",
  "description": "Runs custom Python code",
  "reference_key": "invoke_code_1",
  "operation": { "namespace": "devrev", "slug": "invoke_code" },
  "input_values": [{
    "port_name": "input",
    "fields": [
      {
        "name": "code",
        "value": {
          "type": "text_template",
          "value": "import re\n\ndef run(inputs):\n    text = inputs.get('description', '')\n    cleaned = re.sub(r'\\\\bagent\\\\b', 'computer', text, flags=re.IGNORECASE)\n    return {'cleaned': cleaned}"
        }
      },
      {
        "name": "input_values",
        "value": {
          "type": "list_value",
          "value": {
            "items": [{
              "type": "composite_value_list",
              "value": [{
                "fields": [
                  { "name": "value", "value": { "type": "jsonata_expression", "value": "$get('trigger_1', 'output').description" } },
                  { "name": "name", "value": { "type": "literal", "value": "description" } }
                ]
              }]
            }]
          }
        }
      },
      {
        "name": "output_schema",
        "value": {
          "type": "literal",
          "value": {
            "fields": [{
              "description": "",
              "field_type": "text",
              "is_filterable": false,
              "name": "cleaned",
              "ui": {
                "create_view": {},
                "detail_view": {"is_hidden": true},
                "display_name": "cleaned",
                "summary_view": {"is_hidden": true}
              }
            }]
          }
        }
      }
    ]
  }],
  "next_steps": [
    { "port_name": "output", "next_step_reference_key": "next_step_1", "next_port_name": "input" }
  ]
}
```

### Input Fields

| Field | Value Type | Required | Description |
|-------|------|----------|-------------|
| `code` | `text_template` | Yes | Python code with a `def run(inputs):` function that returns a dict |
| `input_values` | `list_value` of `composite_value_list` | No | Array of `{name, value}` pairs passed as the `inputs` dict |
| `output_schema` | `literal` object with UI metadata | Yes* | `{fields: [{name, field_type, description, is_filterable, ui}]}` — defines outputs available downstream |

*Output schema is required if downstream steps reference the code's outputs.

### Output Port Schema

The `output` port's `field_descriptors` must match the `output_schema` fields:
```json
"output_ports": [
  {"name": "output", "schema": {"field_descriptors": [
    {"field_type": "text", "data_name": "cleaned", "db_name": "cleaned", "description": "cleaned", "name": "cleaned", "oasis": {"name": "cleaned"}}
  ], "type": "field_descriptor"}, "type": "default"},
  <error_port>
]
```

### Output Schema Field Types

| `field_type` | Python Return Type | Example |
|-------------|-------------------|---------|
| `text` | `str` | `"hello"` |
| `int` | `int` | `42` |
| `double` | `float` | `3.14` |
| `bool` | `bool` | `True` / `False` |
| `timestamp` | `str` (ISO 8601) | `"2026-01-28T18:08:38+0000"` |
| `id` | `str` (DevRev ID) | `"don:core:dvrv-us-1:devo/0:ticket/123"` |
| `[]text` | `list[str]` | `["a", "b", "c"]` |

### Accessing Code Outputs

Downstream steps reference code outputs via JSONata:

```
$get('invoke_code_1', 'output').word_count
$get('invoke_code_1', 'output').is_long
```

### Python Environment

**Execution limits:**
- Timeout: 30s default, max 120s (configurable)
- Memory: 256MB
- Output + log size: 512KB

**Available libraries:** `json`, `re`, `math`, `datetime`, `requests`, `collections`, `itertools`, `functools`, `string`, `random`, `decimal`, `statistics`, `typing`, `copy`, `textwrap`, `hashlib`, `hmac`, `secrets`, `base64`, `uuid`, `urllib`, `html`, `xml`, `csv`, `zipfile`, `gzip`, `zoneinfo`, `io`, `dataclasses`, `operator`

**Blocked libraries:** `os`, `sys`, `subprocess`, `shutil`, `pathlib`, `multiprocessing`, `threading`, `concurrent`, `pickle`

### Code Patterns

**Data transformation:**
```python
def run(inputs):
    tags_str = inputs.get('tags', '')
    return {'tag_list': [t.strip() for t in tags_str.split(',') if t.strip()]}
```

**Text processing with regex:**
```python
import re
def run(inputs):
    text = inputs.get('text', '')
    emails = re.findall(r'[\\w.-]+@[\\w.-]+\\.\\w+', text)
    cleaned = re.sub(r'@\\w+', '', text).strip()
    return {'emails': emails, 'cleaned_text': cleaned, 'has_emails': len(emails) > 0}
```

**Conditional routing logic:**
```python
def run(inputs):
    severity = inputs.get('severity', 'low').lower()
    scores = {'critical': 100, 'high': 75, 'medium': 50, 'low': 25}
    score = scores.get(severity, 25)
    return {'priority_score': score, 'escalate': score >= 75, 'team': 'tier-2' if score >= 75 else 'tier-1'}
```

**Date arithmetic:**
```python
from datetime import datetime, timedelta
def run(inputs):
    created = inputs.get('created_date', '')
    dt = datetime.fromisoformat(created.replace('Z', '+00:00'))
    sla_deadline = (dt + timedelta(hours=24)).isoformat()
    return {'sla_deadline': sla_deadline}
```

**Dynamic message generation:**
```python
def run(inputs):
    name = inputs.get('customer_name', 'Customer')
    ticket_id = inputs.get('ticket_id', 'N/A')
    summary = inputs.get('summary', '')[:100]
    msg = f"Hello {name}, your ticket #{ticket_id} has been received. Summary: {summary}"
    return {'notification_message': msg, 'subject_line': f'[#{ticket_id}] Support request received'}
```

**HTTP request (using requests library):**
```python
import requests
def run(inputs):
    url = inputs.get('api_url', '')
    resp = requests.get(url, timeout=10)
    data = resp.json()
    return {'status': resp.status_code, 'result': data.get('result', '')}
```

### Best Practices

- Always use `inputs.get('key', default)` — never `inputs['key']`
- Define output schema for every key returned — outputs are invisible downstream without it
- Add `try`/`except` for error handling; return error info in outputs
- Keep code focused — one Code node per logical operation
- Use `\\n` for newlines when embedding code in the JSON `code` field (the value is a single string)

## UI Metadata

Assign positions to steps for visual layout. Space them logically:
- Triggers at the top (y ~0)
- Subsequent steps flow downward (increment y by ~150)
- Branches spread horizontally (vary x by ~200-300)

```json
"ui_metadata": {
  "position": { "x": 0, "y": 0 }
}
```

## Reference Key Naming Convention

Use the operation slug with a numeric suffix (matching the pattern from example templates):
- Triggers: `ticket_created_1`, `issue_updated_1`, `opportunity_updated_1`
- Actions: `add_comment_1`, `update_ticket_1`, `send_notification_1`, `get_account_1`
- Controls: `if_else_1`, `for_each_1`, `while_1`
- Blockings: `sleep_for_1`, `sleep_until_1`

## Available Operations

Consult these files for the full list of available operations:

- **Triggers (145):** `.claude/skills/create-workflow-template/operations/triggers.md`
- **Actions (245):** `.claude/skills/create-workflow-template/operations/actions.md`
- **Blockings (2):** `.claude/skills/create-workflow-template/operations/blockings.md`
- **Controls (8):** `.claude/skills/create-workflow-template/operations/controls.md`

## Operation Schemas (Input/Output Ports)

For 106 operations, detailed resolved schemas are available with exact field names, types, required flags, enum values, and composite sub-fields:

- **Schema Index:** `.claude/skills/create-workflow-template/operations/schema-index.md` (39 triggers, 62 actions, 5 other)
- **Per-operation schemas:** `.claude/skills/create-workflow-template/operations/schemas/<slug>.md`

Use these schemas to determine the exact field names and types when building `input_values` for a step. For example, to see what fields `create_ticket` accepts, read `operations/schemas/create_ticket.md`.

## Example Templates

Study these real-world templates in `.claude/skills/create-workflow-template/examples/` for patterns:

### Validated Working Examples (`working-*.json`)

These templates have been confirmed to import successfully. **Always study these first** when building similar workflows:

| File | Steps | Pattern Demonstrated |
|------|-------|---------------------|
| `working-loop-variable-sample.json` | 7 | **Loop + variable pattern**: timer_trigger -> init_variable -> loop_over_issues -> [http -> set_variable] -> ask_ai -> send_notification. Shows `loop_over_*`, `init_variable`, `set_variable`, `$get_variable`, `block_step_reference_key`, `uenum` fields, `block_callback` port, timer trigger with uenum |
| `working-invoke-code-sample.json` | 4 | **Code node pattern**: enhancement_updated -> if_else -> invoke_code -> update_enhancement. Shows `invoke_code` with `text_template` code, `composite_value_list` inputs, `output_schema` with UI metadata |
| `working-csat-score-on-ticket-resolved.json` | 5 | **HTTP + AI pattern**: ticket_updated -> if_else (check resolved) -> http (timeline-entries API) -> ask_ai (CSAT analysis) -> add_comment. Shows HTTP with Bearer auth, `text_template` body with expressions, AI prompt composition |
| `working-enhancement-replace-agent.json` | 4 | **Code + update pattern**: enhancement_updated -> if_else (contains check) -> invoke_code (regex replace) -> update_enhancement. Shows `invoke_code` with Python regex, passing data between steps |

### Reference Examples (from production)

| File | Steps | Pattern Demonstrated |
|------|-------|---------------------|
| `4392-Async opportunity review agent` | 2 | Simple trigger -> action, agent interaction |
| `4505-Auto-update issue tcd` | 3 | Trigger with `fields_to_watch` + `_filter`, chained updates |
| `5216-Account segment missing notification` | 4 | Trigger -> get -> if_else -> comment, `_filter` with stage check |
| `4441-Ticket escalator from customer message` | 11 | Complex: trigger -> ask_ai -> if_else -> get -> nested if_else -> update + notify |
| `3592-Generate rca from pia` | 8 | Incident trigger -> ask_ai -> create_article, if_else branching |
| `5040-Devrevu enablement journey poc emails` | 8 | Custom object trigger, HTTP calls, sleep_for, third-party operations |
| `5158-Devrevu enablement journey mailing` | 43 | Complex sequential flow with many sleep_for + if_else + third-party email |

To read an example: `python3 -c "import json; d=json.load(open('.claude/skills/create-workflow-template/examples/FILENAME.json')); inner=json.loads(d['data']); print(json.dumps(inner, indent=2))"`

## Complete Example: Opportunity Updated -> Check Account -> Notify

Based on the real `5216-Account segment missing notification` template pattern:

```json
{
  "serialization_version": { "major": 2, "minor": 0, "patch": 0 },
  "title": "Account Segment Missing Notification",
  "description": "When an opportunity moves to evaluate stage, check if the account has a segment set. If not, notify the opportunity owner.",
  "steps": [
    {
      "name": "Opportunity Updated",
      "description": "Triggers when an opportunity is updated",
      "reference_key": "opportunity_updated_1",
      "operation": { "namespace": "devrev", "slug": "opportunity_updated" },
      "input_values": [{
        "port_name": "input",
        "fields": [
          {
            "name": "fields_to_watch",
            "value": {
              "type": "list_value",
              "value": { "items": [{ "type": "literal", "value": ["stage"] }] }
            }
          },
          {
            "name": "_filter",
            "value": {
              "type": "literal",
              "value": {
                "type": "group",
                "logical_operator": "and",
                "negate": false,
                "conditions": [{
                  "type": "group",
                  "logical_operator": "and",
                  "negate": false,
                  "conditions": [{
                    "type": "rule",
                    "operator": "equals",
                    "operands": [
                      { "type": "jsonata_expression", "value": "$get('opportunity_updated_1', 'output').stage.name" },
                      { "type": "literal", "value": "3-evaluate" }
                    ]
                  }]
                }]
              }
            }
          }
        ]
      }],
      "next_steps": [
        { "port_name": "output", "next_step_reference_key": "get_account_1", "next_port_name": "input" }
      ],
      "ui_metadata": { "position": { "x": 0, "y": 0 } }
    },
    {
      "name": "Get Account",
      "description": "Fetch the account linked to the opportunity",
      "reference_key": "get_account_1",
      "operation": { "namespace": "devrev", "slug": "get_account" },
      "input_values": [{
        "port_name": "input",
        "fields": [{
          "name": "id",
          "value": { "type": "jsonata_expression", "value": "$get('opportunity_updated_1', 'output').account.id" }
        }]
      }],
      "next_steps": [
        { "port_name": "output", "next_step_reference_key": "if_else_1", "next_port_name": "input" }
      ],
      "ui_metadata": { "position": { "x": 0, "y": 150 } }
    },
    {
      "name": "Check Segment Exists",
      "description": "Check if the account has a segment set",
      "reference_key": "if_else_1",
      "operation": { "namespace": "devrev", "slug": "if_else" },
      "input_values": [{
        "port_name": "input",
        "fields": [{
          "name": "condition",
          "value": {
            "type": "literal",
            "value": {
              "type": "group",
              "logical_operator": "and",
              "negate": false,
              "conditions": [{
                "type": "group",
                "logical_operator": "and",
                "negate": false,
                "conditions": [{
                  "type": "rule",
                  "operator": "exists",
                  "operands": [
                    { "type": "jsonata_expression", "value": "$get('get_account_1', 'output').tnt__segment" }
                  ]
                }]
              }]
            }
          }
        }]
      }],
      "next_steps": [
        { "port_name": "true", "next_step_reference_key": "add_comment_1", "next_port_name": "input" }
      ],
      "ui_metadata": { "position": { "x": 0, "y": 300 } }
    },
    {
      "name": "Add Comment",
      "description": "Notify the opportunity owner about missing segment",
      "reference_key": "add_comment_1",
      "operation": { "namespace": "devrev", "slug": "add_comment" },
      "input_values": [{
        "port_name": "input",
        "fields": [
          {
            "name": "object",
            "value": { "type": "jsonata_expression", "value": "$get('opportunity_updated_1', 'output').id" }
          },
          {
            "name": "visibility",
            "value": { "type": "literal", "value": "internal" }
          },
          {
            "name": "body",
            "value": {
              "type": "text_template",
              "value": "Hey {% expr $get('opportunity_updated_1', 'output').owned_by[0].id %}, the opportunity has moved to evaluate stage but the account is missing a segment. Please update it."
            }
          }
        ]
      }],
      "ui_metadata": { "position": { "x": 0, "y": 450 } }
    }
  ]
}
```

## Validation Checklist

Before outputting the template, verify:

1. `serialization_version` is `{"major": 2, "minor": 0, "patch": 0}`
2. `title` is present and 1-256 chars
3. Every step has a unique `reference_key` matching `^[a-zA-Z_][a-zA-Z0-9_]*$`
4. Every step has a valid `operation` with `namespace` and `slug`
5. At least one step is a trigger operation
6. All `next_step_reference_key` values reference existing steps
7. `if_else` steps use `"true"` and `"false"` port names
8. `loop_over_*`/`for_each`/`while` steps use `"block_start"` and `"output"` port names
9. Steps inside loops have `block_step_reference_key` pointing to their parent loop
10. JSONata expressions use `$get('reference_key', 'output')` syntax referencing existing steps
11. Input field names match the operation's input schema (check `.claude/skills/create-workflow-template/operations/*.md`)
12. No circular references in `next_steps`
13. Value types use string names: `"literal"`, `"jsonata_expression"`, `"text_template"`, `"list_value"`, `"composite_value"`, `"composite_value_list"`
14. Trigger `_filter` conditions use the nested group-of-groups structure
15. The final output is wrapped: `{"templateVersion": "2.0.0", "data": "<stringified inner JSON>"}`
16. **All `uenum` fields have `allowed_values`** with `{id, label, ordinal, value}` objects — missing this causes "Missing required field: allowed_values"
17. **All `array` fields have `base_type`** (`id`, `text`, `composite`, `enum`) — missing this causes "discriminator not set: base_type"
18. **`loop_over_*` steps** have `block_callback` input port and `block_start` output port with `type: "block_start"`
19. **`init_variable` output port** includes `scope_variables` array matching the variable definitions
20. **`invoke_code` code field** uses `text_template` type (not `literal`), input_values items use `composite_value_list` (not `composite_value`)
21. **Use `loop_over_*`** instead of `for_each` for native DevRev objects (issues, tickets, accounts, etc.) — `for_each` has dynamic schemas that cause import errors
22. **After loops**, read accumulated variables with `$get_variable('init_variable_1','var_name',{'output_port_name':'output'})` (not `$get`)
