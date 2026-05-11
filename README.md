# 📡 Telecom Customer Churn Analysis
### End-to-End Business Intelligence & Churn Prediction Dashboard

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)

---

## 📌 Project Overview

A full-scale business intelligence project analyzing **7,043 telecom customer records** across 35 attributes including demographics, service usage, billing behavior, and churn reasons. Data was modeled using a **star schema in Power BI** with a central fact table and 5 dimension tables, producing a **7-page interactive dashboard** — uncovering critical insights around churn drivers, revenue at risk, customer segmentation, and retention strategy.

| Metric | Value |
|--------|-------|
| Total Revenue | $21.37M |
| Revenue Lost to Churn | $3.68M |
| Net Revenue | $17.69M |
| Monthly Recurring Revenue (MRR) | $291.40K |
| Lost MRR | $137.09K |
| Overall Churn Rate | 26.54% |
| Retention Rate | 71.63% |
| Total Customers | 7,043 |
| Avg Revenue per Customer | $3.03K |
| Average Tenure | 32 months |

---

## 🗂️ Repository Structure

```
telecom-churn-analysis/
│
├── data/                        # Source data (not tracked — see note below)
│   └── telecom_customers.csv    # 7,043 records, 35 attributes
│
├── powerbi/
│   └── telecom_churn_dashboard.pbix
│
├── screenshots/
│   ├── telecom_overview.png
│   ├── telecom_revenue.png
│   ├── telecom_customer.png
│   ├── telecom_details.png
│   ├── telecom_more_details.png
│   └── telecom_schema.png
│
└── README.md
```

> ⚠️ **Note:** Source data is not tracked in this repository. The dataset contains 7,043 customer records with 35 attributes covering demographics, service subscriptions, billing, usage, and churn outcomes.

---

## 🏗️ Data Model — Star Schema

![Schema](telecom_schema.png)

The Power BI data model follows a **star schema** design with one central fact table surrounded by 5 dimension tables and a dedicated measures table.

| Table | Type | Key Fields |
|-------|------|-----------|
| `fact_telecom_customer_churn` | Fact | Customer ID, contract_id, internet_id, phone_id, Churn Category, Churn Reason, City, GBs Usage, Extra Data Charges, Has Refund |
| `dim_customer` | Dimension | customer_key, Age, Gender, Married, Number of Dependents, Customer Status |
| `dim_contract` | Dimension | contract_id, Contract type, Payment Method, Paperless Billing |
| `dim_internet_service` | Dimension | internet_id, Internet Type, Online Security, Online Backup, Device Protection, Has Streaming |
| `dim_phone_service` | Dimension | phone_id, Phone Service, Multiple Lines |
| `dim_population` | Dimension | Zip Code, Population, Population Bracket |
| `_measures` | Measures | Churn Rate, MRR, Lost MRR, Net Revenue, LTV, Retention Rate, Revenue Lost, Avg Monthly Charges, Avg Tenure |

### Data Flow Steps

1. **Ingestion** — Loaded the source CSV (7,043 rows, 35 columns) into Power BI
2. **Modeling** — Structured data into a star schema: 1 fact table + 5 dimension tables + 1 measures table
3. **Transformation** — Created calculated columns: GBs usage brackets, long distance charge brackets, extra data charge brackets, population brackets, tenure brackets
4. **DAX Measures** — Built all KPIs using DAX: Churn Rate, MRR, Lost MRR, Net Revenue, LTV by segment, Revenue at Risk
5. **Dashboard** — Delivered a 7-page interactive report with slicers for Contract, Payment, Customer Status, Internet Type, and Gender

---

## 📊 Power BI Dashboard

The dashboard spans **7 pages** — Overview, Revenue, Customers, Details, More Details, Insights, and Recommendations — each with shared slicers for: **Contract, Payment, Customer Status, Internet Type, and Gender.**

---

### Page 1 — Overview

![Overview](telecom_overview.png)

