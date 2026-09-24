# ADR 001 — Single GPU native serving

Status: PROPOSED. Date: 2026-09-23. Decision owner: user/architect at phase review.

## Context
A custom engine obscures the serving concepts and increases correctness burden; distributed serving adds failure modes before evidence of need.

## Recommended decision
One private Linux NVIDIA GPU host, one model and native vLLM API.

## Alternatives
Custom FastAPI wrapper only if native and ingress controls leave a proven contract gap; multi-GPU or managed API deferred.

## Consequences and risks
Single point of failure and planned downtime are accepted; hardware compatibility and VRAM remain feasibility gates.

## Evidence and acceptance
Phase 00 hardware fit; Phase 01 reproducibility; Phase 06 recovery. No evidence exists yet. Accept only after relevant review; supersede with a linked ADR if scope changes.
