# Xcode Project Team

A set of Claude Code skills and agents for running Swift/Xcode projects — new development, feature work, or C/C++-to-Swift ports — under a director + role-subagent structure.

## Layout

- `agents/` — real Claude Code Agent definitions (`*.md` with model/tools frontmatter).
- `skills/` — Skill definitions (`<name>/SKILL.md`), flat one level deep.
- `docs/delphine-l/claude_global/` — a gitignored working checkout of an external skills repo (see "Exploring marketplace options" below).

## Directors (Agents, not Skills)

- **`swift-project-director`** — directs new or existing Swift/macOS/iOS development (greenfield design or brownfield feature work, not porting) using a PDCA plan/build/check/document cycle.
- **`swift-port-director`** — directs C/C++-to-Swift porting projects, preserving functional parity, using the same PDCA cycle plus a mandatory parity-audit gate before any component is marked done.

Both directors act as their own Planner (sequencing, decisions log, rulings on open questions) rather than delegating that role out, and orchestrate the roles below as subagents, each pointed at its own skill. Both scale their own process to fix size — small, non-architectural items get a lighter dispatch/review path by default rather than the full pre-brief → code → report → ruling → audit pipeline on every item (see each agent's "Scale process to fix size" section).

These two live in `agents/` (not as Skill directories) because Skill frontmatter can't enforce a model or tool set — only a real Claude Code Agent definition does, and both directors are meant to always run on Sonnet with full tool access regardless of whatever model invoked them.

**Install**: symlink `agents/*.md` into `~/.claude/agents/`, and each directory under `skills/` into `~/.claude/skills/<name>/`, so edits made in this repo take effect immediately without re-copying.

## Roles

- **`xcode-admin`** — repo hygiene and housekeeping: git pre-flight checks, stale-lock cleanup, notes-doc archiving, plain status reporting. Never writes code, never rules on decisions.
- **`xcode-implementer`** — writes a pre-brief before coding, implements a component/feature, builds and tests it, and files a completion report with before/after test counts and flagged judgment calls.
- **`xcode-quality-auditor`** — adversarial review, run on Opus. Two modes: **PARITY** (hand-traces ported Swift against the original C/C++ oracle, function by function) and **QUALITY** (reviews new/existing Swift work against `Plan.md`/tests/conventions, plus the relevant Apple-domain best-practice skills — SwiftUI, App Intents, accessibility, security). Files findings only; never writes fixes, never marks work done.

`swift-port-director` spawns the Quality Auditor in PARITY mode as a mandatory gate after every full-track component. `swift-project-director` spawns it in QUALITY mode at its own discretion (before a risky merge, at a milestone, or when a component touches a domain worth a second look).

## Other skills

- **`xcode-network-engineer`** — accumulated Network.framework findings for Swift (both the structured-concurrency API and the older completion-handler API), plus a known EINVAL hosting-bug pattern.
- **`delphine-l-claude-collaboration`**, **`delphine-l-claude-skill-management`**, **`delphine-l-command-discipline`**, **`delphine-l-documentation`**, **`delphine-l-token-efficiency`** — general Claude Code working-practice skills pulled in from [Delphine-L/claude_global](https://github.com/Delphine-L/claude_global)'s `claude-meta` category (team collaboration, skill authoring/symlinking, bare shell-command style, session documentation, token-efficient tool use). Prefixed to avoid confusion with this repo's own conventions and with similarly-named skills from other sources (e.g. `superpowers:systematic-debugging`).

## How it fits together

Each director creates and maintains a small set of living documents in the target project's repo (`Plan.md`, `DecisionLog.md`, `agent_notes.md`, etc.), then spawns Admin/Implementer/Quality-Auditor subagents against those files — telling each one which files are its "plan doc" and "notes doc," since the role skills themselves are project-agnostic. All roles share the same discipline: orient from the repo state (never trust a stale summary), do the work within scope, file a report, and commit before considering anything done — an entry that only exists in chat is invisible to the next subagent.

## Exploring marketplace options

Right now this repo (and Delphine's) are installed by symlinking individual `agents/`/`skills/` entries into `~/.claude/agents/` and `~/.claude/skills/` by hand. Claude Code also supports a more turnkey distribution path: a **plugin marketplace**, installable in one step via `/plugin marketplace add <repo>`.

As a proof of concept, `docs/delphine-l/claude_global` (a gitignored local checkout, not part of this repo's own history) has a branch, `add-plugin-marketplace`, that turns her `claude_global` skills repo into a real marketplace:

- A `.claude-plugin/marketplace.json` at the repo root, listing one self-hosting plugin at `source: "./"` — the same shape used by two already-working installed marketplaces on this machine (`xcode-skills`, `karpathy-skills`), confirmed by inspection rather than guessed.
- Her `skills/<category>/<name>/SKILL.md` layout flattened to `skills/<category>-<name>/SKILL.md`, because the plugin loader only discovers skills exactly one level under `skills/` — the same constraint that shaped this repo's own flat `skills/` layout and the `delphine-l-*` naming above.
- `commands/`, `hooks/`, `templates/`, and her `enable-skills.sh` symlink workflow left untouched, so nothing breaks for people not using the marketplace path.

That branch is local-only (we have no push access to her GitHub); it's meant as a working reference she can pull in or fold back into her own repo if she wants a one-command install path, and as a template if this repo ever wants the same treatment — turning `agents/` + `skills/` here into a `.claude-plugin/marketplace.json` at this repo's root would follow the identical recipe, since our `skills/` is already flat.
