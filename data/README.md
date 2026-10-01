# Data Documentation

## Files

| File | Records | Purpose |
|---|---:|---|
| synthetic_reviews.csv | 200 | Development data for prompt work, threshold calibration, and simulated alert testing. |
| real_eval.csv | 30 | Manually labelled public held-out evaluation data. |

## Schema

| Column | Meaning |
|---|---|
| comment_id | Unique record identifier. |
| comment_text | Content classified by the model. |
| platform | Source platform. |
| true_category | Manual label: quality complaint, logistics complaint, competitor comparison, price-sensitive, or other. |

Synthetic data also includes batch_id and source_type. Real data includes source_url and source_type.

## Categories

Quality complaint covers defects, material, durability, sizing, and construction. Logistics complaint covers delivery, tracking, parcel handling, fulfillment, and return shipping. Competitor comparison requires an explicit other-brand comparison. Price-sensitive covers affordability, discount, value, and price rises. Other covers praise, questions, and content outside these issue classes.

## Provenance and safeguards

The synthetic set is balanced at 40 comments per category. The real set contains 30 manually collected public comments from Xiaohongshu, TikTok, and Instagram, with category counts of 8, 6, 6, 5, and 5. No usernames, avatars, timestamps, order numbers, addresses, or other direct personal identifiers are stored. Source URLs are retained only for public-post provenance.

The real set is held out. Do not modify prompts, labels, or confidence thresholds after inspecting its predictions. Because it is small and modestly uneven, report macro-F1 and balanced accuracy alongside ordinary accuracy.
