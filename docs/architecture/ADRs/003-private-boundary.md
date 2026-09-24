# ADR 003 — Private bounded API and content-free telemetry

Status: PROPOSED. Date: 2026-09-23. Decision owner: user/architect at phase review.

## Context
Native API-key behavior is not a blanket endpoint security policy; content logs create avoidable exposure.

## Recommended decision
Only authenticated private text requests through allowlisted ingress; management endpoints private.

## Alternatives
Public anonymous demo; application-layer keys without network boundary; raw prompt traces.

## Consequences and risks
Less convenient public demo; recorded sanitized demonstration is default. No multi-tenant assurance.

## Evidence and acceptance
Phase 02 boundary checks; Phase 06 leakage and recovery drills. No evidence exists yet. Accept only after relevant review; supersede with a linked ADR if scope changes.
