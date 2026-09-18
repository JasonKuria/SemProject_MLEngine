# SemProject_MLEngine

**ICS 3202: Artificial Intelligence — Semester Project, Deliverable 1**
BSc. Informatics & Computer Science, Strathmore University
group members
Jason Kuria 189946
John Kioko 189983
Michelle Wachanga 176833

Machine-learning engine for **MloHub — An Affordable Multi-Tenant Mobile Food Ordering and Business Intelligence Platform for Small Food Vendors**.

---

## 1. The project in one paragraph

Small restaurants, kiosks and home-based cooks in Kenya advertise on TikTok, Instagram and WhatsApp, but customers still have to call or message to ask what is available. Existing delivery marketplaces charge commissions that cut into already-thin margins. **MloHub** is a single multi-tenant platform giving every vendor its own digital storefront (menu, photos, availability, ordering, delivery, tracking, analytics) on a low flat subscription instead of per-sale commission. Its ML component is a **food-demand prediction model** that tells a vendor how many portions of each meal to prepare for an upcoming time window, reducing both wastage and stock-outs.

## 2. What this deliverable covers

| Instruction | Where it is answered |
|---|---|
| Discover all relevant open-source datasets | Notebook, Section 1 — 13 datasets surveyed |
| Identify the expected output variable for each dataset | Notebook, Section 1 — final column of the survey table |
| Select the most appropriate dataset | Notebook, Section 2 — with justification and a column-to-schema mapping |
| (a) Rows and columns *(3 marks)* | Notebook, Section 4 (a) |
| (b) Datatypes *(3 marks)* | Notebook, Section 4 (b) |
| (c) Completeness / missing values *(2 marks)* | Notebook, Section 4 (c) |
| (d) `df_sample` = first 15 rows + last 20 rows *(4 marks)* | Notebook, Section 4 (d) |

## 3. Selected dataset

**Food Demand Forecasting** — Genpact & Analytics Vidhya Machine Learning Hackathon (2018).

| File | Rows | Columns | Contents |
|---|---|---|---|
| `train_file.csv` | 456,548 | 9 | Weekly demand per fulfilment centre × meal |
| `meal_info.csv` | 51 | 3 | Meal category and cuisine |
| `fulfilment_center_info.csv` | 77 | 5 | Centre city, region, type, operating area |
| **merged `df`** | **456,548** | **15** | The analysis dataset explored in the notebook |

**Expected output variable: `num_orders`** — the number of orders (portions) placed for a meal at a centre in a week. This is the direct analogue of the quantity MloHub must predict per menu item, per vendor, per time window.

**Mirrors:** Kaggle `kannanaikkal/food-demand-forecasting` and `ghoshsaptarshi/av-genpact-hack-dec2018`. The notebook loads the CSVs from a public GitHub mirror, so **no Kaggle account or API token is needed**.

## 4. Headline findings

- **456,548 rows × 15 columns** after joining the two lookup tables onto the demand table.
- **Three datatypes:** 9 integer columns, 3 float columns, 3 text (object) columns.
- **The dataset is complete** — 0 missing cells out of 6,848,220, 0 duplicate rows, and full referential integrity between the demand table and both lookups.
- `df_sample` (first 15 + last 20 rows) = **35 rows × 15 columns**, spanning weeks 1 and 145.
- Meals promoted by both emailer and homepage feature average **≈3.9×** the orders of unpromoted meals — evidence that the promotion features carry real predictive signal for MloHub's "featured meal" slot.

## 5. Running the notebook

### Google Colab (recommended)
1. Open <https://colab.research.google.com> → **GitHub** tab → paste this repository's URL.
2. Open `FoodDemandForecasting_Exploration.ipynb`.
3. **Runtime → Run all.** The data downloads automatically; runtime is about a minute.

### Locally
```bash
pip install pandas numpy matplotlib jupyter
jupyter notebook FoodDemandForecasting_Exploration.ipynb
```

No credentials, no manual downloads. If you prefer to work offline, drop the three CSVs into a `data/` folder beside the notebook and they will be picked up automatically.

## 6. Repository structure

```
SemProject_MLEngine/
├── FoodDemandForecasting_Exploration.ipynb   # Deliverable 1 notebook (outputs included)
└── README.md
```
