# Phase 04 — Cache and scheduling experiments

Status: NOT_STARTED. Owner: implementing agent after authorization. Reviewer: learner plus architecture/security reviewer as relevant. No work below has been executed.

## Goal
Measure prefix caching and bounded scheduling choices against baseline.

## Why
Teach the actual bottleneck rather than enabling every performance flag.

## Prerequisites
Phase 03 COMPLETE and approved; identical model and quality fixtures.

## Architecture impact
Candidate profile changes only cache first, then one scheduling knob per trial.

## Planned files
profiles/cache.yaml; reports/cache/; docs/architecture/ADRs/005-cache-promotion.md. These are candidate implementation paths, not existing deliverables. Reuse native tooling and existing repository patterns before creating custom files.

## Execution sequence
1. Clone the verified baseline profile and change prefix caching only; record exact tokenizer/template so shared-prefix identity is interpretable.
2. Run repeated 1,500-token-prefix and length-matched unique-prefix cohorts with cold/empty and warmed cache states explicitly recorded.
3. Compare TTFT, tails, memory and quality spot checks against matching baseline trials; corroborate reuse using observed release-specific metrics.
4. Only after cache comparison, vary one scheduling setting per trial within the declared envelope; retain effective arguments and negative results.
5. Recommend promotion or rejection against predeclared thresholds; retain the baseline regardless of experiment outcome.

## Tests
Planned checks only; none have been run. Use accepted Phase 00 budgets if explicitly revised before measurement.

| Input / condition | Action | Expected result |
|---|---|---|
| Repeated-prefix warm cohort | Compare paired medians | Target >=20% TTFT reduction; null/negative findings recorded honestly. |
| Unique-prefix matched cohort | Compare p95 TTFT | Promotion requires <=10% regression against matching baseline. |
| Different prefix after chat templating | Check token prefix identity and reuse | Do not count expected cache hit without verified shared token prefix. |
| Changed scheduling knob | Repeat matched concurrency cell | Only intended variable differs; same token/quality bounds continue to hold. |
| Memory pressure under concurrency four | Observe queue and memory | No OOM or unbounded waiting; reject candidate if it breaks service envelope. |

## Failure scenarios
Cache hit metric misread; shared prefix unexpectedly differs after templating; memory pressure; faster median with worse tails.

## Acceptance criteria
Publish cache gain/null result; promotion only if repeated-prefix target and unique-prefix regression limits pass; baseline remains available; exact effective settings recorded.

## Learning objectives
Cache reuse, queueing and causal experiments.

## Interview questions
What changes if prefixes differ by a token? Why can hit rate and user latency disagree?

## Review gate and rollback
Attach environment, exact test invocation, date, outcomes, evidence paths and limitations. Do not mark COMPLETE until reviewer accepts the evidence and explain-back. Restore the last verified profile on regression; for pre-runtime documentation decisions, revert the decision change while preserving investigation evidence. Blocked hardware or unsupported execution is recorded explicitly; never substitute claimed success. Next phase remains NOT_STARTED until approved.
