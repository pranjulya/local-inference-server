# Planning gap analysis — pre-implementation findings

Status: OPEN_FOR_REVIEW — not an ADR and not authorization to write application code. Author: implementing agent. Findings and proposed locks below were produced by the agent that wrote the plan, so reviewer disposition is the gate. When this file conflicts with the owning document, do not implement from this file; repair the owning document first.

Date: 2026-09-24. Audience: reviewer (learner/operator plus architecture and security reviewer).

Authoritative documents remain [PRD](../product/PRD.md), [HLD](HLD.md), [LLD](LLD.md), the accepted ADRs, [benchmark strategy](../evaluation/benchmark-strategy.md), and [Definition of Done](../definition-of-done.md).

## Reviewer brief

Please decide:

1. Whether each **contradiction** in §1 is real, and which proposed lock is correct.
2. Whether each **spec gap** in §2 must close before its named phase or can wait.
3. The **Phase 00 scope** in §3 — documentation-only with GPU execution blocked, or blocked entirely until a host exists.
4. Whether the **missing process files** in §4 are in scope for V1.
5. That §5 "do not add" still matches the PRD non-goals.

Record disposition as accept / revise / reject per numbered item. Do not start application code from this list.

## Current repository state

| Area | What exists | Status |
|---|---|---|
| Product and architecture docs | PRD, HLD, LLD, threat model, architecture review | DRAFT / PROPOSED for review |
| Decisions | `docs/architecture/ADRs/` 001–003 plus README | All PROPOSED; none accepted |
| Phases 00–06 | Specifications only | All `NOT_STARTED` |
| Learning | 4 concepts, 4 scenarios, 18 interview answers | Outline-level |
| Application code | None | Not started |
| Runtime profiles, benchmarks, reports | None | Phase 01+ |
| Git | `origin/main`, root commit `15e3b88`, corrected by `d68ed4a` | Present and clean |
| Hardware target | None available | Blocking for Phases 01–06 |

---

## 1. Contradictions to resolve before code

### C-01 — Repository status stated as unresolved (RESOLVED)

- **Was:** `Implementation.md` said no suitable repository was found and "it is not a git worktree"; `CLAUDE.md` said "fallback folder, not a configured worktree"; `README.md` said setup "remains pending".
- **Lock (applied):** repository established at `origin` on `main`; the statements were corrected in `d68ed4a`.
- **Residual:** the isolated-worktree question is now a Phase 00 decision (see G-05).

### C-02 — "PLANNING_COMPLETE" against DRAFT and PROPOSED documents

- **`Implementation.md`:** `Status: PLANNING_COMPLETE; implementation NOT_STARTED`.
- **PRD:** `Status: DRAFT for review`. **ADRs:** all `PROPOSED`, none accepted. This review artifact: open.
- **Proposed lock:** distinguish *package complete* from *reviewed*. Either retitle to `PLANNING_PACKAGE_COMPLETE; REVIEW_PENDING`, or accept the ADRs and PRD so `PLANNING_COMPLETE` becomes accurate.
- **Owner:** `Implementation.md`.

### C-03 — "Evaluation strategy" does not exist under that name

- **HLD:** "Read evaluation strategy before accepting LLD."
- **Actual file:** `docs/evaluation/benchmark-strategy.md`, titled "Benchmark and quality strategy".
- **Proposed lock:** rename the reference in HLD to "benchmark and quality strategy", or rename the file. Prefer fixing the reference; the file name is already linked from `Implementation.md` and `implementation/README.md`.
- **Owner:** `HLD.md`.

### C-04 — Runbook referenced but not owned by any document

- **`observability.md`:** "An alert links the deployment/profile and runbook".
- **`phase-06.md`:** lists "docs/operations/ runbook updates" as a planned file.
- **Problem:** no runbook exists and no document states where it is created; "updates" implies a predecessor that was never planned.
- **Proposed lock:** add the runbook as a Phase 06 creation (not update), with a named skeleton path.
- **Owner:** `phase-06.md`.

