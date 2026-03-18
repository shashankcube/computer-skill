# computer-skill

A collection of [DevRev Computer Skills](https://github.com/devrev/computer-skills) — AI-powered skills that extend Claude Code with DevRev-specific capabilities.

## Skills

### Create Workflow Template

Generate valid DevRev workflow template JSON from natural language descriptions.

**What it does:** Takes a plain-English description of a workflow and produces a production-ready DevRev workflow template that can be imported directly into DevRev.

**Usage:**

```
/create-workflow-template "When a ticket is created with critical priority, send a Slack notification and assign it to the on-call team"
```

**How it works:**

1. Parses your natural language description to identify triggers, actions, conditions, and loops
2. Looks up the correct operation slugs, namespaces, and schemas from 130+ operation definitions
3. References validated example templates to match production format
4. Outputs a complete workflow JSON ready for import

**Supported operations include:**

- **Triggers** — ticket/issue/conversation created/updated, timer, webhook, and more
- **Actions** — create/update tickets, issues, conversations; send emails/notifications; HTTP calls; invoke code; AI agent interactions
- **Controls** — if/else branching, while loops, routers, delays
- **Blockings** — loop over lists (articles, issues, conversations, etc.)

### Directory Structure

```
create-workflow-template/
  create-workflow-template.md   # Main skill definition
  examples/                     # 11 example templates (4 validated working-* templates)
  operations/
    actions.md                  # Action operations reference
    triggers.md                 # Trigger operations reference
    controls.md                 # Control flow operations reference
    blockings.md                # Blocking/loop operations reference
    schema-index.md             # Schema lookup index
    schemas/                    # 130 detailed operation schemas
```

### Example Templates

| Template | Description |
|----------|-------------|
| `working-csat-score-on-ticket-resolved.json` | Set CSAT score when a ticket is resolved |
| `working-enhancement-replace-agent.json` | Agent-based enhancement replacement workflow |
| `working-invoke-code-sample.json` | Custom code invocation example |
| `working-loop-variable-sample.json` | Loop with variable manipulation |

## Setup

To use this skill with Claude Code, copy the `create-workflow-template/` directory into your project's `.claude/skills/` folder:

```bash
# From your project root
mkdir -p .claude/skills
cp -r create-workflow-template .claude/skills/
```

Then invoke it with `/create-workflow-template` in Claude Code.

## License

This skill is based on the [DevRev Computer Skills](https://github.com/devrev/computer-skills) repository.
