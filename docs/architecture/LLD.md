# Low-level design

Status: PROPOSED; candidate file paths below do not imply implemented files.

## API contract
Expose only GET /v1/models and POST /v1/chat/completions to authenticated private clients. Pin and test the selected vLLM release's exact behavior; compatibility means this documented subset, not every provider feature. Model must equal the configured alias. Messages contain allowed roles and text only. Validate positive bounded max_tokens, n=1, body size <=128 KiB, combined token budget <=4,096 using the same tokenizer/chat template as serving, and supported sampling fields. Reject unknown extension fields at ingress if they bypass resource policy. Alias-not-found, malformed, unauthorized, too-large, overloaded and unavailable cases need stable client-visible error categories; record native status mapping in contract fixtures during implementation.

Candidate error mapping: 400 invalid request/token budget; 401 invalid credential; 413 body too large; 429 capacity/admission deadline; 503 not ready; 504 upstream deadline. A native server mismatch must be documented or adapted at the existing ingress, not silently advertised as compliant.

## Streaming and cancellation
Streaming uses the server's supported event format. Measure TTFT at the first actual generated token, excluding role-only/empty events; separately record header latency. A stream ending without its expected terminal condition is incomplete. Once headers are sent, a failure cannot become a new HTTP error response; client reports partial/incomplete output and does not count it as success. Client disconnect must release inference work promptly; proposed acceptance is no orphaned request after 5 seconds. Upstream hard deadline initially 60 seconds; ingress deadline must exceed engine cancellation propagation allowance. No retry after any token has been emitted.

## Configuration and state
Profile fields: profile_id, image_digest, model_revision, tokenizer_revision, chat_template_hash, GPU/driver identifiers, weight_dtype, quantization_backend, kv_dtype, max_context, output_limit, max_active, admission_policy, memory_utilization, prefix_cache_mode, seed and effective server arguments. Values are pinned in Phase 00/01, never inferred from a moving default. Secrets remain external to profile files. Distinguish configured max_active from observed engine running/waiting requests.

Memory planning separates weights, KV allocations, runtime workspaces and safety margin. For conventional transformer attention estimate KV bytes as 2 × layers × KV_heads × head_dimension × cached_tokens × bytes_per_element; actual model architecture, batching and allocation overhead determine measured fit. Do not use parameter count alone or assume quantized weights imply quantized KV.

## Candidate file map
| Future path | Responsibility |
|---|---|
| deploy/README.md and deploy/compose.yaml | one-host lifecycle and pinned runtime |
| profiles/baseline.yaml | reviewed baseline settings, no secrets |
| profiles/cache.yaml and profiles/quantized.yaml | controlled candidate differences |
| ingress/ | minimal allowlist/auth/resource configuration |
| tests/contract/ | response, bounds and streaming tests |
| benchmarks/ | synthetic manifest and repeatable load runner |
| reports/ | immutable sanitized run summaries and provenance |

These are planned artifacts only. Prefer vLLM's provided benchmark tools where they cover the declared measurements; add a small harness only for missing quality/contract checks.

## Integration contract
Optional Project 09 export: request_id, trace context if supported by pinned integration, service/version/profile, status, duration, input/output token counts and measurement source. No raw prompt/completion or credentials; user identifiers are not metric labels. Project 10 remains an upstream optional consumer that can enforce a stricter policy; this service still enforces auth and resource bounds independently. Report external provider API fees as zero for every verified native-local attempt, including failures, when no externally billed service was used. Token usage may remain unknown after a failed or interrupted request; do not convert missing usage into zero tokens. Local compute cost may still accrue for failed attempts and is reported separately with its estimation method; unknown cost remains unknown rather than zero.
