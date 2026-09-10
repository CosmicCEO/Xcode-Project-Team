# Xcode Project Team

A set of Claude Code skills for running Swift/Xcode projects — new development, feature work, or C/C++-to-Swift ports — under a director + role-subagent structure.

## Directors

- **`swift-project-director`** — directs new or existing Swift/macOS/iOS development (greenfield design or brownfield feature work, not porting) using a PDCA plan/build/check/document cycle.
- **`swift-port-director`** — directs C/C++-to-Swift porting projects, preserving functional parity, using the same PDCA cycle plus a mandatory parity-audit gate before any component is marked done.

Both directors act as their own Planner (sequencing, decisions log, rulings on open questions) rather than delegating that role out, and orchestrate the roles below as subagents, each pointed at its own skill.

## Roles

- **`xcode-admin`** — repo hygiene and housekeeping: git pre-flight checks, stale-lock cleanup, notes-doc archiving, plain status reporting. Never writes code, never rules on decisions.
- **`xcode-implementer`** — writes a pre-brief before coding, implements a component/feature, builds and tests it, and files a completion report with before/after test counts and flagged judgment calls.
- **`xcode-quality-auditor`** — adversarial review, run on Opus. Two modes: **PARITY** (hand-traces ported Swift against the original C/C++ oracle, function by function) and **QUALITY** (reviews new/existing Swift work against `Plan.md`/tests/conventions, plus the relevant Apple-domain best-practice skills — SwiftUI, App Intents, accessibility, security). Files findings only; never writes fixes, never marks work done.

`swift-port-director` spawns the Quality Auditor in PARITY mode as a mandatory gate after every component. `swift-project-director` spawns it in QUALITY mode at its own discretion (before a risky merge, at a milestone, or when a component touches a domain worth a second look).

## Other skills

- **`swift-network-engineer`** — accumulated Network.framework findings for Swift (both the structured-concurrency API and the older completion-handler API), plus a known EINVAL hosting-bug pattern.

## How it fits together

Each director creates and maintains a small set of living documents in the target project's repo (`Plan.md`, `DecisionLog.md`, `agent_notes.md`, etc.), then spawns Admin/Implementer/Quality-Auditor subagents against those files — telling each one which files are its "plan doc" and "notes doc," since the role skills themselves are project-agnostic. All roles share the same discipline: orient from the repo state (never trust a stale summary), do the work within scope, file a report, and commit before considering anything done — an entry that only exists in chat is invisible to the next subagent.
