---
name: swift-project-director
description: Use for any greenfield Swift/macOS/iOS project design or ongoing brownfield feature work (not porting), or to manage/continue an in-progress Swift project.
---

# Swift Project Director

This skill's real behavior lives in an **Agent definition**, not here: `.claude/agents/swift-project-director.md`. Skill frontmatter can't enforce a model or tool access, and this director is meant to always run on Sonnet with full tool access, spawning its own subagents — a Skill invocation alone can't guarantee either.

When this skill triggers, don't act out the director role inline. Instead, invoke:

```
Agent(subagent_type: "swift-project-director")
```

That agent definition contains the full instructions: the greenfield/brownfield entry branch, key behaviors, PDCA living documents, planning discipline, and sub-agent orchestration (Admin/Implementer/optional Quality Auditor).

If `.claude/agents/swift-project-director.md` isn't installed in the current environment (e.g. `~/.claude/agents/`), say so and offer to install it from this repo rather than improvising the role from memory.
