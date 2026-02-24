# TELEtext Package Template

Use this template structure when compiling a TELEtext package. Choose the appropriate tier (Light, Standard, or Deep) based on session weight assessment.

---

## PREAMBLE (always first line of every package file)

```
> **TELEtext context-transfer package.** This document contains the full state of a prior AI conversation. Read the BOOT SEQUENCE immediately below to orient and take your first action. Consult deeper sections as needed. Do not re-ask questions that are answered in this document.
```

This preamble tells the receiving AI what the document is, how to use it, and sets the ground rule (don't re-ask). It appears once, at the top of every package file regardless of tier.

---

## BOOT SEQUENCE (immediately after preamble, all tiers)

```
BOOT SEQUENCE
Continuing for: [user name or description]
Where we left off: [1 line — the most recent state]
Immediate next action: [1 concrete thing the receiving AI should do first]
What NOT to redo: [2–5 bullets of settled ground]
Session confidence: [high/medium/low — overall reliability of this package]
Read order: [which sections to prioritize if short on context]
```

The boot sequence is the sticky note on top of the manual. A receiving AI should be able to act on it before reading anything else.

---

## CHECKPOINT HEADER (include only when updating a previous package)

When a previous teletext file exists and is being updated, add a checkpoint header immediately after the Boot Sequence:

```
---
CHECKPOINT: Updated from teletext-2026-02-23-feature-shaping.md
Changes since last checkpoint:
- [New decision]: Selected Shape B over Shape A
- [Resolved]: R6 flagged unknown now has a concrete mechanism
- [New open loop]: Performance testing approach TBD
- [Status change]: Moved from shaping → slicing phase
---
```

List only what changed — new decisions, resolved questions, new open loops, status shifts. The full body below is the updated state; the checkpoint header tells the receiving AI what to pay attention to.

---

## Tier Selection

Assess session weight before compiling. Choose automatically — the user never configures this.

| Tier | Triggers | Sections |
|------|----------|----------|
| **Light** | < ~10 exchanges, ≤2 decisions, no artifacts | Preamble + Boot + Compact body |
| **Standard** | 10–50 exchanges, multiple decisions, some artifacts | Preamble + Boot + Full Sections 0–12, empty sections get one-line N/A |
| **Deep** | 50+ exchanges, many decisions, artifacts, experiments, dead ends | Preamble + Boot + Full Sections 0–12 expanded, compression appendices if needed |

---

## LIGHT TIER — Compact Format

Use when the session is short and simple:

```markdown
> **TELEtext context-transfer package.** This document contains the full state of a prior AI conversation. Read the BOOT SEQUENCE immediately below to orient and take your first action. Consult deeper sections as needed. Do not re-ask questions that are answered in this document.

BOOT SEQUENCE
[as above]

---

## Session
- Title:
- Domain:
- Objective:
- Status:

## Context
- Participants: [inline — only if beyond user + AI]
- User preferences: [inline — tone, constraints, do/don't rules]
- Key facts: [numbered list; tag only degraded confidence — omit tag if high]

## Decisions
[numbered list, each with: decision, rationale, owner]

## Performance
- Corrections: [any user redirects, or "none"]
- Dead ends: [any abandoned approaches, or "none"]

## Open Loops & Next Actions
- Open questions: [list]
- Fragile assumptions: [anything that could break the handoff if wrong]
- TODOs: [prioritized list with suggested next message]

## Handoff
- Start here: [2–3 lines]
- What success looks like:
- 3 candidate next steps:
```

---

## STANDARD / DEEP TIER — Full Format

### 0) ACCESS & COVERAGE DECLARATION

- Sources accessible in this session:
- Conversation coverage (earliest visible point → latest):
- Files/documents read (names + what used them for):
- Links actually opened/read in this session (list):
- Likely missing context: [what might exist outside what was accessible]

### 1) SESSION IDENTITY

- Title:
- Domain / topic:
- Primary objective(s):
- Current status / where we left off:
- Intended next deliverable(s):

### 2) PARTICIPANTS & ENTITIES

- People (name → role → relevance):
- Orgs / teams:
- Products / projects:
- Systems / tools:
- Glossary (terms coined here, acronyms, canonical definitions/phrasing):

### 3) USER PREFERENCES & WORKING STYLE

- Tone / voice preferences:
- Formatting conventions:
- Decision-making style:
- Constraints (scope, time, budget, policy, privacy):
- "Do / Don't" rules learned:

### 4) CANONICAL FACTS (source-of-truth list)

For each fact:

- Fact: (U/A/J) …
- Evidence pointer: quote/paraphrase + where it appeared
- ⚠️ Fragile: [only if this fact could break the handoff if wrong — omit for solid facts]

Confidence encoding: tag only degraded confidence. If a fact is high-confidence, its presence is its own marker. Only annotate when confidence is medium or low:
- No tag = high confidence (default)
- `[med]` = required some iteration or inference
- `[low]` = best-guess, unvalidated

### 5) DECISION LOG (chronological)

Only include items that changed the plan, scope, constraints, direction, commitments, or a resolved fork.

For each decision:

- Decision: (U/A/J) …
- When: (timestamp if present, otherwise relative position)
- Context / trigger:
- Options considered:
- Rationale:
- Dependencies / assumptions:
- Consequences / follow-on impacts:
- Reversibility: easy/medium/hard
- Owner:
- ⚠️ Fragile: [only if this decision rests on an assumption that could be wrong]

### 6) PROGRESS LOG (what we did)

Chronological:

- Step: (U/A/J) …
- Output produced (include reusable text verbatim if load-bearing):
- Result:

### 7) KEY CONCLUSIONS & WORKING THEORY

- Conclusions (numbered, each tagged U/A/J):
- Working theory / strategy:
- Principles that emerged:

### 8) CONVERSATION PERFORMANCE

This section captures how the collaboration itself performed.

| Field | What to capture |
|-------|-----------------|
| **Corrections** | Times the user redirected the AI. What was wrong, what was corrected to. |
| **Dead ends** | Approaches tried and abandoned. Who proposed it, why it failed, what replaced it. |
| **What worked** | Collaboration patterns worth repeating. |
| **What didn't** | Patterns the receiving AI should avoid. |
| **Experiments** | Technical attempts with method, result, interpretation. Include failures explicitly. |

### 9) OPEN LOOPS

- Open questions (with context and why they matter):
- Unresolved decisions / forks:
- Unknowns / missing inputs:
- Risks / concerns surfaced:
- Things explicitly NOT to redo / rejected approaches:
- Missing information that could restore fidelity: [up to 10 items, ranked by how blocking they are — include only if context was truncated or gaps were detected]

### 10) TODOs & NEXT ACTIONS (prioritized)

List each item as:

- Priority: P0/P1/P2
- Action:
- Owner:
- Blockers / dependencies:
- Definition of done:
- Suggested "next message" for the new AI (one line):

### 11) ARTIFACT INDEX (everything created or referenced)

- Drafts / prompts / templates (paste key parts or describe location):
- Plans / frameworks:
- Files available in-session:
- Links referenced (mark: opened/read vs not opened):
- Reusable snippets:

### 12) HANDOFF INSTRUCTIONS (for the receiving AI)

Note: The Boot Sequence covers immediate orientation. This section provides deeper context.

- Start-here brief (5–10 lines, concrete):
- What success looks like in the next step:
- What not to redo (expand on Boot Sequence bullets):
- Minimal set of questions to ask ONLY if truly necessary:
- 3 candidate next steps (if ambiguity remains):

### Compression appendices (Deep tier only, include only if session is too large to fit)

- Appendix A: Load-bearing excerpts (short quotes only)
- Appendix B: Omitted detail index (what was skipped and why)

Never omit Decision Log, Open Loops, or Next Actions regardless of tier or size constraints.