**KPIs:** Total Revenue ($21.37M) · Churn Rate (26.54%) · Revenue of Churned Customers ($73.35) · Lost MRR ($137.09K) · Monthly Rev per Customer ($61.74) · MRR ($291.40K)

**Visuals:**
- 📊 Bar chart: Churned customers by Churn Category — Competitor leads at ~841 customers, followed by Dissatisfaction, Attitude, Price, Other
- 📊 Bar chart: Churned customers by detailed Churn Reason — "Competitor had better devices" and "Competitor made better offer" are the top two specific reasons
- 🔀 Decomposition tree: Churn rate broken down by Internet Type → Contract → Payment Method → Offer → Gender → Married status
  - Fiber Optic: 40.72% churn | Cable: 25.66% | DSL: 18.58% | No Internet: 7.40%
  - Month-to-Month: 58.82% | One Year: 16.70% | Two Year: 5.48%
  - Mailed Check: 73.33% | Bank Withdrawal: 62.05% | Credit Card: 44.76%

---

### Page 2 — Revenue

![Revenue](telecom_revenue.png)

**KPIs:** Total Revenue ($21.37M) · Revenue Lost ($3.68M) · Net Revenue ($17.69M) · Churn Rate (26.54%) · Average Tenure (32 months)

**Visuals:**
- 📊 Bar chart: Revenue by Offer — None leads at $11.35M · Offer B: $4.19M · Offer A: $3.66M · Offer C: $1.15M · Offer D: $792K · Offer E: $243K
- 📈 Scatter plot: Revenue per customer by Tenure in Months — clear positive linear relationship; longer tenure = significantly higher revenue
- 🍩 Donut: Total Revenue by Payment Method — Mailed Check 59.3% ($12.67M) · Bank Withdrawal 38.17% ($8.16M) · Credit Card 2.54% ($0.54M)
- 🍩 Donut: Total Revenue by Contract — Month-to-Month 42.29% ($9.04M) · Two Year 28.88% ($6.17M) · One Year 28.84% ($6.16M)
- 📊 Bar chart: Total Revenue by Internet Type — Fiber Optic dominates at ~$13M · DSL: ~$4M · Cable: ~$2.5M · No Internet: ~$1.5M

---

### Page 3 — Customers

![Customers](telecom_customer.png)

**KPIs:** Total Revenue ($21.37M) · Revenue Lost ($3.68M) · Net Revenue ($17.69M) · Churn Rate (26.54%) · Total Customers (7K) · Revenue per Customer ($3.03K)

**Visuals:**
- 📊 Bar chart: Total Revenue by Customer Status — Stayed generates ~$15M · Churned: ~$4M · Joined: minimal
- 🍩 Donut: Total Revenue by Gender — Male 50.62% ($10.82M) · Female 49.38% ($10.55M) — nearly equal split
- 📊 Bar chart: Revenue by Number of Dependents — Customers with 0 dependents generate the majority of revenue; customers with dependents generate disproportionately high LTV relative to their count
- 📈 Area chart: Revenue by Age — peaks in the 30–55 age range; sharp decline after age 65; youngest and oldest segments generate significantly less revenue

---

### Page 4 — Details

![Details](telecom_details.png)

**KPIs:** Total Revenue ($21.37M) · Total Refunds ($13.82K) · Churn Rate (26.54%) · Average Tenure (32 months)

**Visuals:**
- 📊 Bar chart: Churn Rate by Population Bracket — 3,300–50K population: 30.84% · 4,500K+: 30.22% · 2,100–30K: 24.68% · Less than 10K: 24.39% — smaller markets outperform
- 📈 Area chart: Churn Rate by Age — rises dramatically after age 60; single seniors (65+) show the highest churn at 50.92%
- 📈 Scatter plot: Churn Rate by Tenure in Months — strong negative correlation; as tenure increases churn rate drops sharply and stabilizes below 10% after month 30
- 🍩 Donut: Churn Rate by Has Refund — Customers with refunds: 20.38% churn · Without refunds: 27.03% churn — refunds actively reduce churn
- 📊 Bar chart: Churn Rate by Number of Dependents — 0 dependents: ~35% churn · 1 dependent: ~7% · 2+ dependents: ~6.66% — family customers are dramatically more loyal

