# `invoke_code` — Schema Reference

Executes custom Python code within a workflow. Secure sandbox: 30s default timeout (max 120s), 256MB memory, 512KB output limit.

**Template requirements:** Must include `input_ports`, `output_ports`, AND `input_values` with all three fields configured.

## Input Port: `input`

Standard input_ports schema (same for all invoke_code steps):

```json
"input_ports": [{
  "name": "input",
  "schema": {
    "composite_schemas": [{
      "description": "A variable",
      "fields": [
        {"field_type": "tokens", "data_name": "name", "db_name": "name", "description": "The name of the variable", "is_required": true, "name": "name", "oasis": {"name": "name"}},
        {"field_type": "json_value", "data_name": "value", "db_name": "value", "description": "The value of the variable", "name": "value", "oasis": {"name": "value"}}
      ],
      "name": "variable"
    }],
    "field_descriptors": [
      {"field_type": "array", "base_type": "composite", "composite_type": "variable", "data_name": "input_values", "db_name": "input_values", "description": "The input variables to pass to the code. The inputs will be passed to run(...) as a dictionary.", "name": "input_values", "oasis": {"name": "input_values"}},
      {"field_type": "composite", "composite_type": "devrev:schema", "data_name": "output_schema", "db_name": "output_schema", "description": "The schema of the output variables to return from the code. The outputs should be returned from run(...) as a dictionary.", "name": "output_schema", "oasis": {"name": "output_schema"}},
      {"field_type": "text", "data_name": "code", "db_name": "code", "default_value": "def run(inputs):\n    return {}", "description": "Write your code here. Do not update the run function signature.", "is_required": true, "name": "code", "oasis": {"name": "code"}, "ui": {}}
    ],
    "invalidate_on_field_update": ["output_schema", "input_values"],
    "type": "field_descriptor"
  },
  "type": "default"
}]
```

## Output Ports

The `output` port must include resolved `field_descriptors` matching the `output_schema` fields. Plus the standard `error` port.

```json
"output_ports": [
  {"name": "output", "schema": {"field_descriptors": [
    {"field_type": "text", "data_name": "my_output", "db_name": "my_output", "description": "my_output", "name": "my_output", "oasis": {"name": "my_output"}}
  ], "type": "field_descriptor"}, "type": "default"},
  {"name": "error", "schema": {"field_descriptors": [
    {"field_type": "text", "data_name": "message", "db_name": "message", "description": "Error message with more details about the error", "name": "message", "oasis": {"name": "message"}, "ui": {"display_name": "Error Message"}},
    {"field_type": "enum", "allowed_values": ["bad_request", "not_found", "internal"], "data_name": "type", "db_name": "type", "description": "Type of the error", "name": "type", "oasis": {"name": "type"}, "ui": {"display_name": "Error Type"}}
  ], "type": "field_descriptor"}, "type": "error"}
]
```

## Input Values Pattern

All three fields must be set in `input_values`:

### `code` — uses `text_template` type (NOT `literal`)

```json
{
  "name": "code",
  "value": {
    "type": "text_template",
    "value": "import re\n\ndef run(inputs):\n    text = inputs.get('description', '')\n    cleaned = re.sub(r'\\bagent\\b', 'computer', text, flags=re.IGNORECASE)\n    return {'cleaned': cleaned}"
  }
}
```

### `input_values` — uses `composite_value_list` (NOT `composite_value`)

```json
{
  "name": "input_values",
  "value": {
    "type": "list_value",
    "value": {
      "items": [{
        "type": "composite_value_list",
        "value": [{
          "fields": [
            {"name": "value", "value": {"type": "jsonata_expression", "value": "$get('trigger_1', 'output').description"}},
            {"name": "name", "value": {"type": "literal", "value": "description"}}
          ]
        }]
      }]
    }
  }
}
```

### `output_schema` — literal with field definitions including UI metadata

```json
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
```

## Complete Working Example

From production template "Knowledge Gaps Workflow V1":

- Input: `object_ids` (array from hybrid_search output)
- Code: extracts article ID from object IDs array
- Output: `objectid` (text)

```python
def run(inputs):
    object_ids = inputs.get("object_ids", [])
    return {"objectid": f"ART-{object_ids[0].split('/')[-1]}" if object_ids else ""}
```

## Available Python Libraries

`json`, `re`, `math`, `datetime`, `requests`, `collections`, `itertools`, `functools`, `string`, `random`, `decimal`, `statistics`, `typing`, `copy`, `textwrap`, `hashlib`, `hmac`, `secrets`, `base64`, `uuid`, `urllib`, `html`, `xml`, `csv`, `zipfile`, `gzip`, `zoneinfo`, `io`, `dataclasses`, `operator`

**Blocked:** `os`, `sys`, `subprocess`, `shutil`, `pathlib`, `multiprocessing`, `threading`, `concurrent`, `pickle`
