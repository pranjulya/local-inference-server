# Phase 02 — Request and streaming boundaries

Status: NOT_STARTED. Owner: implementing agent after authorization. Reviewer: learner plus architecture/security reviewer as relevant. No work below has been executed.

## Goal
Enforce documented API subset and bounded resource usage.

## Why
An apparently working server can still leak capacity or expose routes.

## Prerequisites
Phase 01 COMPLETE and approved; LLD contract review.

## Architecture impact
Adds minimal ingress policy; native validation retained wherever sufficient.

## Planned files
ingress/ policy files; tests/contract/test_limits.py; tests/contract/test_streaming.py; docs/api-contract.md. These are candidate implementation paths, not existing deliverables. Reuse native tooling and existing repository patterns before creating custom files.

## Execution sequence
1. Inventory native validation and ingress capabilities; map each declared limit to its actual enforcing component, adding custom logic only for a demonstrated gap.
2. Enforce route/field allowlist, credential validation, n=1, 128-KiB body limit and shared-tokenizer 4,096 total-token budget with maximum 512 output tokens.
3. Configure four active requests, queue capacity eight and five-second admission deadline or explicitly reject excess at ingress if bounded queue enforcement cannot be proved.
4. Verify streamed terminal conditions, distinguish first generated token from empty events, and propagate disconnect/deadline cancellation to engine work.
5. Document observed native-to-client error mappings and prove management routes cannot bypass the ingress policy.

## Tests
Planned checks only; none have been run. Use accepted Phase 00 budgets if explicitly revised before measurement.

| Input / condition | Action | Expected result |
|---|---|---|
| Missing or invalid credential | Call client routes | 401; no engine admission. |
| 128-KiB and 128-KiB+1-byte bodies | Submit otherwise valid requests | Boundary-sized body proceeds to other validation; larger body returns 413. |
| 4,096 and 4,097 combined tokens | Tokenize with effective template and requested output budget | At-limit request eligible; over-limit request returns 400 without inference. |
| max_tokens=513 or n=2 | Submit chat request | 400 invalid resource policy; no work admitted. |
| 4 active + 8 waiting, then another arrival | Run controlled saturation | No extra queued work; 429 on overflow, waiting requests admitted or rejected within 5 seconds. |
| Client disconnect after first generated token | Close streaming connection | Request released within 5 seconds; incomplete output is not success. |
| Worker exits after response headers | Observe stream termination | Client records incomplete stream; no automatic replay or fabricated completed answer. |

## Failure scenarios
Unbounded native queue; byte limits mistaken for token limits; stream proxy buffering; orphaned generation; management endpoint exposed.

## Acceptance criteria
Declared status mapping verified; overflow rejected within admission budget; no bypass of limits; disconnect releases work within proposed 5 seconds; partial stream never counted successful.

## Learning objectives
Trust boundaries and streaming semantics.

## Interview questions
Why do body and token limits both matter? Why not automatically retry a partial stream?

## Review gate and rollback
Attach environment, exact test invocation, date, outcomes, evidence paths and limitations. Do not mark COMPLETE until reviewer accepts the evidence and explain-back. Restore the last verified profile on regression; for pre-runtime documentation decisions, revert the decision change while preserving investigation evidence. Blocked hardware or unsupported execution is recorded explicitly; never substitute claimed success. Next phase remains NOT_STARTED until approved.
