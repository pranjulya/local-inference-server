# Worker dies during streaming

## Situation
A client has received 20 tokens when the worker exits.

## Diagnose before changing anything
Observe partial stream, terminal marker absence, readiness and restart metadata. Check whether any automatic retry duplicates content.

## Recommended response
Mark the client result incomplete, stop admission until ready, restart known-good profile and require the client to initiate any fresh request.

## Success evidence
No partial answer is counted as success; model readiness and smoke request precede reopening.

## Reflection
Which symptom could mislead you? Which measurement distinguishes the competing explanations? What one control would prevent recurrence? Record actual evidence only when this drill is executed.
