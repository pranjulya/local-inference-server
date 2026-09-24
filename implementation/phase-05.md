# Phase 05 — Quantization experiments

Status: NOT_STARTED. Owner: implementing agent after authorization. Reviewer: learner plus architecture/security reviewer as relevant. No work below has been executed.

## Goal
Compare supported quantized weights and optionally KV dtype with quality safeguards.

## Why
Demonstrate memory tradeoffs without equating compression with correctness.

## Prerequisites
Phase 04 COMPLETE and approved; supported licensed quantized artifact; baseline quality report.

## Architecture impact
Adds candidate profiles; keeps baseline and changes weight/KV axes separately.

## Planned files
profiles/quantized.yaml; optional profiles/kv-quantized.yaml; reports/quantization/; docs/architecture/ADRs/006-quantization-promotion.md. These are candidate implementation paths, not existing deliverables. Reuse native tooling and existing repository patterns before creating custom files.

## Execution sequence
1. Verify quantized artifact provenance, model lineage, tokenizer, license and backend support against the same pinned hardware/runtime.
2. Run weight-only candidate against floating-point baseline with unchanged KV dtype, cache mode, sampling, dataset and request envelope.
3. Score paired quality cases before selecting a candidate; inspect numeric critical cases and report disagreement without relabeling tests.
4. Measure peak VRAM and full latency/throughput distributions; report memory reduction independently of speed.
5. If supported, run a separate KV-dtype experiment with weight representation fixed; otherwise record compatibility evidence and the unsupported outcome.
6. Recommend candidate promotion only with quality evidence; preserve baseline and document rejected candidates as learning results.

## Tests
Planned checks only; none have been run. Use accepted Phase 00 budgets if explicitly revised before measurement.

| Input / condition | Action | Expected result |
|---|---|---|
| Quantized weight candidate | Run identical 100-item suite | Aggregate score decline <=2 percentage points and zero predefined critical regressions for promotion. |
| Floating and quantized profiles | Compare immutable metadata | Same model lineage/tokenizer/workload; only stated representation changes. |
| Long-context concurrency-four load | Measure peak memory and stability | Memory reduction must be observed, not inferred from file size; no OOM within envelope. |
| Quantized candidate slower than baseline | Compare paired latency results | Publish slowdown; no unsupported speed claim. |
| Unsupported KV dtype/backend | Review compatibility/startup result in authorized experiment | Record unsupported; do not fabricate KV benchmark or silently change weights too. |
| Critical numeric answer regresses | Apply quality gate | Reject promotion even if aggregate score and memory look acceptable. |

## Failure scenarios
Unsupported kernel; bad scaling; tokenizer mismatch; numeric degradation; lower memory but slower service.

## Acceptance criteria
Quality within 2 points and zero critical regressions for any promoted candidate; measured memory comparison; speed claims match evidence; unsupported KV experiment documented explicitly.

## Learning objectives
Quantization axes, calibration and quality tolerance.

## Interview questions
Why separate KV and weights? When should a quantized candidate be rejected despite memory savings?

## Review gate and rollback
Attach environment, exact test invocation, date, outcomes, evidence paths and limitations. Do not mark COMPLETE until reviewer accepts the evidence and explain-back. Restore the last verified profile on regression; for pre-runtime documentation decisions, revert the decision change while preserving investigation evidence. Blocked hardware or unsupported execution is recorded explicitly; never substitute claimed success. Next phase remains NOT_STARTED until approved.
