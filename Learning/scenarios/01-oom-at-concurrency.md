# The model loads but concurrent requests OOM

## Situation
Four long requests arrive after a successful one-request smoke test.

## Diagnose before changing anything
Compare active/waiting requests, actual token lengths, peak VRAM and profile limits. Separate startup memory from KV growth. Verify ingress is enforcing the combined token and concurrency budget.

## Recommended response
Stop admission, restore baseline and lower the tested envelope if needed. Do not blindly increase GPU memory utilization; preserve runtime safety margin.

## Success evidence
Repeat boundary workload and 30-minute soak without OOM. Explain which resource limit changed and why.

## Reflection
Which symptom could mislead you? Which measurement distinguishes the competing explanations? What one control would prevent recurrence? Record actual evidence only when this drill is executed.
