---
description: "Maintainer rules for the thoughtful-ai-template repository itself. Not part of the template; not copied into project repositories."
applyTo: "**"
---
# Maintaining this repository

This repository houses AI-assistant guidance that is copied by hand into other
project repositories. That makes it different from the projects it serves, and
these rules apply only here.

## Nothing on `main` may reference maintainer-only paths

Files on `main` are the copy surface. A reference to the `documentation` branch
or to `.github/CHANGELOG.md` dangles as soon as the file is copied, so those
paths must never appear in `AGENTS.md`, `CLAUDE.md`, `.github/`, or
`.cursor/`. The same applies to any path this template does not itself ship.

`.claude/` is maintainer configuration for this repository and is not copied
into project repositories.

## Record the *why* in the changelog

After changing the operating model — `AGENTS.md`, the files in
`.github/instructions/`, the WAF reference, or any platform adapter — add an
entry to `.github/CHANGELOG.md` on the `documentation` branch. Git history
captures what changed; the changelog captures why, and which alternatives were
rejected. Skip it for typo fixes and formatting.

Do not create a changelog or worklog on `main`.

## Verify platform behavior before asserting it

These materials make claims about how Claude Code, Copilot, Codex, and Cursor
discover instruction files. Those behaviors change. Confirm a claim against
current official documentation before writing it down or acting on it.

<!-- This file is loaded automatically by Claude Code (any .md under
.claude/rules/) and by VS Code Copilot (.claude/rules is a default
chat.instructionsFilesLocations folder, and applyTo: "**" makes it apply to
every request). Codex reads neither, so it will not see these rules. -->