---

### Page 5 — More Details

![More Details](telecom_more_details.png)

**KPIs:** Total Revenue ($21.37M) · Total Extra Charges ($48K) · Total Charges ($16.06M) · Average Monthly Charges ($63.60) · Churn Rate (26.54%)

**Visuals:**
- 📊 Combo chart: Churn Rate and Average Revenue by Premium Service Count — 0 services: 33.59% churn · 1 service: 41.61% (HIGHEST — danger zone) · 2 services: 24.33% · 3 services: 12.51% · 4 services: 5.32% (LOWEST); average revenue line rises steadily with each additional service
- 🍩 Donut: Churn Rate by Has Streaming — Has Streaming: 31.27% (48.74% of customers) · No Streaming: 32.88% (51.26%)
- 📊 Bar chart: Churn Rate by GBs Usage Bracket — Light Users (<10 GB): 38.22% · Medium Users (10–30 GB): 32.68% · Heavy Users (30+ GB): 25.52% · No Internet: 7.40%
- 📊 Highlighted table: Churn Rate by Long Distance Charges Bracket — Low LD: 38.49% · No LD: 24.93% · Medium LD: 17.19% · High LD (Whales): 12.20%
- 📊 Bar chart: Churn Rate by Extra Data Charges Bracket — Small Overages: highest churn · No Overages: moderate · Large Overages: lowest (they upgraded to unlimited)

---

## 📈 Key Findings & Strategic Analysis

### Executive Summary

The analysis reveals that telecom churn is not random — it is driven by a precise set of predictable, actionable factors. With a **26.54% churn rate** and **$3.68M in revenue lost**, the data uncovers clear paths to retention through contract migration, premium service bundling, engagement programs, and targeted demographic interventions. The strongest predictors of churn are contract type, premium service count, referral activity, marital status, and data usage behavior.

---

### 1. Contract Type — The Strongest Churn Predictor

Contract type is the single most powerful predictor of whether a customer stays or leaves.

| Contract | Churn Rate | Avg LTV | Avg Tenure |
|----------|-----------|---------|-----------|
| Month-to-Month | 58.82% | $1,910 | 19.34 months |
| One Year | 16.70% | $4,043 | 41.87 months |
| Two Year | 5.48% | $4,854 | 54.53 months |

Two-year contracts generate **2.5× more lifetime value** than month-to-month despite having lower monthly charges ($61.45 vs $67.19). The critical risk window is the **first 6 months for month-to-month customers**, where churn rates reach 84–90% — the highest risk period across the entire customer base.

**Strategic Actions:**
1. Incentivize month-to-month customers to switch to annual contracts — offer 2 months free for switching to a 2-year plan
2. Run a "lock in your rate" campaign targeting customers approaching 12-month tenure
3. Flag all month-to-month customers in their first 6 months and assign dedicated onboarding specialists with weekly check-ins for the first 3 months
4. Offer contract upgrade incentives at the 3-month mark — before the churn window peaks

---

### 2. Premium Services — The Retention Goldmine

The number of premium services a customer subscribes to is among the strongest retention levers in the entire dataset, but it contains a dangerous trap.

| Premium Services | Churn Rate | Avg LTV | Avg Tenure |
|-----------------|-----------|---------|-----------|
| 0 services | 33.59% | $1,504 | — |
| 1 service | **41.61% (HIGHEST)** | $2,664 | — |
| 2 services | 24.33% | $4,148 | — |
| 3 services | 12.51% | $5,394 | — |
| 4 services | **5.32% (LOWEST)** | $7,116 | 61.5 months |

The "danger zone" is exactly 1 premium service — these 1,370 customers are testing the service but are not yet committed, and they churn at the highest rate in the entire dataset. The best combination is all 4 premium services together (Online Security + Online Backup + Device Protection + Premium Tech Support), which drives churn down to just 5.32% and LTV up to $7,116.

