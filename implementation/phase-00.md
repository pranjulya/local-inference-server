# Phase 00 — Feasibility and decisions

Status: NOT_STARTED. Owner: implementing agent after authorization. Reviewer: learner plus architecture/security reviewer as relevant. No work below has been executed.

## Goal
Prove a legal, affordable, supported execution target before building.

## Why
Prevent unsupported hardware assumptions and expensive rework.

## Prerequisites
Planning review; repository destination and permitted hardware inventory.

## Architecture impact
Fixes deployment boundary and benchmark feasibility without running a service.

## Planned files
docs/architecture/ADRs/004-runtime-selection.md; docs/evaluation/environment-manifest.md; repository/worktree record. These are candidate implementation paths, not existing deliverables. Reuse native tooling and existing repository patterns before creating custom files.

## Execution sequence
1. Inventory the intended Linux GPU host: GPU model/VRAM, driver, OS, available disk, ownership and access method; distinguish measured inventory from assumptions.
2. Check a pinned vLLM release against the GPU and candidate model architecture, then choose one licensed floating-point model and a supported quantized sibling.
3. Estimate weight, KV and runtime memory at 4,096 total tokens and concurrency four; select a smaller baseline if floating-point weights cannot fit safely.
4. Record approved compute/spend ceiling, download size, license terms and private network boundary; no purchase or license acceptance is implied.
5. Locate or create the approved repository and isolated worktree during authorized implementation; freeze dataset/protocol and provisional budgets before optimization.

## Tests
Planned checks only; none have been run. Use accepted Phase 00 budgets if explicitly revised before measurement.

| Input / condition | Action | Expected result |
|---|---|---|
| GPU inventory absent | Review available host evidence | Mark GPU execution blocked; do not substitute Apple client hardware or claim compatibility. |
| 4,096 tokens × 4 requests | Calculate architecture-specific KV estimate plus weights/workspace margin | Record a plausible fit or reduce proposed envelope/model before approval. |
| Moving model tag or image tag | Resolve immutable revisions/digests | Manifest contains exact candidate revisions and license source; no latest-only selection. |
| No approved repository destination | Review workspace record | Keep fallback package and record unresolved destination; no unrelated repository worktree. |

## Failure scenarios
No GPU; insufficient VRAM; gated license; no repository ownership; quantization kernel unsupported.

## Acceptance criteria
Reviewed hardware/budget/license record and exact candidate revisions; baseline can plausibly fit with safety margin; suitable repository/worktree exists or blocker recorded; benchmark budgets accepted before runs.

## Learning objectives
Memory accounting and evidence-based choices.

## Interview questions
Why is model size alone insufficient? Which assumption would force a smaller model?

## Review gate and rollback
Attach environment, exact test invocation, date, outcomes, evidence paths and limitations. Do not mark COMPLETE until reviewer accepts the evidence and explain-back. Restore the last verified profile on regression; for pre-runtime documentation decisions, revert the decision change while preserving investigation evidence. Blocked hardware or unsupported execution is recorded explicitly; never substitute claimed success. Next phase remains NOT_STARTED until approved.
