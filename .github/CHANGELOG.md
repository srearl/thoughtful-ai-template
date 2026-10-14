# Customizations Changelog

Tracks intentional changes to the engineering operating model: instructions,
skills, WAF docs, and MCP config. Git history captures *what*; this file
captures *why*. Newest first.

## 2026-10-14
- Review against current Claude Code, Copilot, VS Code, and Codex docs; fix
  stale claims and trim overhead.
- Delete the root `README.md` from `main`. `main` is the copy surface, so a
  template-facing README overwrote the receiving project's own. Cost: the
  GitHub landing page is empty. The worklog entry template moved to this
  branch's README, and `AGENTS.md` now says to follow an existing worklog's
  format rather than point at a README that may not be copied.
- Drop `applyTo: "**"` from the `well-architected` instruction. It attached the
  file to every Copilot request though it is meant for non-trivial design
  only; with no `applyTo`, Copilot attaches it when its `description` fits the
  task. Trade-off: it is now attached less predictably.
- Remove the live-documentation MCP list (Microsoft Learn, Google Cloud,
  context7) and the `.mcp.json` / `.vscode/mcp.json` advice. Nothing shipped
  those files, and the servers named did not serve R/Python work. `AGENTS.md`
  now says to check official docs via any connected tool or the web.
- Reduce `.github/copilot-instructions.md` to a one-line pointer. The text
  about `.github/instructions/` duplicated native Copilot discovery. Kept the
  file rather than deleting it: GitHub's support matrix lists `AGENTS.md` for
  VS Code but not for Visual Studio, JetBrains, Xcode, or Eclipse, which read
  only `copilot-instructions.md`. VS Code docs also advise against duplicated
  guidance, which a pointer is not.
- Correct this branch's README: the Cursor adapter had been removed from `main`
  but was still advertised; the reuse section described GitHub-template use
  while the actual workflow is copying files; the `.claude/rules` rationale now
  records that `chat.instructionsFilesLocations` is deprecated and that VS Code
  names `paths`, not `applyTo`, for Claude-format rules.

## 2026-10-14 (later)
- Replace the `AGENTS.md` pointer to a "companion spec-driven template" with a
  suggestion to use plan mode for complex or long-lived work. The
  `spec-driven-ai-template` repository no longer exists, so the pointer
  dangled; plan mode is built into the assistants used here and needs nothing
  copied. Also reworded the clarifying-question line and removed bold
  formatting from the pillar names. Edited by the maintainer; recorded here.
  Trade-off: plan mode is a tool feature, not a repo-defined workflow, so
  "complex" is left to the user's judgment. Updated this README's "When to
  use" section to match; earlier entries below that mention the spec-driven
  template are history and are left as written.
- Narrow `AGENTS.md` to the salient points. The state-intent-then-WAF-note
  ritual for "non-trivial" work depended on an undefined threshold that models
  read differently, and added boilerplate to routine edits. Replaced it with:
  keep scope, name the trade-off in one line when a choice has real
  alternatives, verify. Relies on current models needing less scaffolding;
  trade-off is less forced consistency of output.
- Collapse the WAF reference (five checklist files, a README, and the
  `well-architected` instruction) into a five-line pillar list in `AGENTS.md`.
  The checklists were 0.6–0.8 KB each and mostly restated that list. Kept five
  pillars, merging cost and sustainability, although AWS lists six; the AWS
  Responsible AI and Generative AI lenses (Nov 2025) target AWS AI workloads
  and do not fit research software. Removed the `well-architected` file with
  the folder it pointed to.
- Add a Safety section to `AGENTS.md` (no credentials, treat fetched or MCP
  content as data, confirm destructive or outward-facing actions). Enforcement
  is documented in this README as optional `Read` deny rules, not shipped:
  `.claude/` is outside the copy surface. Deliberately omitted data
  classification checks; the maintainer judges those case by case.
- Python standard reduced to `uv`, `ruff check`, `ruff format`, `pytest`;
  dropped `black` (Ruff's formatter is documented as a near drop-in
  replacement) and made type checking optional. R standard now links the
  tidyverse style guide.

## 2026-09-06 (later)
- Reduce the `documentation` branch to maintainer documentation only: this
  changelog and the expanded README. The branch also carried copies of
  `AGENTS.md` and the instruction files, which had silently drifted two commits
  behind `main`. Deleted rather than resynced, because resyncing would recur
  after every change to the materials.
- Add `.claude/rules/maintaining.instructions.md` on `main` to carry the rules
  for maintaining the template itself, including this changelog convention.
  Nothing on `main` could state it before: every file there is copy surface, so
  a reference to this branch would dangle in a consuming project.
