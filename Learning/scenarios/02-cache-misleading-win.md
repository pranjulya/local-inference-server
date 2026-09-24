# The cache benchmark looks spectacular

## Situation
Repeated identical prompts show a large gain, but real unique prompts slow down.

## Diagnose before changing anything
Separate repeated-prefix and unique-prefix cohorts; check cache state, lengths and generator placement. Ensure TTFT excludes role-only events.

## Recommended response
Report both cohorts, retain cache only if the intended workload benefits within regression limits. Do not present repeated-prompt numbers as universal throughput.

## Success evidence
A reviewer can reconstruct workload mix and compare like-for-like trials.

## Reflection
Which symptom could mislead you? Which measurement distinguishes the competing explanations? What one control would prevent recurrence? Record actual evidence only when this drill is executed.
