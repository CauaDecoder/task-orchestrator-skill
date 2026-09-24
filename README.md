# Task Orchestrator

Skills for coordinating tasks through planning, controlled multi-agent work, and independent verification.

The repository ships two editions with different names, so they can be installed side by side without conflicts:

| Edition | Skill name | Folder | Invoke with |
|---|---|---|---|
| Codex (also used by opencode and Antigravity) | `task-orchestrator` | `codex/` | `$task-orchestrator <task>` |
| Claude Code | `claude-task-orchestrator` | `claude/` | `/claude-task-orchestrator <task>` |

## Codex edition

Copy `codex/task-orchestrator/SKILL.md` to the skills directory of each tool:

| Tool | Destination |
|---|---|
| Codex | `$CODEX_HOME/skills/task-orchestrator/SKILL.md` (default `~/.codex/skills/...`) |
| opencode | `~/.config/opencode/skills/task-orchestrator/SKILL.md` |
| Antigravity | `~/.gemini/antigravity/skills/task-orchestrator/SKILL.md` |

Refresh the agent environment if needed.

## Claude Code edition

Copy the skill and its subagents:

```text
claude/skills/claude-task-orchestrator/SKILL.md -> ~/.claude/skills/claude-task-orchestrator/SKILL.md
claude/agents/orchestrator-*.md                 -> ~/.claude/agents/
```

Restart Claude Code so it loads the new agents. The skill uses `disable-model-invocation`, so it only runs when invoked explicitly.

Each role is a subagent whose model and effort are pinned in its frontmatter:

| Role | Subagent | Model | Effort |
|---|---|---|---|
| Coordinator | `orchestrator-coordinator` | Opus 5.5 | medium |
| Reader / researcher | `orchestrator-reader` | Opus 5.5 | low |
| Implementer | `orchestrator-implementer` | Sonnet 5 | high |
| Implementation advisor (hard tasks) | `orchestrator-advisor` | Opus 5.5 | low |
| Test / QA | `orchestrator-qa` | Sonnet 5 | medium |
| Operational validation | `orchestrator-validator` | Sonnet 5 | medium |
| Security | `orchestrator-security` | Opus 5.5 | medium |

The root session launches every agent, because subagents cannot spawn subagents. Only the implementer writes code. The advisor is consulted read-only when the implementer returns `BLOCKED_NEEDS_SECOND_OPINION`. Requires a Claude Code version that supports these models (`claude update`).

## Agent roles

Both editions include prompts for the coordinator, read-only discovery, implementation, and test/QA agents, plus optional validation and defensive security review. The role contracts are self-contained and do not require repository-specific AGENTS.md or CLAUDE.md files.
