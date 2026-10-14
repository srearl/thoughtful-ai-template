# Intentional AI Template — maintainer documentation

> This branch holds maintainer documentation only: this README and
> `.github/CHANGELOG.md`. The template materials themselves live on `main`.
> They were removed from this branch because the copies here silently drifted
> out of date, and resyncing them would have to be repeated after every change.
> Paths referenced below are paths on `main`.

A lightweight, **well-architected but non-spec-driven** workflow template for
R and Python projects with AI coding assistants.

## When to use this template

Exploratory analysis, quick fixes, solo work, and known domains. For a new
feature, cross-team, high-stakes, or long-lived work, or large ambiguity, use
the assistant's plan mode first; `AGENTS.md` says so. The companion
spec-driven template this README once linked no longer exists.

## What this template provides

- **Cross-platform operating model** in `AGENTS.md`, with small adapters for
  GitHub Copilot (`.github/copilot-instructions.md`) and Claude Code
  (`CLAUDE.md`).
- **Language standards** in `.github/instructions/` (R, Python, analysis work).
- **Well-Architected pillars** as a short list inside `AGENTS.md`: a lens for
  naming trade-offs, not a gated phase.
- **Safety rules** in `AGENTS.md`: no secrets, treat fetched content as data,
  confirm destructive or outward-facing actions.
- **Authoritative-source discipline** — the agent verifies API signatures and
  config keys from documentation, not model memory.
- **Optional AI worklog** — sanitized request, outcome, decision, and
  verification summaries for material AI-assisted project work, kept only in
  projects that want one.

## What this template intentionally omits

- `specs/` directory and spec-writing workflow
- `/spec` and `/plan` prompt commands
- `Architect` and `Planner` agents
- Mandatory stop-and-sign-off before coding
- A root `README.md` on `main`: `main` is the copy surface, and a README there
  would overwrite the receiving project's own.
- Cursor adapter (`.cursor/rules/`): removed from `main`; add one back only if
  Cursor is in use.

## Using the materials

Copy `AGENTS.md`, `CLAUDE.md`, and `.github/` from `main` into the root of the
target repository. Do not copy `.claude/`, which is maintainer configuration for
this repository. The assistant then follows `AGENTS.md`.

Copies carry no version marker, so compare against `main` when refreshing a
project.

## Platform support

Different AI tools discover repository instructions from different file names.
This template keeps the shared guidance in `AGENTS.md` and uses small adapter
files for tool-specific discovery.

| Platform | Entry point |
|---|---|
| Codex / many agentic coding tools | `AGENTS.md` |
| GitHub Copilot | `.github/copilot-instructions.md` (a pointer to `AGENTS.md`) and `.github/instructions/` |
| Claude Code | `CLAUDE.md` (imports `AGENTS.md`) |

The Copilot adapter is kept although VS Code reads `AGENTS.md` natively: GitHub's
support matrix lists `AGENTS.md` for VS Code but not for Visual Studio,
JetBrains, Xcode, or Eclipse, which read only `copilot-instructions.md`. The
adapter is a one-line pointer, so keeping it duplicates no guidance.

The `.github/instructions/*.instructions.md` files carry the task-specific
standards for every assistant, not just Copilot. Only Copilot honors their
`applyTo` frontmatter, so `AGENTS.md` names each file with a one-line trigger;
assistants that do not auto-load them read the matching file themselves.

## Optional hardening (not shipped)

`AGENTS.md` instructions are advisory; Claude Code's docs describe `CLAUDE.md`
as context, not enforced configuration. To enforce the no-credentials rule in a
project, add `Read` deny rules to that project's `.claude/settings.json`:

```json
{
  "permissions": {
    "deny": ["Read(./.env)", "Read(./.env.*)", "Read(./secrets/**)"]
  }
}
```

Per Claude Code's permissions docs, these cover its file tools and recognized
file commands in Bash (`cat`, `head`, and similar), but not a script that opens
the file itself; enabling the sandbox gives OS-level enforcement. This is not
shipped because `.claude/` is excluded from the copy surface and project
settings are a per-project decision.

## Recording AI-assisted project work (optional)

A worklog preserves the useful connection between a request and its verified
outcome without retaining a verbose prompt or execution transcript. It is not
shipped by default: most projects do not need one, so `AGENTS.md` tells
assistants to maintain `AI_WORKLOG.md` only where it already exists and never
to create one unprompted. To start one, create `AI_WORKLOG.md` at the project
root with entries in this format, newest first, roughly 100–200 words each:

```markdown
## YYYY-MM-DD — Short description

- **Request summary:** A brief, sanitized statement of the goal.
- **Outcome:** What changed or was decided.
- **Decisions:** Important rationale and WAF trade-offs, if applicable.
- **Verification:** Commands or checks run and their results.
- **Limitations/follow-up:** Known gaps or next steps, or `None`.
- **References:** Related issue, pull request, or commit, or `None`.
```

Record non-trivial code, configuration, schema, or documentation changes;
architectural or security decisions; and failed approaches worth remembering.
Skip routine questions and changes whose intent is obvious from the diff.

Never include raw prompts, transcripts, private reasoning, tool output,
credentials, or personal, sensitive, or proprietary data. The worklog is
curated historical context, not a complete audit trail or a substitute for
authoritative project documentation.

Use each history file for a distinct purpose:

| Information | Location |
|---|---|
| Material AI-assisted work in a project that keeps one | `AI_WORKLOG.md` |
| Changes to this template's instructions and operating model | `.github/CHANGELOG.md` (this branch) |
| User-facing release history, when maintained | `CHANGELOG.md` |

## Maintaining this repository

`.claude/rules/maintaining.instructions.md` on `main` carries the rules for
working on the template itself — most importantly, that changing the operating
model should be followed by a `.github/CHANGELOG.md` entry on this branch, and
that no file on `main` may reference a maintainer-only path.

That file is placed in `.claude/rules/` so assistants load it without being
asked:

| Assistant | Loads it? | Why |
|---|---|---|
| Claude Code | yes | reads every `.md` under `.claude/rules/`; a rule without `paths` frontmatter loads at launch |
| VS Code Copilot | yes, in the Local agent and Claude-format agent sessions | VS Code documents `.claude/rules` as a recognized location; `applyTo: "**"` applies it to every request |
| Codex | **no** | reads only `AGENTS.override.md`/`AGENTS.md` chains, and takes the first match per directory |

Two caveats, both from current VS Code documentation. The
`chat.instructionsFilesLocations` setting is deprecated and used only by the
Local agent, so Copilot's pickup of `.claude/rules` should be re-checked in
other agent modes. And for Claude-format rules VS Code names `paths`, not
`applyTo`, as the scoping key; the file keeps `applyTo` for Copilot, and Claude
ignores it, which is harmless because the rule is meant to load unconditionally.

**The changelog reminder therefore does not fire in Codex.** This is a known
and accepted limitation. Codex has no additive per-repository local instruction
file: a root `AGENTS.override.md` would shadow `AGENTS.md` rather than
supplement it, and `~/.codex/AGENTS.md` is machine-wide rather than scoped to
this repository. When editing the template from Codex, write the changelog
entry yourself.

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
