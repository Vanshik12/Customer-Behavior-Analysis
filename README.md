# Customer Behavior Analysis

Data cleaning, exploratory analysis, and SQL-based business analysis of an
e-commerce customer shopping dataset (3,900 customers, 18 attributes).

The goal of this project is to answer common retail/e-commerce business
questions — revenue drivers, discount behavior, product performance, and
customer segmentation — using a full Python → SQL analysis workflow.

---

## Project Structure

```
customer-behavior-analysis/
├── data/
│   ├── raw/
│   │   └── customer_shopping_behavior.csv     # Original dataset
│   └── cleaned/
│       └── customer.csv                       # Cleaned dataset (output of notebook)
├── notebook/
│   └── Customer_Behavior_Analysis.ipynb        # Data cleaning + EDA
├── sql/
│   └── customer_behavior_queries.sql           # 10 business questions in T-SQL
├── requirements.txt
└── README.md
```

---

## Dataset

**Source:** [Customer Shopping Trends Dataset — Kaggle](https://www.kaggle.com/datasets/iamsouravbanerjee/customer-shopping-trends-dataset)

3,900 rows × 18 columns, covering customer demographics, purchase details,
product ratings, subscription status, shipping preferences, discounts, and
purchase history.

---

## Workflow

### 1. Data Cleaning (Python / pandas)
- Filled missing `Review Rating` values using the **median rating within
  each product category** (rather than a single global median).
- Standardized column names to `lower_snake_case`.
- Engineered `age_group` (quartile-based: Young Adult / Adult / Middle Aged / Senior).
- Engineered `purchase_frequency_days` from the categorical `Frequency of
  Purchases` field (e.g. `Weekly` → 7, `Bi-Weekly` → 14).
- Dropped `promo_code_used`, which was found to be identical to
  `discount_applied` for every row.
- Exported the cleaned dataset to `data/cleaned/customer.csv` for SQL analysis.

### 2. Exploratory Analysis (Python / matplotlib, seaborn)
Visual sanity-checks of the cleaned data covering revenue by gender, top-rated
products, revenue by age group, subscription spend comparison, discount usage,
and customer segmentation — see the notebook for full charts and commentary.

### 3. Business Analysis (SQL)
10 business questions answered in T-SQL against the cleaned `customer` table,
covering:
- Revenue breakdowns (gender, age group, shipping type)
- Discount behavior and its relationship to spend
- Top-rated and most-discounted products
- Customer segmentation (New / Returning / Loyal)
- Top products per category (window functions)
- Subscription status vs. repeat-purchase behavior

See [`sql/customer_behavior_queries.sql`](sql/customer_behavior_queries.sql)
for the full, commented query set.

---

## Key Insights

- **Gender:** Male customers generate roughly **2x** the total revenue of
  female customers.
- **Ratings:** Gloves, Sandals, and Boots have the highest average review
  ratings (~3.8+).
- **Age groups:** Revenue is fairly evenly spread across age groups, with
  Young Adults contributing slightly more than Seniors.
- **Subscriptions:** Subscription status does **not** correlate with higher
  average spend — non-subscribers actually contribute more total revenue,
  simply because there are more of them. Average spend per customer is
  nearly identical between the two groups.
- **Discounts:** Hats, Sneakers, and Coats have the highest discount usage
  rates (~47–50% of purchases).
- **Loyalty:** The large majority of customers (~80%) fall into the "Loyal"
  segment (11+ previous purchases), pointing to strong repeat-purchase
  behavior overall.

---

## How to Run

**Notebook:**
```bash
pip install -r requirements.txt
jupyter notebook notebook/Customer_Behavior_Analysis.ipynb
```
Run all cells top to bottom — this regenerates `data/cleaned/customer.csv`
and the chart images.

**SQL:**
1. Load `data/cleaned/customer.csv` into a table named `customer` in SQL
   Server (or adapt the syntax for your engine of choice — e.g. replace
   `TOP N` with `LIMIT N` for MySQL/PostgreSQL).
2. Run `sql/customer_behavior_queries.sql`.

---

## Tools Used
- **Python** — pandas, matplotlib, seaborn
- **SQL** — T-SQL (SQL Server), window functions, CTEs

---

