# AUTO-LEARNED

## Working Preferences

- Prefer minimal, test-backed fixes before proposing broader refactors.
- Keep responses concise by default; expand only when requested.
- After code edits, run affected tests and report concrete outcomes.

## Coding Workflow

- Use `rg` for text/file discovery before broader scans.
- For Python scripts in this repo, use `uv run` instead of direct `python`.
- When debugging failures, inspect first failing error before summary-level output.

## Communication

- When uncertain, state assumption explicitly and continue with best-effort execution.
- Separate "what changed" from "how it was validated" in final summaries.

## Topic Index

- Debugging playbook: `learned-debugging.md`
