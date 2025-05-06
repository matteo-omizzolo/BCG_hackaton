# Customer Churn Prediction Challenge

> End‑to‑end analytics and modelling pipeline for predicting churn, understanding its drivers and estimating the €‑impact of retention strategies.

&#x20;

## Problem Statement

Telecom providers lose a significant share of revenue when dissatisfied clients decide to leave (“churn”).
This repository contains our solution to **BCG’s Customer‑Churn Hackathon**: given two raw Excel files with 7 043 customers and their complaint history, build a model that flags customers at risk of churning and quantify the financial upside of saving them.

* `clients.xlsx` – demographic & subscription attributes
* `complaints.xlsx` – free‑text complaints, one row per ticket

## Repository layout

```
├── code/
│   ├── EDA.ipynb          # data exploration & cleansing
│   ├── Logistic.ipynb     # baseline model + business‑friendly explainability
│   ├── XGBoost.ipynb      # gradient‑boosting, class‑imbalance pipeline & € impact
│   └── BCG_text.ipynb     # NLP: topic / emotion mining on complaint text
├── data                   # data used for the challenge
├── BCG_presentation.pptx  # presentation of results
└── README.md              # you are here 🚀
```

## Quick start

```bash
# 1 ‑ clone
git clone https://github.com/<your‑handle>/<repo‑name>.git
cd <repo‑name>

# 2 ‑ create environment
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt        # ~5 min

# 3 ‑ reproduce results
jupyter lab
# run notebooks in the order shown above – every notebook is seeded for reproducibility
```

> **Tip:** large plots are saved to `figures/` automatically; set `SAVE_FIG=False` in `src/config.py` if you prefer inline only.

## Results

| Model                  | Accuracy  | Recall    | Precision | F1‑score | ROC‑AUC   |
| ---------------------- | --------- | --------- | --------- | -------- | --------- |
| Logistic Regression    | **0.746** | 0.821     | 0.513     | 0.632    | 0.846     |
| XGBoost (best 50 iter) | 0.740     | **0.842** | 0.506     | 0.633    | **0.853** |

*The XGBoost pipeline slightly improves AUC and recall – the two metrics the business cares about – while keeping precision on par with the baseline.*

## Business impact

* Retaining the **churneers actually predicted by the model** yields an estimated **€ 560k** additional recurring revenue over the next 37 months of which 41% in the first 12 months (see `XGBoost.ipynb → Cost analysis`).
* Text‑mining highlights *price*, *billing issues* and *internet issues* as the complaint topics most associated with churn.
  Marketing can prioritise these pain points in retention campaigns.

## How it works

1. **EDA & cleaning**
   Missing `TotalCharges` values are imputed from `MonthlyCharges × tenure`; categorical features are one‑hot encoded.
2. **Modelling**

   * **Baseline:** logistic regression with scaled numerics, *SelectKBest* and class‑weight tuning.
   * **Advanced:** XGBoost inside an imbalanced‑learn pipeline + grid hyper‑parameter search with cross-validation and log-loss function.
3. **Explainability**

   * Feature importance (SHAP & log‑odds)
   * Partial Dependence & demographic slice plots
4. **Text analytics**
   BERT ➜ PCA ➜ K‑Means to group complaints, LDA topics & NRC emotion lexicon to surface dominant feelings.

## Contributing

Pull requests are welcome! Please open an issue first to discuss major changes.

## Author

Giacomo Negri – [@GiacomoNegri](https://github.com/GiacomoNegri)
Jacopo D'Angelo – [@jacopomattia03](https://github.com/jacopomattia03)
Matteo Omizzolo – [@matteo-omizzolo](https://github.com/matteo-omizzolo)
Ioannis Thomopoulos – [@ioannis0909](https://github.com/ioannis0909)
- feel free to reach out for questions or collaboration.
