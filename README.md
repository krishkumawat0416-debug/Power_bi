[README (2).md](https://github.com/user-attachments/files/27549470/README.2.md)
<div align="center">

<img src="https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black" />
<img src="https://img.shields.io/badge/Microsoft-0078D4?style=for-the-badge&logo=microsoft&logoColor=white" />
<img src="https://img.shields.io/badge/DAX-FF6B6B?style=for-the-badge&logo=databricks&logoColor=white" />
<img src="https://img.shields.io/badge/Status-Complete-00C853?style=for-the-badge" />

# 🛒 RetailPulse — Power BI Analytics Dashboard

### *A full-scale, end-to-end retail analytics solution for a multi-store Indian retail chain*

<br/>

![Revenue](https://img.shields.io/badge/Total%20Revenue-₹1.07%20Cr-6C63FF?style=flat-square)
![Profit](https://img.shields.io/badge/Total%20Profit-₹50.3%20L-00C853?style=flat-square)
![Orders](https://img.shields.io/badge/Orders-2%2C500-FF6B6B?style=flat-square)
![Margin](https://img.shields.io/badge/Profit%20Margin-46.66%25-F2C811?style=flat-square&logoColor=black)
![Period](https://img.shields.io/badge/Period-Jan%202023%20–%20Dec%202024-0078D4?style=flat-square)

</div>

---

## 📋 Table of Contents

- [🔭 Project Overview](#-project-overview)
- [📁 Dataset Summary](#-dataset-summary)
- [📊 Key Business Metrics](#-key-business-metrics)
- [🏗️ Project Architecture](#️-project-architecture)
- [🔢 Phase Breakdown](#-phase-breakdown)
- [⭐ Star Schema Design](#-star-schema-design)
- [📺 Dashboard Pages](#-dashboard-pages)
- [🧮 DAX Measures](#-dax-measures)
- [⚡ Challenges Faced](#-challenges-faced)
- [📂 File Structure](#-file-structure)
- [🛠️ Tools & Technologies](#️-tools--technologies)
- [👤 Author](#-author)

---

## 🔭 Project Overview

**RetailPulse** is a comprehensive Power BI portfolio project simulating a real-world retail analytics scenario for a multi-store chain operating across India. The project spans a full two-year period **(January 2023 – December 2024)** and covers the complete Power BI development workflow.

> 💡 **Goal:** Build a production-quality analytics solution that answers critical business questions around revenue, product profitability, customer behavior, store operations, and workforce efficiency.

---

## 📁 Dataset Summary

| File | Type | Rows | Description |
|------|------|:----:|-------------|
| `sales.csv` | 🟥 Fact Table | 2,500 | Core transaction records |
| `returns.csv` | 🟥 Fact Table | 199 | Product return records |
| `targets.csv` | 🟥 Fact Table | 240 | Monthly sales targets per store |
| `customers.csv` | 🟦 Dimension | 300 | Customer demographics & segments |
| `products.csv` | 🟦 Dimension | 35 | Product catalog with categories |
| `stores.csv` | 🟦 Dimension | 10 | Store locations across 4 regions |
| `employees.csv` | 🟦 Dimension | 60 | Employee roles & departments |
| `dates.csv` | 🟩 Date Dimension | 731 | Calendar table (2023–2024) |
| `categories.json` | 🟨 Lookup | 5 | Product category hierarchy |
| `regions.json` | 🟨 Lookup | 6 | Regional management hierarchy |

**Coverage:** Jan 2023 – Dec 2024 &nbsp;|&nbsp; **Regions:** North · South · East · West &nbsp;|&nbsp; **Domain:** Multi-store Retail, India

---

## 📊 Key Business Metrics

<div align="center">

| 💰 Metric | 📈 Value |
|:----------|:--------:|
| Total Revenue | **₹1,07,82,463** |
| Total Profit | **₹50,30,663** |
| Avg Profit Margin | **46.66%** |
| Total Orders | **2,500** |
| Return Rate | **7.96%** (199 returns) |
| Stores | **10** |
| Employees | **60** |
| Products | **35** across 5 categories |
| Customers | **300** unique |

</div>

---

## 🏗️ Project Architecture

```
📂 Raw Data Files (CSV + JSON)
        │
        ▼
┌─────────────────────────┐
│  ⚙️  Power Query (ETL)  │  ← Fix types · Clean nulls · Derive columns · Merge tables
└─────────────────────────┘
        │
        ▼
┌─────────────────────────┐
│  ⭐ Star Schema Model   │  ← 9 relationships · RLS · Hierarchies · Calc columns
└─────────────────────────┘
        │
        ▼
┌─────────────────────────┐
│  🧮 DAX Measures        │  ← KPIs · Profit Margin · USERELATIONSHIP
└─────────────────────────┘
        │
        ▼
┌─────────────────────────┐
│  📺 Dashboard (6 Pages) │  ← Executive · Sales · Products · Customers · Stores · Ops
└─────────────────────────┘
```

---

## 🔢 Phase Breakdown

### 🟦 Phase 1 — Foundation *(Days 1–3)*

- Installed and configured **Power BI Desktop**
- Disabled **Auto Date/Time** globally — prevents hidden table bloat across date columns
- Created a dedicated **Measures table** using `Measures = {BLANK()}` — industry standard for organizing measures
- Loaded all **10 source files** into Power Query Editor via *Transform Data* (not Load)
- Documented all data quality issues per table before any cleaning

---

### 🟨 Phase 2 — Power Query & Data Transformation *(Days 4–7)*

All 10 files cleaned and enriched:

| Table | Transformations Applied | New Columns Added |
|-------|------------------------|-------------------|
| `sales` | OrderDate → Date type | Discount%, SaleYear, SaleMonth, ProfitMargin%, **DiscountTier** |
| `customers` | Remove duplicates · Capitalize city/state · Lowercase email · Filter null Age | — |
| `products` | **Left Outer Join** with categories.json on CategoryID | CategoryName, Description |
| `stores` | OpenDate → Date type | Country, YearsOpen, **StoreLocation** |
| `employees` | Fix typo in Role · Trim whitespace · HireDate → Date · Remove zero salary | — |
| `dates` | Date → Date type · IsWeekend → True/False | **FiscalYearLabel**, MonthShort |
| `returns` | ReturnDate → Date type | — |
| `targets` | — | **DateKey** (#date formula), MonthYearLabel |
| `categories.json` | JSON list expansion | — |
| `regions.json` | JSON list expansion | — |

> ✅ A **Right Anti Join** data quality check (`CategoryCheck` query, load disabled) confirmed zero orphan CategoryIDs across products.

---

### 🟩 Phase 3 — Data Modeling *(Days 8–10)*

Built a full **Star Schema** with `sales` as the central fact table:

- ✅ Created all **9 relationships** — 8 active (solid) + 1 inactive (dashed)
- ⚠️ `returns → dates` is **intentionally INACTIVE** — only one active path allowed between two tables; activated in DAX via `USERELATIONSHIP()`
- ✅ Hidden all foreign key columns from Report view
- ✅ Fixed **MonthName sort order** → Sort by Column → Month (integer)
- ✅ Created **3 drill-down hierarchies**: Date · Product · Geography
- ✅ Added calculated columns: `AgeGroup` · `StoreLabel` · `FullCategory`
- ✅ Implemented **Row-Level Security**: role `North Region Manager` → `[Region] = "North"`

---

### 🟥 Phase 4 — Dashboard & Visualizations *(Days 11+)*

Built a **6-page interactive dashboard** with **30 visuals** total — connected via cross-filtering and slicers.

---

## ⭐ Star Schema Design

```
                    ┌──────────────┐
                    │  categories  │
                    └──────┬───────┘
                           │ CategoryID
                    ┌──────▼───────┐       ┌───────────┐
                    │   products   │       │  regions  │
                    └──────┬───────┘       └─────┬─────┘
                           │ ProductID           │ Region
┌───────────┐    ┌─────────▼──────────┐   ┌─────▼──────┐
│ customers │◄───│                    │   │   stores   │
└───────────┘    │    sales ⭐         │──►│            │
┌───────────┐◄───│   (Fact Table)     │   └─────▲──────┘
│ employees │    │                    │         │
└───────────┘    └─────────┬──────────┘   ┌─────┴──────┐
                           │ OrderDate    │  targets   │
                    ┌──────▼───────┐      └────────────┘
                    │    dates     │◄ ─ ─ ─  returns
                    │  (Date Dim)  │       (⚠️ INACTIVE)
                    └──────────────┘
```

| # | From | Column | To | Column | Status |
|---|------|--------|----|--------|:------:|
| 1 | sales | CustomerID | customers | CustomerID | ✅ Active |
| 2 | sales | ProductID | products | ProductID | ✅ Active |
| 3 | sales | StoreID | stores | StoreID | ✅ Active |
| 4 | sales | EmployeeID | employees | EmployeeID | ✅ Active |
| 5 | sales | OrderDate | dates | Date | ✅ Active |
| 6 | returns | ReturnDate | dates | Date | ⚠️ Inactive |
| 7 | targets | StoreID | stores | StoreID | ✅ Active |
| 8 | targets | DateKey | dates | Date | ✅ Active |
| 9 | products | CategoryID | categories | CategoryID | ✅ Active |

---

## 📺 Dashboard Pages

| # | Page | Visuals |
|---|------|---------|
| 1 | 🏢 **Executive Summary** | KPI cards — Total Revenue · Profit · Orders · Margin |
| 2 | 📈 **Sales Analysis** | Monthly trend · Category bar · Regional donut · Year slicer |
| 3 | 🛍️ **Product Analysis** | Top products · Category mix · Price vs Profit scatter · Matrix |
| 4 | 👥 **Customer Insights** | Age distribution · AgeGroup chart · Gender comparison · KPI |
| 5 | 🏪 **Store Performance** | Sales vs Target gauge · Store column · Returns matrix · Map |
| 6 | ⚙️ **Operations & HR** | Discount impact · Margin heatmap · Dept headcount · Return funnel |

---

## 🧮 DAX Measures

```dax
-- 💰 Total Revenue
Total Revenue = SUM(sales[TotalSales])

-- 📈 Total Profit
Total Profit = SUM(sales[Profit])

-- 🧾 Total Orders
Total Orders = COUNTROWS(sales)

-- 📊 Profit Margin
Profit Margin Card = DIVIDE([Total Profit], [Total Revenue], 0)

-- 👥 Total Customers
Total Customers = DISTINCTCOUNT(sales[CustomerID])

-- 🔁 Returns by actual return date (activates inactive relationship)
Returns by Return Date =
CALCULATE(
    SUM(returns[QuantityReturned]),
    USERELATIONSHIP(returns[ReturnDate], dates[Date])
)
```

---

## ⚡ Challenges Faced

### 1. 🔕 Auto Date/Time hidden table bloat
Power BI creates one hidden date table per date column by default. With 5+ date columns, this added invisible tables that inflated memory and slowed every visual. Disabling it globally before any data load was a critical first step discovered early in the process.

### 2. 🔗 Inactive relationship — returns vs dates
Power BI enforces a single active relationship between any two tables. Both `sales` and `returns` needed to connect to `dates`, but only one could be active. Understanding that `USERELATIONSHIP()` inside a DAX measure temporarily activates the inactive path — and why that is the correct architectural pattern — was a key conceptual milestone.

### 3. 📅 MonthName alphabetical sorting
Every monthly chart initially showed months as April, August, December, February... because Power BI sorts text columns alphabetically by default. The fix (Sort by Column → Month integer in Data view) is easy to miss entirely, yet it makes every time-series chart meaningful. One small setting, huge visual impact.

### 4. 🔀 Merging JSON lookups into CSV dimensions
Connecting `categories.json` to `products.csv` via a Left Outer Join in Power Query required understanding M language join semantics — particularly the expand step and unchecking "Use original column name as prefix." The follow-up Right Anti Join data quality check to verify no orphan CategoryIDs was a professional practice added on top.

### 5. 📆 targets.csv had no date column
The targets table had separate Year and Month integer columns but no actual Date column, making a relationship to the dates table impossible. Constructing a `DateKey` column using `#date([Year], [Month], 1)` — always anchoring to the 1st of each month — was the solution. Without this, all target-vs-actual comparisons would have failed silently.

### 6. 🔒 XPress9 compressed DataModel binary
The `.pbix` file's DataModel is stored in Microsoft's proprietary XPress9 compression format — not readable outside Power BI Desktop. This means relationship metadata, RLS rules, and DAX expressions cannot be extracted programmatically. A known architectural limitation of the `.pbix` format for external auditing and version control.

### 7. 🛡️ Row-Level Security testing workflow
Creating the RLS role with DAX filter `[Region] = "North"` was straightforward. The challenge was understanding that Power BI Desktop only *simulates* RLS via "View as Role" — actual enforcement only happens after publishing to Power BI Service with user email assignments. A critical distinction between development and production behavior.

---

## 📂 File Structure

```
RetailPulse/
│
├── 📁 Data/
│   ├── sales.csv
│   ├── customers.csv
│   ├── products.csv
│   ├── stores.csv
│   ├── employees.csv
│   ├── dates.csv
│   ├── returns.csv
│   ├── targets.csv
│   ├── categories.json
│   └── regions.json
│
├── 📁 Docs/
│   ├── Phase1_Foundation.docx
│   ├── Phase2_PowerQuery.docx
│   └── Phase3_DataModeling.docx
│
├── 📊 RetailPulse_PowerBI_Final.pbix
└── 📄 README.md
```

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| ![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat-square&logo=powerbi&logoColor=black) | Dashboard development, data modeling, DAX |
| ![Power Query](https://img.shields.io/badge/Power%20Query-0078D4?style=flat-square&logo=microsoft&logoColor=white) | ETL — data cleaning and transformation |
| ![DAX](https://img.shields.io/badge/DAX-FF6B6B?style=flat-square) | Measures, calculated columns, KPIs |
| ![Star Schema](https://img.shields.io/badge/Star%20Schema-6C63FF?style=flat-square) | Data modeling pattern for analytics |
| ![RLS](https://img.shields.io/badge/Row--Level%20Security-00C853?style=flat-square) | Regional data access control |
| ![CSV/JSON](https://img.shields.io/badge/CSV%20%2F%20JSON-F2C811?style=flat-square&logoColor=black) | Source data formats |

---

## 👤 Author

<div align="center">

### Krish Kumawat

![Made with](https://img.shields.io/badge/Made%20with-❤️%20%26%20Power%20BI-F2C811?style=for-the-badge)
![Domain](https://img.shields.io/badge/Domain-Data%20Analytics-6C63FF?style=for-the-badge)

*Power BI Developer · Data Analytics Enthusiast*

Built as a structured, phased learning project covering the full Power BI development lifecycle — from raw data to production-ready dashboard.

---

> *"Data is only as useful as the story it tells."*

</div>
