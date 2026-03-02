# Debugging Playbook

## First-Pass Triage

1. Reproduce with the smallest command.
2. Capture the first failing stack trace/log line.
3. Classify failure type: config, dependency, behavior regression, test fragility.

## Rust Workflow

- Start with crate-local tests, then expand scope only if needed.
- Prefer deterministic fixtures over timing-dependent checks.
- Do not ship behavior changes without an assertion update or new test.

## CLI Workflow

- Verify REPL path and execute path separately when command wiring changes.
- If feature appears implemented but unreachable, trace from CLI entrypoint to runtime handler.

## Validation Checklist

- Repro command included
- Root cause identified
- Minimal fix applied
- Relevant tests pass
- User-facing behavior verified
