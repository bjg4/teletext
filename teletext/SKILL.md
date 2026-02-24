---
name: teletext
description: "Package a conversation for continuation in a new AI session. Compiles decisions, facts, performance, preferences, and open work into a structured handoff with an actionable boot sequence. Triggers on: 'teletext', 'transfer context', 'create a handoff', 'compile context', 'session handoff', 'context package', 'package this up', 'save this conversation', 'continue this in a new chat', 'I'm switching sessions', or when the user wants to continue work in a different AI session. Adapts automatically — lightweight for short sessions, comprehensive for deep ones. Supports updating previous packages. Do NOT use for simple conversation summaries, recaps, or when the user is not switching sessions."
metadata:
  author: Blake Graham
  version: 3.0.1
  license: MIT
---

# TELEtext — Context-Transfer Compiler

## Overview

TELEtext compiles the full arc of a conversation into a structured, self-describing package that lets a different AI continue the work immediately. The output is a single markdown file — self-contained, portable, and fronted by a boot sequence the receiving AI can act on before reading anything else.

## When to Use

- The user invokes `/teletext` or asks to "teletext this conversation"
- The user asks to transfer context, create a handoff, or compile a session for another AI
- The user is about to switch AI tools, start a new session, or hit a context limit and wants continuity
- The user says "package this up" or "compile this" in the context of session transfer

## When NOT to Use

- The user asks for a summary or recap of the conversation (not a handoff)
- The user wants meeting notes or a brief (different format, different audience)
- The conversation has no substance to transfer (e.g., a single question-and-answer)

## Non-Negotiables

1. **Preserve specificity** — Names, dates, numbers, constraints, definitions, rationale, tradeoffs. Never generalize away detail.
2. **Do not invent details** — When uncertain, mark confidence and state what was observed.
3. **Separate clearly** — FACTS vs DECISIONS vs HYPOTHESES vs OPEN QUESTIONS vs TODOs must never bleed into each other.
4. **Tag origin everywhere** — (U)=user, (A)=assistant, (J)=jointly agreed.
5. **Structured and skimmable** — Prefer lists and table-like formatting over prose.
6. **Privacy filter** — Include personal/sensitive details only if relevant to continuing the work.
7. **Boot sequence first** — The receiving AI's first 10 lines must be actionable, not archival.
8. **Capture performance** — Dead ends, corrections, what worked, what didn't.
9. **Self-describing** — The package tells the receiving AI what it is and how to use it. No external instructions needed.
10. **Encode the exception** — Tag only degraded confidence and fragile assumptions. High confidence is the default; don't annotate the norm.

## Thoroughness

This compilation demands completeness. Do not truncate, abbreviate, or skip sections to save tokens. If the output is long, that is correct — a lossy handoff defeats the purpose. Quality and completeness are more important than speed.

## Compilation Workflow

### Step 1: Assess Session Weight

Before compiling, assess the session to determine the compilation tier:

| Signal | Light | Standard | Deep |
|--------|-------|----------|------|
| Exchange count | < ~10 | 10–50 | 50+ |
| Decisions made | ≤ 2 | Several | Many |
| Artifacts produced | None | Some | Multiple |
| Dead ends / corrections | Minimal | Some | Significant |
| Experiments tried | None | Few | Multiple |

When in doubt, go one tier up. Selection is automatic — the user never configures this.

### Step 2: Check for Previous Package

Before compiling fresh, check the current working directory for existing `teletext-*.md` files.

**If a previous package exists:**
- Read it to understand the prior state
- Compile a full updated package (not a diff — the body is always complete)
- Add a **Checkpoint Header** after the Boot Sequence listing what changed: new decisions, resolved questions, new open loops, status shifts
- Use the same filename slug with an incremented version or updated date

**If no previous package exists:**
- Compile fresh, no checkpoint header

### Step 3: Assess Available Sources

Inventory what is actually accessible:

- Conversation history in the current chat
- User-attached files available in the session
- Connected documents accessible from within the session
- Links referenced in chat (only summarize content if actually opened/read; never infer from link titles)

