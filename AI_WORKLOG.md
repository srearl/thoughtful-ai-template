# AI Worklog

This file is a curated, non-exhaustive record of material AI-assisted work in
this project. It connects a request to its outcome, important decisions, and
verification without preserving a full conversation or tool transcript.

## What to record

Add an entry for non-trivial code, configuration, schema, or documentation
changes; architectural or security decisions; consequential external actions;
and failed approaches that would be useful to future contributors.

Do not add entries for routine questions, formatting-only changes, or other
work whose intent and outcome are already obvious from the diff.

## Content and safety

- Summarize the request; do not copy the prompt verbatim.
- Record observable outcomes and verification results, not unverified claims.
- Note material assumptions, trade-offs, limitations, and follow-up work.
- Link to issues, pull requests, or commits instead of duplicating their
  contents.
- Never include credentials, tokens, personal or sensitive data, proprietary
  prompt content, raw tool output, full transcripts, or private reasoning.

Treat this worklog as historical context, not as current system documentation,
a complete audit trail, or a reproducible record of an AI session. Update the
project's authoritative documentation when behavior or operating procedures
change.

Use a root `CHANGELOG.md`, when present, for user-facing release history.

## 2026-08-13 — Cross-platform instruction cleanup

- **Request summary:** Review the AI instruction template for copied-project
  use, cross-platform Copilot behavior, and the repository-specific changelog.
- **Outcome:** Removed references that tell copied projects to maintain
  `.github/CHANGELOG.md`; made analysis and Well-Architected instruction files
  explicit with `applyTo` frontmatter for more consistent automatic matching.
- **Decisions:** Kept `AGENTS.md` as the canonical operating model and
  platform-specific files as adapters. The template repository's customization
  changelog belongs on the documentation branch, not in copied project files.
- **Verification:** Inspected the instruction files and compared the behavior
  against official VS Code and GitHub Copilot documentation.
- **Limitations/follow-up:** Confirm whether any copy scripts or template
  generation workflows need to exclude branch-only documentation files.
- **References:** None.

## Entry template

Keep each entry concise (normally no more than 100-200 words) and place the
newest entry first.

```markdown
## YYYY-MM-DD — Short description

- **Request summary:** A brief, sanitized statement of the goal.
- **Outcome:** What changed or was decided.
- **Decisions:** Important rationale and WAF trade-offs, if applicable.
- **Verification:** Commands or checks run and their results.
- **Limitations/follow-up:** Known gaps or next steps, or `None`.
- **References:** Related issue, pull request, or commit, or `None`.
```
