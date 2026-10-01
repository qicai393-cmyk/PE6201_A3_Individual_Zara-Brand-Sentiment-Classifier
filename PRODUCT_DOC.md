# Product Documentation: Zara Brand Sentiment Classifier

## Product purpose

The Zara Brand Sentiment Classifier converts public social-media comments into operational signals for brand monitoring. It is a decision-support tool for prioritising review, not an autonomous crisis-management system.

## Persona

**Mei, Social Media Operations Manager**

Mei monitors Zara-related comments every morning across Xiaohongshu, TikTok, and Instagram. She understands brand operations and basic sentiment-analysis concepts but does not write Python or build machine-learning models. She needs a quick answer to three questions:

1. What type of issue is each comment reporting?
2. Which issue type is increasing compared with the previous batch?
3. Which team should review the signal first?

Mei uses the output to route quality concerns to product teams, logistics concerns to fulfilment teams, price-sensitive feedback to commercial teams, and uncertain cases to a human reviewer.

## Inputs

| Input | Format | Description |
|---|---|---|
| Synthetic development data | data/synthetic_reviews.csv | 200 labelled Zara comments, balanced at 40 comments per category. Used for development, calibration, and alert simulation. |
| Real held-out evaluation data | data/real_eval.csv | 30 manually labelled public comments from Xiaohongshu, TikTok, and Instagram. Used only for final evaluation. |
| API key | Environment variable or Colab secret | Authenticates the selected LLM provider. Never commit this value to GitHub. |
| Classification prompt | Notebook system prompt | Defines the five permitted categories and requires one JSON prediction with confidence. |

Each comment input includes comment_id, comment_text, platform, and true_category when the dataset is evaluated. Synthetic data also includes batch_id.

## Outputs

| Output | Description |
|---|---|
| classification_log_synthetic.csv | Audit log for the synthetic model run. |
| classification_log_real.csv | Audit log for the real held-out model run. |
| predicted_labels.csv | Synthetic predictions, confidence, abstention decision, and keyword-baseline label. |
| eval_results.csv | Synthetic evaluation metrics. |
| real_eval_predictions.csv | Real held-out predictions, confidence, and abstention decision. |
| real_eval_results.csv | Real held-out evaluation metrics. |
| confusion_matrix.png | Visual summary of synthetic prediction errors. |
| Alert_Report.md | Batch-level category summary and Green or Yellow alert. |

The final operational result is a category, a confidence score, an optional uncertain status, and a batch alert. No personal-user decision is made.

## High-level product architecture

    Public social comments
            |
            v
    CSV input and validation
      - required columns
      - valid five-class labels
      - no blank or duplicate text
            |
            v
    Deduplication and run cap
      - first instance kept
      - maximum 250 comments per run
            |
            v
    External intelligence: LLM API
      - Qwen 2.5 72B or DeepSeek-V3
      - fixed structured prompt
      - output: category and confidence
            |
            v
    Confidence handling
      - below calibrated threshold: uncertain
            |
            +------------------------------+
            |                              |
            v                              v
    Classification audit log       Deterministic evaluation
                                      - accuracy
                                      - macro-F1
                                      - balanced accuracy
                                      - keyword baseline
            |                              |
            +--------------+---------------+
                           |
                           v
    Batch aggregation and alert logic
      - category percentages
      - Yellow if category rises by 10+ points
      - Yellow if uncertainty exceeds 20%
      - Yellow if other exceeds 30%
                           |
                           v
    Mei's review and team routing

## Code logic and external intelligence

The LLM is used only for semantic classification. It receives one comment at a time and must return one of five labels plus a confidence score. Temperature is set to zero to reduce output variation. The selected model for this prototype is DeepSeek-V3, after comparison with Qwen 2.5 72B Instruct.

The following logic is deterministic Python and does not rely on the LLM:

- Dataset validation checks required columns, empty comments, invalid labels, and duplicate text.
- Deduplication retains only the first occurrence of identical text.
- The pipeline limits a run to 250 comments.
- The threshold is calibrated on a stratified 20-record synthetic subset, with four records per class.
- A keyword classifier provides a non-AI baseline.
- Metrics are calculated from predicted and manual labels.
- Batch alerts use fixed percentage thresholds.
- Predictions are written to CSV files for review and audit.

The real evaluation set is held out: it must not be used to edit the prompt or select the confidence threshold.

## Categories and routing

| Category | Meaning | First operational owner |
|---|---|---|
| quality_complaint | Defect, material, durability, sizing, or construction issue | Product or quality team |
| logistics_complaint | Delivery, tracking, parcel, fulfilment, or return-shipping issue | Logistics or customer operations |
| competitor_comparison | Explicit comparison with another brand | Brand or market-insights team |
| price_sensitive | Price, affordability, discount, coupon, or value concern | Commercial or pricing team |
| other | Praise, questions, style discussion, or content outside issue classes | Social-media operations review |
| uncertain | Low-confidence model prediction | Human reviewer |

## Target metrics

| Metric | Target |
|---|---:|
| Synthetic forced-choice accuracy | At least 85% |
| Real held-out forced-choice accuracy | At least 75% |
| Abstention rate | At most 15% |
| Alert precision in simulated scenario | No false positive |

Balanced accuracy and macro-F1 are also reported because the real set is modestly class-imbalanced. They prevent a larger class from dominating the performance interpretation.

## Metrics reached

| Dataset and metric | Qwen 2.5 72B | DeepSeek-V3 selected model |
|---|---:|---:|
| Synthetic forced-choice accuracy | 89.5% | 95.0% |
| Synthetic macro-F1 | 89.5% | 95.1% |
| Synthetic abstention rate | 0.0% | 2.0% |
| Real forced-choice accuracy | 83.3% | 83.3% |
| Real balanced accuracy | 80.7% | 81.3% |
| Real macro-F1 | 81.8% | 83.5% |
| Real abstention rate | 0.0% | 3.3% |
| Real keyword-baseline accuracy | 66.7% | 66.7% |

Both models met the project targets. DeepSeek-V3 was selected because it was stronger on synthetic data and slightly stronger on class-balanced real metrics. The real set has only 30 comments, so the observed difference is not large enough to claim a definitive universal model advantage.

## Limitations and safe use

This prototype supports triage rather than final decision-making. Thirty real comments provide a useful pilot test but not a production-quality benchmark. Packaging damage and competitor comparisons can overlap with quality concerns, so a written annotation-priority guide is needed. The next version should use more independently labelled, platform-balanced real comments and measure model cost, latency, and alert precision on genuine weekly data.
