# Production scenarios and recovery

| Scenario | Detection | Immediate action | Recovery proof |
|---|---|---|---|
| GPU OOM on long concurrent requests | OOM event, unavailable/readiness failure | stop admission; retain sanitized profile/error metadata | lower envelope or restore baseline; replay boundary and soak tests |
| Driver/image incompatibility | startup failure | do not route traffic; no endless restart | restore known-good digest/driver pair; readiness plus smoke inference |
| Warm TTFT improves but unique requests slow | split workload histogram | withhold cache promotion | compare matching token/concurrency runs; publish negative result |
| Quantized answer corrupts numbers | critical quality regression | keep baseline profile | investigate calibration/backend; rerun frozen suite before promotion |
| Client disconnect leaves generation running | active requests remain after deadline | bound duration; stop traffic if accumulating | disconnect drill shows resources released within 5 seconds |
| GPU host disappears | probe failure | return unavailable; no paid API fallback | restart on same approved host; load exact artifacts; contract test |
| Credentials appear in logs | canary scan/security report | stop affected logging and rotate key | redaction/omission tests and retained-data cleanup |
| Metrics store unavailable | scrape gaps/export drops | keep inference serving; bound buffers | restore sink; verify counters reveal missing telemetry |
| Burst exceeds limits | rejection/queue metrics | return capacity error, no infinite queue | sustained goodput returns after load drops |
| Disk fills during download | provisioning error | keep old profile; abort partial artifact | verify complete hash/revision before startup |

Rollback: record current candidate, drain ingress for up to 60 seconds, terminate remaining work with clear client failure, start previous verified profile, wait model-aware readiness, run a synthetic smoke request, then reopen ingress. On a single GPU this is a downtime operation. Measure elapsed recovery; do not advertise zero-downtime release. Recovery target is provisional five minutes after artifacts are local; update the budget if measured load time proves impossible.

Each drill records trigger, timestamps, observed client/engine behavior, response, recovery time and corrective decision. Run against synthetic local traffic only. No failure evidence exists at planning time.
