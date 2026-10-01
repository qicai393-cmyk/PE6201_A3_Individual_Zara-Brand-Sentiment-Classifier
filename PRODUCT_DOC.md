# Product Documentation — BrandSentry

## Persona
Mei, social media operations manager at a mid-sized electronics brand.
Checks overnight comments at 9:00 AM, needs red/yellow/green alerts.

## Input
- comment_text: string
- platform: Xiaohongshu / Douyin / Instagram

## Output
- predicted_category: one of 5 risk categories
- confidence: 0–1
- alert_briefing: Markdown with batch alerts (Green/Yellow/Red)

## Architecture (box diagram)
[Input CSV] → [DeepSeek API Classifier] → [Confidence Threshold] → [Aggregator] → [Alert Rules] → [Alert_Report.md]
                                      ↓
                              [classification_log.csv]

## Metrics Targeted vs Reached
| Metric | Target | Reached |
| :--- | :--- | :--- |
| Synthetic accuracy | ≥85% | 94.92% |
| Real accuracy | ≥75% | 83.33% |
| Abstention rate | ≤15% | 1.5% / 0.0% |
| Keyword baseline | — | 76.0% / 66.7% |
