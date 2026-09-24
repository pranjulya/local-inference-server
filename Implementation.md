# Project 08 — Local Inference Server

Status: PLANNING_COMPLETE; implementation NOT_STARTED. Planning date: 2026-09-23. This file is the master source of truth for scope, phase status and approved decisions. Every ADR remains PROPOSED until reviewed. No performance results or deployment are claimed.

## Outcome and boundary
Build a reproducible, private, single-GPU text inference service using vLLM. Demonstrate which workload benefits from prefix caching and whether a supported quantized model reduces memory without unacceptable quality loss. No provider inference API fees; hardware purchase/rental, electricity, storage and maintenance still cost money. This is a learning and portfolio deployment with production practices, not an HA or multi-tenant service.

## Workspace and assumptions
Planning began without a Project 08 repository: none was found in the bounded registered-project and src/genAI inventory examined by the coordinating architect, and no graph tools/index generation were available. On 2026-09-24 the repository was created and this folder became its working tree — origin `https://github.com/pranjulya/local-inference-server.git`, default branch `main`, planning package committed as the root commit. Whether implementation proceeds in this working tree or in an isolated worktree remains a Phase 00 decision.

Recommended target: one Linux host with one supported NVIDIA GPU. The present Apple workstation is a planning/client machine, not the assumed CUDA inference host, and no Linux GPU host is available to this project as of 2026-09-24. Phase 00 therefore proceeds as a documentation-only feasibility record with GPU execution explicitly blocked; Phases 01–06 cannot produce measured evidence until a host is authorized. Begin with one instruction-tuned, permissively licensed model small enough for a floating-point baseline and a supported quantized sibling. Exact GPU, VRAM, model revision, vLLM release/image digest, driver and quantization backend are unresolved until compatibility evidence exists. No cloud spend, model license acceptance or public exposure is authorized by this plan.

## Read order and gates
1. [PRD](docs/product/PRD.md): approve product boundary.
2. [ADRs](docs/architecture/ADRs/README.md) and [HLD](docs/architecture/HLD.md): review alternatives and trust boundaries.
3. [Threat model](docs/architecture/threat-model.md), [planning gap analysis](docs/architecture/planning-gap-analysis.md) and [architecture review](docs/architecture/architecture-review.md): dispose findings and record ADR accept/revise/reject before any phase starts.
4. [Benchmark strategy](docs/evaluation/benchmark-strategy.md): freeze workload and quality gates before tuning.
5. [LLD](docs/architecture/LLD.md): review concrete contracts against those gates.
6. [Phase map](implementation/dependency-map.md) and [workflow](docs/coding-agent-workflow.md): authorize only the next phase.
7. [Learning path](Learning/learning-path.md): explain the design before implementation.

## Phase ledger
| Phase | Outcome | Status |
|---|---|---|
| 00 | Hardware, repository, licenses and benchmark feasibility | NOT_STARTED |
| 01 | Reproducible baseline private server | NOT_STARTED |
| 02 | Request limits, streaming and contract checks | NOT_STARTED |
| 03 | Baseline quality and performance evidence | NOT_STARTED |
| 04 | Prefix-cache and scheduling experiments | NOT_STARTED |
| 05 | Weight and optional KV quantization experiments | NOT_STARTED |
| 06 | Operational recovery, privacy and release evidence | NOT_STARTED |

## Major decisions and review focus
Use native vLLM serving rather than a custom inference engine. A narrow reverse-proxy ingress enforces network policy and bounded access; add custom request validation only for gaps demonstrated by native settings. One GPU/model/process avoids distributed failure modes. Benchmarks vary one factor at a time. Keep the baseline as rollback candidate. Prefix reuse is a prefill optimization; weight compression and KV representation are separate experimental axes. Security is defense in depth and does not imply safe generated content.

Project 09 may consume bounded metadata/metrics later; Project 10 may provide an upstream policy boundary later. Neither is needed to run or validate this project. Project 11 may publish sanitized evidence; Project 12 should link a recorded demo as the safe initial fallback, with no unauthenticated inference endpoint. Its full all-live target is not fulfilled by a recording: P08 live access requires a separately reviewed authenticated, budget-bounded deployment and acceptance test.

## Approval before implementation
Review hardware access/spend ceiling, model license and revision, private-only scope, provisional budgets, the implementation working-tree/worktree choice, and proposed ADRs. These do not block completion of planning. Phase 00 must make unresolved feasibility explicit rather than silently selecting paid infrastructure. Subsequent gates require test evidence, learning explanation and an updated ledger. See [Definition of Done](docs/definition-of-done.md).
