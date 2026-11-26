---
name: systematic-debug
description: Investigate bugs, crashes, incorrect outputs, numerical discrepancies, deadlocks, hangs, OOMs and distributed execution failures with reproducible experiments and evidence. Use for correctness and reliability regressions.
---

# Systematic Debug

## Principles

Evidence before conclusions; reproduce before fixing; root cause before workaround. Change one variable at a time where practical. Do not conceal errors by weakening tests or silently swallowing exceptions.

## 1. Reproduce

Record the commit, software versions, hardware/runtime environment, exact command, input and configuration, expected/actual behavior, logs/stack traces and reproducibility. Construct a minimal reproducible example when feasible. For intermittent failures, record frequency, seeds, timeouts and concurrency conditions. If reproduction is unavailable, continue with available evidence and explicitly mark conclusions provisional.

## 2. Trace

Read the actual implementation and follow data, state and control flow. Find the earliest observable divergence, not merely the location where the exception is raised. Reference files and symbols.

For AI/GPU/distributed systems, inspect as relevant:

- Tensor shape, stride, layout, dtype, device and numerical ranges.
- CUDA streams, events, synchronization, asynchronous error reporting and allocation lifetime.
- Kernel launch configuration, rank/device/process mapping, collective ordering and communication timeouts.
- Quantization scale shape, granularity, rounding, saturation, accumulation precision and reference outputs.
- Memory pressure, fragmentation and allocation size; distinguish OOM from a leak.

## 3. Hypothesize

Rank plausible causes by supporting and contradictory evidence and cost to test. Each hypothesis must make a falsifiable prediction. Keep a table for complex investigations:

| Hypothesis | Evidence for/against | Discriminating experiment | Status |
| --- | --- | --- | --- |

Distinguish observations, inferences and unknowns. Do not claim a root cause from correlation alone.

## 4. Experiment

Prefer small, safe experiments with high information gain. Control relevant variables; record commands, configurations, seeds, raw outputs and results. Check whether added logging, synchronization or profiling changes the failure behavior. Never run destructive operations or expensive shared-resource experiments without appropriate authorization. If an experiment fails to discriminate between hypotheses, revise the experiment rather than assert a conclusion.

## 5. Fix

When supported by evidence and authorized, implement a minimal repair that addresses the causal mechanism. Avoid unrelated refactoring. Label workarounds explicitly, including trade-offs and the remaining underlying issue. If the user requested diagnosis only, do not modify code.

## 6. Verify

- Re-run the original failing case under comparable conditions.
- Add or run a regression test that would fail before the fix.
- Check relevant edge cases, numerical tolerances, concurrency and performance impact.
- Report unexecuted or blocked checks honestly; do not claim a fix is verified without evidence.

## Deliver

Report symptom, reproduction, confirmed facts, hypotheses/experiments, root cause and confidence, fix (if requested), regression results, and remaining uncertainty. Explain the mechanism so the user can independently reproduce the diagnosis.
