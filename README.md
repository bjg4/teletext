# TELEtext

Context-transfer compiler for AI sessions. Compiles a full conversation into a structured handoff package so a different AI can continue the work immediately.

## Install

```bash
npx skills add bjg4/teletext
```

## What it does

TELEtext captures everything needed to continue a conversation in a new session:

- **Decisions** with rationale, options considered, and tradeoffs
- **Facts** with confidence levels and evidence pointers
- **Conversation performance** — dead ends, corrections, what worked, what didn't
- **User preferences** and working style
- **Open loops** — unresolved questions, fragile assumptions, missing inputs
- **TODOs** with suggested next messages for the receiving AI

The output is a single self-contained markdown file fronted by an actionable boot sequence.

## Usage

Say any of these in a conversation:

- `/teletext`
- "teletext this"
- "transfer context"
- "create a handoff"
- "compile context"

TELEtext adapts automatically based on session weight:

| Tier | When | Output |
|------|------|--------|
| **Light** | < 10 exchanges, few decisions | Compact single-block package |
| **Standard** | 10–50 exchanges, multiple decisions | Full 13-section package |
| **Deep** | 50+ exchanges, many decisions and artifacts | Expanded package with appendices |

## How it works

1. Assesses session weight to pick the right tier
2. Checks for previous packages to produce an update with a checkpoint header
3. Compiles a self-describing preamble + boot sequence + full package body
4. Runs internal quality checks (coverage, contradictions, fragility)
5. Writes to `teletext-YYYY-MM-DD-[slug].md` in the working directory

The boot sequence (~10 lines) gives the receiving AI enough to start working before reading the full package.

## Compatibility

Works with any AI coding agent that supports the [Agent Skills](https://github.com/anthropics/skills) specification (SKILL.md format). Built for Claude Code.

## License

MIT