**Strategic Actions:**
1. Never sell just 1 premium service in isolation — it increases churn to 41.61%
2. Create an "Ultimate Protection Bundle" (all 4 premium services) at a 15–20% discount to make adoption frictionless
3. Immediately target all 1,370 customers with exactly 1 premium service for urgent upsell to 3–4 services
4. Converting 500 customers from 1 to 4 services reduces churn by 36 percentage points and generates an estimated +$2.2M in additional LTV

---

### 3. Streaming Services — Bundle Strategy Matters

Streaming services follow the same bundling logic as premium services — single-service customers are at high risk.

| Streaming Setup | Churn Rate | Avg LTV |
|----------------|-----------|---------|
| No Streaming | 36.58% | — |
| Has Streaming (any) | 31.96% | — |
| All 3 Services (TV + Movies + Music) | 27.32% | $5,371 |
| Only Music Streaming | **67.37% (WORST)** | — |

Customers with only music streaming churn at nearly **2.5× the rate** of those with all three streaming services. Single-service streaming customers are high-risk and should never be left in that state.

**Strategic Actions:**
1. Never sell a single streaming service without cross-selling the full bundle
2. Create an "Entertainment Plus" bundle (all 3 streaming) at a 20% discount
3. Proactively cross-sell to all existing single-service streaming customers
4. This intervention alone can reduce streaming customer churn from 36.58% to 27.32%, retaining an estimated 177 additional customers worth $951K

---

### 4. Demographics — Marriage & Age Matter Most

**Marital Status Impact:**

| Marital Status | Churn Rate | Avg LTV |
|---------------|-----------|---------|
| Married | 14–19% | $3,844–$4,588 |
| Single | 30–36% | $2,186–$2,735 |

Married customers churn at **half the rate** of single customers across every age group. This is the most consistent demographic signal in the dataset.

**Age & Marital Status Combined:**

| Segment | Churn Rate | LTV |
|---------|-----------|-----|
| Under 65 — Married | 14–19% | $4,588 |
| Under 65 — Single | 30–36% | $2,735 |
| Seniors 65+ — Married | 34.98% | $4,588 |
| Seniors 65+ — Single | **50.92% (HIGHEST RISK)** | $2,186 |

Despite their high churn, married senior customers generate $4,588 LTV — making them worth significant retention investment.

**Strategic Actions:**
1. Create a dedicated senior retention program — simplified billing, a dedicated senior support line, and loyalty discounts for long-tenure 65+ customers
2. Proactively target single seniors (50.92% churn risk) with outreach calls and retention offers before churn signals appear
3. In acquisition targeting, prioritize married customers — they deliver 2× higher LTV and half the churn rate of single customers
4. Reduce senior churn from 50.92% to 35% — this retains an estimated 87 additional customers worth $238K saved

---

### 5. Dependents — Family Equals Loyalty

Customers with dependents are dramatically more loyal and represent a critically under-targeted acquisition segment.

| Dependents | Churn Rate | Customers | Avg Tenure |
|-----------|-----------|----------|-----------|
| 0 dependents | 34.97% | 5,042 | 33 months |
| 1 dependent | 7.22% | 526 | 40+ months |
| 2+ dependents | 6.66% | 1,021 | 40+ months |

Customers with dependents are **5× more loyal** than those without. They stay 7–8 months longer and generate higher LTV despite lower monthly charges — because they never leave.

**Strategic Actions:**
1. Create family bundle packages with multi-line discounts specifically designed for households with dependents
2. Build a "Family Safety Package" combining all 4 premium services with parental controls
3. Target married customers with dependents as the primary acquisition profile — they deliver the best long-term economics in the entire customer base
4. Acquiring 1,000 family customers at 6.66% churn vs. 34.97% generates an additional $3.4M in LTV over 3 years

---

### 6. Referrals — The Strongest Retention Signal

The referral program is the single best leading indicator of whether a customer will stay long-term.

