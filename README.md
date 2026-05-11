#!/bin/bash
# =============================================================
#  Telecom Churn Analysis — GitHub Repository Setup Script
#  Run: bash setup_repo.sh
#  Requirements: git, curl (images downloaded automatically)
# =============================================================

set -e

REPO="telecom-churn-analysis"

echo "🚀 Creating repository: $REPO"
mkdir -p "$REPO"/{dashboards,docs,data,recommendations}
cd "$REPO"

# ── Git init ─────────────────────────────────────────────────
git init
git branch -M main

# =============================================================
# .gitignore
# =============================================================
cat > .gitignore << 'EOF'
# Power BI
*.pbix
*.pbit

# Raw data
data/raw/
data/*.csv
data/*.xlsx

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
# 📡 Telecom Customer Churn Analysis

> A comprehensive Power BI analysis of customer churn behavior across 7,043 customers — uncovering key drivers, risk segments, and actionable retention strategies worth **$8M+** in Year 1 impact.

---

## 📊 Dashboard Gallery

### 1. Overview Dashboard
![Overview Dashboard](dashboards/01_overview.png)
> Key metrics at a glance: churn rate, MRR, lost revenue, and churn breakdown by category and reason.

---

### 2. Revenue Dashboard
![Revenue Dashboard](dashboards/02_revenue.png)
> Revenue by offer, internet type, payment method, contract type, and customer tenure trends.

---

### 3. Customers Dashboard
![Customers Dashboard](dashboards/03_customers.png)
> Customer distribution by status, gender, number of dependents, and age — with revenue impact.

---

### 4. Details Dashboard
![Details Dashboard](dashboards/04_details.png)
> Churn rate by population bracket, age, tenure, refund status, and number of dependents.

---

### 5. More Details Dashboard
![More Details Dashboard](dashboards/05_more_details.png)
> Deep-dive into premium services, streaming, data usage, long distance charges, and extra charges.

---

## 🗃️ Data Model — Star Schema

![Power BI Schema](dashboards/schema.png)

The Power BI model follows a **star schema** with one central fact table and five dimension tables:

| Table | Type | Description |
|---|---|---|
| `fact_telecom_customer_churn` | Fact | Core table with customer metrics, charges, churn category/reason, and foreign keys |
| `dim_customer` | Dimension | Demographics — age, gender, marital status, dependents, customer status |
| `dim_contract` | Dimension | Contract type, payment method, paperless billing |
| `dim_internet_service` | Dimension | Internet type, streaming, online security, online backup, device protection |
| `dim_phone_service` | Dimension | Phone service, multiple lines |
| `dim_population` | Dimension | ZIP code, population, population bracket |
| `_measures` | Measures Table | All DAX KPIs — churn rate, MRR, lost MRR, LTV, net revenue, retention rate, etc. |

### Key Fact Table Fields
- `Avg Monthly GB Download` — data usage metric
- `Avg Monthly Long Distance Charges` — LD usage indicator
- `Churn Category` / `Churn Reason` — why customers left
- `Extra Data Charges Bracket` / `GBs Usage Bracket` — usage segmentation
- `Has Refund` — refund flag for retention correlation

### DAX Measures
`churn_rate` · `lost MRR` · `MRR` · `Net Revenue` · `revenue lost` · `Retention Rate` · `Revenue per customer` · `Average Monthly Charges` · `Average Tenure Months` · `joined_customer` · `Churn_customers`

---

## 🗂️ Repository Structure

```
telecom-churn-analysis/
│
├── dashboards/                  # Power BI dashboard screenshots & schema
│   ├── 01_overview.png
│   ├── 02_revenue.png
│   ├── 03_customers.png
│   ├── 04_details.png
│   ├── 05_more_details.png
│   └── schema.png
│
├── docs/
│   ├── full_analysis.md         # Complete insights & findings
│   └── data_dictionary.md       # Field definitions and descriptions
│
├── recommendations/
│   ├── tier1_immediate.md       # Actions for next 30 days
│   ├── tier2_short_term.md      # Actions for 60–90 days
│   └── tier3_medium_term.md     # Actions for 6 months+
│
├── data/
│   └── schema.md                # Dataset schema overview
│
├── INSIGHTS.md                  # Key findings quick reference
├── setup_repo.sh                # This script
└── README.md
```

---

## 📈 Executive Summary

| Metric | Value |
|---|---|
| **Total Customers** | 7,043 |
| **Churned** | 1,869 |
| **Stayed** | 4,720 |
| **Joined** | 454 |
| **Overall Churn Rate** | 26.54% |
| **Retention Rate** | 71.63% |
| **Total Revenue** | $21.37M |
| **Revenue Lost to Churn** | $3.68M (lifetime) |
| **Monthly Revenue Lost (MRR)** | $137,087 |
| **Avg LTV — Stayed** | $3,736 |
| **Avg LTV — Churned** | $1,971 |

---

## 🔑 Top 5 Key Findings

### 1. 🔴 Contract Type is the #1 Churn Predictor
| Contract | Churn Rate | Avg LTV | Avg Tenure |
|---|---|---|---|
| Month-to-Month | 28.37% | $1,910 | 19.3 months |
| One Year | Lower | $4,043 | 41.9 months |
| Two Year | Lowest | $4,854 | 54.5 months |

> **Critical:** First 6 months for month-to-month customers show **84–90% churn rate**.

---

### 2. 🛡️ Premium Services are the Retention Goldmine
| Premium Services | Churn Rate | Avg LTV |
|---|---|---|
| 0 Services | 33.59% | $1,504 |
| 1 Service | **41.61% ⚠️ HIGHEST** | $2,664 |
| 2 Services | 24.33% | $4,148 |
| 3 Services | 12.51% | $5,394 |
| 4 Services | **5.32% ✅ LOWEST** | $7,116 |

> **Danger Zone:** 1 premium service = highest churn. Customers are testing, not committed.

---

### 3. 👨‍👩‍👧 Family Customers are 5× More Loyal
| Dependents | Churn Rate |
|---|---|
| 0 Dependents | 34.97% |
| 1 Dependent | 7.22% |
| 2+ Dependents | 6.66% |

---

### 4. 🔗 Referrals are the Strongest Retention Signal
| Referrals | Churn Rate | Avg LTV |
|---|---|---|
| 0 | 36.11% | $2,432 |
| 1 | 47.34% (anomaly) | — |
| 2–3 | 12.18% | $4,164 |
| 4+ | **3.70%** | $4,404 |

> Customers with 4+ referrals have **10× lower churn** than zero-referral customers.

---

### 5. 💸 Refunds Reduce Churn by 29%
Customers who received refunds churn at **20.58%** vs **29.03%** for those who didn't.
Refunds signal responsiveness — customers feel heard and valued.

---

## 💰 Financial Impact

| Scenario | Customers Retained | Revenue Saved (Annual) |
|---|---|---|
| 10% Churn Reduction | 187 | $164,504 |
| 20% Churn Reduction | 374 | $329,008 |
| Premium Bundle Upsell (500 customers) | 180 additional | $1.3M saved |

**Expected Total Year 1 Impact with full program:**
- 🧑‍🤝‍🧑 **1,500+ customers retained**
- 💵 **$3.0M+ in prevented churn**
- 📈 **$5.0M+ in LTV increases from upsells**
- 🏆 **$8.0M+ total financial impact**
- 📊 **5.3× ROI** on ~$1.5M investment

---

## 🚨 High-Risk Segments

| Segment | Size | Churn Risk | Revenue at Risk |
|---|---|---|---|
| New Month-to-Month Customers | 1,095 | 71.32% | $428K |
| Single Seniors 65+ | 546 | 50.92% | $1.5M |
| San Diego Market | 285 | 66.55% | $1.1M |
| Light Data Users (<10 GB) | 904 | 40.38% | $3.1M |
| 1 Premium Service Customers | 1,370 | 41.61% | $3.7M |

---

## 🏆 Ideal Customer Profile

| Attribute | Value |
|---|---|
| Contract | Two Year |
| Premium Services | All 4 (Security + Backup + Device Protection + Tech Support) |
| Streaming | All 3 (TV + Movies + Music) |
| Demographics | Married, any age |
| Behavior | 4+ referrals, 30+ GB usage, high long distance |
| **Expected LTV** | **$7,000+** |
| **Churn Rate** | **< 5%** |

---

## 🗺️ Geographic Insights

- **Best performing:** Small markets (<10K population) — 25.91% churn
- **Worst performing:** High density (50K+ population) — 32.02% churn
- **Crisis market:** San Diego — 66.55% churn (285 customers, 185 churned)
- **Under-penetrated opportunity:** Los Angeles 90011 (101K population, only 5 customers)

---

## 🛠️ Tools Used

- **Power BI** — Interactive dashboard development
- **DAX** — Custom measures and KPI calculations
- **Data Modeling** — Star schema with customer, service, and geographic dimensions

---

## 📁 Detailed Documentation

- 📄 [Full Analysis & Insights](docs/full_analysis.md)
- 📋 [Data Dictionary](docs/data_dictionary.md)
- 🚀 [Tier 1 — Immediate Actions (30 Days)](recommendations/tier1_immediate.md)
- 📅 [Tier 2 — Short Term (60–90 Days)](recommendations/tier2_short_term.md)
- 📆 [Tier 3 — Medium Term & Long Term](recommendations/tier3_medium_term.md)

---

*Analysis based on 7,043 customer records with 35 attributes including demographics, services, usage patterns, and financial metrics.*
HEREDOC

# =============================================================
# INSIGHTS.md
# =============================================================
cat > INSIGHTS.md << 'EOF'
# 💡 INSIGHTS.md — Quick Reference

> Fast-access summary of the most critical findings from the Telecom Churn Analysis.

---

## 🔥 Top 10 Actionable Insights

1. **Bundle all 4 premium services** — churn drops from 41.61% (1 service) to 5.32% (4 services)
2. **Never sell a single premium service** — 1 service = highest churn of any group (41.61%)
3. **First 6 months are critical** — month-to-month new customers churn at 84–90%
4. **4+ referrals = 10× lower churn** — 3.70% vs 36.11% for zero referrals
5. **Families are 5× more loyal** — dependents cut churn from 34.97% to 6.66%
6. **Refunds reduce churn by 29%** — proactive refund policy = retention tool
7. **High LD users are whales** — $6,767 LTV, 12.20% churn, 59.5 months tenure
8. **Light data users are at risk** — <10 GB customers churn at 40.38%
9. **San Diego is in crisis** — 66.55% churn needs immediate intervention
10. **Married customers churn at half the rate** of single customers

---

## ⚡ Fastest Wins

| Action | Cost | Revenue Impact |
|---|---|---|
| Refund policy overhaul | $50K | $400K saved |
| Overage alert system | Low | $180K saved |
| Bundle 4 premium services | Low | $2.2M in LTV |
| San Diego audit | Medium | $177K saved |

---

## 🎯 Segment Priority Matrix

| Priority | Segment | Churn Risk | Revenue at Risk |
|---|---|---|---|
| 🔴 Critical | New MTM customers (<6 months) | 71.32% | $428K |
| 🔴 Critical | Single seniors 65+ | 50.92% | $1.5M |
| 🔴 Critical | San Diego market | 66.55% | $1.1M |
| 🟠 High | 1 premium service customers | 41.61% | $3.7M |
| 🟠 High | Light data users (<10 GB) | 40.38% | $3.1M |
| 🟢 Protect | 3–4 premium service customers | 5–13% | $9.0M |

---

## 📐 Churn Risk Profile

A customer is **CRITICAL RISK** if they match 5+ of these:
- [ ] Contract = Month-to-Month
- [ ] Tenure < 6 months
- [ ] Referrals = 0
- [ ] Marital status = Single
- [ ] Age 65+
- [ ] No dependents
- [ ] Premium services = 0 or 1
- [ ] Data usage < 10 GB

---

## 💎 The Gold Standard Customer

- **Contract:** Two Year
- **Services:** 4 premium + 3 streaming
- **Demographics:** Married, any age, with dependents
- **Behavior:** 4+ referrals, 30+ GB/month, high long distance usage
- **Result:** $7,000+ LTV | <5% churn | 60+ months tenure
EOF

# =============================================================
# docs/full_analysis.md
# =============================================================
cat > docs/full_analysis.md << 'EOF'
# 📊 Complete Telecom Churn Analysis — Full Insights

## Dataset Overview

| Metric | Value |
|---|---|
| Total Customers | 7,043 |
| Stayed | 4,720 |
| Churned | 1,869 |
| Joined | 454 |
| Overall Churn Rate | 26.54% |
| Retention Rate | 71.63% |
| Lifetime Revenue Lost | $3.68M |
| MRR Lost | $137,087/month |
| Avg LTV (Stayed) | $3,736 |
| Avg LTV (Churned) | $1,971 |

---

## 1. Contract Type — Strongest Churn Predictor

| Contract | Churn Rate | Avg LTV | Avg Tenure |
|---|---|---|---|
| Month-to-Month | 28.37% | $1,910 | 19.34 months |
| One Year | Lower | $4,043 | 41.87 months |
| Two Year | Lowest | $4,854 | 54.53 months |

- Two-year contracts generate **2.5× more LTV** despite lower monthly charges ($61.45 vs $67.19)
- **Critical Risk Period:** First 6 months for MTM customers: **84–90% churn rate**

---

## 2. Premium Services — The Retention Goldmine

| Count | Churn Rate | Avg LTV |
|---|---|---|
| 0 | 33.59% | $1,504 |
| 1 | **41.61% ⚠️** | $2,664 |
| 2 | 24.33% | $4,148 |
| 3 | 12.51% | $5,394 |
| 4 | **5.32% ✅** | $7,116 |

Best combo: Online Security + Online Backup + Device Protection + Premium Tech Support
470 customers · 5.32% churn · $88.92/month · 61.5 months tenure

---

## 3. Streaming Services

| Streaming | Churn Rate | Avg LTV |
|---|---|---|
| No Streaming | 36.58% | — |
| Has Streaming | 31.96% | — |
| All 3 (TV + Movies + Music) | 27.32% | $5,371 |
| Only Music Streaming | **67.37% ⚠️** | — |

---

## 4. Demographics

| Segment | Churn Rate | Avg LTV |
|---|---|---|
| Married (any age) | 14–19% | $3,844–$4,588 |
| Single (any age) | 30–36% | $2,186–$2,735 |
| Seniors 65+ Married | 34.98% | $4,588 |
| Seniors 65+ Single | **50.92% 🚨** | — |

---

## 5. Dependents

| Dependents | Churn Rate | Customers |
|---|---|---|
| 0 | 34.97% | 5,042 |
| 1 | 7.22% | 526 |
| 2+ | 6.66% | 1,021 |

Customers with dependents are **5× more loyal** and stay 7–8 months longer.

---

## 6. Referrals

| Referrals | Churn Rate | Avg LTV |
|---|---|---|
| 0 | 36.11% | $2,432 |
| 1 | 47.34% (anomaly) | — |
| 2–3 | 12.18% | $4,164 |
| 4+ | **3.70%** | $4,404 |

---

## 7. Data Usage

| Usage | Churn Rate |
|---|---|
| No Internet | 8.41% |
| < 10 GB | **40.38% ⚠️** |
| 10–20 GB | 33.40% |
| 20–30 GB | 35.39% |
| 30+ GB | 26.69% |

---

## 8. Pricing & Charges

| Monthly Bracket | Churn Rate |
|---|---|
| < $30 | 12.21% |
| $30–$50 | 34.78% |
| $50–$70 | 21.66% |
| $70–$90 | **39.84% ⚠️** |
| $90+ | 33.37% |

| Long Distance | Churn Rate | Avg LTV |
|---|---|---|
| No LD | 26.40% | $1,590 |
| Low ($1–$500) | **44.33%** | $1,457 |
| Medium ($501–$1,500) | 17.19% | $3,864 |
| High ($1,500+) | **12.20%** | $6,767 |

---

## 9. Refunds

| Status | Churn Rate | Avg LTV |
|---|---|---|
| No Refunds | 29.03% | $3,204 |
| Has Refunds | **20.58% (−29%)** | $3,599 |

---

## 10. Geographic Patterns

| Density | Churn Rate |
|---|---|
| < 10K population | 25.91% ✅ |
| 50K+ population | 32.02% |

**Crisis cities:** San Diego 66.55% · Fallbrook 63.41% · Temecula 61.11%

---

## 11. Churn Reasons

| Category | Customers | % of Churn |
|---|---|---|
| Competitor | 841 | **45%** |
| Dissatisfaction | 321 | 17% |
| Attitude | 314 | 17% |
| Price | 211 | 11% |
| Other | 182 | 10% |

---

## 12. Risk Segmentation

| Tier | Customers | Churn Rate | Revenue at Risk |
|---|---|---|---|
| Critical Risk | 1,095 | 71.32% | $428K |
| High Risk | 1,044 | 47.70% | $1.26M |
| Low Risk | 4,350 | 13.38% | $19.58M |
EOF

# =============================================================
# docs/data_dictionary.md
# =============================================================
cat > docs/data_dictionary.md << 'EOF'
# 📋 Data Dictionary

7,043 customer records · 35 attributes

## Customer Demographics
| Field | Type | Description |
|---|---|---|
| `customer_id` | String | Unique identifier |
| `age` | Integer | Customer age |
| `gender` | String | Male / Female |
| `married` | Boolean | Marital status |
| `dependents` | Integer | Number of dependents (0–7+) |
| `city` | String | City |
| `zip_code` | String | ZIP code |
| `population` | Integer | ZIP area population |

## Account Information
| Field | Type | Description |
|---|---|---|
| `customer_status` | String | Stayed / Churned / Joined |
| `churn_category` | String | Competitor / Dissatisfaction / Attitude / Price / Other |
| `churn_reason` | String | Specific churn reason |
| `tenure_months` | Integer | Months with the company |
| `contract` | String | Month-to-Month / One Year / Two Year |
| `payment_method` | String | Mailed Check / Bank Withdrawal / Credit Card |
| `offer` | String | Promotional offer (A–E / None) |
| `num_referrals` | Integer | Friends referred |

## Services
| Field | Type | Description |
|---|---|---|
| `internet_type` | String | Fiber Optic / DSL / Cable / No Internet |
| `online_security` | Boolean | Online Security add-on |
| `online_backup` | Boolean | Online Backup add-on |
| `device_protection` | Boolean | Device Protection add-on |
| `premium_tech_support` | Boolean | Premium Tech Support add-on |
| `streaming_tv` | Boolean | TV Streaming |
| `streaming_movies` | Boolean | Movie Streaming |
| `streaming_music` | Boolean | Music Streaming |
| `unlimited_data` | Boolean | Unlimited data plan |
| `phone_service` | Boolean | Phone service |
| `multiple_lines` | Boolean | Multiple lines |

## Usage & Financial
| Field | Type | Description |
|---|---|---|
| `avg_monthly_gb_download` | Float | Monthly data usage (GB) |
| `avg_monthly_long_distance_charges` | Float | Monthly LD charges ($) |
| `monthly_charge` | Float | Current monthly bill ($) |
| `total_charges` | Float | Total billed to date ($) |
| `total_revenue` | Float | Total revenue incl. extras ($) |
| `total_refunds` | Float | Total refunds issued ($) |
| `total_extra_data_charges` | Float | Overage charges ($) |
| `total_long_distance_charges` | Float | Cumulative LD charges ($) |

## Derived / Engineered Fields
| Field | Description |
|---|---|
| `premium_service_count` | Sum of 4 premium services (0–4) |
| `has_streaming` | Any streaming service active |
| `gb_usage_bracket` | Light / Medium / Heavy / No Internet |
| `population_bracket` | <10K / 10–30K / 30–50K / 50K+ |
| `ld_charges_bracket` | No LD / Low / Medium / High (Whale) |
| `extra_data_bracket` | No Overages / Small / Large |
EOF

# =============================================================
# recommendations/tier1_immediate.md
# =============================================================
cat > recommendations/tier1_immediate.md << 'EOF'
# 🚀 Tier 1 — Immediate Actions (Next 30 Days)

## 1. Premium Service Bundle Strategy
- Create "Ultimate Protection Bundle" (all 4 services) at 15–20% discount
- Never sell just 1 premium service — it pushes churn to 41.61%
- Target 1–2 service customers for immediate upgrade
- **Impact:** Churn 41.61% → 5.32% · LTV $2,664 → $7,116 · ROI $2.2M

## 2. First 6 Months Intensive Onboarding
- Flag all MTM customers in first 6 months (84–90% churn risk)
- Weekly check-in calls for first 3 months
- Contract upgrade incentives at month 3
- Assign dedicated onboarding specialist
- **Impact:** Retain 340 customers = $670K saved/year

## 3. San Diego Market Crisis Response
- Immediate service quality audit (66.55% churn)
- Competitive analysis of local market
- Win-back campaign for recently churned customers
- **Impact:** Retain 90 customers = $177K saved

## 4. Overage Charge Prevention Program
- Alert customers at 80% of data limit
- Auto-offer unlimited upgrade BEFORE overages hit
- "Forgive first overage" policy
- **Impact:** Retain 54 customers = $180K saved

## 5. Refund Policy Overhaul
- Empower support to issue refunds up to $50 without approval
- Market "100% Satisfaction Guarantee"
- Train support on proactive refund offers
- **Impact:** Cost $50K · Return $400K in retained revenue
EOF

# =============================================================
# recommendations/tier2_short_term.md
# =============================================================
cat > recommendations/tier2_short_term.md << 'EOF'
# 📅 Tier 2 — Short Term Actions (60–90 Days)

## 6. Referral Program Enhancement
- Escalating rewards: $25 (1st) · $50 (2nd–3rd) · $100 (4th+)
- "Referral Champion" VIP tier
- Gamified leaderboards and monthly prizes
- **Impact:** Retain 200 customers = $880K saved

## 7. Data Usage Engagement Program
- Flag light users (<10 GB) proactively
- Offer plan downgrades or usage tips
- VIP treatment for 30+ GB heavy users
- **Impact:** Retain 94 customers = $319K saved

## 8. Long Distance User VIP Program
- Identify $1,500+/year LD users (whales — $6,767 LTV)
- Create "Business Plus" / "Family Connect" premium plans
- Dedicated account managers + quarterly retention calls
- **Impact:** Retain 97 customers = $657K saved

## 9. Family & Dependent Targeting
- Family bundle packages with multi-line discounts
- "Family Safety Package" (all 4 premium + parental controls)
- Partner with schools and community organizations
- **Impact:** $3.4M in LTV over 3 years

## 10. Senior Customer Retention Program
- Simplified billing and support for 65+ customers
- Dedicated senior support line
- Proactive outreach to single seniors (50.92% churn risk)
- **Impact:** Retain 87 customers = $238K saved
EOF

# =============================================================
# recommendations/tier3_medium_term.md
# =============================================================
cat > recommendations/tier3_medium_term.md << 'EOF'
# 📆 Tier 3 — Medium Term (6 Months) & Tier 4 — Long Term (12+ Months)

## Tier 3

### 11. Contract Migration Program
- 2 months free for switching to 2-year contract
- "Lock in your rate" campaign
- Auto upgrade offer at 12-month tenure
- **Impact:** Convert 1,000 MTM → 2-year · LTV $1,910 → $4,854 · +$2.9M

### 12. Streaming Bundle Strategy
- Never sell single streaming services
- "Entertainment Plus" bundle (all 3) at 20% discount
- Cross-sell to single-service customers
- **Impact:** Retain 177 customers = $951K saved

### 13. Price Optimization — $70–$90 Bracket
- Audit highest-churn pricing bracket (39.84%)
- Add value or lower price; A/B test strategies
- Consider price-lock guarantees for contract upgrades
- **Impact:** Retain 170 customers = $550K saved

---

## Tier 4

### 14. Geographic Expansion
- Prioritize small markets (<10K pop.) — 25.91% churn vs 32.02% urban
- Target under-penetrated high-population ZIP codes
- **Impact:** 5,000 new customers · +$18.6M LTV over 5 years

### 15. Competitive Response Program
- Monitor competitor offers (45% of churn is competitor-driven)
- Price match guarantee for loyal customers
- Proactive retention offers 90 days before renewal
- **Impact:** Retain 281 customers = $554K saved

### 16. Service Quality Improvement
- Address dissatisfaction + attitude churn (34% of churners)
- Support agent training, reduced wait times, CSAT tracking
- **Impact:** Retain 159 customers = $313K saved

---

## Implementation Timeline

| Month | Actions |
|---|---|
| 1 | Premium Bundle · Onboarding · San Diego · Refund Policy |
| 2 | Overage Prevention · Data Engagement · Referrals |
| 3 | LD VIP · Senior Retention · Family Targeting |
| 4–6 | Contract Migration · Streaming Bundle · Price Optimization |
| 7–12 | Geographic Expansion · Competitive Response · Service Quality |

## Year 1 Expected Impact

| Metric | Target |
|---|---|
| Customers Retained | 1,500+ |
| Revenue Saved | $3.0M+ |
| LTV Increase (Upsells) | $5.0M+ |
| **Total Impact** | **$8.0M+** |
| Investment | ~$1.5M |
| **ROI** | **5.3×** |
EOF

# =============================================================
# data/schema.md
# =============================================================
cat > data/schema.md << 'EOF'
# 🗄️ Dataset Schema

7,043 records · 35 attributes

## Record Counts
| Status | Count |
|---|---|
| Stayed | 4,720 |
| Churned | 1,869 |
| Joined | 454 |

## Key Aggregates
| Metric | Value |
|---|---|
| Total Revenue | $21.37M |
| Net Revenue | $17.69M |
| Revenue Lost | $3.68M |
| MRR Lost | $137,087 |
| Avg Monthly Charge | $63.60 |
| Avg Tenure | 32 months |

## Contract Distribution
- Month-to-Month: 42.29% of revenue
- Two Year: 28.84%
- One Year: 28.88%

## Payment Methods
- Bank Withdrawal: 59.3%
- Mailed Check: 38.17%
- Credit Card: 2.54%

## Churn Reasons
- Competitor: 45%
- Dissatisfaction: 17%
- Attitude: 17%
- Price: 11%
- Other: 10%
EOF

# =============================================================
# Copy dashboard images (place them in the same folder as this script)
# =============================================================
echo ""
echo "📸 Copying dashboard images..."

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

images=(
  "01_overview.png"
  "02_revenue.png"
  "03_customers.png"
  "04_details.png"
  "05_more_details.png"
  "schema.png"
)

for img in "${images[@]}"; do
  if [ -f "$SCRIPT_DIR/$img" ]; then
    cp "$SCRIPT_DIR/$img" dashboards/
    echo "  ✅ $img"
  else
    echo "  ⚠️  $img not found — place it next to this script and re-run, or add it manually to dashboards/"
  fi
done

# =============================================================
# Git commit
# =============================================================
echo ""
echo "📦 Creating initial commit..."
git add .
git commit -m "feat: initial commit — Telecom Churn Analysis

- Power BI dashboards (overview, revenue, customers, details, more details)
- Star schema diagram
- Full churn analysis across 12 dimensions
- Tiered recommendations (30 days → 12 months)
- Data dictionary and schema docs
- Expected impact: \$8M+ Year 1, 5.3x ROI"

echo ""
echo "✅ Repository ready!"
echo ""
echo "👉 Next steps to push to GitHub:"
echo "   1. Create a repo on https://github.com/new"
echo "   2. Run:"
echo "      git remote add origin https://github.com/YOUR_USERNAME/telecom-churn-analysis.git"
echo "      git push -u origin main"
