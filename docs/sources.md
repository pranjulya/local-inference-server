# Primary sources and version policy

Reviewed 2026-09-23. These documentation URLs are moving references; Phase 00 must resolve and record release-specific equivalents with image/model hashes before implementation. Product limits and targets are proposed project decisions, not statements from these references.

- [vLLM automatic prefix caching](https://docs.vllm.ai/en/stable/features/automatic_prefix_caching/): prefix reuse avoids repeated prompt computation; compare repeated and unique prefixes.
- [vLLM quantization](https://docs.vllm.ai/en/stable/features/quantization/): hardware/backend support must be checked for selected artifacts.
- [vLLM quantized KV cache](https://docs.vllm.ai/en/stable/features/quantization/quantized_kvcache/): KV representation is a separate choice from weight compression.
- [vLLM security guidance](https://docs.vllm.ai/en/stable/usage/security/): network boundaries, API-key scope, resource controls and cache privacy need explicit review.
- [vLLM metrics source documentation](https://github.com/vllm-project/vllm/blob/main/docs/design/metrics.md): use native metrics and verify actual names for pinned release.
- [vLLM serving benchmark CLI](https://docs.vllm.ai/en/latest/cli/bench/throughput/): reuse supported benchmark tooling, while matching the measurement definition to the experiment.

No particular current model/GPU pair is recommended without inventory and license review. No external benchmark numbers are reused as project results.