If the full conversation history is not accessible (e.g., limited context window):

1. State the earliest visible point and what portion may be missing
2. Compile from what is visible
3. Include missing-info items in the Open Loops section, ordered by importance

### Step 4: Compile the Preamble

Write the self-describing preamble as the first line of the file:

> **TELEtext context-transfer package.** This document contains the full state of a prior AI conversation. Read the BOOT SEQUENCE immediately below to orient and take your first action. Consult deeper sections as needed. Do not re-ask questions that are answered in this document.

This tells the receiving AI what the document is, how to use it, and sets the ground rule. One line. Always present.

### Step 5: Compile the Boot Sequence

The most important part of the package. Must be actionable in isolation.

Contents (always ~10 lines):

- **Continuing for:** User name or description
- **Where we left off:** One line — the most recent state
- **Immediate next action:** One concrete thing the receiving AI should do first
- **What NOT to redo:** 2–5 bullets of settled ground
- **Session confidence:** High/medium/low — overall reliability of this package
- **Read order:** Which sections to prioritize if short on context

### Step 6: Compile the Package Body

Read the template in `references/teletext-template.md` for the exact structure per tier.

**Light tier:** Compact format — Session, Context, Decisions, Performance, Open Loops & Next Actions, Handoff. One streamlined block.

**Standard tier:** Full Sections 0–12. Empty sections get a one-line "N/A — [reason]."

**Deep tier:** Full Sections 0–12 expanded. Add compression appendices if needed.

Key guidance:

- **Section 0 (Access & Coverage)** — Be honest about what was accessible. Include "likely missing context" inline here — don't save it for a separate QA section.
- **Section 4 (Canonical Facts)** — Encode the exception: no tag means high confidence. Only annotate `[med]` or `[low]`. Add `⚠️ Fragile:` inline on any fact that could break the handoff if wrong.
- **Section 5 (Decision Log)** — Only decisions that changed plan/scope/direction. Add `⚠️ Fragile:` inline on decisions resting on shaky assumptions.
- **Section 8 (Conversation Performance)** — Corrections, dead ends, what worked, what didn't, experiments. This calibrates trust for the receiving AI.
- **Section 9 (Open Loops)** — Include "missing information that could restore fidelity" here (up to 10 items, ranked) if context was truncated or gaps were detected. This replaces the old Section B missing-info prompts.
- **Section 10 (TODOs)** — Each includes a "suggested next message" for the receiving AI.
- **Section 12 (Handoff)** — Deeper context beyond the Boot Sequence. What not to redo, what success looks like, 3 candidate next steps.

### Step 7: Internal Quality Checks (do not include in output)

Before writing the file, run these checks internally. Resolve issues by fixing the body — do not ship the QA process as output:

1. **Coverage** — Every required section for the tier is present and non-empty where applicable.
2. **Contradiction scan** — No conflicts between sections. If found, resolve them in the body.
3. **Assumptions** — Any assumption is tagged inline as `⚠️ Fragile:` on the relevant fact or decision.
4. **Fragility** — Anything that could break the handoff is flagged inline where it appears.

These checks improve the package. The user and receiving AI never see the checklist — they see the result.

### Step 8: Write to File

Write the compiled package to a markdown file in the current working directory:

**Filename:** `teletext-YYYY-MM-DD-[slug].md`

- Date is the current date
- Slug is a short kebab-case version of the session title (e.g., `teletext-2026-02-23-espresso-grinder-optimization.md`)
- Keep the slug under 50 characters
- If updating a previous package, use the same slug with updated date

Display the Boot Sequence in the conversation so the user sees the summary immediately. Confirm the file path.

## Output Structure

```
[In conversation: Boot Sequence displayed + file path confirmation]

[In file:]
  Preamble (1 line, self-describing)
  BOOT SEQUENCE (~10 lines, actionable)
  [Checkpoint Header — only if updating a previous package]
  Package Body (Light: compact | Standard/Deep: Sections 0–12)
```

