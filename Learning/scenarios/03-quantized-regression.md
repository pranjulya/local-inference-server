# Memory improves but quality regresses

## Situation
Quantized profile changes numeric answers in grounded summaries.

## Diagnose before changing anything
Confirm model/tokenizer equivalence, sampling, scaling and supported backend. Review predefined critical cases before adjusting thresholds.

## Recommended response
Keep baseline serving. Investigate candidate; do not rewrite evaluation labels to rescue its score.

## Success evidence
Paired held-out quality evidence passes or the candidate is explicitly rejected.

## Reflection
Which symptom could mislead you? Which measurement distinguishes the competing explanations? What one control would prevent recurrence? Record actual evidence only when this drill is executed.
