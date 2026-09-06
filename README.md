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
- **Optional AI worklog** — sanitized request, outcome, decision, and
  verification summaries for material AI-assisted project work, kept only in
  projects that want one.

## What this template intentionally omits

- `specs/` directory and spec-writing workflow
- `/spec` and `/plan` prompt commands
- `Architect` and `Planner` agents
- Mandatory stop-and-sign-off before coding

## Quick start

1. Use this repo as a GitHub template ("Use this template" button).
2. Clone the new repo and open it in your editor or assistant of choice.
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

The `.github/instructions/*.instructions.md` files carry the task-specific
standards for every assistant, not just Copilot. Only Copilot honors their
`applyTo` frontmatter, so `AGENTS.md` names each file with a one-line trigger;
assistants that do not auto-load them read the matching file themselves.

## Reuse model

Use this repository primarily as a GitHub template so instruction files are
copied into the root of each new project. A Git submodule can version the shared
guidance, but most AI tools do not automatically discover instruction files
nested inside a submodule. If a consuming project uses a submodule, keep
root-level adapter files in that project that point into the submodule.

## Recording AI-assisted project work (optional)

A worklog preserves the useful connection between a request and its verified
outcome without retaining a verbose prompt or execution transcript. It is not
shipped by default: most projects do not need one, so `AGENTS.md` tells
assistants to maintain `AI_WORKLOG.md` only where it already exists and never
to create one unprompted. The main-branch README carries the entry template and
inclusion rules for a project that wants one.

The worklog intentionally omits routine questions, trivial changes, raw
prompts, transcripts, private reasoning, tool output, credentials, and
sensitive or proprietary data. It is curated historical context, not a
complete audit trail or a substitute for authoritative project documentation.

Use each history file for a distinct purpose:

| Information | Location |
|---|---|
| Material AI-assisted work in a project that keeps one | `AI_WORKLOG.md` |
| Changes to this template's instructions and operating model | `.github/CHANGELOG.md` (this branch) |
| User-facing release history, when maintained | `CHANGELOG.md` |

## Connecting live documentation (optional but recommended)

Configure MCP servers where your assistant expects them (`.vscode/mcp.json`
for VS Code, `.mcp.json` for Claude Code) for live documentation lookup:
- **Microsoft Learn** — Azure and .NET docs
- **context7** — live R-package documentation
- **Google Cloud docs** — GCP architecture guidance

## PostgreSQL workflow (recommended)

For database-heavy R/Python projects, use
`.github/instructions/postgresql.instructions.md` and prefer MCP-backed
PostgreSQL operations where a server is connected.

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
