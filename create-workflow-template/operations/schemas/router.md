# `router` — Schema Reference

Route workflow into parallel paths, executing only the routes whose conditions are met.

**Release Stage:** Beta

## Input Port: `input`

Schema is **dynamic** (Overridden by schema handler). The input includes route conditions configured at design time.

## Output Port: `output`

Schema is **dynamic** (Overridden by schema handler). Multiple output routes are defined at design time, each with its own condition.

> **Note:** The router operation's schema (both input conditions and output routes) is fully managed by the schema handler. Routes and their conditions are configured in the workflow editor.
