# Benchmark integrity and cost

## Mental model
A cold start, cache-empty request and warm repeated-prefix request measure different things. Tail latency needs sufficient samples and a clear denominator. Tokens/sec must say input, output or total and whether failures count. Local inference has zero provider fees but nonzero compute economics.

## Exercise before implementation
Write a mock report with unknown results, not invented numbers. Include power/rental cost method, dataset hash, failure count and confidence limitation.

## Explain-back checkpoint
**How do you estimate local cost per request?**

Attribute measured or estimated compute and operating cost over the same interval to completed requests; state utilization, currency/time basis and missing costs. Do not label an unknown as zero.

## Evidence to capture later
One annotated diagram or measured example, the relevant profile/workload identifier, and one observation that would disprove your hypothesis. See the benchmark strategy for controls; do not convert this reading exercise into a performance claim.
