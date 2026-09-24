# Architecture review and readiness checklist

Review result: PENDING_USER_REVIEW — NOT APPROVED_FOR_IMPLEMENTATION. Author: implementing agent. A coding agent must not accept an ADR or authorize a phase, so this file records findings and evidence only; the reviewer's disposition is the missing gate. Consolidates the readiness checklist from [planning gap analysis](planning-gap-analysis.md).

Reviewed scope: the planning package as committed (`15e3b88`, corrected by `d68ed4a`), comprising PRD, HLD, LLD, three PROPOSED ADRs, benchmark and quality strategy, operations documents, Learning material and phase-00 through phase-06. No application code, runtime configuration or measurement exists, so no implementation evidence is claimed.

## 1. Requirements and consistency

- [x] PRD R1–R8 each name a verification method, and R1–R8 are traced in [Definition of Done](../definition-of-done.md) and phase-06 acceptance.
- [x] HLD and LLD agree on the enforced envelope: routes `GET /v1/models` and `POST /v1/chat/completions`, body ≤128 KiB, ≤4,096 combined tokens, `n=1`, bounded concurrency.
- [x] LLD error mapping is marked candidate and requires contract fixtures before it is claimed compliant.
- [x] Phase dependency map is acyclic; each arrow requires predecessor COMPLETE plus review approval.
- [x] Native vLLM serving is preferred over a custom engine or scheduler everywhere it is discussed.
- [x] OpenAI compatibility is scoped to a documented route/field subset, not blanket parity.
- [x] Prefix reuse, weight compression and KV representation are kept as separate experimental axes.

## 2. Scope and simplicity

- [x] No database, custom cache service, orchestrator, Kubernetes or multi-GPU design is introduced.
- [x] Ingress is limited to enforcing network policy and bounded access; custom validation is added only for a demonstrated gap.
- [x] One GPU, one model, one process avoids distributed failure modes.
- [x] Candidate implementation paths are labeled as candidates, not deliverables.
- [x] Public exposure, paid provisioning and provider fallback are excluded by default.

## 3. Benchmark and quality validity

- [x] Baseline is floating-point with cache state declared explicitly; one factor changes per experiment.
- [x] Workloads, repetitions, warm-up exclusion and randomization are specified before tuning.
- [x] Combined winning settings are explicitly not evidence of an isolated causal effect.
- [x] Failed requests may not be dropped to improve reported latency.
- [x] Quality gate is paired against the baseline with a ≤2-point aggregate bound and critical-case protection.
- [x] Unsupported KV quantization is an accepted outcome, not a phase failure.
- [x] Provisional budgets are marked for Phase 00 ratification rather than treated as measured.

## 4. Reproducibility and evidence

- [x] Profile fields enumerate image digest, model/tokenizer revisions, chat-template hash, GPU/driver, dtype/backend, cache mode and effective arguments.
- [x] Moving tags and `latest` references are disallowed in favor of immutable revisions/digests.
- [x] Evidence manifest binds every claim to a run id, environment, dataset hash, protocol and artifact hashes.
- [x] Missing values must remain explicit unknowns rather than being converted to zero or omitted.
- [x] Publication is separated from measurement by an explicit approval step.

## 5. Security and privacy

- [x] Trust boundaries and threats are enumerated in the [threat model](threat-model.md).
- [x] No raw prompt, completion or credential persistence in telemetry is a stated rule.
- [x] Management, metrics and health surfaces are kept off the client-facing path.
- [x] Model artifact, image and license review precede download and execution.
- [x] Remote code execution and runtime model downloads are excluded from V1.
- [x] Content safety is explicitly not claimed as a serving control.

## 6. Operability

- [x] Lifecycle (`provisioning → loading → ready → draining → stopped`, `failed`) and readiness semantics are defined.
- [x] Failure policy is bounded: reject overflow, fail closed on auth/validation, no infinite retry, bounded supervisor recovery.
- [x] Telemetry-sink failure must not block serving; buffers and dropped-event counters are bounded.
- [x] Recovery target (5 minutes), soak duration (30 minutes) and rollback profile are named for later measurement.
- [ ] No operator runbook exists yet; `docs/operations/observability.md` and phase-06 both reference one.

## 7. Testing and learning

- [x] Every phase declares tests with expected results, failure scenarios, acceptance criteria, learning objectives and interview questions.
- [x] Contract, limit, streaming, overload, rollback and leakage checks are planned for named phases.
- [x] Definition of Done requires real evidence, reviewer understanding and learner explain-back.
- [x] Learning path, four concepts, four scenarios and 18 interview answers are present.
- [ ] Learning material is outline-level (15–16 lines per page) and needs worked numeric examples in the implementing phase.

## 8. Known limitations accepted for V1

- One GPU host is a single failure domain with planned downtime; no HA or multi-tenant isolation is claimed.
- Hardware, GPU model, VRAM, driver and vLLM release remain unresolved pending inventory; no compatibility is claimed.
- Prefix-cache sharing assumes one trusted operator domain; timing and salting behavior are unverified until measured.
- The 100-item quality suite is small and synthetic; it cannot represent general model quality.
- Budgets in the PRD and LLD are provisional and may require a documented revision after inventory.
- Generated content is not guaranteed factual, safe or injection-resistant.

## 9. Non-blocking decisions deferred to phases

Exact GPU, model and license, vLLM release and image digest, quantization backend, ingress technology, tokenizer source, management-plane tooling, and the absolute latency budgets are selected in Phase 00 or later for the real environment. Their contracts are fixed enough that these choices do not block review. The implementation working-tree/worktree choice is recorded in Phase 00 and does not change any contract.

## 10. Blocking findings

- **No GPU execution target exists.** No Linux host with a supported NVIDIA GPU is available to this project, and the plan forbids substituting the Apple client machine or simulated results. Phase 00 can complete as a documentation-only feasibility record with GPU execution explicitly blocked, but Phases 01–06 cannot produce measured evidence until a host is authorized. This is the single largest readiness gap.
- **All three ADRs are PROPOSED and unreviewed**, including ADR-003 (private boundary), which the security posture depends on.
- **Spend ceiling, model license acceptance and model selection are unapproved**, so Phase 00 step 4 has no approved input.
- **No operator runbook exists**, though two documents reference one.

## 11. Approval gate

No phase is `IN_PROGRESS`; all are `NOT_STARTED` and no implementation is authorized. Review requires: accept or revise ADR-001/002/003 with dated records, disposition the findings in §10, and confirm the Phase 00 documentation-only boundary. Reviewer disposition goes in the response template at the end of the [planning gap analysis](planning-gap-analysis.md). Implementation then proceeds one phase at a time under [coding-agent workflow](../coding-agent-workflow.md).
