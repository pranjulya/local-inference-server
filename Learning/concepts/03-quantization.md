# Quantization and quality evidence

## Mental model
A lower precision representation may reduce storage/memory, but kernel support, scaling/calibration, architecture and GPU determine behavior. Weight quantization and KV quantization affect different tensors. A smaller artifact does not guarantee lower TTFT or unchanged factual output.

## Exercise before implementation
Design a paired comparison that changes only weight representation, keeps prompts and tokenizer fixed, and reports quality plus memory. Name what makes a comparison invalid.

## Explain-back checkpoint
**Why can quantization be slower?**

A supported representation may introduce conversion or kernel overhead and the workload may be bottlenecked elsewhere; only matched measurements support a speed claim.

## Evidence to capture later
One annotated diagram or measured example, the relevant profile/workload identifier, and one observation that would disprove your hypothesis. See the benchmark strategy for controls; do not convert this reading exercise into a performance claim.