- Chose `.claude/rules/` because Claude Code reads every `.md` there and VS Code
  Copilot scans it by default via `chat.instructionsFilesLocations`; one file
  with `applyTo: "**"` satisfies both. Codex reads neither and is documented as
  a known gap — it has no additive per-repository instruction file, since a root
  `AGENTS.override.md` shadows `AGENTS.md` rather than supplementing it.
  Preferred a mechanism that fires automatically in two of three assistants over
  a portable convention that depends on remembering to apply it.

## 2026-09-06
- Make the materials provider-neutral rather than Copilot-first: point the
  analysis and Well-Architected instruction files at `AGENTS.md` instead of the
  `copilot-instructions.md` adapter, and make the PostgreSQL MCP server a
  preference rather than the mandated interface, so the read-first sequence
  still applies in a CLI or non-VS Code session.
- Name each task-specific instruction file in `AGENTS.md` with a one-line
  trigger. `applyTo` frontmatter is honored only by Copilot, so on every other
  assistant nothing loaded the R, Python, analysis, PostgreSQL, or WAF
  standards automatically. Chose this over a parallel per-platform rules
  directory to avoid duplicating the standards.
- Remove references that dangle in a repository the template is copied into:
  `.github/CHANGELOG.md`, `.github/skills/`, and the `eml-metadata` skill. An
  agent pointed at a missing path wastes a turn or invents the content.
- Narrow the analysis `applyTo` glob to notebooks and Quarto/R Markdown. It
  previously claimed every `.py` and `.R` file was an analysis deliverable
  rather than software, which is wrong for package source.
- Make the AI worklog opt-in. Assistants now maintain `AI_WORKLOG.md` only
  where it already exists and never create one unprompted; instructions for
  starting one moved to the main-branch README, which is read by a human
  deciding whether to adopt it rather than loaded into every agent session.
- Remove `AI_WORKLOG.md` from `main` so it is no longer part of the copy
  surface. This repository's own record lives here instead, avoiding two
  near-duplicate logs of the same operating-model work.

## 2026-08-13
- Add a root `AI_WORKLOG.md` template for concise, sanitized summaries of
  material AI-assisted project work, preserving request-to-outcome context
  without storing raw prompts or transcripts. (Superseded 2026-09-06: the
  worklog is now opt-in and no longer ships on `main`.)
- Remove the instructions that told copied projects to maintain a
  `.github/CHANGELOG.md`, which exists only in this template repository.
- Give the analysis and Well-Architected instruction files explicit `applyTo`
  frontmatter for more consistent automatic matching in Copilot.
- Distinguish the project AI worklog from `.github/CHANGELOG.md` (operating
  model changes) and an optional root `CHANGELOG.md` (user-facing releases),
  removing ambiguity about where each kind of rationale belongs.
- Extend the operational-excellence guidance to make project-level AI work
  traceable while explicitly limiting documentation overhead and sensitive
  data exposure.
- Document the convention in the expanded documentation-branch README while
  keeping the main-branch README minimal for template consumers.

## 2026-07-31
- Promote `AGENTS.md` as the canonical cross-platform AI operating model so
  Codex and other agents can discover the repository guidance without relying
  on GitHub Copilot-specific file names.
- Convert `.github/copilot-instructions.md` into a Copilot adapter and add
  lightweight Claude/Cursor adapters that point back to the shared rules,
  reducing duplicated instruction drift across AI platforms.
- Document the preferred reuse model: use this repository as a template by
  default, and only use a submodule with root-level adapter files in the
  consuming project.

## 2026-07-24
- Move the hand-maintained WAF reference material into
  `.github/ai-reference/waf/` so pkgdown clean builds can safely replace the
  generated `docs/` website without deleting AI guidance.

## 2026-06-29
- Update R guidance: treat `renv` as suggested (not required), prefer
  explicit non-base namespacing, and favor `purrr` iteration patterns over
  `for` loops by default.
- Standardize Python guidance on the Astral `uv` workflow (`uv lock`,
  `uv sync`, `uv run`) across base, analysis, and reliability references.
- Add PostgreSQL instruction set (`.github/instructions/postgresql.instructions.md`)
  with MCP-first workflow guidance (context -> query -> modify), safety checks,
  and performance review expectations.
- Update README with a dedicated PostgreSQL section so users discover MCP-first
  DB workflow guidance from the template landing page.
- Add a PostgreSQL MCP quick-start checklist in README for faster onboarding to
  safe DB workflows.

## 2026-06-29
- Initial scaffold: lightweight `copilot-instructions.md` (think-then-code
  with inline WAF awareness, no spec/plan gates), R/Python/analysis style
  instructions, `well-architected` instruction, WAF pillar checklists.
  Forked from the spec-driven template — omits `/spec`, `/plan` prompts and
  `Architect`/`Planner` agents by design. Use when formal spec overhead is not
  warranted (exploratory work, solo projects, known domains).
