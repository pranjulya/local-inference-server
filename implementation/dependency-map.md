# Phase dependency map

```mermaid
flowchart LR
 P00[00 Feasibility] --> P01[01 Baseline server]
 P01 --> P02[02 Bounded API]
 P02 --> P03[03 Baseline evidence]
 P03 --> P04[04 Cache experiment]
 P04 --> P05[05 Quantization]
 P05 --> P06[06 Recovery and release]
```

Each arrow means predecessor COMPLETE plus review approval, not merely files present. Evaluation protocol is reviewed before accepting the LLD and frozen in Phase 03 before optimization. Optional KV experiment in Phase 05 can be recorded unsupported without blocking a supported weight experiment and release. A failed quality candidate falls back to the baseline; negative experimental evidence is valuable.

No hard dependency on Projects 09 or 10. Optional telemetry and guardrail adapters are later scope with their own review, not hidden prerequisites. Project 11 consumes sanitized evidence after release; Project 12 links that evidence or a recorded demo. Neither requires publicly exposing the GPU host.

The full Project 12 all-live target needs actual reviewed live P08 access; a recording is a fallback and must be labeled as such. Keep inference private by default. A later authenticated, rate-/budget-bounded access design and explicit exposure approval are required before claiming P08 live-demo coverage.
