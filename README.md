# 🏢 Enterprise Data Analysis & ERP Module Simulation

## 📌 Project Overview

A business was sitting on 100,000+ orders spread across 7 separate tables — with no visibility into revenue drivers, customer behaviour, or operational performance across its Sales, Inventory, and Customer modules.

This project builds an **end-to-end ERP data analysis pipeline** — mirroring the structure of SAP SD (Sales & Distribution), MM (Materials Management), CRM (Customer Relationship Management), and FI (Financial Accounting) modules — using Open SQL-compatible queries (MySQL), Python automation, and structured business reporting.

The pipeline covers two stages:
- **Stage 1** (`csv_to_mysql.ipynb`) — automated CSV ingestion into MySQL with dynamic schema creation, NULL handling, and data type inference
- **Stage 2** (`ecommerce_sql_python.ipynb`) — 12 structured SQL queries across all ERP modules, 5 business report visualisations, and executive-ready insights

---

## 🛠 Tools & Technologies

| Tool | Purpose |
|---|---|
| MySQL | Database storage and Open SQL-compatible data extraction |
| Python | Data pipeline automation, ingestion, and report generation |
| pandas | Data manipulation, cleaning, NULL handling, and transformation |
| matplotlib | Business report visualisations (line, bar, area, pie charts) |
| seaborn | Statistical plots and order volume heatmaps |
| Jupyter Notebook | Interactive development and technical documentation environment |
| mysql-connector-python | Python ↔ MySQL integration bridge |

---

## 🗂 Dataset — ERP Module Mapping

**Brazilian Ecommerce Dataset** — 7 interconnected tables, 100,000+ records

Each table maps to a functional SAP ERP module — the same data structures an ABAP developer writes SELECT queries against:

| Table | Rows | SAP Module Equivalent | Description |
|---|---|---|---|
| customers | ~100K | SAP CRM — Customer Master | Customer IDs, cities, states |
| orders | ~100K | SAP SD — Sales Orders | Order timestamps, status, lifecycle |
| order_items | ~115K | SAP SD — Line Items / SAP MM — Inventory | Products, sellers, prices per line |
| products | ~33K | SAP MM — Material Master | Category, dimensions, attributes |
| payments | ~104K | SAP FI — Financial Accounting | Payment type, value, installments |
| sellers | ~3K | SAP SD — Partner Functions | Seller location and revenue performance |
| geolocation | ~1M | SAP Basis — Regional Master Data | Zip codes, coordinates |

---

## ⚙️ Stage 1 — Data Ingestion Pipeline (`csv_to_mysql.ipynb`)

Automated ingestion of all 7 CSV files into MySQL with:
- Dynamic `CREATE TABLE` generation using pandas dtype inference (`INT`, `FLOAT`, `DATETIME`, `TEXT`)
- Automatic NULL handling — `NaN` replaced with SQL `NULL` before insertion
- Column name sanitisation — spaces, hyphens, and dots replaced with underscores
- Per-file transaction commits for data integrity
- Iterative `INSERT` with parameterised queries to prevent SQL injection

This mirrors the data loading and preprocessing work performed before ABAP report development — ensuring clean, structured data in the database before any SELECT queries are written.

---

## 🔍 Stage 2 — ERP Module Analysis (`ecommerce_sql_python.ipynb`)

All SQL queries use Open SQL-compatible syntax (MySQL), following the same structured data extraction logic used in SAP ABAP report development. Every query uses `JOIN`, aggregation, or window functions across multiple tables — equivalent to cross-module reporting in SAP.

### Module 1 — Sales & Revenue Reporting (SAP SD equivalent)
> Structured revenue extraction — mirrors SD module report queries

| # | Query | SQL Technique |
|---|---|---|
| Q1 | Total sales per product category | Multi-table JOIN + GROUP BY + SUM |
| Q5 | Revenue percentage per product category | Subquery + percentage calculation |
| Q6 | Seller revenue ranked | Window function: DENSE_RANK() OVER |
| Q8 | Cumulative sales per month per year | Window function: SUM() OVER |
| Q9 | Year-over-year growth rate | CTE + LAG() window function |

### Module 2 — Customer Analytics (SAP CRM equivalent)
> Customer master data queries and retention reporting

| # | Query | SQL Technique |
|---|---|---|
| Q2 | Customer count by state | GROUP BY + COUNT + visualisation |
| Q7 | Moving average of order values per customer | Window function: AVG() OVER with ROWS BETWEEN |
| Q10 | Customer retention rate (6-month window) | Double CTE + DATE_ADD + LEFT JOIN |
| Q11 | Top 3 customers by spend per year | DENSE_RANK() OVER PARTITION BY year |

