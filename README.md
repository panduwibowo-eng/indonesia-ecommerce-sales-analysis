# 📊 Indonesia E-Commerce Sales Performance Analysis

<div align="center">

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Power Query](https://img.shields.io/badge/Power%20Query-217346?style=for-the-badge&logo=microsoft&logoColor=white)
![Star Schema](https://img.shields.io/badge/Star%20Schema-FF6B35?style=for-the-badge&logo=databricks&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

**An end-to-end Business Intelligence project analyzing Shopee seller performance across Indonesia — transforming 18,868 raw transactions into actionable sales insights using Power BI.**

[📊 Dashboard Preview](#-dashboard-preview) · [💡 Insights](#-insights) · [🎯 Recommendations](#-recommendations) · [👤 About Me](#-about-me)

</div>

> 📸 *Dashboard screenshots available in the [`/images`](/images/dashboard-overview.png) folder.*

---

## ⚡ Executive Summary

> *A quick 30-second overview for busy recruiters.*

| | |
|---|---|
| **Dataset** | 20,848 raw transactions → 18,868 after cleaning |
| **Period** | December 2023 – November 2025 (2 years) |
| **Total Revenue** | **Rp958,948,812** (~Rp1 Billion) |
| **Total Orders** | **18,868 transactions** |
| **Avg Order Value** | **Rp50,870 per transaction** |
| **Completion Rate** | **85%** orders successfully delivered |
| **Top Category** | **Seal/Baut/Roof** — highest revenue & AOV (Rp424K) |
| **Top Market** | **Jawa Barat** — dominant province by revenue |
| **#1 Finding** | High-value categories drive revenue despite low order volume |
| **#1 Action** | Reduce 15% cancellation rate → estimated +Rp75M/year |

---

## 📌 Project Overview

This project is a **full-cycle Business Intelligence analysis** of an Indonesian Shopee seller's transaction data from 2023 to 2025. The pipeline covers everything from raw data ingestion to an interactive 3-page Power BI dashboard with DAX-powered KPIs and business storytelling.

| Info | Detail |
|---|---|
| **Tool** | Microsoft Power BI Desktop |
| **Data Source** | Kaggle — Indonesia E-Commerce Sales & Shipping 2023–2025 |
| **Period** | December 2023 – November 2025 |
| **Domain** | Retail Analytics / E-Commerce |
| **Audience** | Business Owner, Marketing Team, Operations Manager |
| **Output** | Interactive 3-page Power BI Dashboard |

---

## 🏢 Business Background

The seller operates on **Shopee Indonesia**, one of Southeast Asia's largest e-commerce platforms, selling household products (piggy banks, bowls, door accessories, industrial seals, etc.) to customers nationwide.

With transactions growing across 24 months and orders coming from dozens of provinces, the business needed a **data-driven performance view** to move beyond spreadsheet-based decision making.

---

## ❗ Business Problem

> *"The seller has no centralized visibility into business performance — they don't know which products are most profitable, when peak sales occur, or which regions to prioritize for growth."*

Specific problems identified:

1. **Fragmented data** — spread across 24 separate monthly Excel files
2. **High cancellation rate** (~15%) with no root-cause analysis
3. **No geographic insight** to guide marketing or logistics decisions
4. **Unclear product strategy** — volume vs. profitability tradeoff unknown

---

## 🎯 Project Objectives

Three core business questions drive this analysis:

```
Q1 → Sales Performance & Trend
     How has revenue and order volume trended from 2023 to 2025?
     Which months are peak sales periods?

Q2 → Product & Category Analysis
     Which categories generate the highest revenue?
     Is there a Pareto pattern — 20% of products driving 80% of revenue?

Q3 → Geographic & Payment Behavior
     Which provinces are the largest markets?
     How do payment preferences differ across regions?
```

---

## 📂 Dataset Information

| Attribute | Detail |
|---|---|
| **Source** | [Kaggle — Indonesia E-Commerce Sales & Shipping](https://www.kaggle.com/datasets/bakitacos/indonesia-e-commerce-sales-and-shipping-20232025) |
| **Format** | ZIP → CSV (`all_months_clean.csv`) + 24 monthly Excel files |
| **Raw Rows** | 20,848 |
| **Clean Rows** | 18,868 (after removing null timestamps) |
| **Original Columns** | 19 |
| **Final Columns** | 23 (+ 4 engineered time columns) |

### Key Columns

| Column | Type | Description |
|---|---|---|
| `order_id` | Text | Unique transaction ID |
| `Total Pembayaran` | Number | Revenue per transaction |
| `Status Pesanan` | Text | Selesai / Batal / Sedang Dikirim |
| `product_categories` | Text | Product category |
| `Provinsi` | Text | Buyer province |
| `Metode Pembayaran` | Text | COD, ShopeePay, Bank Transfer, etc. |
| `Waktu Pesanan Dibuat` | DateTime | Transaction timestamp |

---

## 🔍 Data Understanding

### Data Profiling Results

| Column | Finding | Action |
|---|---|---|
| `order_id` | No duplicates — 20,848 distinct | ✅ No action needed |
| `Status Pesanan` | Inconsistent long-form values | ⚠️ Standardize |
| `Waktu Pesanan Dibuat` | ~9% null (~1,876 rows) | ⚠️ Remove nulls |
| `Total Pembayaran` | Values of 0 exist (cancelled orders) | ℹ️ Expected |
| `Total Diskon` | 99% = 0 (discounts rarely used) | ℹ️ Expected |
| `Alasan Pembatalan` | 86% null (completed orders have none) | ✅ Expected |

---

## 🧹 Data Preparation

All cleaning steps were performed in **Power Query Editor** — non-destructive, fully auditable via Applied Steps.

| # | Task | Method | Result |
|---|---|---|---|
| 1 | Standardize `Status Pesanan` | Replace Values | Removed long-form text → clean labels |
| 2 | Handle null `Alasan Pembatalan` | Replace null → "Tidak Ada" | No data loss |
| 3 | Remove null `Waktu Pesanan Dibuat` | Filter rows | 20,848 → 18,868 rows |
| 4 | Extract time components | Add Column (Date functions) | New: `Tahun`, `Bulan`, `Nama Bulan`, `Nama Hari` |
| 5 | Rename `source_file` | Column rename | → `Periode` |
| 6 | Standardize column names | Batch rename | 5 columns renamed for consistency |
| 7 | Validate data types | Type conversion | `Tahun` & `Bulan` → Whole Number |

---

## 🗂️ Data Model

The data model follows a **Star Schema** architecture — industry standard for Power BI analytics.

```
                    ┌─────────────────┐
                    │   Dim_Waktu     │
                    │─────────────────│
                    │ Waktu Pesanan   │
                    │ Tahun           │
                    │ Bulan           │
                    │ Nama Bulan      │
                    │ Nama Hari       │
                    └────────┬────────┘
                             │ 1
                             │
   ┌─────────────────┐       │       ┌─────────────────┐
   │   Dim_Produk    │       │       │   Dim_Lokasi    │
   │─────────────────│  *    │    *  │─────────────────│
   │ product_cat...  │───────┼───────│ Kota/Kabupaten  │
   │ Jumlah Kategori │       │       │ Provinsi        │
   └─────────────────┘       │       └─────────────────┘
                      ┌──────┴──────┐
                      │Fact_Penjualan│
                      │─────────────│
                      │ order_id     │
                      │ Total Pemb.. │
                      │ Total Qty    │
                      │ Total Diskon │
                      │ Status Pes.. │
                      │ Metode Pemb. │
                      │ Opsi Pengir. │
                      │ Provinsi     │
                      │ ... (23 col) │
                      └─────────────┘
```
*(/images/data-model.png)*
**Relationship Rules:** One-to-Many (1:*) · Filter: Single (Dim → Fact) · All Active

---

## 🔄 Project Workflow

```
  ┌─────────────┐
  │  Raw Data   │  20,848 rows · 24 Excel files · 1 CSV
  └──────┬──────┘
         ▼
  ┌─────────────┐
  │ Power Query │  Connect · Profile · Inspect
  └──────┬──────┘
         ▼
  ┌─────────────┐
  │   Cleaning  │  Standardize · Remove nulls · Fix types
  └──────┬──────┘
         ▼
  ┌──────────────────┐
  │Feature Engineering│  Extract: Tahun · Bulan · Nama Bulan · Nama Hari
  └──────┬───────────┘
         ▼
  ┌─────────────┐
  │ Star Schema │  Fact_Penjualan + 3 Dimension Tables
  └──────┬──────┘
         ▼
  ┌─────────────┐
  │     DAX     │  8 Measures · CALCULATE · DIVIDE · SAMEPERIODLASTYEAR
  └──────┬──────┘
         ▼
  ┌─────────────┐
  │  Dashboard  │  3 Pages · Slicers · Page Navigator · Text Box Insights
  └──────┬──────┘
         ▼
  ┌─────────────┐
  │   Insight   │  5 Key Findings · Business Interpretation
  └──────┬──────┘
         ▼
  ┌──────────────────┐
  │  Recommendation  │  5 Actionable Steps · Priority · Expected Impact
  └──────────────────┘
```

---

## 📊 Dashboard Preview

### Page 1 — Sales Overview
> High-level performance metrics, monthly revenue trend, year-over-year comparison, and order status distribution.

| Visual | Description |
|---|---|
| 4 KPI Cards | Total Revenue · Total Orders · Avg Order Value · Completion Rate |
| Line Chart | Monthly revenue trend by year (2023 / 2024 / 2025) |
| Bar Chart | Revenue per year comparison |
| Donut Chart | Order status distribution |
| Text Box | Key insights summary |

📸 *![Sales Overview](/images/sales-overview.png)*

---

### Page 2 — Product Analysis
> Category-level performance — which products drive revenue vs. volume, and AOV comparison.

| Visual | Description |
|---|---|
| Bar Chart | Top 10 categories by revenue |
| Bar Chart | Top 10 categories by order count |
| Bar Chart | Avg Order Value per category |
| Column Chart | Revenue per category per year (clustered) |
| Slicer | Year filter (Tile style) |
| Table | Top 5 products — Revenue · Orders · AOV |

📸 *![Product Analysis](/images/product-analysis.png)*

---

### Page 3 — Geographic Insight
> Regional sales distribution, market share treemap, and payment behavior by province.

| Visual | Description |
|---|---|
| Bar Chart | Top 10 provinces by revenue |
| Bar Chart | Top 10 cities by order count |
| Treemap | Market share % per province |
| Stacked Bar | Payment method distribution per province |
| Table | Province summary — Revenue · Orders · Cancellation Rate |
| Slicer | Province filter (Dropdown) |

📸 *![Geographic Insight](/images/geographic-insight.png)*

---

## 📈 KPI

### Business KPIs
*Metrics that directly reflect revenue outcomes and business growth.*

| KPI | Value | DAX Function | Description |
|---|---|---|---|
| **Total Revenue** | Rp958,948,812 | `SUM` | Total payment received across all transactions |
| **Total Orders** | 18,868 | `COUNTROWS` | Number of transactions after cleaning |
| **Average Order Value** | Rp50,870 | `DIVIDE` | Revenue per transaction |
| **Revenue YoY Growth** | — | `SAMEPERIODLASTYEAR` | Year-over-year revenue growth percentage |

### Operational KPIs
*Metrics that reflect fulfillment quality and operational efficiency.*

| KPI | Value | DAX Function | Description |
|---|---|---|---|
| **Order Completion Rate** | 85.0% | `CALCULATE` + `DIVIDE` | Percentage of successfully completed orders |
| **Cancellation Rate** | ~15.0% | `CALCULATE` + `DIVIDE` | Percentage of cancelled orders |
| **Total Qty Terjual** | — | `SUM` | Total units sold across all categories |
| **Total Diskon** | — | `SUM` | Total discount value given to buyers |

---

## 💡 Insights

### 1. Revenue Approaching Rp1 Billion Despite Low AOV

| | |
|---|---|
| **Finding** | Total revenue reached Rp958M from 18,868 transactions over 2 years |
| **Evidence** | Avg Order Value = Rp50,870 — low per-transaction value |
| **Business Meaning** | Business runs on a high-volume, low-ticket model — sustainable but vulnerable to order loss |
| **Recommendation** | Introduce product bundling to increase AOV without additional marketing cost |

---

### 2. Seal/Baut/Roof — The Hidden Revenue Champion

| | |
|---|---|
| **Finding** | Seal/Baut/Roof generates the highest revenue despite fewer orders than Celengan |
| **Evidence** | AOV Seal/Baut/Roof = **Rp424,029** vs Celengan = **Rp26,440** — a **16x difference** |
| **Business Meaning** | High-value categories disproportionately drive revenue — classic Pareto (80/20) pattern |
| **Recommendation** | Prioritize stock, visibility, and promotions for Seal/Baut/Roof to maximize margin |

---

### 3. 15% Cancellation Rate Is a Silent Revenue Leak

| | |
|---|---|
| **Finding** | 1 in 7 orders ends in cancellation — consistent across all periods |
| **Evidence** | Cancellation Rate = ~15% of all transactions |
| **Business Meaning** | Each cancelled order is lost revenue, wasted fulfillment cost, and a damaged customer experience |
| **Recommendation** | Analyze `Alasan Pembatalan` column; implement low-stock alerts and pre-order confirmation |

---

### 4. Jawa Barat Dominates — Other Provinces Are Untapped

| | |
|---|---|
| **Finding** | Jawa Barat leads in revenue, followed by Banten and DKI Jakarta |
| **Evidence** | Top 3 provinces (Jabodetabek area) account for the majority of total revenue |
| **Business Meaning** | Market concentration in one region = geographic risk if demand shifts |
| **Recommendation** | Launch targeted campaigns in Jawa Timur and Sumatera Selatan — both show growing order numbers |

---

### 5. COD Dominates — Digital Payment Adoption Is Still Developing

| | |
|---|---|
| **Finding** | COD is the most common payment method across nearly all top provinces |
| **Evidence** | COD appears as the largest segment in every province's payment stacked bar |
| **Business Meaning** | Customer trust in digital wallets is not yet universal — higher logistics risk for seller |
| **Recommendation** | Partner with ShopeePay for cashback incentives to gradually shift buyers to digital payments |

---

## 🎯 Recommendations

| # | Recommendation | Expected Impact | Priority |
|---|---|---|---|
| 1 | Investigate & reduce cancellation rate (15% → 8%) | +Rp75M estimated revenue recovery/year | 🔴 High |
| 2 | Increase stock & promotions for Seal/Baut/Roof | AOV lift from Rp51K → Rp70K (+37%) | 🔴 High |
| 3 | Launch campaigns in Jawa Timur & Sumatera Selatan | +2,000–3,000 new transactions/year | 🟡 Medium |
| 4 | Introduce ShopeePay buyer incentives | Reduce COD risk; faster payment settlement | 🟡 Medium |
| 5 | Create product bundles for low-AOV categories | Increase basket size per transaction | 🟢 Low |

---

## 💼 Business Impact

> ⚠️ *Estimated Scenario — based on analytical assumptions, not actual realized results. Numbers are projections if recommendations are implemented.*

```
📉 Scenario 1 — Reduce Cancellation Rate (15% → 8%)
   ├── Estimated orders recovered/year : ~1,311 transactions
   ├── Avg Order Value                 : Rp50,870
   └── Estimated Revenue Recovery      : ~Rp66,700,000/year

📈 Scenario 2 — Focus Promotions on High-Value Categories
   ├── Increase Seal/Baut/Roof orders by 20%
   ├── AOV maintained at Rp424,029
   └── Estimated Revenue Gain          : ~Rp50,000,000/year

🗺️ Scenario 3 — Geographic Expansion (3 new provinces)
   ├── Target: Jawa Timur · Sumatera Selatan · Kalimantan Timur
   ├── Estimated new transactions      : 2,000–3,000/year
   └── Estimated Revenue Gain          : ~Rp100M–Rp150M/year
```

---

## 🧠 Skills Demonstrated

| Category | Skills |
|---|---|
| **Data Engineering** | Data cleaning · Transformation · Feature engineering · Power Query (M Language) |
| **Data Modeling** | Star Schema · Fact & Dimension table design · Relationship management |
| **Analytics & DAX** | Calculated measures · CALCULATE · DIVIDE · SAMEPERIODLASTYEAR · Time intelligence |
| **Business Intelligence** | KPI design · Business vs operational metrics · Interactive dashboard design |
| **Analytical Thinking** | Data profiling · Pareto analysis · Problem framing · Insight generation |
| **Communication** | Data storytelling · Executive summary · Actionable recommendations · Impact estimation |

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **Microsoft Power BI Desktop** | Data modeling, DAX measures, interactive dashboard |
| **Power Query (M Language)** | Data cleaning, transformation, feature engineering |
| **DAX** | KPI calculations — SUM, COUNTROWS, CALCULATE, DIVIDE, SAMEPERIODLASTYEAR |
| **Microsoft Excel** | Source data format (24 monthly files) |
| **CSV** | Consolidated source file (`all_months_clean.csv`) |

---

## 📁 Repository Structure

```
📦 indonesia-ecommerce-sales-analysis/
│
├── 📂 dashboard/
│   └── Indonesia_Ecommerce_Sales_Dashboard.pbix
│
├── 📂 data/
│   └── all_months_clean.csv
│
├── 📂 docs/
│   └── data_dictionary.md
│
├── 📂 images/
│   ├── page1_sales_overview.png
│   ├── page2_product_analysis.png
│   └── page3_geographic_insight.png
│
├── 📄 README.md
└── 📄 LICENSE
```

---

## 🚀 Future Improvement

- [ ] **Forecasting** — Apply time-series forecast to project Q1 2026 revenue
- [ ] **RFM Segmentation** — Segment buyers by Recency, Frequency, and Monetary value
- [ ] **Cancellation Deep-Dive** — Build dedicated page analyzing `Alasan Pembatalan`
- [ ] **Real-time Refresh** — Publish to Power BI Service with scheduled refresh
- [ ] **Cohort Analysis** — Track buyer retention by first-purchase month
- [ ] **City-level Drill-through** — Add granular geographic analysis at city level

---

## 👤 About Me

<div align="center">

### Pandu Kukuh Waskito Wibowo

*Informatics Engineering Student · Universitas Widyatama*

Passionate about **Data Science**, **Business Intelligence**, and **Machine Learning**  
— building real-world projects to bridge the gap between raw data and business decisions.

<br>

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/pandu-wibowo)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/panduwibowo-eng)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](pandu.k.wibowo@gmail.com)
[![Portfolio](https://img.shields.io/badge/Portfolio-FF5722?style=for-the-badge&logo=google-chrome&logoColor=white)](https://your-portfolio.com)
[![CV](https://img.shields.io/badge/Download%20CV-4CAF50?style=for-the-badge&logo=adobeacrobatreader&logoColor=white)](https://link-to-your-cv.pdf)

<br>

*Open to Data Analyst · Business Intelligence · Data Science opportunities*

</div>

---

<div align="center">

**⭐ If you found this project useful, please consider giving it a star!**

*Made with ❤️ using Power BI · Part of my Data Analytics Portfolio*

</div>
