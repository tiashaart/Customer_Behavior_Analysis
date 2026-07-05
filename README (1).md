# 🛍️ Customer Shopping Behavior Analysis

An end-to-end data analytics project analyzing retail customer shopping behavior — from raw data to a business-ready dashboard.

**Tech Stack:** Python (Pandas) · PostgreSQL (SQL) · Power BI

---

## 📖 Overview

Retail businesses generate large volumes of data on what customers buy, how much they spend, how often they return, and how they respond to discounts. This project analyzes a retail customer shopping dataset to uncover purchasing patterns, customer segments, and revenue drivers — covering the full analytics workflow: **data cleaning → feature engineering → SQL analysis → dashboard reporting.**

## 🎯 Objectives

- Identify which customer segments generate the most revenue
- Determine whether discounts and subscriptions actually influence spending
- Find which products and categories perform best
- Segment customers by loyalty (New, Returning, Loyal)
- Surface insights to support marketing and inventory decisions

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **Python (Pandas, Jupyter Notebook)** | Data cleaning, preprocessing, feature engineering |
| **PostgreSQL** | Storing cleaned data and running business queries |
| **SQL** | Aggregation, window functions, CTEs to answer business questions |
| **SQLAlchemy / psycopg2** | Connecting Python to PostgreSQL and loading data |
| **Power BI** | Interactive dashboard for visual insights |

## 📊 Dataset

Retail customer shopping dataset with **3,900 records** and **18 columns**, covering demographics, purchase details, and shopping preferences.

| Column(s) | Description |
|---|---|
| Customer ID | Unique identifier per customer |
| Age, Gender | Customer demographics |
| Item Purchased, Category | Product bought and its category |
| Purchase Amount (USD) | Transaction value |
| Location, Size, Color, Season | Purchase context and product attributes |
| Review Rating | Product rating (1–5) |
| Subscription Status | Whether the customer is a subscriber |
| Shipping Type | Standard, Express, Free Shipping, Next Day Air, etc. |
| Discount Applied, Promo Code Used | Whether a discount/promo was used |
| Previous Purchases | Number of past purchases |
| Payment Method, Frequency of Purchases | How and how often the customer buys |

**Key facts:**
- Categories: Clothing (1,737) · Accessories (1,240) · Footwear (599) · Outerwear (324)
- Gender split: Male (2,652) · Female (1,248)
- Age range: 18–70 years | Purchase amount range: $20–$100
- Missing values: 37 in `Review Rating` (only column with nulls)

## 🔄 Project Workflow

1. **Data Loading** — Imported raw CSV into a Pandas DataFrame
2. **Data Exploration** — `.info()`, `.describe()`, `.isnull().sum()` to understand structure and missing values
3. **Data Cleaning** — Filled missing review ratings, standardized column names
4. **Feature Engineering** — Created `age_group` and `purchase_frequency_days`; dropped a redundant column
5. **Database Loading** — Pushed cleaned data into PostgreSQL via SQLAlchemy
6. **SQL Analysis** — 10 business-focused SQL queries (aggregation, CTEs, window functions)
7. **Dashboard Building** — Visualized findings in an interactive Power BI dashboard

## 🧹 Data Cleaning & Feature Engineering

- **Missing Review Ratings:** 37 missing values filled using the **median rating per product category** (preserves category-level patterns better than a global average).
- **Standardized column names:** lowercase, underscores (e.g., `Purchase Amount (USD)` → `purchase_amount`).
- **New features:**
  - `age_group` — quantile-based binning (`pd.qcut`) into Young Adult / Adult / Middle-aged / Senior
  - `purchase_frequency_days` — mapped text frequency (Weekly, Monthly, etc.) to approximate numeric days
- **Redundant column removed:** `Promo Code Used` was identical to `Discount Applied` for every record, so it was dropped.
- Cleaned data loaded into PostgreSQL (`customer_behavior` database, `customer` table) via `to_sql()`.

## 🔎 Key SQL Insights

| # | Question | Key Finding |
|---|---|---|
| 1 | Revenue by gender | Male customers drove ~68% of revenue — due to a larger customer base, not higher spend per transaction |
| 2 | Discount users vs. average spend | *(see notebook/SQL file for full breakdown)* |
| 4 | Shipping type vs. spend | Express shipping users spend slightly more ($60.48) than Standard ($58.46) |
| 5 | Subscribers vs. non-subscribers | Subscribers do **not** spend more per transaction ($59.49 vs $59.87) |
| 6 | Products with highest discount usage | Hat (50.0%) and Sneakers (49.7%) are the most discounted items |
| 7 | Customer segmentation by loyalty | ~80% of customers are "Loyal" (10+ purchases); only ~2% are New |
| 8 | Top 3 products per category | Identified via `ROW_NUMBER()` window function per category |
| 9 | Repeat buyers vs. subscription | Repeat buyers are more likely to be **non-subscribers** |
| 10 | Revenue by age group | Fairly even across groups; Young Adults contribute slightly more |

Full queries and results are in `customer_shopping_behavior.sql`.

## 📈 Power BI Dashboard

An interactive dashboard (`Customer_Behavior_Dashboard.pbix`) translates the SQL insights into visuals for non-technical stakeholders, including:

- Revenue breakdown by gender, category, and age group
- Customer segmentation view (New / Returning / Loyal)
- Discount usage vs. spending relationship
- Top-performing products and categories
- Slicers for season, location, and payment method

## ✅ Key Findings

- Male customers generate significantly more revenue — driven by customer base size, not spend per transaction
- ~80% of customers are "Loyal" (10+ previous purchases) — a strong repeat-customer base
- Subscription status does **not** correlate with higher spend or more repeat purchases
- Discounts are used broadly across spend levels; Hats and Sneakers are discounted most often
- Revenue is spread fairly evenly across age groups — broad market appeal

## ⚠️ Challenges Faced

- Handling missing `Review Rating` values without losing data from other columns
- Spotting that `Discount Applied` and `Promo Code Used` were fully redundant
- Mapping inconsistent text categories (including typos like "Fortnighty") to numeric values
- Translating pandas-based exploration into clean, well-structured SQL (CTEs, window functions)
- Designing a Power BI dashboard that's both visually clear and business-relevant

## 📚 Skills Gained

- End-to-end analytics pipeline: raw data → database → dashboard
- Data cleaning & feature engineering with Pandas
- SQL: `GROUP BY`, subqueries, CTEs, window functions (`ROW_NUMBER`)
- Connecting Python to PostgreSQL via SQLAlchemy
- Building interactive dashboards in Power BI
- Translating raw data into business-actionable insights

## 📁 Project Files

```
Customer_Behavior_Analysis/
├── first_project.ipynb                  # Data cleaning & feature engineering
├── customer_shopping_behavior.sql       # SQL business queries
├── Customer_Behavior_Dashboard.pbix     # Power BI dashboard
├── customer_shopping_behavior.csv       # Raw dataset
└── README.md
```

## 🚀 Getting Started

### Prerequisites
```bash
pip install pandas sqlalchemy psycopg2
```

### Run the analysis
```bash
jupyter notebook first_project.ipynb
```

### Load into PostgreSQL
Update your connection string in the notebook, then run the SQLAlchemy `to_sql()` cell to load cleaned data into your `customer_behavior` database.

### Run SQL queries
```bash
psql -d customer_behavior -f customer_shopping_behavior.sql
```

### View the dashboard
Open `Customer_Behavior_Dashboard.pbix` in Power BI Desktop.

---

*Data Analyst Internship Project — practical experience with the complete data analytics workflow, from raw data to actionable business insights.*
