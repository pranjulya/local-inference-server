# Phase 01 — Private reproducible baseline

Status: NOT_STARTED. Owner: implementing agent after authorization. Reviewer: learner plus architecture/security reviewer as relevant. No work below has been executed.

## Goal
Start one pinned model privately and verify lifecycle.

## Why
A stable baseline is necessary before optimization.

## Prerequisites
Phase 00 COMPLETE and approved; artifacts/license access authorized.

## Architecture impact
Adds native vLLM runtime, local artifacts and model-aware readiness.

## Planned files
deploy/README.md; deploy/compose.yaml; profiles/baseline.yaml; tests/contract/test_smoke.py. These are candidate implementation paths, not existing deliverables. Reuse native tooling and existing repository patterns before creating custom files.

## Execution sequence
1. Prepare the approved pinned runtime and model artifacts; verify model/tokenizer revision and effective chat template before startup.
2. Bind serving privately, keep management routes private and supply credentials outside checked-in files.
3. Start with the reviewed floating-point baseline and explicit cache settings; record effective arguments instead of relying on defaults.
4. Probe readiness during loading, issue one synthetic bounded request only after model-aware readiness, then restart the same profile.

## Tests
Planned checks only; none have been run. Use accepted Phase 00 budgets if explicitly revised before measurement.

| Input / condition | Action | Expected result |
|---|---|---|
| Local artifacts; process loading | Poll readiness then submit a synthetic request | Readiness is false until model can serve; successful response uses configured alias. |
| Unknown model alias | Submit chat request | Explicit model error; no dynamic download or silent model substitution. |
| Restart same immutable profile | Stop/start and repeat smoke request | Same revisions/settings recorded; service returns ready within provisional 10-minute startup budget. |
| Unapproved network client | Attempt serving and management routes | Connection blocked at intended boundary; no prompt or credential in logs. |

## Failure scenarios
Driver mismatch; incomplete artifacts; tokenizer/template mismatch; process ready before model; missing secret.

## Acceptance criteria
Known image/model/tokenizer digests recorded; successful synthetic request after readiness; inaccessible unapproved network surfaces; restart reproduces behavior.

## Learning objectives
Serving lifecycle, prefill and decode.

## Interview questions
How does readiness differ from liveness? What makes this deployment reproducible?

## Review gate and rollback
Attach environment, exact test invocation, date, outcomes, evidence paths and limitations. Do not mark COMPLETE until reviewer accepts the evidence and explain-back. Restore the last verified profile on regression; for pre-runtime documentation decisions, revert the decision change while preserving investigation evidence. Blocked hardware or unsupported execution is recorded explicitly; never substitute claimed success. Next phase remains NOT_STARTED until approved.
