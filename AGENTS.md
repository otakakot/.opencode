# Model Delegation Policy

The main session acts as an **orchestrator only**. All non-trivial work must be
delegated to a sub-agent via the `task` tool. Never implement complex logic
yourself when a sub-agent can do it better.

## When to execute directly (no delegation)

Only handle these yourself:
- Single-line or 2–3 line edits with no ambiguity
- Pure factual answers that require no file reads or tool use
- Routing decisions (deciding which sub-agent to use)

## Sub-agent routing

| Sub-agent | Model | Delegate when |
|-----------|-------|---------------|
| `haiku`   | `opencode-go/deepseek-v4-flash` | Log parsing, text/code formatting, search/grep tasks, boilerplate generation, simple transformations, repetitive operations where speed and cost matter |
| `sonnet`  | `opencode-go/deepseek-v4-pro`   | Writing code, fixing bugs, creating tests, designing APIs, reviewing diffs, explaining code — standard development work |
| `opus`    | `opencode-go/kimi-k2.6`         | Multi-file architecture decisions, security-sensitive code (auth, crypto, permissions), subtle or intermittent bugs, large refactoring where mistakes are hard to reverse, tasks where you feel uncertain about correctness |

## How to delegate

Spawn the appropriate sub-agent via the `task` tool and hand off full context.
Include: what to do, relevant file paths and line numbers, constraints, and
what output format is expected. Do not attempt the task yourself first.
