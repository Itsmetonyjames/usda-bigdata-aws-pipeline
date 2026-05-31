# USDA Food Nutrition — Big Data Analytics on AWS

A fully serverless AWS big data pipeline to ingest, process, and analyse 2.68 GB of USDA nutritional data across 350,000+ branded food items — with K-Means clustering and Linear Regression for nutritional insight.

---

## Business Problem

A multinational food company needs scalable infrastructure to analyse large-scale nutrition data to support product development, regulatory compliance, and market segmentation — at minimal cost.

---

## AWS Architecture

```
USDA Dataset (2.68 GB)
        │
        ▼
Amazon S3 (raw-data/)          ← S3 Intelligent-Tiering, versioning, encryption
        │
        ▼
AWS Glue Crawler               ← Auto-schema discovery → Data Catalogue
        │
        ▼
AWS Glue ETL (Apache Spark)    ← Clean, join, transform → Parquet output
        │
        ▼
Amazon S3 (processed-data/)    ← 87% smaller than CSV, Snappy compressed
        │
        ▼
Amazon Athena                  ← Serverless SQL queries ($5/TB scanned)
        │
        ▼
Amazon CloudWatch              ← Performance monitoring, budget alerts, audit logs
```

---

## Dataset

| File | Size | Content |
|---|---|---|
| `branded_food.csv` | 906.8 MB | Brand names, serving sizes, ingredients |
| `food_nutrient.csv` | 1.4 GB | Nutrient values per food item |
| `food.csv` | 357.3 MB | Master food item list |
| `nutrient.csv` | 21.1 KB | Nutrient reference data |
| **Total** | **2.68 GB** | 350,000+ branded food items |

**Source:** USDA FoodData Central — CC0 1.0 Universal Public Domain licence

---

## ETL Pipeline (AWS Glue + Apache Spark)

Six preprocessing operations applied at scale:

| Operation | Method | Purpose |
|---|---|---|
| Remove nulls | `dropna()` | Data completeness |
| Remove duplicates | `dropDuplicates()` | Eliminate skewed records |
| Standardise values | `round(amount, 2)` | Numerical consistency |
| Filter invalid records | `filter(serving_size > 0)` | Remove invalid data |
| Join datasets | Inner join on `fdc_id` | Unified dataset |
| Convert format | `write.parquet()` | 87% query cost reduction |

**ETL performance:** 1.53 minutes using 10 DPUs (Apache Spark)

---

## SQL Analysis (Amazon Athena — 5 Queries)

| Query | Finding |
|---|---|
| Top calorie categories | Poultry and snacks had highest average calories |
| Macronutrients by category | Significant protein/fat variation across categories |
| Highest protein foods | Seafood and poultry dominated |
| Sugar by category | Beverages and cereals showed highest sugar levels |
| Nutrient density score | Seafood scored highest overall |

---

## ML Analysis (Python — Google Colab)

### Model 1 — K-Means Clustering (k=4)

Four food segments identified:

| Cluster | Characteristics |
|---|---|
| 🔴 High Calorie | Dense, energy-rich processed foods |
| 🔵 High Protein | Seafood, poultry, meat-based categories |
| 🟡 High Sugar | Beverages, cereals, confectionery |
| 🟢 Low Fat / Healthy | Vegetables, fruits, light snacks |

### Model 2 — Linear Regression (Calorie Prediction)

| Metric | Result |
|---|---|
| R² Score | **0.9663** — 96.63% of calorie variance explained |
| RMSE | **2.84 kcal** — extremely precise predictions |
| Top predictor | Fat content (highest coefficient) |

---

## Cost Optimisation

| Strategy | Saving |
|---|---|
| S3 Intelligent-Tiering | Up to 40% vs S3 Standard |
| Parquet format | 87% reduction in Athena scan costs |
| S3 Lifecycle Policy | Automatic tier migration (Standard → IA → Glacier) |
| **Total monthly cost** | **$1.18 USD** (full pipeline) |

---

## Security & Compliance

- **Encryption:** Server-side encryption with AWS-managed keys (SSE-S3)
- **Access control:** IAM least-privilege policy (read-only analyst role)
- **Audit:** AWS CloudTrail logging all management events
- **Data residency:** EU (London) eu-west-2 region — UK GDPR compliant
- **Public access:** S3 Block Public Access fully enabled

---

## Files

| File | Description |
|---|---|
| `USDA_BigData_AWS_Portfolio.ipynb` | ML analysis notebook (K-Means + Linear Regression) |
| `food_macro_data.csv` | Aggregated macronutrient data exported from Athena |
| `BigData_Report_Clean.pdf` | Full project report (AWS implementation + analysis) |

---

## How to Run the Notebook

```bash
pip install pandas numpy scikit-learn matplotlib seaborn
jupyter notebook USDA_BigData_AWS_Portfolio.ipynb
```

---

## AWS Services Used

`Amazon S3` `AWS Glue` `Apache Spark` `Amazon Athena` `Amazon CloudWatch` `AWS IAM` `AWS CloudTrail` `AWS Config` `AWS Cost Explorer`

## Local Tools Used

`Python` `Pandas` `NumPy` `Scikit-learn` `Matplotlib` `Seaborn`

---

*Tony James — MSc Data Science, York St John University, London*  
[LinkedIn](https://linkedin.com/in/tony-james-4bba63195) | [GitHub](https://github.com/itsmetonyjames)
