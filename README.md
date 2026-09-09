# Intentional AI Template

Minimal starter template for intentional (non-spec-driven) R/Python projects
with AI coding assistants.

## Start a new project

1. Create a repository from this template.
2. Open it in your editor or assistant of choice.
3. Follow `AGENTS.md`.

`AGENTS.md` is the canonical operating model. Platform-specific adapters live
in `.github/` and `CLAUDE.md`. Copy those; `.claude/` holds configuration for
maintaining this template repository and is not part of it.

## Optional: keep an AI worklog

Most projects do not need one. If you want a curated record connecting a
request to its verified outcome, create `AI_WORKLOG.md` at the project root;
`AGENTS.md` then tells assistants to maintain it. Keep entries to roughly
100–200 words, newest first:

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
curated historical context — not an audit trail, and not a substitute for the
project's authoritative documentation. Use a root `CHANGELOG.md` for
user-facing release history.