### Module 3 — Operations & Order Management (SAP MM/FI equivalent)
> Order processing and payment behaviour reporting

| # | Query | SQL Technique |
|---|---|---|
| Q3 | Percentage of orders paid in installments | CASE WHEN + conditional aggregation |
| Q4 | Order volume per month in 2018 | YEAR/MONTHNAME filters + seaborn barplot |

---

## 📊 Business Report Visualisations

5 report-style charts produced as executive deliverables — equivalent to SAP ABAP Form and Report output:

| # | Report | Chart Type | Key Finding |
|---|---|---|---|
| V1 | Monthly Revenue Trend (2016–2018) | Line Chart | Consistent growth; peak in late 2017 |
| V2 | Order Volume Heatmap by Month & Year | Heatmap | Mid-year months show peak demand |
| V3 | Cumulative Revenue Growth Over Time | Area Chart (fill_between) | Crossed 16M BRL by mid-2018 |
| V4 | Top 10 Product Categories by Revenue | Horizontal Bar | Bed Table Bath leads at 10.7% of revenue |
| V5 | Payment Type Distribution | Pie Chart | ~74% of transactions via credit card |

---

## 📈 Key Business Insights

- **Bed Table Bath** is the top revenue category — contributing **10.7%** of total revenue *(SD module finding)*
- **99.99%** of orders involve installment payments — critical configuration input for FI module *(FI module finding)*
- Revenue grew **~20% year-over-year** from 2017 to 2018 — positive SD KPI trend *(SD module finding)*
- **Customer retention is critically low** — majority of customers are one-time buyers, a key CRM risk flag *(CRM module finding)*
- **São Paulo** has the highest customer concentration of any state *(CRM + MM regional planning)*
- **Mid-year months (May–August)** consistently show the highest order volumes *(MM demand planning)*

---

## 💡 ABAP Skills Demonstrated

This project directly mirrors the core technical responsibilities of a Junior SAP ABAP Developer:

| ABAP Responsibility | Demonstrated In This Project |
|---|---|
| Writing Open SQL SELECT queries | 12 structured queries using JOIN, GROUP BY, CTEs, window functions |
| Developing data extraction reports | Full SD / CRM / MM / FI reporting suite |
| Working with SAP module data structures | Explicit module mapping across all 7 tables |
| Debugging and troubleshooting | NaN debugging in ingestion pipeline; iterative query refinement |
| Data type handling | Dynamic dtype inference: INT / FLOAT / DATETIME / TEXT |
| Preparing technical documentation | Structured README, query reference docs, chart reference docs |
| Supporting cross-module integration | Cross-table JOINs bridging customer, order, payment, product data |
| Following coding standards | Consistent query structure, naming conventions, parameterised queries |

---

## ▶️ How to Run

### Step 1 — Clone the repository
```bash
git clone https://github.com/arpitasarkardata/sql-python-ecommerce-analysis.git
```

### Step 2 — Install dependencies
```bash
pip install pandas mysql-connector-python matplotlib seaborn
```

### Step 3 — Set up MySQL
```sql
CREATE DATABASE ecommerce;
```

### Step 4 — Run Stage 1: Data Ingestion
Open `csv_to_mysql.ipynb` and update credentials in Cell 1:
```python
host = "localhost"
user = "your_username"
password = "your_password"
database = "ecommerce"
folder_path = "path_to_your_csv_folder"
```
Run all cells — this creates all 7 tables and loads all CSV data automatically.

### Step 5 — Run Stage 2: Analysis
Open `ecommerce_sql_python.ipynb` and update credentials in Cell 1:
```python
host = "localhost"
user = "root"
password = "your_password"
database = "ecommerce"
```
Run **Kernel → Restart & Run All**

---

## 📁 Repository Structure

```
sql-python-ecommerce-analysis/
    csv_to_mysql.ipynb              ← Stage 1: Automated CSV ingestion pipeline
    ecommerce_sql_python.ipynb      ← Stage 2: ERP module analysis & visualisations
    questions.docx                  ← All 12 SQL business queries with module reference
    charts.docx                     ← All 5 business report visualisations with code
    README.md                       ← This file
```

---

## 📂 Dataset Source

Brazilian Ecommerce Public Dataset — available on [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

---

*Built by Arpita Sarkar — B.Tech ECE, Kalyani Government Engineering College (MAKAUT), 2025*  
*GitHub: [arpitasarkardata](https://github.com/arpitasarkardata) | Contact: arpitasarkarkgec@gmail.com*