| Referrals | Churn Rate | Avg Tenure | Avg LTV |
|-----------|-----------|-----------|---------|
| 0 referrals | 36.11% | 26.67 months | $2,432 |
| 1 referral | **47.34% (ANOMALY)** | — | — |
| 2–3 referrals | 12.18% | 44.35 months | $4,164 |
| 4+ referrals | **3.70% (LOWEST)** | 47.07 months | $4,404 |

Customers with 4+ referrals have **10× lower churn** than those with zero. The 1-referral anomaly (47.34% churn) mirrors the 1-premium-service danger zone — these customers referred someone right before deciding to leave themselves.

**Strategic Actions:**
1. Redesign the referral program to incentivize reaching 4+ referrals — not just the first one: $25 for 1st referral, $50 for 2nd–3rd, $100 for 4th+
2. Create a "Referral Champion" VIP tier with exclusive benefits and priority support
3. Gamify with leaderboards and monthly prizes to sustain referral activity
4. Moving 500 customers from 0–1 referrals to 4+ referrals can reduce churn from 36–47% to 3.7%, retaining an estimated 200 additional customers worth $880K saved

---

### 7. Data Usage — Engagement as a Churn Predictor

How much data a customer uses is a direct proxy for how engaged they are with the service.

| Usage Bracket | Churn Rate |
|--------------|-----------|
| No Internet (phone only) | 8.41% |
| Light Users (< 10 GB) | **40.38% (HIGHEST)** |
| Medium Users (10–30 GB) | 32.68% |
| Heavy Users (30+ GB) | 25.52% |

Light internet users (<10 GB) have **5× higher churn** than customers without internet at all. They are paying for a service they barely use — making them prime churn candidates who feel no value in staying.

**Strategic Actions:**
1. Flag all light users (<10 GB) — 904 customers at 40.38% churn risk with $3.1M in revenue at risk
2. Proactive outreach: "We noticed you're not using much of your internet — let us help you get more value"
3. Offer plan downgrades for price-sensitive light users rather than losing them entirely
4. Give 30+ GB heavy users VIP treatment and priority support — they are your most engaged and retained customers
5. Reducing light user churn from 40.38% to 30% retains an estimated 94 additional customers worth $319K saved

---

### 8. Pricing & Charges — The Value Perception Problem

Monthly charge levels reveal a clear value perception issue at specific price brackets.

| Monthly Charge | Churn Rate |
|---------------|-----------|
| < $30 | 12.21% |
| $30–$50 | 34.78% |
| $50–$70 | 21.66% |
| $70–$90 | **39.84% (HIGHEST)** |
| $90+ | 33.37% |

The $70–$90 bracket is the danger zone — customers paying this amount are not perceiving enough value to justify the price, yet premium customers paying $90+ see the value and stay. Churned customers paid $73.35/month on average vs. $61.74 for those who stayed — an $11.61 gap that reflects pricing without sufficient perceived value.

**Extra Data Charge Impact:**

| Overage Tier | Churn Rate |
|-------------|-----------|
| No Extra Charges | 27.60% |
| Small Overages ($1–$100) | **38.29% (+39% increase)** |
| Medium Overages ($101–$500) | 25.56% (they upgraded to unlimited) |

Small overages are a churn trigger — customers hitting unexpected charges become frustrated before they have a chance to upgrade. The fix is proactive intervention before the overage hits.

**Long Distance Charges — Loyalty Indicator:**

| LD Tier | Churn Rate | Avg LTV |
|---------|-----------|---------|
| Low LD ($1–$500) | **44.33% (HIGHEST)** | $1,457 |
| No LD | 26.40% | $1,590 |
| Medium LD ($501–$1,500) | 17.19% | $3,864 |
| High LD ($1,500+) | **12.20% (LOWEST)** | $6,767 |

High long distance users are the "whales" of this customer base — business users or family-oriented customers deeply embedded in the service, with 59.5 months average tenure and $6,767 LTV.

