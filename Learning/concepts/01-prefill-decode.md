# Prefill, decode and continuous batching

## Mental model
Prefill processes the prompt; decode generates successive tokens using prior state. TTFT often reflects prefill and waiting, while inter-token latency reflects decode. Continuous batching allows work from different requests to enter and leave execution over time. Faster aggregate throughput can coexist with worse individual latency.

## Exercise before implementation
Sketch two requests of different output lengths sharing a scheduler. Label admission, wait, prefill, first token and completion. Explain why requests/sec alone hides long-output costs.

## Explain-back checkpoint
**Why can a saturated service have high throughput and poor UX?**

Queueing and batching can keep the GPU busy while delaying each user; measure latency distributions and successful goodput under a bound.

## Evidence to capture later
One annotated diagram or measured example, the relevant profile/workload identifier, and one observation that would disprove your hypothesis. See the benchmark strategy for controls; do not convert this reading exercise into a performance claim.
