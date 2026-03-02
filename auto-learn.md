# Auto-Learn

Auto-Learn is Quorum's persistent personal memory layer for coding workflows.
It helps the agent retain stable preferences and reusable engineering practices across sessions, so each new task starts with less re-explaining and better execution quality.

## What It Is

Auto-Learn stores reusable experience in your user scope:

- `~/.quorum/AUTO-LEARNED.md` (high-signal index, loaded at session start)
- `~/.quorum/learned/*.md` (topic details)

This is personal memory, not project policy.

- Project rules: `AGENTS.md`, `CLAUDE.md`
- Project knowledge: `.quorum/MEMORY.md`
- Personal reusable experience: Auto-Learn (`~/.quorum/...`)

## Why It Matters

Auto-Learn is designed to improve day-to-day coding throughput:

- Less repetition: you do not restate stable preferences every session.
- Better consistency: the agent follows the same working style over time.
- Faster recovery: recurring troubleshooting patterns are already available.
- Cross-project leverage: useful habits carry from repo to repo.

## Best-Fit Use Cases

Save these:

- Stable communication and delivery preferences
- Reusable coding/debugging/checking workflows
- Recurring problem-solving heuristics

Do not save these:

- One-off task context
- Project-private implementation details
- Secrets, credentials, or tokens
- Unverified assumptions

## Trigger Save in TUI (Prompts That Work)

Use explicit "remember" intent.

Examples:

- `Remember this preference: propose minimal fix first, then optional refactor.`
- `Remember my workflow: run affected tests after every code change.`
- `Remember under tooling: use rg for search and uv run for Python scripts.`

## Trigger Forget in TUI (Prompts That Work)

Use explicit "forget/remove" intent.

Examples:

- `Forget this memory: responses must always be long.`
- `Please remove this preference: always run full test suite before commit.`
- `Forget under debugging: restart machine first before investigation.`

## Recommended Prompt Templates

Save:

```text
Please remember this long-term preference:
<preference or reusable practice>
If a similar memory exists, update it instead of creating duplicates.
```

Forget:

```text
Please forget this memory:
<memory to remove>
If close duplicates exist, remove those as well.
```

Verify:

```text
Please summarize what you currently remember about <topic>.
```

## Operational Commands

Use natural-language prompts plus `/learn` commands for control and verification:

- `/learn list`
- `/learn show <topic>`
- `/learn clean`
- `/learn off`
- `/learn on`

Suggested flow:

1. Trigger save/forget in plain language.
2. Verify with `/learn show`.
3. Clean periodically with `/learn clean`.

## Sample Files

- [AUTO-LEARNED sample](./samples/AUTO-LEARNED.md)
- [Topic sample (debugging)](./samples/learned/debugging.md)

## Security Note

Never ask Auto-Learn to remember secrets. If sensitive text appears in memory, remove it immediately and rotate exposed credentials.
