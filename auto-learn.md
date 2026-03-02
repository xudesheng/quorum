# Auto-Learn Guide

This guide explains what Auto-Learn is for, the intended use cases, and which prompts in the TUI can reliably trigger memory save/remove behavior.

## 1. Purpose of Auto-Learn

Auto-Learn helps the agent accumulate reusable working knowledge across sessions, so you do not need to repeat stable preferences and workflows.

It is mainly useful for:

1. Repeated personal preferences (commit style, response format, testing habits).
2. Reusable engineering habits across projects (debugging order, validation flow).

## 2. Intended Use (What to Save)

Good candidates to save:

- Stable user preferences (for example, conventional commits).
- Cross-project engineering habits (for example, run affected tests after edits).
- Recurrent troubleshooting patterns that are generally applicable.

Avoid saving:

- One-off session context.
- Project-private internals (private paths, internal APIs, sensitive details).
- Unverified guesses.

## 3. Prompts That Trigger Save in TUI

The most reliable trigger is an explicit “remember” request.

Examples:

- `Remember this: always propose the minimal fix before refactor options.`
- `Please remember my preference: use conventional commit messages.`
- `Remember this workflow preference: run affected tests after code changes.`

Topic-scoped examples:

- `Remember under debugging: check the first failing error before summary output.`
- `Remember under tooling: prefer rg for search and uv run for Python scripts.`

## 4. Prompts That Trigger Forget/Remove in TUI

The most reliable trigger is an explicit “forget/remove this memory” request.

Examples:

- `Forget my previous preference about running full test suites before every commit.`
- `Please remove this memory: responses must always be very long.`
- `Forget this memory: always use provider X by default.`

Topic-scoped example:

- `Forget this in debugging: restart machine first before investigation.`

## 5. Recommended Prompt Templates

Save template:

```text
Please remember this long-term preference:
<preference or reusable practice>
If a similar memory already exists, update it instead of duplicating it.
```

Forget template:

```text
Please forget this memory:
<content to remove>
If there are close duplicates, clean those up as well.
```

Verification template:

```text
Please summarize what you currently remember about <topic>.
```

## 6. Use Together With `/learn` Commands

Recommended command pairing:

- `/learn list` to inspect available topics.
- `/learn show <topic>` to review a topic file.
- `/learn clean` to remove stale or low-quality entries.
- `/learn off` to temporarily disable auto-learn writes.
- `/learn on` to re-enable it.

Suggested flow:

1. Trigger save/forget with natural language.
2. Verify with `/learn show` or `/learn list`.
3. Periodically run `/learn clean`.

## 7. Practical Tips

- Keep each memory short and actionable.
- Prefer “how to do” rules over long context narratives.
- Do not use “remember” prompts for secrets or credentials.
