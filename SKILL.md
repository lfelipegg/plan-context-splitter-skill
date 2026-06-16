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
3. Identify unresolved decisions before implementation work.
4. Separate shared context from packet-specific context. Put coding standards, security rules, compatibility constraints, naming rules, deployment constraints, and terminology in one shared block.
5. Build a dependency map. Put prerequisites such as decisions, schema/model work, scaffolding, and migrations before dependent API, UI, testing, rollout, or docs work.
6. Split by execution boundary. Create a packet when work type, dependency, ownership, or context needs change.
7. Keep tightly coupled work together only when splitting would remove essential context.
8. Assign a practical size: Small for 1-3 files or one focused task, Medium for 3-8 files or one feature slice, Large for 8-15 files only when the phase is tightly coupled.
9. Write a ready-to-use Codex prompt and multi-session handoff for each packet.
10. Flag open questions, oversized packets, risky dependencies, and missing validation.

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

Use `D###` for decision packets and `P###` for implementation, validation, rollout, or cleanup packets when that improves clarity. Do not require `D###` for every decision; use it as a clarity convention.

Use dependency direction from the current packet's point of view:
- `Depends on: P001 blocks this packet`
- `Depends on: D001 decision required before this packet`
- `Receives context from: P002 informs this packet`
- `Related: parallel with P003`
- `Validates: P004`
- `Validated by: P005`

Represent `validation` dependencies through `Validates` when this packet verifies prior work, or `Validated by` when another packet verifies this one.

## Unresolved Decisions

Do not create implementation packets that depend on unresolved product, legal, data, security, or architecture decisions. Create a decision packet first, mark it `Needs decision`, and mark dependent implementation packets as `Blocked` or `Needs decision`.

For blocked downstream work, create only lightweight placeholder packets with goal, dependency, source anchors, and blocked reason. Do not provide detailed implementation steps until the decision is resolved.

Do not pretend unknown inputs are known. Do not invent suggestion data sources, analytics availability, legal-sensitive personalization rules, file paths, services, feature flags, owners, data pipelines, or exact welcome-email locations. Mark unknowns as `TBD`, `likely area`, `needs repo inspection`, or `decision required`.

## Migration Plans

For migrations, explicitly account for schema/model changes, backfill/data migration, invalid or partial source data, idempotency, dry-run or sample validation, partial failure behavior, read-path changes, write-path changes, dual-read or fallback behavior, feature flags or rollout decisions, rollback risks after new writes are enabled, later-release cleanup, and tests for migration and app behavior.

Split schema, backfill, app read/write changes, tests, rollout, rollback, and cleanup into separate packets when their context differs. Schema usually `blocks` backfill and app-code work. Backfill may `inform` rollout validation. Mark missing feature-flag or deployment details as `decision`. Treat fallback removal as a later-release cleanup packet unless the source plan clearly says otherwise.

Require migration packets to state how invalid or partial source data is skipped, recorded, or repaired; how backfills stay idempotent; how partial failures are detected and retried; and what dry-run or sample validation proves before broad rollout. Rollback after enabling the new write path must explicitly account for data written after rollout.

## Messy Roadmaps

For messy roadmaps, infer execution phases from shippability, dependency order, and decision readiness rather than preserving the note order.

Keep Phase 1 shippable. Defer deeper personalization until decisions and data exist. Mark ambiguous work as `Needs decision`; mark unknown code locations as `likely area`, `TBD`, or `needs repo inspection`. Avoid inventing analytics availability, data services, owners, exact files, or suggestion data sources.

Create a decision packet before legal-sensitive, data-dependent, or architecture-dependent implementation work. Mark dependent personalization or suggestion implementation packets as `Blocked` or `Needs decision`.

## Multi-Codex Handoff

Assume packets may run in separate Codex conversations. Each packet must state what prior packet output must be pasted in, what branch or PR strategy is suggested when applicable, and what to report back when complete.

Branch/PR guidance should be included only when code changes are expected; otherwise write `None`.

Use this report-back format: files changed, commands run, validation results, decisions made, deviations from the prompt, new follow-up work, and carry-forward notes for dependent packets.

