# computer-skill

AI-powered skills for Claude Code — sales intelligence, deal management, workflow automation, and agent debugging, built on DevRev.

## Skills

| Skill | Description |
|-------|-------------|
| **create-workflow-template** | Generate DevRev workflow template JSON from natural language descriptions |
| **deal-review-meddpicc** | Full MEDDPICC deal assessment with evidence-tagged scoring |
| **sales-call-plan-coach** | Interactive call plan coaching using Franklin Covey methodology |
| **account-evaluation** | Account health report combining DevRev pipeline, engagement, and external data |
| **account-research** | Company/person research — web + DevRev + enrichment layers |
| **customer-brief** | Pre-meeting briefing with internal intelligence and external context |
| **sales-search-and-lookup** | Query pipeline, forecast, accounts, and opportunities via natural language |
| **sales-context** | Always-active behavioral skill for consistent sales terminology |
| **next-step-for-opportunity** | Extract planned actions from meetings, emails, and discussions |
| **trace-diagnosis** | Debug Computer agent execution traces and auto-create engineering tickets |

## Setup

Copy any skill folder into your project's `.claude/skills/` directory:

```bash
mkdir -p .claude/skills
cp -r <skill-name> .claude/skills/
```

Or install with the skills CLI:

```bash
npx skills add https://github.com/shashankcube/computer-skill --skill <skill-name>
```

## License

Based on [DevRev Computer Skills](https://github.com/devrev/computer-skills).