**Strategic Actions:**
1. Alert customers at 80% of their data limit and auto-offer an unlimited upgrade before overage charges hit
2. Implement a "forgive first overage" policy to prevent the churn trigger
3. Review all pricing in the $70–$90 bracket — either add more value (premium services) or A/B test price adjustments
4. Identify and assign dedicated account managers to all high LD users ($1,500+) — they are your highest-value, most loyal customers

---

### 9. Refunds — The Counter-Intuitive Finding

Issuing refunds actively reduces churn — not increases it.

| Refund Status | Churn Rate | Avg LTV |
|--------------|-----------|---------|
| No Refunds | 29.03% | $3,204 |
| Has Refund ($1–$50) | **20.58% (−29% lower)** | $3,599 |

Customers who received refunds churn at a 29% lower rate and generate higher LTV than those who never needed one. The mechanism is trust — when a complaint is resolved with a refund, the customer feels heard and valued.

Notably, stayed customers have a slightly higher refund rate (8.75%) than churned customers (5.72%) — meaning churned customers often didn't receive a refund when they should have.

**Strategic Actions:**
1. Empower support agents to issue refunds up to $50 without requiring manager approval
2. Make refunds proactive — if a customer calls about a billing issue, offer the refund before they ask
3. Market this as a "100% Satisfaction Guarantee — No Questions Asked" to set expectations upfront
4. Cost: ~$50K in refunds. Estimated return: $400K in retained customer revenue — an 8× ROI

---

### 10. Geographic Patterns — Market Size and Churn Crisis Cities

**Churn Rate by Population Density:**

| Population Bracket | Churn Rate |
|-------------------|-----------|
| Less than 10K | **24.39% (BEST)** |
| 2,100–30K | 24.68% |
| 4,500K+ | 30.22% |
| 3,300–50K+ | **30.84% (WORST)** |

The company performs **24% better in smaller markets** than in high-density urban areas. This is a structural advantage that is currently being underexploited.

**Highest Churn Cities — Crisis Markets:**

| City | Churn Rate | Customers | Customers Lost |
|------|-----------|----------|---------------|
| San Diego | **66.55%** | 285 | 185 |
| Fallbrook | 63.41% | — | — |
| Temecula | 61.11% | — | — |
| Santa Rosa | 52.38% | — | — |

**Under-Penetrated High-Value Markets:**

| Location | Population | Current Customers |
|----------|-----------|------------------|
| Los Angeles (90011) | 101,215 | 5 |
| Bell (90201) | 105,285 | 5 |
| Santa Ana (92704) | 91,188 | 4 |

Average market penetration rate across all areas is just 0.01% — indicating massive untapped opportunity in high-population areas that have barely been touched.

**Strategic Actions:**
1. Immediate service quality audit in San Diego — identify what competitors are offering and launch a win-back campaign for recently churned customers
2. Prioritize expansion in small markets (<10K population) where the company already demonstrates structural performance advantages
3. Approach under-penetrated high-population markets (LA, Bell, Santa Ana) with focused acquisition campaigns — the addressable market is enormous relative to current presence

---

### 11. Churn Reasons — Competitive Threat Dominates

| Churn Category | Customers | % of Churn | Avg Monthly Charge |
|---------------|----------|-----------|-------------------|
| Competitor | 841 | **45%** | $75.79 |
| Dissatisfaction | 321 | 17% | $72.38 |
| Attitude | 314 | 17% | $69.73 |
| Price | 211 | 11% | $69.19 |
| Other | 182 | 10% | $74.81 |

Competitor offers are the #1 reason for churn, accounting for nearly half of all exits. Service quality issues (Dissatisfaction + Attitude) account for a combined 34% — meaning nearly 1 in 3 churned customers left due to a service experience failure rather than a competitor offer. These are the most preventable losses.

---

### 12. Early Warning Signs — High-Risk Customer Profiles

**Critical Risk Combinations (71–90% churn rates):**

