# YouTube Video Popularity Prediction (Metadata + Early Engagement)

Predicting YouTube video **view count** using publicly available **metadata** and **early engagement** signals (likes, comments, etc.).  
This project follows an end-to-end supervised machine-learning workflow aligned to an MSc Data Science report structure (University of Hertfordshire style): data preparation → EDA → feature engineering → modelling → evaluation → tuning → discussion.

---

## Project Aim
To assess **how accurately machine-learning models can predict video view counts** using features available at (or shortly after) upload, without relying on long-term view-history trajectories.

---

## Dataset
**Source:** Kaggle – *YouTube Statistics* dataset (`youtube_statistics.csv`). :contentReference[oaicite:0]{index=0}  
The dataset contains YouTube video-level records with:

- **Engagement metrics:** `views`, `likes`, `dislikes`, `comment_count`
- **Metadata:** `title`, `tags`, `category_id`, `channel_title`, etc.
- **Temporal fields:** `publish_time` (used to derive timing features)
- **Status indicators:** flags such as comments/ratings disabled and removed/error status (where present)

> Note: This dataset represents a **snapshot** of engagement at collection time (not a full growth time-series), which suits early-stage popularity modelling. :contentReference[oaicite:1]{index=1}

---

## Workflow (High-Level)
1. **Load and inspect** dataset
2. **Clean data**
   - Remove duplicates and missing rows
   - Keep only records with valid positive views (for ratio features)
3. **Exploratory Data Analysis (EDA)**
   - Views by publish hour
   - Correlation heatmap for numeric variables
   - Category vs average views (ID + readable names)
   - Views vs engagement ratio scatterplots :contentReference[oaicite:2]{index=2}
4. **Feature engineering**
5. **Train/test split** (80/20, `random_state=42`)
6. **Model training**
7. **Evaluation** using R², MAE, RMSE + plots
8. **Hyperparameter tuning (manual/empirical)** for tree/boosting models
9. **Compare models** and interpret results

---

## Feature Engineering (Used in Modelling)
The notebook builds structured predictors from raw metadata: :contentReference[oaicite:3]{index=3}

### Text-length and tag structure
- `title_length` = length of title text
- `tag_count` = number of tags (split by `|`)

### Publish-time derived features
From `publish_time`:
- `publish_hour`
- `publish_day` (day-of-week)
- `publish_month`

### Engagement ratios (quality signals)
- `like_ratio` = likes / views
- `dislike_ratio` = dislikes / views
- `comment_ratio` = comment_count / views

> Infinite values created by division are replaced with 0 to keep the feature matrix stable. :contentReference[oaicite:4]{index=4}

### Final feature set
```text
likes, dislikes, comment_count,
title_length, tag_count,
publish_hour, publish_day, publish_month,
like_ratio, dislike_ratio, comment_ratio
