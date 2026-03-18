# `init_variable` — Schema Reference

Initializes a variable of a given type. Used to create scope variables that can be modified by `set_variable` steps (typically inside loops) and read later by `$get_variable()`.

## Input Port: `input`

Schema is **dynamic** (Overridden by schema handler), but the template pattern is well-defined:

```json
"input_ports": [{"name": "input", "schema": {"field_descriptors": [
  {"field_type": "composite", "composite_type": "devrev:schema", "data_name": "variables", "db_name": "variables",
   "description": "The variables to initialize.", "name": "variables", "oasis": {"name": "variables"}}
], "invalidate_on_field_update": ["variables"], "type": "field_descriptor"}, "type": "default"}]
```

## Input Values

Define the variable(s) as a schema with fields:

```json
"input_values": [{"port_name": "input", "fields": [{"name": "variables", "value": {"type": "literal", "value": {
  "fields": [{
    "description": "",
    "field_type": "text",
    "is_filterable": false,
    "name": "summary",
    "ui": {
      "create_view": {},
      "detail_view": {"is_hidden": true},
      "display_name": "summary",
      "summary_view": {"is_hidden": true}
    }
  }]
}}}]}]
```

Supported `field_type` values: `text`, `int`, `double`, `bool`, `timestamp`, `id`

## Output Port: `output`

The output port MUST include `scope_variables` — an array of field descriptors matching the variables defined in input_values:

```json
"output_ports": [
  {"name": "output", "schema": {"type": "field_descriptor"},
   "scope_variables": [{"field_descriptor": {
     "field_type": "text", "data_name": "summary", "db_name": "summary", "description": "",
     "is_filterable": false, "name": "summary", "oasis": {"name": "summary"},
     "ui": {"create_view": {}, "detail_view": {"is_hidden": true}, "display_name": "summary", "summary_view": {"is_hidden": true}}
   }}],
   "type": "default"},
  {"name": "error", "schema": {"field_descriptors": [
    {"field_type": "text", "data_name": "message", "db_name": "message", "description": "Error message with more details about the error", "name": "message", "oasis": {"name": "message"}, "ui": {"display_name": "Error Message"}},
    {"field_type": "enum", "allowed_values": ["bad_request", "not_found", "internal"], "data_name": "type", "db_name": "type", "description": "Type of the error", "name": "type", "oasis": {"name": "type"}, "ui": {"display_name": "Error Type"}}
  ], "type": "field_descriptor"}, "type": "error"}
]
```

## Reading Variables After Loop

Use `$get_variable()` (NOT `$get()`) to read accumulated variable values after the loop:

```
{% expr $get_variable('init_variable_1','summary',{'output_port_name' : 'output'}) %}
```

Syntax: `$get_variable('<init_step_ref>', '<variable_name>', {'output_port_name': 'output'})`