The file is self-contained. A receiving AI needs nothing else.

## What Good Looks Like

A successful compilation:

- Boot sequence alone is enough to start working
- Receiving AI does not re-ask established questions
- Decisions have rationale, not just outcomes
- Dead ends and corrections are captured honestly
- Fragile assumptions are flagged inline where they appear
- Tier matches session weight
- No empty scaffolding — every section carries real content

A failed compilation:

- Sections present but empty or boilerplate
- Decisions listed without rationale
- Boot sequence is vague
- No performance section, or reduced to "everything went well"
- Fragile assumptions buried or untagged
- QA checklist shipped as output instead of applied to the body

## Examples

### Example 1: Light Tier — Quick Technical Decision

User says: "Teletext this" after a short session choosing between two database options.

```
> **TELEtext context-transfer package.** This document contains the full state of a prior AI conversation. Read the BOOT SEQUENCE immediately below to orient and take your first action. Consult deeper sections as needed. Do not re-ask questions that are answered in this document.

BOOT SEQUENCE
Continuing for: Sarah
Where we left off: Selected PostgreSQL over MongoDB for the user events system
Immediate next action: Begin schema design for the events table
What NOT to redo:
- Database choice is settled (PostgreSQL)
- Events will be append-only, not updated in place
- No need for document-style flexibility (ruled out MongoDB)
Session confidence: high
Read order: Decisions → Open Loops

---

## Session
- Title: User events storage — database selection
- Domain: Backend infrastructure
- Objective: Choose database for user event tracking
- Status: Database selected; schema design next

## Context
- Key facts:
  1. (U) Expected volume: ~50M events/day
  2. (U) All queries are time-range + user_id
  3. (J) No need for flexible schema

## Decisions
1. (J) PostgreSQL with partitioned tables — append-only events with
   time-range queries favor relational; MongoDB's flexibility
   unnecessary. Owner: Sarah.

## Performance
- Corrections: none
- Dead ends: none

## Open Loops & Next Actions
- ⚠️ Fragile: volume estimate (50M/day) taken at face value — if wrong, partitioning strategy may change
- P0: Design events table schema. "I need a schema for append-only events partitioned by month, queried by user_id + time range."
- P1: Decide on retention policy (not yet discussed)

## Handoff
- Start here: Database is PostgreSQL, settled. Design the schema.
- Success: A CREATE TABLE statement with partitioning strategy.
- Next steps: (1) Schema design, (2) Retention policy, (3) Index strategy
```

### Example 2: Updated Package with Checkpoint

User says: "Teletext this" again after more progress in the same session. A previous `teletext-2026-02-23-events-db.md` exists.

```
> **TELEtext context-transfer package.** [same preamble]

BOOT SEQUENCE
Continuing for: Sarah
Where we left off: Events table schema designed; retention policy decided
Immediate next action: Implement the migration and write index strategy
[rest of boot sequence...]

---
CHECKPOINT: Updated from teletext-2026-02-23-events-db.md
Changes since last checkpoint:
- [New decision]: 90-day retention with monthly partitions, auto-drop oldest
- [Resolved]: Schema design complete — see Artifact Index for CREATE TABLE
- [New open loop]: Index strategy for user_id + time range TBD
- [Status change]: Moved from schema design → implementation
---

[Full updated package body follows...]
```

## Edge Cases

**Conversation is mostly code with few decisions:**
Compile at Light tier. Focus the boot sequence on what was built and what state the code is in.

**Context window is truncated:**
State the earliest visible point in Section 0. Add missing-info items to Open Loops (Section 9), ranked by importance.

**User asks for teletext mid-conversation:**
Compile a snapshot. Mark status as "in progress." The package is still valid.

**Session has no decisions — just exploration:**
Decision Log will be minimal. Progress Log, Facts, and Open Loops carry the value.

**Previous teletext file exists in working directory:**
Read it, compile a full updated package with Checkpoint Header showing what changed.

## References

- `references/teletext-template.md` — Full section-by-section template for all tiers. Read before compiling.
