# Evaluation Documentation

## Protocol

1. Develop and calibrate using the 200-record synthetic set.
2. Use a stratified 20-record calibration slice: four records per category.
3. Scan thresholds from 0.3 to 0.9 and select the highest non-abstained accuracy subject to abstention at or below 15%.
4. Freeze prompt and threshold.
5. Run the 30-record real set once as a held-out evaluation.
6. Compare against a transparent keyword baseline and inspect error rows manually.

## Metrics

Forced-choice accuracy is the raw percentage correct. Non-abstained accuracy excludes comments sent to human review. Abstention rate is the share marked uncertain. Balanced accuracy is the average recall across five classes. Macro-F1 is the average F1 across five classes. Both equal-weight metrics reduce the effect of modest class imbalance.

## Model comparison

| Metric | Qwen 2.5 72B | DeepSeek-V3 |
|---|---:|---:|
| Synthetic forced-choice accuracy | 89.50% | 95.00% |
| Synthetic balanced accuracy | 89.50% | 95.00% |
| Synthetic macro-F1 | 89.51% | 95.03% |
| Synthetic abstention | 0.00% | 1.50% |
| Real forced-choice accuracy | 83.33% | 83.33% |
| Real non-abstained accuracy | 83.33% | 83.33% |
| Real balanced accuracy | 80.67% | 81.33% |
| Real macro-F1 | 81.85% | 83.47% |
| Real abstention | 0.00% | 0.00% |
| Real keyword baseline | 66.67% | 66.67% |

Both models meet the project targets: synthetic accuracy at least 85%, real accuracy at least 75%, and abstention no higher than 15%. DeepSeek-V3 is selected because it has a 5.5-point synthetic accuracy advantage and slightly stronger equal-weight real metrics. Both models correctly classify 25 of 30 real records, so this is a practical selection rather than a conclusive universal performance claim.

## Error and alert interpretation

DeepSeek makes 10 synthetic raw-label errors: five logistics comments are predicted as quality, while five other comments are predicted as quality or logistics. Its real set has five raw-label errors: one competitor comparison and one logistics complaint are predicted as quality, two other comments are predicted as quality, and one price-sensitive comment is predicted as other.

Both model runs produce Green alerts in weeks one and two and a Yellow alert in week three because logistics complaints rise by at least ten percentage points. This validates the deterministic alert rule on the planned simulation. It does not yet measure real-world alert precision.

## Outputs

Each model result folder contains synthetic and real classification logs, predicted labels, synthetic and real metric summaries, a confusion matrix, and Alert_Report.md. Both models produce a Yellow week-three logistics alert after a rise of at least 10 percentage points.

## Limitations

The principal errors are category overlaps: package damage may be logistics or quality, and competitor comparisons may also include quality dissatisfaction. Future evaluation should use a larger platform-balanced real set, a written annotation guide, a second annotator, and cost and latency measurements.
