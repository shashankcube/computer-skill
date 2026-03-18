# `for_each` — Schema Reference

Iterate over a list of items. Executes a block of steps for each item in the list.

**WARNING:** `for_each` has a **dynamic schema** (populated by a schema handler at runtime). Its `items_to_iterate` field has `field_type: "array"` but requires a `base_type` discriminator that is resolved dynamically. This makes `for_each` **unreliable in templates** — it often causes `"discriminator not set: base_type"` errors on import.

**Prefer `loop_over_*` operations** for iterating over native DevRev objects. Available:
- `loop_over_issues`, `loop_over_tickets`, `loop_over_accounts`, `loop_over_articles`
- `loop_over_customers`, `loop_over_dev_users`, `loop_over_enhancements`
- `loop_over_incidents`, `loop_over_meetings`, `loop_over_opportunity`, `loop_over_sprints`

Only use `for_each` when no dedicated `loop_over_*` operation exists for the data you need to iterate.

## Input Port: `input`

Schema is **dynamic** (populated by schema handler). Typically includes the list/array to iterate over.

## Input Port: `block_callback`

- Type: BlockCallback
- Schema: empty (receives data back from the loop body block).

```json
{"name": "block_callback", "schema": {"type": "field_descriptor"}, "type": "block_callback"}
```

## Output Port: `output`

Schema is **dynamic** (populated by schema handler). Contains aggregated results after iteration completes.

## Output Port: `block_start`

Schema is **dynamic** (populated by schema handler). Contains the current item from the list being iterated. This port connects to the body of the loop. Uses `type: "block_start"` (NOT `"default"`).

```json
{"name": "block_start", "schema": {"type": "field_descriptor"}, "type": "block_start"}
```

Access current item: `$get('for_each_1', 'block_start').item`

## Block Body Pattern

Steps inside the loop body MUST:
1. Set `"block_step_reference_key": "for_each_1"` on each body step
2. The `for_each` step must have `next_steps` with both `block_start` (to body) and `output` (after loop)
