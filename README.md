# 📊 Electronic Sales Business Performance Report
### Power BI | Time Intelligence Analytics | 2011–2014 | Nigeria (31 States)

> A comprehensive business intelligence dashboard analysing electronic sales performance across revenue, profitability, growth trends, geographic reach, product categories, and sales channels — built with Power BI and DAX time intelligence measures.

---

## Project Overview

This Power BI project provides a full-period business performance analysis of an electronics retail operation spanning **2011 to 2014** across **31 Nigerian states**. It was designed to give both technical analysts and non-technical stakeholders a clear picture of:

- How the business has grown (or declined) year-over-year
- Which products, states, and channels are driving the most revenue
- Where costs are concentrated and how profit margins hold up
- How current performance compares to the same period last year

The report uses **9 custom DAX measures**, including 5 time intelligence functions, to surface meaningful trends beyond simple aggregations.

---

## Dashboard Previews

| Page | Description |
|------|-------------|
| **Summarized Measures** | KPI card view of all 9 core measures at a glance |
<img width="595" height="332" alt="Screenshot_measures" src="https://github.com/user-attachments/assets/764e73f9-addd-4ae8-923f-73589d8bba64" />

| **Yearly Sales Visualization (1)** | Promotions, product sub-category, yearly trends, and state-level bar charts |
<img width="607" height="338" alt="Screenshot_sales" src="https://github.com/user-attachments/assets/63428973-c364-4d5e-a736-4cb4190aa512" />

| **Yearly Sales Visualization (2)** | Zone cost distribution, channel performance, year-over-year pie chart, and top KPIs |
<img width="587" height="331" alt="Screenshot_sales vs cost" src="https://github.com/user-attachments/assets/c99b2a3b-54d9-421c-87eb-4ceb963bb6dc" />