### C-05 — Phase file naming diverges from the project series

- **This repository:** `phase-00.md` … `phase-06.md`.
- **P05/P07:** `phase-00-foundation.md`, `phase-01-golden-dataset.md`, and so on.
- **Impact:** cosmetic, but link targets and grep patterns differ across the series; renaming later invalidates any external references.
- **Proposed lock:** decide now — rename to slug form to match the series, or record the divergence as intentional.
- **Owner:** `implementation/`.

---

## 2. Spec gaps

Close each gap in the owning document before the named phase rather than improvising in code.

### Feasibility and foundation — before Phase 00 / 01

| ID | Missing item | Proposed lock | Before |
|---|---|---|---:|
| G-01 | GPU execution target | No host exists. Record the blocker explicitly; do not substitute the Apple client machine, do not simulate GPU results, and do not claim compatibility. | 00 |
| G-02 | Approved compute/spend ceiling | Reviewer must supply the ceiling; Phase 00 records it or records it as unapproved. | 00 |
| G-03 | Model and license | Reviewer approves one floating-point model plus a quantized sibling, or records the choice as pending. License acceptance is an explicit reviewer action. | 00 |
| G-04 | vLLM release and image digest | Without a host, compatibility cannot be verified. Record candidate revisions as *candidates*, never as verified pins. | 00 |
| G-05 | Implementation working tree vs isolated worktree | The worktree requirement was premised on the repo not existing. Recommend implementing in this working tree and retiring the isolated-worktree item. | 00 |
| G-06 | Repository license | No `LICENSE` file. P05 had one at planning; P07 did not. Decide whether the repo is licensed and under what terms. | 00 |
| G-07 | ADR acceptance-record format | ADRs are PROPOSED with no template and no acceptance record (reviewer, date, evidence, consequences). P05 had a decisions README; P07 had an ADR template. | 00 |
| G-08 | `environment-manifest.md` schema | Referenced as a Phase 00 deliverable; field list and format undefined. | 00 |
| G-09 | Profile file format and schema | LLD names profile fields but not the serialization (YAML assumed) or validation rules for a malformed profile. | 01 |
| G-10 | Auth mechanism | "API key" is assumed: no format, storage, rotation, comparison-safety or revocation definition. | 01 |
| G-11 | Bind addresses, ports, TLS policy | TLS is "required for any non-loopback path carrying credentials"; loopback default and port numbers unstated. | 01 |
| G-12 | Readiness implementation | "Readiness represents the ability to serve the configured model" — endpoint, checks and failure transition unspecified. | 01 |
| G-13 | Model artifact layout and mount path | Read-only mount is required where practical; directory layout and identity of the mount point undefined. | 01 |
| G-14 | Supervisor recovery policy | "bounded supervisor policy" is a phrase, not a bound: max restarts, backoff and give-up condition unstated. | 01 |
| G-15 | Repository scaffolding ownership | `pyproject.toml`, `Dockerfile`, `.dockerignore`, `Makefile`, `.env.example` and CI workflow are not assigned to any phase. P05/P07 placed them in Phase 00. | 01 |

### Request boundary and contract — before Phase 02

| ID | Missing item | Proposed lock | Before |
|---|---|---|---:|
| G-16 | Native status mapping | LLD mapping is explicitly "candidate"; must be pinned against the chosen release and recorded in contract fixtures. Blocked until G-04. | 02 |
| G-17 | Streaming event format | "the server's supported event format" — pin the exact schema for the chosen release. | 02 |
| G-18 | Disconnect detection and orphan measurement | "no orphaned request after 5 seconds" is proposed; detection mechanism and how orphans are observed are undefined. | 02 |
| G-19 | Queue-enforcement point | Admission bounds are specified, but whether the engine queue is actually bounded must be proved or overflow rejected at ingress. | 02 |
| G-20 | `docs/api-contract.md` skeleton | Planned deliverable with no agreed structure or audience. | 02 |
| G-21 | Management-route allowlist | "Keep health, metrics, docs, debug and administration private" — enumerate the exact routes to deny on the client surface. | 02 |

