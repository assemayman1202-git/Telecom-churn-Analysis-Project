#!/bin/bash
# =============================================================
#  Restaurant Chain Performance Analytics
#  GitHub Repository Setup Script
#  Run: bash setup_repo.sh
#  Place this script in the same folder as your dashboard images
# =============================================================

set -e

REPO="restaurant-analytics"

echo "🍽️  Creating repository: $REPO"
mkdir -p "$REPO"/{screenshots,data,powerbi,sql}
cd "$REPO"

# ── Git init ─────────────────────────────────────────────────
git init
git branch -M main

# =============================================================
# .gitignore
# =============================================================
cat > .gitignore << 'EOF'
# Large data files
data/*.csv
data/*.json
data/raw/

# Power BI binary (optional — uncomment if too large)
# powerbi/*.pbix

# OS
.DS_Store
Thumbs.db

# Editor
.vscode/
.idea/
EOF

# =============================================================
# README.md
# =============================================================
cat > README.md << 'HEREDOC'
# 🍽️ Restaurant Chain Performance Analytics
### End-to-End Data Engineering & Business Intelligence Pipeline

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![Apache Spark](https://img.shields.io/badge/Apache%20Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)

---

## 📌 Project Overview

A full-scale data engineering and business intelligence project analyzing **11.1 million restaurant transactions** across a multi-branch Egyptian restaurant chain. Raw data (~1.5 GB across 9 files) was uploaded to **Databricks**, merged and transformed using **Spark SQL**, then connected live to a **4-page interactive Power BI dashboard** — uncovering critical insights around revenue patterns, branch performance, product mix, customer satisfaction, and profitability.

| Metric | Value |
|--------|-------|
| Total Revenue | EGP 2.90 Billion |
| Total Orders | 11.1 Million |
| Total Customers | 200K |
| Average Order Value | EGP 260.87 |
| Profit Margin | 94.63% |
| Branches | 6 |
| Categories | 5 |
| Menu Items | 15 |
| Total Quantity Sold | 35.6 Million |

---

## 🏗️ Architecture & Pipeline

```
Raw Data (1.5 GB)          Databricks                  Power BI
──────────────────    ────────────────────────    ──────────────────────
7 CSV Files        →  Upload to DBFS           →  4-Page Interactive
2 JSON Files          Spark SQL Merge &            Dashboard (Live)
                      Transformation
                      (Delta Lake)
```

### Data Flow Steps

1. **Ingestion** — Uploaded all 9 source files to Databricks File System (DBFS)
2. **Parsing** — Read CSVs with automatic schema inference; parsed nested JSON files
3. **Merging** — Unified all tables via Spark SQL JOINs into a single master analytical table (~11M rows)
4. **Enrichment** — Computed derived fields: profit margin %, weekend/weekday flag, order type labels, hourly aggregations
5. **Connection** — Linked Power BI directly to Databricks via Partner Connect for a live data connection

---

## 📊 Power BI Dashboard

The dashboard spans **4 pages**, each with shared cross-filter slicers for:
**Year · Month · Day · Branch · Category · Payment Method**

---

### Page 1 — Overview
![Overview](screenshots/telecom%20overview.png)

**KPIs:** Total Revenue (EGP 2.90B) · Average Order Value (EGP 260.87) · Profit Margin (94.63%) · Average Price (EGP 86) · Revenue per Customer (EGP 14.49K)

**Visuals:**
- 📈 Line chart: Total revenue by date (2020–2025) — flat growth trajectory
- 📊 Bar chart: Revenue by menu item — Kebab leads at ~EGP 510M
- 🟩 Treemap: Revenue by category — Grills (EGP 1.01B) · Tagines (EGP 0.81B) · Poultry (EGP 0.54B) · Starters (EGP 0.34B) · Beverages (EGP 0.20B)
- 📊 Bar chart: Revenue by branch — Cairo dominates, Assiut lowest
- 🍩 Donut: Payment split — Cash 50% · Card 30% · Wallet 20%

---

### Page 2 — Customer
![Customer](screenshots/telecom%20customer.png)

**KPIs:** Total Orders (11.1M) · Total Customers (200K) · Average Discount (3.6%) · Revenue per Customer (EGP 14.49K) · Total Revenue (EGP 2.90B)

**Visuals:**
- 📈 Line chart: Orders by hour — dual peaks at 13:00–16:00 and 19:00–23:00
- 📈 Line chart: Average order value by month — peaks around month 9–10
- 📊 Bar chart: Rating distribution — 4-star dominant, 5-star underrepresented
- 🍩 Donut: Revenue by order type — Dine-in 39.98% · Takeaway 30.03% · Delivery 29.99%

---

### Page 3 — Details
![Details](screenshots/telecom%20details.png)

**KPIs:** Total Items (15) · Total Quantity Sold (35.6M) · Branches (6) · Categories (5) · Total Revenue (EGP 2.90B)

**Visuals:**
- 📊 Bar chart: Items growing vs. declining (slope coefficients) — Stuffed Eggplant declining fastest at -1.33
- 📊 Bar chart: Items by quantity sold — Mango Juice leads at 4.3M units
- 📊 Bar chart: Top 5 products by revenue — Kebab · Chicken Tagine · Kofta · Molokhia Tagine · Bamia Tagine
- 📊 Bar chart: Bottom 5 products — Tea (EGP 20M) · Rural Juice · Rural Salad · Cane Juice · Stuffed Eggplant

---

### Page 4 — More Details
![More Details](screenshots/telecom%20more%20details.png)

**KPIs:** Total Revenue (EGP 2.90B) · Average Order Value (EGP 260.87) · Total Orders (11.1M) · Average Price (EGP 86) · Total Quantity (35.6M)

**Visuals:**
- 🕹️ Gauge: Chain-wide average rating — **3.70 / 5.0**
- 🍩 Donut: Weekend vs. weekday revenue — Weekend 50.03% · Weekday 49.97%
- 📋 Table: Peak hours by branch — Cairo dominates every top slot
- 📈 Line chart: Profit margin % by day — Weekends ~95.5%, weekdays ~93.5%

---

## 🗃️ Data Schema
![Schema](screenshots/telecom%20schema.png)

| File | Type | Description |
|------|------|-------------|
| `restaurant_1.csv` – `restaurant_6.csv` | CSV | Branch transaction records |
| `restaurant_7.csv` | CSV | Additional transaction data |
| `restaurant_JSON1.json` | JSON | Supplementary dimension data |
| `restaurant_JSON2.json` | JSON | Supplementary dimension data |

**Total Size:** ~1.5 GB | **Total Rows:** ~11 million

> ⚠️ Raw data files are not tracked due to size. Use Git LFS or store externally.

---

## 📈 Key Findings & Strategic Analysis

### 1. Revenue — Flat Growth Signals Market Saturation
- YoY revenue growth ranges from **-3.4% to +2.0%** — market maturity in current locations
- Average order value stable at EGP 260 — growth must come from acquisition or frequency, not pricing
- Geographic expansion is now critical; existing markets show saturation

### 2. Branch Performance — The Cairo Advantage

| Branch | Orders/Customer | Revenue/Customer | Avg Rating |
|--------|----------------|-----------------|-----------|
| Cairo | 8.93 | EGP 5,071 | 4.0 |
| Alexandria | — | — | 3.8 |
| Assiut | 2.58 | EGP 773 | 3.1 |

- Every **0.1 rating point increase ≈ 0.7 additional orders per customer**
- Assiut requires immediate operational audit or closure within 90 days

### 3. Profit Margin — The Weekend-Weekday Gap
- Weekends: ~95.5% margin · Weekdays: ~93.5% margin
- 2% gap × EGP 1.45B weekday revenue = **EGP 34.5M left on the table annually**
- Weekday discounts (2.59% avg) are backwards — protecting margin, not eroding it, should be the priority

### 4. Customer Satisfaction — Rating Distribution

| Stars | % of Customers |
|-------|---------------|
| ⭐⭐⭐⭐⭐ 5 stars | 15% |
| ⭐⭐⭐⭐ 4 stars | 44% |
| ⭐⭐⭐ 3 stars | 36.5% |
| ⭐⭐ 2 stars | 4.1% |
| ⭐ 1 star | ~0.4% |

- Target: **4.2+ chain-wide** — adds ~EGP 400M in annual revenue
- 36.5% stuck at 3 stars = biggest single conversion opportunity

### 5. Product Mix — Beverage Underpricing
- Mango Juice: 4.28M units at only EGP 28.38/unit — massively underpriced
- No premium beverage tier (EGP 50–80 range completely absent)
- Raising beverage prices 35% = **+EGP 40M annually**
- Stuffed Eggplant: steepest decline (-1.33 slope) — candidate for removal

### 6. Cross-Selling — The Single-Item Problem
- 21.7% of orders (542K transactions) = only one item
- Converting 50% of single-item to two-item orders = **+EGP 78.6M annually**
- Current avg: 3.2 items/order — target should be 4–5

### 7. Payment & Order Type
- 50% cash transactions = half of customers cannot be tracked for personalization
- All three order types share an identical 3.70 avg rating — no channel service gap

### 8. Peak Hours — Predictable and Manageable
- 70% of all daily orders fall in **13:00–16:00** and **19:00–23:00**
- Cairo dominates every peak slot — other branches severely underutilized
- Top 20 hour-day combinations = only 20.5% of total orders

---

## 🎯 Strategic Recommendations

### Immediate (0–3 Months)

| Action | Estimated Annual Impact |
|--------|------------------------|
| Fix Assiut or close it | +EGP 50M |
| Optimize weekday pricing & reduce discounts | +EGP 35M |
| Launch cross-sell campaign (POS prompts + staff training) | +EGP 80M |
| Raise beverage prices 35% + add premium tier | +EGP 40M |

### Medium Term (3–12 Months)

| Action | Estimated Annual Impact |
|--------|------------------------|
| Cairo expansion (2–3 new locations) | +EGP 300M |
| Customer experience overhaul + 5-star guarantee program | +EGP 400M |
| Digital transformation (loyalty app, cashback, CRM) | +EGP 100M |
| Menu innovation (desserts, premium grills, remove bottom 20%) | +EGP 150M |

### Long Term (12+ Months)

| Action | Estimated Annual Impact |
|--------|------------------------|
| Geographic expansion — new cities + franchise model | +EGP 500M |
| Brand repositioning to premium traditional cuisine | +20% pricing headroom |

---

## 🛠️ Tech Stack

| Tool | Role |
|------|------|
| **Databricks** | Cloud platform — DBFS storage + Spark SQL compute |
| **Apache Spark** | Distributed processing of 1.5 GB / 11M rows |
| **Delta Lake** | Managed storage layer on DBFS |
| **Spark SQL** | Multi-table JOINs, transformation, field enrichment |
| **Power BI** | 4-page interactive dashboard with 6 cross-filter slicers |
| **Partner Connect** | Live Databricks → Power BI direct connection |

---

## 🚀 How to Reproduce

### Prerequisites
- Databricks account (Community Edition or higher)
- Power BI Desktop
- The 9 source data files

### Steps

1. **Upload data to DBFS**
   ```
   Databricks UI → Data → Add Data → Upload Files
   Target: dbfs:/FileStore/restaurant/
   ```

2. **Run the Spark SQL merge pipeline**
   Open `sql/merge_pipeline.sql` in a Databricks SQL Notebook and execute all cells

3. **Connect Power BI to Databricks**
   ```
   Power BI Desktop → Get Data → Databricks
   Enter server hostname + HTTP path → select master table
   ```

4. **Open and refresh the dashboard**
   Open `powerbi/restaurant_dashboard.pbix` and refresh credentials if prompted

---

## 📄 License

MIT License — free to use, modify, and distribute with attribution.
HEREDOC

# =============================================================
# sql/merge_pipeline.sql
# =============================================================
cat > sql/merge_pipeline.sql << 'EOF'
-- ============================================================
-- Restaurant Chain Analytics — Spark SQL Merge Pipeline
-- Run in a Databricks SQL Notebook
-- ============================================================

-- Step 1: Load CSV files as temporary views
CREATE OR REPLACE TEMP VIEW restaurant_1 AS
SELECT * FROM csv.`dbfs:/FileStore/restaurant/restaurant_1.csv`
  OPTIONS (header "true", inferSchema "true");

CREATE OR REPLACE TEMP VIEW restaurant_2 AS
SELECT * FROM csv.`dbfs:/FileStore/restaurant/restaurant_2.csv`
  OPTIONS (header "true", inferSchema "true");

CREATE OR REPLACE TEMP VIEW restaurant_3 AS
SELECT * FROM csv.`dbfs:/FileStore/restaurant/restaurant_3.csv`
  OPTIONS (header "true", inferSchema "true");

CREATE OR REPLACE TEMP VIEW restaurant_4 AS
SELECT * FROM csv.`dbfs:/FileStore/restaurant/restaurant_4.csv`
  OPTIONS (header "true", inferSchema "true");

CREATE OR REPLACE TEMP VIEW restaurant_5 AS
SELECT * FROM csv.`dbfs:/FileStore/restaurant/restaurant_5.csv`
  OPTIONS (header "true", inferSchema "true");

CREATE OR REPLACE TEMP VIEW restaurant_6 AS
SELECT * FROM csv.`dbfs:/FileStore/restaurant/restaurant_6.csv`
  OPTIONS (header "true", inferSchema "true");

CREATE OR REPLACE TEMP VIEW restaurant_7 AS
SELECT * FROM csv.`dbfs:/FileStore/restaurant/restaurant_7.csv`
  OPTIONS (header "true", inferSchema "true");

-- Step 2: Load JSON files
CREATE OR REPLACE TEMP VIEW restaurant_json1 AS
SELECT * FROM json.`dbfs:/FileStore/restaurant/restaurant_JSON1.json`;

CREATE OR REPLACE TEMP VIEW restaurant_json2 AS
SELECT * FROM json.`dbfs:/FileStore/restaurant/restaurant_JSON2.json`;

-- Step 3: Union all CSV sources
CREATE OR REPLACE TEMP VIEW all_transactions AS
SELECT * FROM restaurant_1
UNION ALL SELECT * FROM restaurant_2
UNION ALL SELECT * FROM restaurant_3
UNION ALL SELECT * FROM restaurant_4
UNION ALL SELECT * FROM restaurant_5
UNION ALL SELECT * FROM restaurant_6
UNION ALL SELECT * FROM restaurant_7;

-- Step 4: Build master analytical table with enriched fields
CREATE OR REPLACE TABLE restaurant_master AS
SELECT
  t.*,

  -- Profit margin %
  ROUND((t.revenue - t.cost) / NULLIF(t.revenue, 0) * 100, 2)  AS profit_margin_pct,

  -- Weekend / Weekday flag
  CASE
    WHEN DAYOFWEEK(t.order_date) IN (1, 7) THEN 'Weekend'
    ELSE 'Weekday'
  END AS day_type,

  -- Hour of day
  HOUR(t.order_timestamp) AS order_hour,

  -- Order size bracket
  CASE
    WHEN t.items_count = 1                  THEN '1 Item'
    WHEN t.items_count = 2                  THEN '2 Items'
    WHEN t.items_count BETWEEN 3 AND 5      THEN '3-5 Items'
    WHEN t.items_count BETWEEN 6 AND 7      THEN '6-7 Items'
    ELSE '8+ Items'
  END AS order_size_bracket,

  j1.population_data,
  j2.additional_info

FROM all_transactions t
LEFT JOIN restaurant_json1 j1 ON t.branch_id  = j1.branch_id
LEFT JOIN restaurant_json2 j2 ON t.customer_id = j2.customer_id;

-- Step 5: Verify row count (~11M expected)
SELECT COUNT(*) AS total_rows FROM restaurant_master;
EOF

# =============================================================
# data/README.md
# =============================================================
cat > data/README.md << 'EOF'
# 📦 Data Sources

Raw data files are **not tracked** in this repository (~1.5 GB total).

## Files Required

| File | Type |
|------|------|
| `restaurant_1.csv` – `restaurant_6.csv` | CSV |
| `restaurant_7.csv` | CSV |
| `restaurant_JSON1.json` | JSON |
| `restaurant_JSON2.json` | JSON |

## Upload to Databricks

```
Databricks UI → Data → Add Data → Upload Files
Target path: dbfs:/FileStore/restaurant/
```

Then run `sql/merge_pipeline.sql` to produce the master analytical table.
EOF

# =============================================================
# Copy dashboard screenshots (exact names from GitHub upload)
# =============================================================
echo ""
echo "📸 Copying dashboard screenshots..."

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

images=(
  "telecom overview.png"
  "telecom customer.png"
  "telecom details.png"
  "telecom more details.png"
  "telecom schema.png"
)

for img in "${images[@]}"; do
  if [ -f "$SCRIPT_DIR/$img" ]; then
    cp "$SCRIPT_DIR/$img" "screenshots/$img"
    echo "  ✅ $img"
  else
    echo "  ⚠️  '$img' not found — place it next to this script and re-run"
  fi
done

# =============================================================
# Git commit
# =============================================================
echo ""
echo "📦 Creating initial commit..."
git add .
git commit -m "feat: initial commit — Restaurant Chain Performance Analytics

- 4-page Power BI dashboard (Overview, Customer, Details, More Details)
- Data schema diagram
- Spark SQL merge pipeline (9 sources → 11M row master table)
- Full business analysis: revenue, branches, products, satisfaction
- Tiered strategic recommendations (0–3 months to 12+ months)
- Data dictionary and pipeline reproduction steps"

echo ""
echo "✅ Repository ready!"
echo ""
echo "👉 Next steps to push to GitHub:"
echo "   1. Create a new repo at https://github.com/new"
echo "   2. cd into restaurant-analytics/ then run:"
echo "      git remote add origin https://github.com/YOUR_USERNAME/restaurant-analytics.git"
echo "      git push -u origin main"
echo ""
echo "✅ Repository ready!"
echo ""
echo "👉 Next steps to push to GitHub:"
echo "   1. Create a repo on https://github.com/new"
echo "   2. Run:"
echo "      git remote add origin https://github.com/YOUR_USERNAME/telecom-churn-analysis.git"
echo "      git push -u origin main"
