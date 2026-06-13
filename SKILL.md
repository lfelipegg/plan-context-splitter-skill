---
name: plan-context-splitter
description: Use when a user provides or references a plan, implementation spec, migration plan, roadmap, refactor plan, release plan, or task breakdown and asks to split it into Codex-sized execution packets, context chunks, ordered work packets, or prompts. Do not use for summaries, rewrites, timeline estimates, or business strategy.
---

# Plan Context Splitter

## Overview

Convert plans into dependency-ordered Codex execution packets. Optimize for execution boundaries and context locality, not arbitrary headings or word-count chunks.

## Input Handling

The source plan may be pasted text, a file path, Markdown, plain text, a referenced project artifact, or a plan embedded in an issue, PR, or task description.

If running inside a repository:
1. Inspect relevant repo guidance first, especially `AGENTS.md`, README files, package files, and obvious test/config files.
2. Use repository structure to make likely files/modules and validation commands more accurate.
3. Do not invent files. Mark uncertain files as `TBD`, `likely area`, or `needs repo inspection`.
4. Prefer validation commands that exist in the repository.

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

## Source Traceability

For every packet:
- Reference the source heading, section, task ID, checklist item, or short phrase that justifies the packet.
- Mark inferred support work as `inferred`.
- Do not create packets that cannot be traced to the source plan unless they are necessary support work.

## Dependency Types

Use these dependency labels:
- `blocks`: must be completed first
- `informs`: useful context but not blocking
- `parallel`: can run independently
- `decision`: requires a human/product/architecture choice
- `validation`: verifies prior work

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

| Order | Packet ID | Title | Status | Depends On | Size | Purpose |
|---:|---|---|---|---|---|---|

## 3. Shared Context

Use this context in every packet unless contradicted by packet-specific instructions:

## 4. Work Packets

### P001 - Title

**Status:** Ready | Needs decision | Needs repo inspection | Blocked
**Goal:** ...
**Why this packet exists:** ...
**Source anchors:** ...
**Context to include:** ...
**Do not include:** ...
**Likely files/modules:** ...
**Dependencies:** Use typed entries such as `P001 blocks this packet`, `P002 informs this packet`, `parallel with P003`, or `D001 decision required`.
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

Do not create separate packets for tiny tasks that must be changed together in the same files unless separation materially improves context clarity.

Avoid packets that:
- contain the whole source plan
- mix unrelated phases or owners
- depend on many unresolved decisions
- ask Codex to implement, test, migrate, document, and deploy at once
- repeat the full source plan in every suggested prompt

## Packet Prompt Budget

Each suggested Codex prompt should include only:
- the packet goal
- relevant shared context
- source anchors
- concrete steps
- acceptance criteria
- validation commands
- dependency carry-forward notes

Avoid including the full original plan, unrelated packets, long background sections, speculative file lists, or vague instructions like "implement the plan".

## Splitting Balance

Split when work type, dependency, ownership, or context changes. Keep tightly coupled changes together when splitting would force repeated context or coordinated edits across the same files.

## Quality Pass

Before finalizing, check that each packet is:
- independent enough for a separate Codex session
- small enough to keep context focused
- ordered according to dependencies
- concrete about likely files, modules, or systems
- traceable to source anchors or marked as inferred support work
- clear about status and typed dependencies
- clear about acceptance criteria and validation
- execution-oriented rather than a summary

If a packet fails the check, split it further or flag an open question.
