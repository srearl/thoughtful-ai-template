# Intentional AI Template

A lightweight, **well-architected but non-spec-driven** workflow template for
R and Python projects with AI coding assistants.

## When to use this template

| Situation | Template |
|---|---|
| Exploratory analysis, quick fix, solo work, known domain | **this one** |
| New feature, cross-team, high-stakes, long-lived, or large ambiguity | [spec-driven-ai-template](https://github.com/srearl/spec-driven-ai-template) |

## What this template provides

- **Cross-platform operating model** in `AGENTS.md`, with lightweight adapters
  for GitHub Copilot, Claude Code, and Cursor.
- **Language standards** in `.github/instructions/` (R, Python, analysis work)
  for platforms that support scoped instruction files.
- **WAF awareness** baked into the operating model as an inline check, not a
  gated phase. The agent briefly notes relevant pillars before implementing.
- **Authoritative-source discipline** — the agent verifies API signatures and
  config keys from documentation, not model memory.
- **Local WAF reference** in `.github/ai-reference/waf/` — version-controlled
  pillar checklists the agent consults for non-trivial design choices.
- **Domain skills** in `.github/skills/` — load on demand (e.g. EML/EDI
  metadata, other domain knowledge you add over time).

## What this template intentionally omits

- `specs/` directory and spec-writing workflow
- `/spec` and `/plan` prompt commands
- `Architect` and `Planner` agents
- Mandatory stop-and-sign-off before coding

## Quick start

1. Use this repo as a GitHub template ("Use this template" button).
2. Clone the new repo and open it in VS Code.
3. Start from `AGENTS.md`; it is the canonical operating model for AI agents.
4. Platform-specific adapters point back to `AGENTS.md`:
   `.github/copilot-instructions.md`, `CLAUDE.md`, and `.cursor/rules/`.
5. When asking an assistant to implement something non-trivial, it should
   briefly state the relevant WAF pillars and any trade-offs before writing
   code.

## Platform support

Different AI tools discover repository instructions from different file names.
This template keeps the shared guidance in `AGENTS.md` and uses small adapter
files for tool-specific discovery.

| Platform | Entry point |
|---|---|
| Codex / many agentic coding tools | `AGENTS.md` |
| GitHub Copilot | `.github/copilot-instructions.md` and `.github/instructions/` |
| Claude Code | `CLAUDE.md` |
| Cursor | `.cursor/rules/project.mdc` |

The `.github/instructions/*.instructions.md` files remain useful for Copilot's
path-scoped rules. Other assistants may not load them automatically, so
`AGENTS.md` tells agents to consult task-specific instructions when relevant.

## Reuse model

Use this repository primarily as a GitHub template so instruction files are
copied into the root of each new project. A Git submodule can version the shared
guidance, but most AI tools do not automatically discover instruction files
nested inside a submodule. If a consuming project uses a submodule, keep
root-level adapter files in that project that point into the submodule.

## Connecting live documentation (optional but recommended)

Add MCP servers to `.vscode/mcp.json` for live documentation lookup:
- **Microsoft Learn** — Azure and .NET docs
- **context7** — live R-package documentation
- **Google Cloud docs** — GCP architecture guidance

## PostgreSQL workflow (recommended)

For database-heavy R/Python projects, use
`.github/instructions/postgresql.instructions.md` and prefer MCP-backed
PostgreSQL operations in VS Code.

- Why MCP-first: safer read-first workflow, schema/context inspection before
  changes, easier query-plan and performance diagnostics.
- Suggested sequence: inspect context -> run read-only queries -> apply reviewed
  modifications.
- Keep SQL in version control (`*.sql`) so PostgreSQL instructions auto-apply.

### PostgreSQL MCP quick start

1. Connect with your saved profile and select the target database.
2. Fetch schema context before changes (tables, indexes, functions).
3. Run a read-only query first to validate assumptions.
4. For slow queries, capture query plan/metrics before optimizing.
5. Apply reviewed modifications and re-check context/row counts.

See `.github/CHANGELOG.md` for the rationale behind each operating-model
decision.
