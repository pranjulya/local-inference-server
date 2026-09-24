# Phase 03 — Baseline benchmark evidence

Status: NOT_STARTED. Owner: implementing agent after authorization. Reviewer: learner plus architecture/security reviewer as relevant. No work below has been executed.

## Goal
Freeze and execute fair performance and quality baseline.

## Why
Optimization without an honest reference cannot support claims.

## Prerequisites
Phase 02 COMPLETE and approved; benchmark manifest and rubrics frozen.

## Architecture impact
Adds offline benchmark artifacts and metadata observation, not production dependencies.

## Planned files
benchmarks/workload-manifest.json; benchmarks/ runner or native-tool documentation; reports/baseline/; tests/benchmark-validation/. These are candidate implementation paths, not existing deliverables. Reuse native tooling and existing repository patterns before creating custom files.

## Execution sequence
1. Freeze and hash the synthetic workload manifest, exact tokenized lengths, tuning/held-out split, quality labels and scoring rubric.
2. Validate the measurement harness against a tiny known event timeline before GPU runs, including empty stream events and failed requests.
3. Run the declared input/output-length and concurrency matrix with 20 excluded warm-up requests and 500 measured requests per primary cell for three repetitions.
4. Score the 100-item quality suite and preserve the held-out distinction; record critical-case labels before inspecting quantized results.
5. Publish baseline distributions, errors, environment and raw artifact hashes; ratify any budget revision before subsequent experiments.

## Tests
Planned checks only; none have been run. Use accepted Phase 00 budgets if explicitly revised before measurement.

| Input / condition | Action | Expected result |
|---|---|---|
| Synthetic timeline with empty role event before token | Compute TTFT | TTFT uses first actual generated token; header/empty-event time excluded. |
| 500 measured requests with 5 failures | Aggregate run | Denominator remains 500 and error rate 1%; failures reported separately from completed latency. |
| 512-input/128-output concurrency-one warm cell | Measure complete requests | Compare p95 TTFT to 2 seconds and p95 total latency to 15 seconds; record pass/fail without rewriting observations. |
| Three repeated primary-cell runs | Compare metadata and statistics | Same frozen inputs/profile; per-run results plus ranges/uncertainty, no average of p95 advertised as aggregate p95. |
| Synthetic canary in prompt and credential | Inspect metadata artifacts | Canary absent from logs/metrics; dataset IDs and hashes sufficient for provenance. |

## Failure scenarios
Load generator bottleneck; warm/cold contamination; dropped failures; mislabeled token throughput; measurement clock error.

## Acceptance criteria
Three documented repetitions per primary cell and held-out quality record; environment/data hashes; errors included; provisional budgets either met or explicitly revised before later tuning.

## Learning objectives
Experimental controls and uncertainty.

## Interview questions
Why report goodput? What is the denominator for p95 and error rate?

## Review gate and rollback
Attach environment, exact test invocation, date, outcomes, evidence paths and limitations. Do not mark COMPLETE until reviewer accepts the evidence and explain-back. Restore the last verified profile on regression; for pre-runtime documentation decisions, revert the decision change while preserving investigation evidence. Blocked hardware or unsupported execution is recorded explicitly; never substitute claimed success. Next phase remains NOT_STARTED until approved.
