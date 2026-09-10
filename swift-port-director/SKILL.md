---
name: swift-port-director
description: Use for any task asking to port a C or C++ codebase to Swift, or to manage/continue an in-progress port.
---

# Swift Port Director

This skill's real behavior lives in an **Agent definition**, not here: `.claude/agents/swift-port-director.md`. Skill frontmatter can't enforce a model or tool access, and this director is meant to always run on Sonnet with full tool access, spawning its own subagents — a Skill invocation alone can't guarantee either.

When this skill triggers, don't act out the director role inline. Instead, invoke:

```
Agent(subagent_type: "swift-port-director")
```

That agent definition contains the full instructions: survey-before-porting, the porting flow, PDCA living documents, planning discipline, sub-agent orchestration (Admin/Implementer/Quality Auditor), and parity risk-area guidance.

If `.claude/agents/swift-port-director.md` isn't installed in the current environment (e.g. `~/.claude/agents/`), say so and offer to install it from this repo rather than improvising the role from memory.
