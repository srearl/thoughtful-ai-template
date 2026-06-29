# Intentional AI — GitHub Copilot Template

A lightweight, **well-architected but non-spec-driven** workflow template for
R and Python projects in VS Code with GitHub Copilot.

## When to use this template

| Situation | Template |
|---|---|
| Exploratory analysis, quick fix, solo work, known domain | **this one** |
| New feature, cross-team, high-stakes, long-lived, or large ambiguity | [spec-driven-ai-template](https://github.com/srearl/spec-driven-ai-template) |

## What this template provides

- **Language standards** auto-applied via `.github/instructions/` (R, Python,
  analysis work) — no manual loading required.
- **WAF awareness** baked into the operating model as an inline check, not a
  gated phase. The agent briefly notes relevant pillars before implementing.
- **Authoritative-source discipline** — the agent verifies API signatures and
  config keys from documentation, not model memory.
- **Local WAF reference** in `docs/waf/` — version-controlled pillar checklists
  the agent consults for non-trivial design choices.
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
3. The instructions in `.github/instructions/` activate automatically based on
   file type (`*.R`, `*.py`, etc.).
4. When asking Copilot to implement something non-trivial, it will briefly state
   the relevant WAF pillars and any trade-offs before writing code — no extra
   prompting required.

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