### Benchmark and quality evidence — before Phase 03

| ID | Missing item | Proposed lock | Before |
|---|---|---|---:|
| G-22 | Quality suite location and schema | A "versioned 100-item synthetic/permissively licensed suite" with a 25/25/25/25 split is specified, but no path, file format, manifest or hashing scheme exists. | 03 |
| G-23 | Rubric definition | "a written 0/1/2 human rubric" is undefined: anchors, adjudication and who scores. P07 locked rubric anchors explicitly. | 03 |
| G-24 | Tuning/hold-out split mechanics | 50/50 split is stated; how membership is recorded and enforced is not. | 03 |
| G-25 | Workload manifest schema | `benchmarks/workload-manifest.json` is a planned path with undefined fields. | 03 |
| G-26 | Thermal-stabilization criterion | "idle cooldown sufficient to stabilize GPU temperature" is unmeasurable as written. Needs a numeric criterion (for example, temperature delta below a threshold). | 03 |
| G-27 | Sampling method for GPU telemetry | Utilization/temperature are required in results; sampling tool, interval and aggregation are unstated. | 03 |
| G-28 | Load-generator placement | Same host versus separate client changes the measurement materially and is unspecified. | 03 |
| G-29 | p95 definition | Nearest-rank versus interpolated percentile changes reported numbers. P07 closed this as a gap by locking nearest-rank. | 03 |
| G-30 | Clock and timestamp format | "start/end clocks" specified with no source, monotonicity or timezone convention. | 03 |

### Optimization experiments — before Phase 04 / 05

| ID | Missing item | Proposed lock | Before |
|---|---|---|---:|
| G-31 | Cache configuration vocabulary | `prefix_cache_mode` is a field, but the accepted values and the pinned release's actual knobs are undefined. | 04 |
| G-32 | Cold-start definition and procedure | Cold process, empty cache, warmed cache and steady state are named but the procedure to reach each is not. | 04 |
| G-33 | Cache privacy/salt decision | Cache sharing is acceptable only under one trusted domain; record whether salting is supported and configured, or explicitly unsupported. | 04 |
| G-34 | Quantization backend and sibling model | Backend names, the specific quantized sibling and its license are unresolved (depends on G-03/G-04). | 05 |
| G-35 | KV dtype support matrix | KV quantization is "optional and support-dependent"; the compatibility record format is unspecified. | 05 |
| G-36 | Promotion ADR format | ADRs 005/006 are planned deliverables that depend on the template from G-07. | 04/05 |

### Operations and release — before Phase 06

| ID | Missing item | Proposed lock | Before |
|---|---|---|---:|
| G-37 | Raw-record retention mechanism | "default 30 days" is stated with no enforcement point, sweep or evidence of deletion. | 06 |
| G-38 | Canary set definition | Prompt/output/credential canaries are required for the leakage audit; the actual canary values and where they are injected are undefined. | 06 |
| G-39 | Evidence manifest schema | "file schema is chosen during implementation" — must be closed before Phase 06 assembles the manifest. | 06 |
| G-40 | Operator runbook | Does not exist (see C-04). | 06 |
| G-41 | Release checklist | P05/P06/P07 each carry `docs/operations/release-checklist.md`. This project relies on Definition of Done R1–R8 instead. Decide whether to add one or record the substitution as intentional. | 06 |
| G-42 | Publication approval process and roles | "approved publication assets" appears without naming who approves or what the record looks like. | 06 |
| G-43 | Demo storyboard | Named in phase-06 planned files with no structure. | 06 |

---

## 3. Proposed Phase 00 scope

Reviewer selected documentation-only. Proposed consequence, for confirmation:

