# Interview questions and answer criteria

1. **What does vLLM contribute?** Serving, scheduling and GPU inference infrastructure. Explain the chosen release's behavior using observed requests; do not claim a custom scheduler was built.
2. **Prefill versus decode?** Prefill processes the prompt; decode produces tokens incrementally. TTFT and inter-token latency reveal different bottlenecks.
3. **Why does a loaded model still OOM?** Concurrent KV state, workspaces and allocation overhead exceed remaining memory. Weight fit alone is insufficient.
4. **What does prefix caching optimize?** Eligible shared prompt computation. It is not a stored final answer and does not eliminate novel decode work.
5. **Does prefix caching always help?** No; workload reuse, cache state and overhead matter. Show repeated and unique-prefix measurements.
6. **Weight quantization versus KV quantization?** Different tensors and independent compatibility/quality concerns. Name both profile fields and controlled experiments.
7. **Why no exact GPU/model recommendation yet?** Hardware inventory, license and version compatibility are prerequisites; choosing first would invent feasibility.
8. **How is OpenAI compatibility scoped?** Documented route/field subset with contract tests, not blanket API parity.
9. **What happens during overload?** Bounded admission and explicit rejection; no infinite queue or silent provider fallback.
10. **How does streaming fail?** Partial output cannot be replaced by a new HTTP status after headers; mark incomplete and avoid automatic replay.
11. **What makes the benchmark reproducible?** Immutable environment/model/data, declared cache state, fixed workload, repeated trials, errors and honest uncertainty.
12. **Why can tokens/sec be misleading?** Input/output mix, request lengths, latency and failures change its meaning. Report goodput and distributions.
13. **Is local inference free?** Provider fees can be zero while capital/rental, power and operations remain nonzero. State cost methodology and unknowns.
14. **Why is an API key insufficient?** Endpoint scope and network exposure still matter; protect management surfaces and resource limits independently.
15. **What proves production readiness here?** Bounded contracts, security checks, observability and recovery evidence for one host. HA and multi-tenancy are out of scope.
16. **What if quantization fails quality gates?** Publish the negative experiment and retain the baseline; an honest rejected optimization still demonstrates expertise.
17. **How do Project 09/10 integrate?** Optional metadata export/upstream policy; P08 works independently and retains its own resource/security controls.
18. **What would trigger scaling?** Sustained measured demand beyond the proven single-GPU envelope, followed by an explicit architecture decision and failure-model review.
