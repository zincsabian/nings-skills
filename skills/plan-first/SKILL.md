---
name: plan-first
description: Plan non-trivial engineering work before implementation. Use for architecture changes, features, cross-module refactoring, or significant optimization. Skip trivial edits and simple questions.
---

# Plan First

## 1. Understand

- Clarify the objective, constraints, non-goals and measurable acceptance criteria. State reasonable assumptions instead of blocking on minor unknowns.
- Inspect relevant source, documentation, tests, current implementation and call graph. Cite file paths and symbols.
- Check repository status and preserve existing user changes. Separate confirmed facts from hypotheses and unknowns.
- Do not modify implementation before understanding the existing design.

## 2. Analyze

- Explain the current architecture and design motivation; trace data and control flow and identify relevant dependencies.
- Identify the minimum necessary change surface and potential side effects.
- For material decisions, compare alternatives in terms of correctness, performance, complexity, compatibility and maintainability.
- For optimization, establish a baseline and a hypothesis before proposing implementation. Use `performance-profile` when measurement is needed.

## 3. Plan

For non-trivial work, present a concise plan with:

1. Objective and acceptance criteria.
2. Current implementation and evidence.
3. Constraints, assumptions and non-goals.
4. Proposed design, alternatives and trade-offs.
5. Files/modules to change and ordered implementation steps.
6. Tests, benchmarks, risks and rollback strategy.

Each implementation milestone must have an observable completion criterion. Keep the plan proportional to task complexity. Maintain a plan file only when the task spans multiple sessions or milestones, or the repository requires one.

## 4. Approval gate

- If the user asks for a plan only, or the active mode forbids edits, stop after presenting the plan. Read-only investigation is allowed within existing permissions.
- For major architectural or public-interface changes, or destructive/high-risk actions, seek approval before implementation unless the specific change has already been explicitly approved.
- For ordinary, clearly authorized low-risk tasks, give a brief plan and continue without unnecessary confirmation.
- A skill never overrides tool permissions, sandbox rules, repository instructions or explicit user constraints.

## 5. Execute and verify

- Make the smallest necessary changes, one milestone at a time. Preserve unrelated files and existing behavior.
- Run targeted validation after meaningful changes. Update the plan if material assumptions fail; seek renewed approval if the revised scope requires it.
- Never fabricate test or benchmark results. Distinguish passed, failed, skipped and unavailable checks.

## 6. Deliver

Summarize the actual changes, file/symbol references, validation commands and results, deviations from the plan, unresolved risks, and next actions. Explain important design trade-offs in the user's language.