- Phase 00 produces a **documentation-only feasibility record**: the recorded absence of a GPU target, unapproved budget/license/model, candidate (not verified) revisions, and the memory estimate for the proposed envelope.
- GPU execution is marked **blocked**, not passed. No compatibility claim is made and no benchmark is run.
- Phases 01–06 remain `NOT_STARTED` and cannot produce measured evidence until a host is authorized.
- The phase may not be marked `COMPLETE` on documentation alone unless the reviewer accepts a blocked outcome as the phase's honest result; [Definition of Done](../definition-of-done.md) requires evidence for acceptance checks and forbids presenting a non-run as a pass.

Confirm whether Phase 00 should be authorized on that basis, or held until a host exists.

---

## 4. Missing repository and process files

| Item | Why | Status |
|---|---|---|
| `LICENSE` | Repository terms; model license is separate (G-03/G-06) | Missing |
| `CONTRIBUTING.md` | One phase at a time; no paid or GPU-required tests by default | Missing |
| `SECURITY.md` | Vulnerability reporting matched to the threat model | Missing |
| ADR template | Date, owner, context, decision, consequences, superseded-by (G-07) | Missing |
| CI workflow | Phase 00/01 gate; no automated check exists yet (G-15) | Missing |
| `.env.example` | Declares configuration surface without secrets | Missing |
| `docs/operations/release-checklist.md` | Series convention (G-41) | Missing |
| `docs/operations/runbook.md` | Referenced by two documents (C-04, G-40) | Missing |

### Depth relative to the project series

Measured against the planning commit of each sibling repository:

| Repository | Markdown files | Markdown lines |
|---|---:|---:|
| P05 `h-i-t-l` | 39 | 2,001 |
| P06 `streaming-copilot-ui` | 31 | 2,683 |
| P07 `automated-eval-harness` | 44 | 2,804 |
| **P08 (this)** | **38** | **847** |

File count matches the series; content density is roughly one third. Phase specifications are at parity (49–53 lines each against P07's 50–53), so the shortfall is concentrated in PRD (31 against 125/162), HLD (27 against 152/169) and LLD (32 against 175/255). Decide whether to expand to series depth or record the terse style as intentional.

---

## 5. Do not add in V1

These remain PRD non-goals. Reject review comments that introduce them.

- Training, fine-tuning, or a model marketplace
- Kubernetes, multi-GPU, multi-node, or an orchestration control plane
- High availability, autoscaling, or a multi-tenant isolation promise
- Public or anonymous chat access
- Billing, metering-for-charge, or a cost dashboard
- Provider API fallback of any kind, including "temporary" fallbacks
- A database, custom cache service, or bespoke scheduler alongside vLLM
- Audio, image, embedding, or tool-execution surfaces
- Custom kernels or remote model code without a recorded exception
- Universal OpenAI API parity beyond the documented subset

---

## 6. Reviewer response template

```text
C-01 Repository status: accept — corrected in d68ed4a
C-02 PLANNING_COMPLETE vs DRAFT: accept / revise — notes:
C-03 "evaluation strategy" naming: accept / revise — notes:
C-04 Runbook ownership: accept / revise — notes:
C-05 Phase file naming: accept rename / accept divergence — notes:

G-01..G-15 feasibility/foundation: accept-all / list revisions:
G-16..G-21 request boundary: accept-all / list revisions:
G-22..G-30 benchmark/quality: accept-all / list revisions:
G-31..G-36 optimization: accept-all / list revisions:
G-37..G-43 operations/release: accept-all / list revisions:

Phase 00 scope §3: authorize documentation-only / hold until a host exists
Process files §4: add (list) / drop (list)
Depth §4: expand to series depth / accept terse style
Do-not-add §5: confirm / conflict with PRD

ADR-001: accept / revise / reject — notes:
ADR-002: accept / revise / reject — notes:
ADR-003: accept / revise / reject — notes:

Blockers before authorizing Phase 00:
Additional contradictions found in planning docs:
```
