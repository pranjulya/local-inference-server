# ADR 002 — Controlled cache and quantization experiments

Status: PROPOSED. Date: 2026-09-23. Decision owner: user/architect at phase review.

## Context
Memory, prefill latency and answer quality are different outcomes. Simultaneous changes hide cause.

## Recommended decision
Freeze baseline; evaluate prefix caching, weight quantization and optional KV quantization independently.

## Alternatives
All optimizations enabled immediately; benchmark only a quantized model without reference.

## Consequences and risks
More experimental runs, but interpretable results. Unsupported KV configuration is recorded and skipped.

## Evidence and acceptance
Phase 03 baseline; Phase 04 cache; Phase 05 paired quality and memory evidence. No evidence exists yet. Accept only after relevant review; supersede with a linked ADR if scope changes.
