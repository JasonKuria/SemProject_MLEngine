# SemProject_MLEngine

**ICS 3202: Artificial Intelligence · Semester Project** · BSc. Informatics & Computer Science, Strathmore University

**Group 18:** Jason Kuria Wainaina (189946) · John Kioko Musyimi (189983) · Michelle Wachanga Wangui (176833)

Machine-learning engine for **MloHub**, an affordable multi-tenant mobile food ordering and business-intelligence platform for small food vendors. The engine is a **Random Forest demand forecaster**: for a chosen dish and day it estimates how many portions customers will buy, so a vendor can prepare enough without waste.

## Deliverables in this repository

| Deliverable | File | Contents |
|---|---|---|
| 1 · Dataset discovery & exploration | [`FoodDemandForecasting_Exploration.ipynb`](FoodDemandForecasting_Exploration.ipynb) | 13 datasets surveyed; Genpact Food Demand Forecasting selected (456,548 rows, target `num_orders`); rows/columns, datatypes, completeness, `df_sample` |
| 2 · Base model (CAT 2) | [`RandomForest_Base.ipynb`](RandomForest_Base.ipynb) | Data cleaning with a quarantine and reconciliation, 7 EDA observations, leak-free features, date-based cross-validation, regularisation search, comparison with Ridge / Decision Tree / Gradient Boosting / simple baselines, rolling backtest |
| 3 · **Final ML engine** | [`RandomForest_Final.ipynb`](RandomForest_Final.ipynb) | The engine exactly as the MloHub app runs it (all CRISP-DM stages): data checks, EDA, features with a leakage test, Random Forest theory, rolling-origin evaluation against 4 simple methods, input validation (valid and invalid inputs), **explainable AI** (why each forecast came out as it did), and a check that the notebook's model is identical to the app's |

## Final engine at a glance (Deliverable 3)

* **Data:** 60 days × 21 dishes of *Campus Bites* sales from the MloHub database (3,666 portions; demonstration data, not real customers), in [`data/campus_bites_daily_sales.csv`](data/campus_bites_daily_sales.csv).
* **Inputs (features):** which dish, sales yesterday, sales on the same weekday last week, 7-day average, the restaurant's total yesterday, day of week, weekend flag. All are computed only from days *before* the forecast day.
* **Model:** `RandomForestRegressor(n_estimators=120, max_depth=8, min_samples_leaf=5, random_state=2026)`.
* **Evaluation:** 672 out-of-sample forecasts (32 days, retrained weekly). MAE: Random Forest **2.16** portions vs seven-day average **2.05**, restaurant total × share 2.08, same-weekday average 2.25, same as yesterday 2.44. On this short, noisy history the forest is close to the best simple rule but does not beat it, which is why the app always shows both and labels which method produced each figure.
* **Explainability:** each forecast is split into a starting point plus one contribution per clue (tree-path decomposition), with a certainty range from the spread of the 120 trees.
* **Consistency with the app:** the notebook's forest reproduces the app's 508 stored reference predictions exactly (difference 0.0).

## Running the notebooks

**Google Colab:** open <https://colab.research.google.com> → *GitHub* tab → paste this repository's URL → open a notebook → *Runtime → Run all*. Data files are downloaded from this repository automatically. No accounts or keys are needed.

**Locally:**

```bash
pip install pandas numpy scikit-learn matplotlib joblib jupyter
jupyter notebook RandomForest_Final.ipynb
```

`RandomForest_Final.ipynb` runs in about a minute. Results are identical with scikit-learn 1.9.1, the version the app was trained with. Another version may change the forest slightly, and the notebook will say so.

## Repository structure

```
SemProject_MLEngine/
├── FoodDemandForecasting_Exploration.ipynb   # Deliverable 1
├── RandomForest_Base.ipynb                   # Deliverable 2: base model
├── RandomForest_Final.ipynb                  # Deliverable 3: final engine
├── data/
│   ├── campus_bites_daily_sales.csv          # training/evaluation data for the final engine
│   └── app_reference_cases.json              # 508 predictions stored by the MloHub app (consistency check)
├── outputs/                                  # figures and model manifest written by the final notebook
└── image.png                                 # group working session
```
