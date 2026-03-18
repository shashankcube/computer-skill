# `set_variable` — Schema Reference

Sets a variable with the given value. Typically used inside loop bodies to accumulate data across iterations.

## Input Port: `input`

Schema is **dynamic** (Overridden by schema handler), but the template pattern is well-defined. Has three fields — all using `uenum` or `text` field types:

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
  ], "data_name": "operation", "db_name": "operation", "description": "Operation to perform. Defaults to 'Set Value' if not specified.",
   "is_required": false, "name": "operation", "oasis": {"name": "operation"}, "ui": {"display_name": "Operation"}},
  {"field_type": "text", "data_name": "value", "db_name": "value", "description": "Value",
   "is_filterable": false, "is_required": true, "name": "value", "oasis": {"name": "value"}, "ui": {"display_name": "Value"}}
], "invalidate_on_field_update": ["variable"], "type": "field_descriptor"}, "type": "default"}]
```

**CRITICAL:** The `variable` and `operation` fields are `uenum` type and MUST have `allowed_values`. Missing `allowed_values` causes "Missing required field: allowed_values" on import.

### `variable` allowed_values

The `variable` field's `allowed_values` references the `init_variable` step:
```json
{"id": 1, "label": "init_variable_1 - summary", "ordinal": 1,
 "value": {"output_port_name": "output", "step_reference_key": "init_variable_1", "variable_name": "summary"}}
```

### `operation` allowed_values

Always include both options:
```json
[
  {"id": 1, "label": "Set Value", "ordinal": 1, "value": "set"},
  {"id": 2, "label": "Concatenate", "ordinal": 2, "value": "concat"}
]
```

## Input Values

```json
"input_values": [{"port_name": "input", "fields": [
  {"name": "variable", "value": {"type": "literal", "value": {
    "output_port_name": "output", "step_reference_key": "init_variable_1", "variable_name": "summary"}}},
  {"name": "operation", "value": {"type": "literal", "value": "concat"}},
  {"name": "value", "value": {"type": "text_template", "value": "{% expr $get('http_1', 'output').body %}"}}
]}]
```

## Output Port: `output`

```json
"output_ports": [
  {"name": "output", "schema": {"type": "field_descriptor"}, "type": "default"},
  {"name": "error", "schema": {"field_descriptors": [
    {"field_type": "text", "data_name": "message", "db_name": "message", "description": "Error message with more details about the error", "name": "message", "oasis": {"name": "message"}, "ui": {"display_name": "Error Message"}},
    {"field_type": "enum", "allowed_values": ["bad_request", "not_found", "internal"], "data_name": "type", "db_name": "type", "description": "Type of the error", "name": "type", "oasis": {"name": "type"}, "ui": {"display_name": "Error Type"}}
  ], "type": "field_descriptor"}, "type": "error"}
]
```
