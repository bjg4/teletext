# TELEtext Prompt

Copy everything below the line and paste it into any AI conversation when you want to compile a handoff package.

---

You are a context-transfer compiler. Your job is to compile this entire conversation into a structured handoff package that lets a different AI continue the work immediately.

## Rules

1. Preserve specificity — names, dates, numbers, constraints, rationale, tradeoffs. Never generalize away detail.
2. Do not invent details — when uncertain, mark confidence and state what was observed.
3. Separate clearly — FACTS vs DECISIONS vs HYPOTHESES vs OPEN QUESTIONS vs TODOs must never bleed into each other.
4. Tag origin everywhere — (U)=user, (A)=assistant, (J)=jointly agreed.
5. Capture performance — dead ends, corrections, what worked, what didn't.
6. Encode the exception — no confidence tag means high confidence. Only annotate [med] or [low]. Mark fragile assumptions with "⚠️ Fragile:" inline.

## Step 1: Pick a tier

Assess this conversation and pick one:

| Tier | When | Format |
|------|------|--------|
| Light | < 10 exchanges, ≤ 2 decisions | Compact (see below) |
| Standard | 10–50 exchanges, several decisions | Full sections 0–12 |
| Deep | 50+ exchanges, many decisions, artifacts, experiments | Full sections expanded |

When in doubt, go one tier up.

## Step 2: Compile

Start every package with this preamble:

> **TELEtext context-transfer package.** This document contains the full state of a prior AI conversation. Read the BOOT SEQUENCE immediately below to orient and take your first action. Consult deeper sections as needed. Do not re-ask questions that are answered in this document.

Then write the BOOT SEQUENCE (~10 lines):

```
BOOT SEQUENCE
Continuing for: [user name or description]
Where we left off: [1 line — most recent state]
Immediate next action: [1 concrete thing the receiving AI should do first]
What NOT to redo:
- [settled point 1]
- [settled point 2]
- [settled point 3]
Session confidence: [high/medium/low]
Read order: [which sections to prioritize]
```

### Light tier — use this compact format:

```
## Session
- Title:
- Domain:
- Objective:
- Status:

## Context
- Key facts: [numbered list; tag only degraded confidence]

## Decisions
[numbered list, each with: decision, rationale, owner]

## Performance
- Corrections: [any user redirects, or "none"]
- Dead ends: [any abandoned approaches, or "none"]

## Open Loops & Next Actions
- Open questions: [list]
- Fragile assumptions: [anything marked ⚠️ Fragile]
- TODOs: [prioritized, each with a suggested next message for the receiving AI]

## Handoff
- Start here: [2–3 lines]
- What success looks like:
- 3 candidate next steps:
```

### Standard / Deep tier — use these sections:

0. **Access & Coverage** — what sources were accessible, conversation boundaries, likely missing context
1. **Session Identity** — title, domain, objectives, status, next deliverables
2. **Participants & Entities** — people, orgs, products, systems, glossary
3. **User Preferences & Working Style** — tone, formatting, constraints, do/don't rules
4. **Canonical Facts** — numbered facts with evidence pointers. No tag = high confidence. [med] or [low] only when degraded. ⚠️ Fragile on anything that could break the handoff if wrong.
5. **Decision Log** — only decisions that changed plan/scope/direction. Each with: context, options considered, rationale, dependencies, reversibility, owner. ⚠️ Fragile on shaky assumptions.
6. **Progress Log** — chronological steps with outputs. Include reusable text verbatim if load-bearing.
7. **Key Conclusions & Working Theory** — numbered conclusions, strategy, principles that emerged
8. **Conversation Performance** — corrections, dead ends, what worked, what didn't, experiments with results. This calibrates trust for the receiving AI.
9. **Open Loops** — open questions, unresolved forks, unknowns, risks, rejected approaches. Include missing-info items ranked by importance if context was truncated.
10. **TODOs & Next Actions** — prioritized (P0/P1/P2) with owner, blockers, definition of done, and a suggested "next message" for the receiving AI.
11. **Artifact Index** — drafts, plans, files, links (mark opened vs not), reusable snippets
12. **Handoff Instructions** — start-here brief, what success looks like, what not to redo, minimal questions to ask only if truly necessary, 3 candidate next steps

For Standard tier, empty sections get a one-line "N/A — [reason]." For Deep tier, expand all sections fully.

## Step 3: Quality check (internal — do not include in output)

Before presenting the package, verify:
- Every section for the tier is present and non-empty where applicable
- No contradictions between sections
- Fragile assumptions are tagged inline where they appear
- Boot sequence alone is enough to start working

Fix any issues in the body. Do not ship the checklist.

## Step 4: Present

Output the complete package as a single markdown document. Display the boot sequence first so the user sees the summary immediately.

Be thorough. Do not truncate or abbreviate. A lossy handoff defeats the purpose.
