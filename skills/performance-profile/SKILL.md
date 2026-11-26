---
name: performance-profile
description: Measure, model and optimize latency, throughput, GPU utilization, memory, kernel and communication performance. Use for performance regressions, hardware comparisons, CUDA, LLM prefill/decode, MoE and distributed workloads.
---

# Performance Profiling

## Principles

Measure before optimizing. Find the critical path and verify causal bottlenecks. Distinguish theoretical bounds, model-based estimates and measured results. Optimize the user's end-to-end objective, not isolated utilization metrics. Preserve correctness and never invent benchmark results.

## 1. Define the workload

Specify objective, hardware/topology, software versions and commit, model and tensor shapes, precision, input/output sequence lengths, batch size, concurrency, parallelism, warmup and timing methodology. State which variables are fixed and whether the comparison is per GPU or aggregate.

For LLM workloads, distinguish input tokens/s, output tokens/s, requests/s, TTFT, TPOT and end-to-end latency. Record the aggregation window, percentiles (P50/P95/P99) and actual token counts. Do not compare incompatible metrics or workloads.

## 2. Establish a baseline

- Hold workload and relevant execution conditions constant; record clocks/power, device contention, topology, software versions and configuration when relevant.
- Warm up, use correct asynchronous timing (CUDA events or synchronized wall time for the appropriate measurement scope), repeat measurements and quantify variation.
- Separate profiling overhead from normal execution time. Keep raw data, commands and the baseline before changing code.
- If hardware, tools or representative workloads are unavailable, give a reproducible proposed experiment and label estimates as unmeasured.

## 3. Build a theoretical model

When relevant, derive FLOPs, memory traffic, arithmetic intensity, Roofline bounds, communication volume and effective bandwidth, launch overhead, tile utilization, CTA/wave quantization, occupancy and latency hiding. State all assumptions and units, including FLOP-counting conventions and directionality of communication bandwidth.

- Use hardware peaks matching dtype, dense/sparse mode and device configuration. Distinguish peak hardware rate, estimated achievable ceiling and actual measured rate.
- Compute and communication can overlap only to the extent allowed by the dependency graph, resource contention and implementation. Use the critical path; do not blindly sum times or assume perfect overlap.
- Account for message startup latency, topology, per-link/shared bandwidth, collective algorithm and imbalance where relevant.
- Define MFU explicitly (useful model FLOPs divided by the relevant peak-rate/time product); distinguish it from achieved kernel FLOP utilization, device busy time and bandwidth utilization.
- For mixed precision, model stages with their respective effective ceilings instead of applying one peak rate to all operations.

## 4. Select tools

Use the least invasive tool that answers the question:

- PyTorch Profiler: operator-level attribution and shapes.
- Nsight Systems: CPU/GPU timeline, launch gaps, overlap and critical path.
- Nsight Compute: kernel metrics, instruction mix, memory traffic, occupancy and stalls.
- CUDA events: elapsed GPU time over a well-defined stream/event scope.
- NCCL Tests or appropriate transport microbenchmarks: bandwidth and latency of relevant collective patterns.
- `nvidia-smi`: device state and topology (not proof of compute saturation).
- `perf` / NUMA tools: CPU-side overhead and memory placement.

Check tool availability, hardware, permissions and version-specific metric names. Respect shared-GPU/resource constraints; do not install profilers or run intrusive/expensive experiments without authorization. Record actual commands and distinguish runs from proposals.

## 5. Attribute the bottleneck

Separate compute, bandwidth, memory latency, communication, launch, scheduling, synchronization and imbalance bottlenecks. Quantify their impact on the critical path using timelines, counters and controlled scaling experiments. GPU utilization alone is insufficient evidence of saturation.

For MoE, inspect expert token distribution (mean, tails and skew), per-expert GEMM M/N/K and tile utilization, grouped GEMM scheduling, routing/permutation, dispatch/combine volume and timing, shared experts, rank imbalance and communication/computation overlap. Compare EP/TP/DP with equivalent global or per-GPU workloads and explicitly state which is held fixed. Report prefill and decode separately where applicable.

## 6. Optimize

For each candidate, state the target bottleneck, mechanism, predicted benefit and assumptions, preconditions, implementation cost, correctness/numerical risks and discriminating experiment. Prefer high-information low-cost experiments. Change one material variable at a time or use a clearly documented factorial/ablation design. Use `plan-first` for significant implementation changes; do not edit code for a profile-only request.

## 7. Benchmark

Compare baseline and optimized implementations under equivalent conditions. Report distributions or variation, units and actual measurement commands.

| Metric | Baseline | Optimized | Delta |
| --- | --- | --- | --- |

Validate numerical accuracy, memory use, latency distribution and throughput as appropriate. State any comparability caveats and unmeasured values; do not report a projected benefit as an observed speedup.

## Deliver

Report workload and environment, theoretical model and assumptions, measured baseline, critical path, bottleneck evidence, prioritized optimization candidates, actual benchmark results, remaining gap and next experiments. Explain how measurements support conclusions and how the user can reproduce them.
