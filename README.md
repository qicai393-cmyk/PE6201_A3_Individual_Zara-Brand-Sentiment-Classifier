# BrandSentimentClassifier — Multi-dimensional Sentiment Alert System

## Overview
Classifies brand social comments into 5 risk categories and generates a prioritized alert briefing with shift detection.

## Results
| Dataset | N | Accuracy | Abstention | Keyword Baseline |
| :--- | :--- | :--- | :--- | :--- |
| Synthetic | 200 | 93.9% | 1.5% | 81.5% |
| Real held-out | 30 | 80.0% | 0.0% | 50.0% |

## Files
- `BrandSentry_Project.ipynb` — full runnable pipeline
- `data/synthetic_reviews.csv` — 200 AI-generated comments with ground truth
- `data/real_eval.csv` — 30 hand-labeled real comments (held out)
- `output/Alert_Report.md` — final briefing with batch alerts
- `output/eval_results.csv` — evaluation metrics

## How to run
1. Upload `data/` files to Google Drive under `MyDrive/zara/`
2. Open the notebook in Colab
3. Set `OPENROUTER_API_KEY` in Colab Secrets
4. Run all cells in order

## Limitations
- Threshold is 0.3, calibrated on a 20-comment development slice
- 3 synthetic comments hit HTTP 429 rate limits and were logged as abstentions
- Main confusion on real data: `price_sensitive` vs `quality_complaint`
