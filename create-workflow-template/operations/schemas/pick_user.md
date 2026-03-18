# `pick_user` — Schema Reference

Picks a user from a group or list of users based on the given strategy (e.g., round robin).

## Input Port: `input`

Schema is **dynamic** (Overridden by schema handler). Typically includes:
- The group or list of users to pick from.
- The selection strategy (e.g., round robin).

## Output Port: `output`

Schema is **dynamic** (Overridden by schema handler). Typically includes:
- The selected user ID.

> **Note:** Both input and output schemas are managed by the schema handler at design time.
