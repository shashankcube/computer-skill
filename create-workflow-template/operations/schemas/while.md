# `while` — Schema Reference

Executes a block of steps while a condition is true. Maximum iterations of 1000.

## Input Port: `input`

Schema: empty field descriptors (no explicit input fields). The loop condition is configured separately.

## Input Port: `block_callback`

- Type: BlockCallback
- Schema: empty field descriptors (receives data back from the loop body block).

## Output Port: `output`

Schema: empty field descriptors. Execution continues from this port after the loop ends.

## Output Port: `block_start`

- **`iteration_number`** (`int`) — The current iteration number of the loop starting from 1.

### Scope Variables

- **`break`** (`bool`) — Breaks the loop. Default value is `true` for each iteration. Set to `false` within the loop body to continue iterating.
