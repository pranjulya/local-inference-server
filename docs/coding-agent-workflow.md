# Coding-agent workflow and review gates

Read Implementation.md, current phase, accepted ADRs and relevant learning pages. Confirm repository/worktree and inspect existing files before editing. If graph tools exist use project/generation plus task-directed graph evidence and coverage checks; otherwise state unavailability and inspect exact source. Do not infer implementation from planned paths.

G0: learner approves Phase 00 hardware, scope and licensed model. G1: architecture reviewer approves native-serving boundary, benchmark protocol and LLD consistency. G2 before each phase: identify one bounded diff, assumptions, measurable tests and rollback. Implement only that phase using native features first. G3: run checks on correct target hardware, review security and contract effects, attach evidence and explain failures. G4: learner reviews concept explanation, interview answers and outcomes; only then mark COMPLETE and authorize next phase. A coding agent must not self-approve an ADR or next-phase transition.

Record decisions and exceptions in the master/ADR, not only chat. Never install, download large artifacts, spend money or expose a port merely because a future phase lists it; confirm the current implementation authorization covers the action. During this planning task all phases stay NOT_STARTED and no implementation is authorized.

Reviewer checklist: Does the change meet its requirement? Are workload bounds enforced at every route? Are quality comparisons paired and fair? Are prompts absent from telemetry? Does rollback use verified immutable artifacts? Are unsupported features honestly recorded? Is an abstraction or dependency avoidable? End each phase with changes, tests/evidence, limitations, learning explanation and next gate. No .claude agents/commands are created because this workflow is sufficient; add tool-specific files only after a repeated need is demonstrated.

## Status transitions
Use NOT_STARTED → IN_PROGRESS → IMPLEMENTED → TESTED → REVIEWED → COMPLETE. IMPLEMENTED means the authorized phase artifacts exist, TESTED means required checks have real evidence, REVIEWED means review findings are resolved, and COMPLETE means the learner accepts the phase and ledger update. Record blockers alongside the current status rather than skipping transitions. The current planning package keeps every implementation phase NOT_STARTED.
