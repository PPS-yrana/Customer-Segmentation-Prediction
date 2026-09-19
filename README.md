# Customer Segmentation & Prediction

Segments customers using unsupervised clustering, then trains a dedicated churn-prediction
model for each segment — because different types of customers churn for different reasons,
and a single one-size-fits-all model can miss that.

**Course context:** Week 11 — Advanced Machine Learning Models

---

## Overview

- Clusters 500 customers into 3 segments using **K-Means** (primary), cross-checked against
  **Hierarchical clustering** and **DBSCAN**
- Trains a separate, **hyperparameter-tuned Random Forest** churn model per segment
- Evaluates each model with accuracy, precision, recall, F1, and ROC-AUC
- Translates the results into segment-specific business recommendations and a revenue-impact estimate

## Results at a Glance

| Segment | Customers | Avg Monthly Charges | Churn Rate | Defining Trait |
|---|---|---|---|---|
| **Premium Loyalists** | 232 | $122.87 | 10.0% | Highest spend; 45% still month-to-month |
| **Budget Regulars** | 124 | $107.28 | **15.0%** | 100% Credit Card payment; highest churn |
| **Standard Newcomers** | 144 | $103.56 | **7.0%** | 100% Two-year contracts; lowest churn |

**Key finding:** contract length tracks with churn — the segment with universal Two-year
contracts churns least, even with the shortest average tenure.

| Segment | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| Premium Loyalists | 0.948 | 0.667 | 1.000 | 0.800 | 1.000 |
| Budget Regulars | 0.968 | 1.000 | 0.800 | 0.889 | 0.992 |
| Standard Newcomers | 0.944 | 0.500 | 1.000 | 0.667 | 0.941 |

Full write-up: [`segment_profiles.md`](segment_profiles.md) ·
[`Project_Documentation.docx`](Project_Documentation.docx) ·
[`business_recommendations.pdf`](business_recommendations.pdf)

## Repository Structure

```
customer_segmentation.ipynb      # Full analysis notebook — run this
customer_churn.csv               # Source dataset (500 rows)
segmentation_data.csv            # Output: original data + assigned segment per customer
segment_profiles.md              # Output: detailed per-segment profile write-up
model_evaluation_results.csv     # Output: evaluation metrics per segment's model
business_recommendations.pdf     # Output: business-facing recommendations report
Project_Documentation.docx       # Full project documentation
requirements.txt                 # Python dependencies
```

## Setup

```bash
# 1. Clone the repository
git clone <repository-url>
cd customer-segmentation-prediction

# 2. Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch Jupyter and run all cells
jupyter notebook customer_segmentation.ipynb
```

No API keys or external services required — everything runs locally against the CSV file.
`RANDOM_STATE = 42` is fixed throughout, so results are identical on every re-run.

## Pipeline

```
Raw CSV
  -> Encode categoricals + scale numerics
  -> Cluster (K-Means / Hierarchical / DBSCAN)
  -> Select K-Means as primary segmentation (highest silhouette score)
  -> Profile + name each segment
  -> Per segment: train/test split (stratified) -> Grid Search Random Forest -> evaluate
  -> Aggregate results + estimate business impact
  -> Export CSV / MD / PDF deliverables
```

## Tech Stack

Python 3.11 · pandas · numpy · scikit-learn · matplotlib · seaborn · scipy · Jupyter

## Limitations

- Only 53 churned customers total (10.6% churn rate), so per-segment test-set metrics —
  especially precision and F1 on the smaller segments — are directional, not statistically robust.
- Segments reflect a single snapshot; periodic re-clustering would confirm they stay stable as customers age.
- The contract-length/churn relationship is observational — A/B testing is recommended before scaling retention spend against it.

See [`Project_Documentation.docx`](Project_Documentation.docx) for the full methodology,
architecture notes, visual documentation, and testing evidence.