> 📁 The `.pbix` file is included in this repository. Open with [Power BI Desktop](https://powerbi.microsoft.com/desktop/).

---

## Key Performance Metrics

| # | Measure | Value |
|---|---------|-------|
| 1 | Total Sales | **$56.25M** |
| 2 | Total Cost | **$24.00M** |
| 3 | Profit | **$32.45M** |
| 4 | Profit Margin | **58%** |
| 5 | Sales YTD | **$16.00M** |
| 6 | Same Period Last Year | **$40.25M** |
| 7 | Sales YOY | **$16.00M** |
| 8 | Sales YOY % | **0.40%** |
| 9 | Sales Last Month | **$56.09M** |

> A **58% profit margin** significantly exceeds typical retail electronics benchmarks of 20–30%, indicating strong pricing power and cost discipline.

---

## DAX Measures

All nine measures are written in DAX and rely on a properly configured `Date` table with continuous date coverage.

### 1. Total Sales
```dax
1. Total Sales = SUM(Sales[Sales])
```
Aggregates all revenue across every transaction in the Sales table.

---

### 2. Total Cost
```dax
2. Total Cost = SUM(Sales[Total Cost])
```
Sums all associated costs — product, logistics, and operational — for every sale.

---

### 3. Profit
```dax
3. Profit = [1. Total Sales] - [2. Total Cost]
```
Simple derived measure: revenue minus total cost. The net financial return to the business.

---

### 4. Profit Margin
```dax
4. Profit Margin = DIVIDE([3. Profit], [1. Total Sales])
```
Uses `DIVIDE()` instead of `/` to safely handle zero-denominator scenarios. Returns the proportion of each sale that becomes profit.

---

### 5. Sales YTD *(Time Intelligence)*
```dax
5. Sales YTD = TOTALYTD([1. Total Sales], 'Date'[Date])
```
`TOTALYTD` accumulates sales from the first day of the current year to the last date in the current filter context. Requires a **marked Date table** in the data model.

---

### 6. Same Period Last Year *(Time Intelligence)*
```dax
6. Same Period LY = CALCULATE([1. Total Sales], SAMEPERIODLASTYEAR('Date'[Date]))
```
`SAMEPERIODLASTYEAR` shifts the date context back exactly 12 months, enabling a like-for-like comparison against the prior year's equivalent period.

---

### 7. Sales YOY *(Time Intelligence)*
```dax
7. Sales YOY = [1. Total Sales] - [6. Same Period LY]
```
Computes the absolute dollar difference between the current period and the same period last year. Positive = growth; negative = decline.

---

### 8. Sales YOY % *(Time Intelligence)*
```dax
8. Sales YOY % = DIVIDE([7. Sales YOY], [6. Same Period LY])
```
Expresses the year-over-year change as a percentage of last year's base. A result of `0.40` means **40% growth**.

---

### 9. Sales Last Month *(Time Intelligence)*
```dax
9. Sales Last Month = CALCULATE([1. Total Sales], PARALLELPERIOD('Date'[Date], -1, MONTH))
```
`PARALLELPERIOD` with `-1, MONTH` shifts the filter context back by one complete month. Unlike `DATEADD`, this returns the full prior month regardless of where the current filter sits within a month.

---

## Data Model

```
Sales (Fact Table)
├── Sales[Sales]          → Revenue per transaction
├── Sales[Total Cost]     → Cost per transaction
├── Sales[Order Date]     → Links to Date table
└── Sales[...]            → Product, State, Channel, Zone, Promotion

Date (Dimension Table — marked as Date Table)
└── Date[Date]            → Continuous date column (no gaps)

Product (Dimension)
├── Product Sub Category
└── Product Category

Geography (Dimension)
├── State
└── Zone

Channel (Dimension)
└── Channel Name

Promotion (Dimension)
└── Promotion Name
```

> ⚠️ **Important:** Time intelligence functions (`TOTALYTD`, `SAMEPERIODLASTYEAR`, `PARALLELPERIOD`) require the `Date` table to be **marked as a Date Table** in Power BI and must have **no gaps** in date coverage.

---

## Key Insights

### ✅ Strengths
- **58% profit margin** — exceptional efficiency, well above industry benchmarks
- **Diversified product portfolio** — no single category dominates or underperforms
- **Full-price sales dominate** — promotions are supplementary, not load-bearing
- **Multi-channel presence** — Store ($32.1M) and Online ($11.6M) both performing strongly
- **40% YOY growth** — significant business momentum across the period

### ⚠️ Areas to Watch
- **South-South zone** carries the highest unit costs — logistics review recommended
- **Sales peaked in 2012** and have stabilised — a re-acceleration strategy is needed
- **Online trails physical stores** significantly — digital investment could unlock growth
- **Smaller states** contribute minimally — review whether distribution costs are justified

### 📋 Top States by Revenue
`Ebonyi` → `Kwara` → `Delta` → `Edo` → `Nasarawa`

### 📦 Top Channels by Revenue
`Store ($32.1M)` → `Online ($11.6M)` → `Reseller ($7.2M)` → `Catalog ($5M)`

### 📅 Orders by Year
| Year | Share |
|------|-------|
| 2012 | 37.60% |
| 2013 | 31.29% |
| 2014 | 27.89% |
| 2011 | 0.02% |

---

## Report Structure

```
📁 Electronic-Sales-BI-Report/
├── 📄 README.md                          ← This file
├── 📊 Eletronic_sales_analysis_PBI.pbix  ← Power BI project file
├── 📄 Electronic_Sales_Stakeholder_Report.docx  ← Non-technical stakeholder report
└── 📁 screenshots/
    ├── summarized_measures.png
    ├── yearly_sales_visualization_1.png
    └── yearly_sales_visualization_2.png
```

---

## Tools Used

| Tool | Purpose |
|------|---------|
| **Power BI Desktop** | Dashboard development and DAX authoring |
| **DAX** | All KPI calculations and time intelligence measures |
| **Power Query (M)** | Data transformation and loading |
| **Microsoft Excel / CSV** | Source data preparation |

---

## Author

**Victor Umeh**
*Business Intelligence & Data Analytics*

📧 Feel free to reach out for questions, collaboration, or feedback on this project.

---

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Time%20Intelligence-0078D4?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-2D7A2D?style=for-the-badge)

