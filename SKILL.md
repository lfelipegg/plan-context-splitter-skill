---
name: plan-context-splitter
description: Use when a user provides or references a plan, implementation spec, migration plan, roadmap, refactor plan, release plan, or task breakdown and asks to split it into Codex-sized execution packets, context chunks, ordered work packets, or prompts. Do not use for summaries, rewrites, timeline estimates, or business strategy.
---

# Plan Context Splitter

## Overview

Convert plans into dependency-ordered Codex execution packets. Optimize for execution boundaries and context locality, not arbitrary headings or word-count chunks.

## Required Workflow

1. Read the source plan. If the source is referenced but inaccessible, ask for it.
2. Extract a plan map: goal, phases, systems, global constraints, terms, and risks.
3. Separate shared context from packet-specific context. Put coding standards, security rules, compatibility constraints, naming rules, deployment constraints, and terminology in one shared block.
4. Build a dependency map. Put prerequisites such as decisions, schema/model work, scaffolding, and migrations before dependent API, UI, testing, rollout, or docs work.
5. Split by execution boundary. Create a packet when work type, dependency, ownership, or context needs change.
6. Keep tightly coupled work together only when splitting would remove essential context.
7. Assign a practical size: Small for 1-3 files or one focused task, Medium for 3-8 files or one feature slice, Large for 8-15 files only when the phase is tightly coupled.
8. Write a ready-to-use Codex prompt for each packet.
9. Flag open questions, oversized packets, risky dependencies, and missing validation.

## Output Format

Return the result as:

```markdown
# Codex Execution Split

## 1. Plan Map

**Project goal:** ...
**Major phases:** ...
**Key files/modules/systems:** ...
**Global constraints:** ...
**Important terms:** ...
**Cross-cutting risks:** ...

## 2. Execution Index

| Order | Packet ID | Title | Depends On | Size | Purpose |
|---:|---|---|---|---|---|

## 3. Shared Context

Use this context in every packet unless contradicted by packet-specific instructions:

## 4. Work Packets

### P001 - Title

**Goal:** ...
**Why this packet exists:** ...
**Context to include:** ...
**Do not include:** ...
**Source sections:** ...
**Likely files/modules:** ...
**Dependencies:** ...
**Steps for Codex:** ...
**Acceptance criteria:** ...
**Validation commands:** ...
**Risks:** ...
**Carry-forward summary:** ...
**Suggested Codex prompt:** ...

## 5. Open Questions

## 6. Recommended First Prompt
```

## Splitting Rules

Prefer separate packets for setup/scaffolding, architecture decisions, data/model changes, backend/API work, frontend/UI work, integrations, migrations, tests/hardening, docs, and deployment.

Avoid packets that:
- contain the whole source plan
- mix unrelated phases or owners
- depend on many unresolved decisions
- ask Codex to implement, test, migrate, document, and deploy at once
- repeat the full source plan in every suggested prompt

## Quality Pass

Before finalizing, check that each packet is:
- independent enough for a separate Codex session
- small enough to keep context focused
- ordered according to dependencies
- concrete about likely files, modules, or systems
- clear about acceptance criteria and validation
- execution-oriented rather than a summary

If a packet fails the check, split it further or flag an open question.