| Profile | Churn Rate |
|---------|-----------|
| MTM + Tenure <6 months + Has Streaming + Has Referrals | **90.48%** |
| MTM + Tenure <6 months + Has Streaming + No Referrals | 88.69% |
| MTM + Tenure <6 months + No Streaming + No Referrals | 84.56% |

**Risk Segmentation:**

| Segment | Customers | Churn Rate | Revenue at Risk |
|---------|----------|-----------|----------------|
| Critical Risk | 1,095 | 71.32% | $428K |
| High Risk | 1,044 | 47.70% | $1.26M |
| Low Risk (protect) | 4,350 | 13.38% | $19.58M |

The full high-risk customer profile combines: Month-to-Month contract + Tenure under 6 months + 0 referrals + Single marital status + Age 65+ + No dependents + 0 or 1 premium services + Data usage under 10 GB.

---

### 13. Most Valuable Customer Profile

**The Ideal Customer:**
- Contract: Two Year
- Services: All 4 premium services + All 3 streaming services
- Demographics: Married, any age
- Behavior: 4+ referrals, 30+ GB data usage, high long distance charges
- Performance: $7,000+ LTV · Under 5% churn rate · 60+ months tenure

**Top 5 Highest-Value Segments:**

| Segment | LTV | Churn Rate |
|---------|-----|-----------|
| Two Year + Streaming + Male + Under 30 + Married | $7,432 | 1.41% |
| Two Year + Streaming + Male + 50+ + Married | $7,330 | 5.30% |
| Two Year + Streaming + Female + 30–50 + Married | $7,127 | 1.63% |
| Two Year + Streaming + Male + 30–50 + Married | $6,909 | 5.84% |
| Two Year + Streaming + Female + 50+ + Married | $6,881 | 4.20% |

---

## 🎯 Strategic Recommendations Roadmap

### Tier 1 — Immediate Action (Next 30 Days)

| Action | Detail | Estimated Annual Impact |
|--------|--------|------------------------|
| Premium Service Bundle Strategy | Create "Ultimate Protection Bundle" (all 4 services) at 15–20% discount; never sell 1 service alone; target all 1-service customers for immediate upsell | +$2.2M LTV from 500 conversions |
| First 6 Months Intensive Onboarding | Flag all MTM customers in first 6 months (84–90% churn risk); weekly check-ins for first 3 months; offer contract upgrade at month 3 | +$670K saved annually |
| San Diego Market Crisis Response | Immediate service audit; competitive analysis; win-back campaign for recently churned; assign local market manager | +$177K saved |
| Overage Charge Prevention | Alert at 80% of data limit; auto-offer unlimited upgrade before overage hits; forgive first overage; proactive outreach to $1–$100 overage customers | +$180K saved |
| Refund Policy Overhaul | Empower support to refund up to $50 without approval; market "100% Satisfaction Guarantee"; train on proactive refund offers | 8× ROI — $400K retained for $50K cost |

### Tier 2 — Short Term (60–90 Days)

| Action | Detail | Estimated Annual Impact |
|--------|--------|------------------------|
| Referral Program Enhancement | Escalating rewards: $25 / $50 / $100 for 1st / 2nd–3rd / 4th+ referrals; "Referral Champion" VIP tier; gamify with leaderboards | +$880K saved |
| Data Usage Engagement Program | Flag all light users (<10 GB) — 904 customers at risk; proactive outreach; offer plan downgrades vs. losing them entirely; VIP treatment for 30+ GB users | +$319K saved |
| Long Distance User VIP Program | Identify all high LD users ($1,500+); create "Business Plus" or "Family Connect" premium plans; assign dedicated account managers; quarterly proactive retention calls | +$657K saved |
| Family & Dependent Targeting | Family bundles with multi-line discounts; "Family Safety Package" with all premium services + parental controls; partner with schools and family organizations | +$3.4M LTV over 3 years |
| Senior Customer Retention Program | Simplified billing and support for 65+; dedicated senior support line; loyalty discounts for long-tenure customers; proactive outreach to single seniors (50.92% churn risk) | +$238K saved |

### Tier 3 — Medium Term (6 Months)

