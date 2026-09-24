# Weights, KV memory and capacity

## Mental model
Weights occupy memory largely independent of current token count. KV state grows with cached sequence content and model attention structure; runtime workspaces also consume memory. Prefix reuse avoids repeated prefill work when eligible prefixes match. It does not remove the need to generate new output tokens. Quantizing weights does not automatically change KV dtype.

## Exercise before implementation
Use the LLD approximate KV formula with a hypothetical model; list missing allocator/runtime terms. Explain why four 4k-token requests may fail although the model loads successfully.

## Explain-back checkpoint
**Does a model that fits in VRAM guarantee the declared concurrency?**

No. Loading proves weight/runtime fit, not peak concurrent KV and workspace fit; measure the complete envelope.

## Evidence to capture later
One annotated diagram or measured example, the relevant profile/workload identifier, and one observation that would disprove your hypothesis. See the benchmark strategy for controls; do not convert this reading exercise into a performance claim.
