# Product requirements

Status: DRAFT for review. Owner: learner/operator. Primary user: a developer testing local text generation with a familiar HTTP client. Secondary user: reviewer comparing reproducible latency, memory and quality evidence.

## User journeys
An operator prepares an approved GPU host, verifies a pinned artifact and starts a private service. A client discovers the configured model alias, sends bounded text messages, receives a complete or streamed answer, and sees an explicit error if capacity is unavailable. A learner runs a fixed benchmark matrix and explains the tradeoff between quality, TTFT and concurrent throughput. An operator diagnoses overload and restores the last verified profile.

## Requirements
| ID | Requirement | Verification |
|---|---|---|
| R1 | Serve one approved text model through an OpenAI-compatible subset | model listing and chat contract tests |
| R2 | No paid external inference calls and no silent fallback | restricted-egress test and dependency review |
| R3 | Admit only authenticated private clients with bounded work | missing credential, oversized context and saturation checks |
| R4 | Preserve complete/streamed response semantics and cancellation | stream completion, disconnect and timeout checks |
| R5 | Compare baseline, prefix reuse and weight quantization | controlled benchmark reports with raw sanitized evidence |
| R6 | Explain KV quantization separately, execute only if supported | compatibility record or explicit unsupported result |
| R7 | Expose operational metadata without prompt content | telemetry leakage audit |
| R8 | Recover from worker restart and configuration regression | rollback and readiness drill |

## Provisional service envelope
Text only; one trusted operator domain; one model alias. Proposed initial envelope: 4,096 total tokens including generation, default 256 generated tokens, maximum 512, at most four admitted concurrent requests and a bounded queue of eight with 5-second admission deadline. These are project defaults, not vLLM defaults. Exact queue enforcement must be proved at Phase 02; otherwise reject overflow at ingress instead of pretending the engine queue is bounded. Restrict supported body fields, n=1, no arbitrary model loading, adapters, URL media, embeddings or tool execution.

For the 512-input/128-output concurrency-one warm workload, provisional goals are p95 TTFT <=2 seconds, p95 end-to-end <=15 seconds and no server errors in 500 valid requests. A 30-minute concurrency-four soak must show no OOM or unbounded queue growth. Hardware feasibility may require a documented budget revision before measurements; these are not promises. Readiness within 10 minutes with already downloaded artifacts; restart recovery within 5 minutes is a provisional target.

Quality: fixed 100-item text-task set; quantized aggregate score must not drop more than 2 percentage points relative to baseline and no predefined critical regression may occur. Cache optimization target is >=20% median TTFT reduction on a deliberately repeated-prefix workload; no benefit is an acceptable honest finding, not a fabricated success.

## Non-goals
Training, general model marketplace, high availability, Kubernetes, multi-GPU scaling, public anonymous chat, billing, strong tenant isolation, audio/images, custom kernels and universal OpenAI API parity. Serving availability does not guarantee factual or safe answers.

## Approval choices
Hardware and budget; licensed model; acceptable latency after baseline; exact quantization experiment; permission for any later public demo. The default remains private, synthetic data only and no paid fallback.