| Action | Detail | Estimated Annual Impact |
|--------|--------|------------------------|
| Contract Migration Program | 2 months free for switching to 2-year contract; "lock in your rate" campaign; auto upgrade offers at 12-month tenure | +$2.9M LTV from 1,000 conversions |
| Streaming Bundle Strategy | Never sell single streaming services; create "Entertainment Plus" (all 3 streaming) at 20% discount; cross-sell to all single-service customers | +$951K saved |
| Price Optimization — $70–$90 Bracket | Review all pricing in the 39.84% churn danger zone; add value (premium services) or A/B test price adjustments; implement "price lock" guarantees | +$550K saved |

### Tier 4 — Long Term (12+ Months)

| Action | Detail | Estimated Annual Impact |
|--------|--------|------------------------|
| Geographic Expansion | Prioritize small markets (<10K population) where churn is 24.39%; target under-penetrated high-population areas; partner with local businesses in small towns | +$18.6M LTV over 5 years |
| Competitive Response Program | Monitor competitor offers (45% of churn is competitor-driven); price match guarantee for loyal customers; proactive retention offers before contract renewal | +$554K saved |
| Service Quality Improvement | Address dissatisfaction and attitude issues (34% of churn); improve support training; reduce wait times; implement real-time customer satisfaction tracking | +$313K saved |

---

## 💰 Financial Impact Summary

**Current State:**

| Metric | Value |
|--------|-------|
| Churned Customers | 1,869 |
| Lifetime Revenue Lost | $3,684,460 |
| Monthly Revenue Lost | $137,087 |
| Annual Revenue Lost | $1,645,044 |

**Churn Reduction Scenarios:**

| Scenario | Customers Retained | Revenue Saved (Annual) |
|----------|------------------|----------------------|
| 10% Churn Reduction | 187 | $164,504 |
| 20% Churn Reduction | 374 | $329,008 |

**Expected Total Impact (Year 1):**

| Metric | Value |
|--------|-------|
| Additional Customers Retained | 1,500+ |
| Revenue Saved from Prevented Churn | $3.0M+ |
| LTV Increase from Upsells | $5.0M+ |
| Total Financial Impact | **$8.0M+** |
| Investment Required | ~$1.5M |
| ROI | **5.3× return** |

---

## 🛠️ Tech Stack

| Tool | Role |
|------|------|
| **Power BI Desktop** | Data modeling, DAX measures, 7-page interactive dashboard |
| **Power Query** | Data cleaning, type casting, bracket/flag column creation |
| **DAX** | All KPI measures — Churn Rate, MRR, Lost MRR, LTV, Net Revenue, Retention Rate |
| **Star Schema** | Data model design — 1 fact table, 5 dimension tables, 1 measures table |
| **Excel / CSV** | Source data format — 7,043 rows, 35 attributes |

---

## 🚀 How to Reproduce

### Prerequisites
- Power BI Desktop (free)
- The source telecom customer dataset (CSV, 7,043 rows)

### Steps

1. **Load the data**
   Open Power BI Desktop → `Get Data → Text/CSV` → load `telecom_customers.csv`

2. **Build the star schema**
   In Power Query, split the flat file into 5 dimension tables (contract, customer, internet service, phone service, population) and 1 fact table, then establish relationships in the Model view

3. **Create DAX measures**
   Build all KPIs in the `_measures` table: Churn Rate, MRR, Lost MRR, Net Revenue, Retention Rate, LTV by segment, Revenue at Risk

4. **Build the dashboard**
   Reproduce the 7 report pages using the visuals described above, with slicers for Contract, Payment, Customer Status, Internet Type, and Gender

5. **Open the pre-built file**
   Alternatively, open `powerbi/telecom_churn_dashboard.pbix` directly and refresh the data source path if prompted

---

## 👤 Author

Built as a personal business intelligence and churn analytics project, demonstrating end-to-end data modeling, DAX development, and executive-level dashboard design for a telecom customer dataset.

---

## 📄 License

This project is licensed under the MIT License.
