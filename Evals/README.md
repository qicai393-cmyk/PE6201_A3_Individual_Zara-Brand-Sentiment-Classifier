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
| Synthetic accuracy | 89.5% | 95.0% |
| Synthetic macro-F1 | 89.5% | 95.1% |
| Real accuracy | 83.3% | 83.3% |
| Real balanced accuracy | 80.7% | 81.3% |
| Real macro-F1 | 81.8% | 83.5% |
| Real abstention | 0.0% | 3.3% |
| Real keyword baseline | 66.7% | 66.7% |

Both models meet the targets: synthetic accuracy at least 85%, real accuracy at least 75%, and abstention at most 15%. DeepSeek-V3 is selected because it is stronger on synthetic data and slightly better on real equal-weight metrics. Thirty real examples are not sufficient to claim a decisive universal advantage.

## Outputs

Each model result folder contains synthetic and real classification logs, predicted labels, synthetic and real metric summaries, a confusion matrix, and Alert_Report.md. Both models produce a Yellow week-three logistics alert after a rise of at least 10 percentage points.

## Limitations

The principal errors are category overlaps: package damage may be logistics or quality, and competitor comparisons may also include quality dissatisfaction. Future evaluation should use a larger platform-balanced real set, a written annotation guide, a second annotator, and cost and latency measurements.
