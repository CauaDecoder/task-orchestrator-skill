# Task Orchestrator

A Codex skill for coordinating tasks through planning, controlled multi-agent work, and independent verification.

## Install

Copy the SKILL.md file to the Codex skills directory:

$CODEX_HOME/skills/task-orchestrator/SKILL.md

If CODEX_HOME is not set, use the skills directory configured for your Codex installation. Refresh the agent environment if needed.

Invoke explicitly with $task-orchestrator followed by the task to coordinate.

## Agent roles

The skill includes prompts for the coordinator, read-only discovery, implementation, and test/QA agents, plus optional validation and defensive security review. The role contracts are self-contained and do not require repository-specific AGENTS.md or CLAUDE.md files.
