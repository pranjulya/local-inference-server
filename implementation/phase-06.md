# Phase 06 — Recovery and release review

Status: NOT_STARTED. Owner: implementing agent after authorization. Reviewer: learner plus architecture/security reviewer as relevant. No work below has been executed.

## Goal
Demonstrate bounded operation, privacy and recoverability; prepare portfolio evidence.

## Why
Production practices must survive faults and produce inspectable evidence.

## Prerequisites
Phase 05 COMPLETE and approved; chosen profile and baseline rollback pinned.

## Architecture impact
Completes operational metadata, runbooks and optional export contract without requiring P09.

## Planned files
tests/operations/; reports/release/; docs/operations/ runbook updates; demo storyboard and sanitized evidence manifest. These are candidate implementation paths, not existing deliverables. Reuse native tooling and existing repository patterns before creating custom files.

## Execution sequence
1. Run a 30-minute soak at declared concurrency and capture errors, queue bounds, memory and goodput with chosen profile.
2. Inject worker exit during a synthetic stream, observe readiness/partial output, and restore the pinned baseline through bounded drain and restart.
3. Exercise telemetry sink loss, credential rotation, management-route denial and canary leakage checks without persisting real user content.
4. Measure recovery against the provisional five-minute target with local artifacts; document single-GPU downtime and any budget exception.
5. Assemble sanitized evidence manifest and demo assets for Project 11; review full requirements traceability and explicitly separate recorded demo from any later live access approval.

## Tests
Planned checks only; none have been run. Use accepted Phase 00 budgets if explicitly revised before measurement.

| Input / condition | Action | Expected result |
|---|---|---|
| 30-minute concurrency-four synthetic soak | Observe capacity and stability | No OOM, unbounded queue or unexplained restart; latency/errors reported against accepted budgets. |
| Worker killed after 20 generated tokens | Observe client and restart | Stream incomplete, no automatic replay; readiness false until model can serve. |
| Known-good rollback artifacts local | Drain <=60 seconds and restore baseline | Measure recovery against 5-minute target; reopen only after readiness and smoke inference. |
| Metrics sink unavailable | Continue bounded load | Inference remains available; export buffer bounded and drop/missing telemetry visible. |
| Distinct prompt/output/credential canaries | Audit all release logs and artifacts | No canary leakage; failing release blocked pending correction. |
| Unapproved client or management route | Attempt access | Access denied; optional public live demo remains unauthorized until separately reviewed. |

## Failure scenarios
Runaway restart; stale readiness; disk full; leaked prompts; metrics outage blocks serving; rollback artifact missing.

## Acceptance criteria
DoD and R1–R8 traceability reviewed; measured recovery and residual risks; no critical leakage/quality issue; approved publication assets; no public deployment implied.

## Learning objectives
Incident diagnosis, rollback and truthful engineering communication.

## Interview questions
How would you debug rising TTFT? What does single-host production-grade exclude?

## Review gate and rollback
Attach environment, exact test invocation, date, outcomes, evidence paths and limitations. Do not mark COMPLETE until reviewer accepts the evidence and explain-back. Restore the last verified profile on regression; for pre-runtime documentation decisions, revert the decision change while preserving investigation evidence. Blocked hardware or unsupported execution is recorded explicitly; never substitute claimed success. Next phase remains NOT_STARTED until approved.
