# `go_back` — Schema Reference

Go back to a previous step in the workflow.

## Input Port: `input`

Schema is **dynamic** (implemented in schema handler). The input schema is determined at design time based on which step to go back to.

## Output Port

No output ports defined. This is a terminal control flow operation that redirects execution to a previous step.

> **Note:** The input schema is managed by the schema handler. The target step to go back to is configured at design time.