`Carry-forward summary` must capture concrete implementation facts later packets need, such as schema/table names, API names, feature flags, compatibility choices, changed files, generated artifacts, skipped data, and validation outcomes. Do not write a vague progress summary.

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

### P001 or D001 - Title

**Status:** Ready | Needs decision | Needs repo inspection | Blocked
**Goal:** ...
**Why this packet exists:** ...
**Source anchors:** ...
**Context to include:** ...
**Do not include:** ...
**Likely files/modules:** ...
**Depends on:** Use current-packet phrasing, such as `P001 blocks this packet` or `D001 decision required before this packet`.
**Receives context from:** Use `P002 informs this packet`, or `None`.
**Related:** Use `parallel with P003`, or `None`.
**Validates:** Use `P004` if this packet verifies that packet, or `None`.
**Validated by:** Use `P005` if another packet verifies this packet, or `None`.
**Inputs from prior packets:** ...
**Steps for Codex:** ...
**Acceptance criteria:** ...
**Validation commands:** ...
**Risks:** ...
**Branch/PR guidance:** ...
**Report back:** files changed; commands run; validation results; decisions made; deviations; follow-up work; carry-forward notes.
**Carry-forward summary:** ...
**Suggested Codex prompt:** ...

## 5. Open Questions

## 6. Recommended First Prompt
```

## Splitting Rules

Prefer separate packets for setup/scaffolding, architecture decisions, data/model changes, backend/API work, frontend/UI work, integrations, migrations, tests/hardening, docs, and deployment.

Do not create separate packets for tiny tasks that must be changed together in the same files unless separation materially improves context clarity.

For small features, prefer 2-4 concise packets. Avoid any packet larger than Medium unless the source plan clearly requires it. Keep one execution index, shared context, concise work packets, and one recommended first prompt; do not create a packet for every tiny API action or UI action when those changes belong together.

For simple Ready packets with no dependencies, write `None` tersely and avoid repeating generic handoff text. For small features, collapse low-risk fields to one line each when possible while preserving handoff/report-back fields and concrete carry-forward notes.

Avoid packets that:
- contain the whole source plan
- mix unrelated phases or owners
- depend on many unresolved decisions
- ask Codex to implement, test, migrate, document, and deploy at once
- repeat the full source plan in every suggested prompt

## Validation Commands

When repo validation commands are available, prefer exact existing commands. If commands cannot be inspected or confirmed, do not invent them. Write `Needs repo inspection` and list validation intent instead, such as unit tests for changed model/API behavior, integration tests for persistence or migration behavior, UI tests for changed user flows, manual verification steps, or migration dry-run/sample validation.

## Packet Prompt Budget

Each suggested Codex prompt should include only:
- the packet goal
- relevant shared context
- source anchors
- concrete steps
- acceptance criteria
- validation commands
- inputs from prior packets and carry-forward notes

Avoid including the full original plan, unrelated packets, long background sections, speculative file lists, or vague instructions like "implement the plan".

## Splitting Balance

Split when work type, dependency, ownership, or context changes. Keep tightly coupled changes together when splitting would force repeated context or coordinated edits across the same files.

File count is only an approximation. Prefer conceptual coupling and context size over raw file count: a single complex file may need its own packet, while many mechanical files may belong together. Large packets should be rare; if one exists, explain why splitting would harm execution.

Calibration:
- Good split: backend/model and API packet, UI packet, tests/hardening packet.
- Bad split: one packet per tiny create/rename/delete action, or one giant packet containing server, UI, tests, rollout, and docs.

## Quality Pass

Before finalizing, check that each packet is:
- independent enough for a separate Codex session
- small enough to keep context focused
- ordered according to dependencies
- concrete about likely files, modules, or systems
- traceable to source anchors or marked as inferred support work
- clear about status and directional typed dependencies, including `Validates` and `Validated by`
- clear about acceptance criteria and validation intent or commands
- clear about multi-Codex handoff and concrete carry-forward facts
- execution-oriented rather than a summary

If a packet fails the check, split it further or flag an open question.
