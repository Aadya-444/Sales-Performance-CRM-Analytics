# Sales Performance & Customer Insights Dashboard (CRM Analytics)

A Power BI CRM analytics project built to analyze sales performance, sales-team effectiveness, customer segments, and product performance using a multi-table B2B sales opportunities dataset.

The project demonstrates end-to-end BI workflow skills including **Power Query data cleaning, data modeling, DAX measures, KPI design, interactive filtering, conditional formatting, and multi-page dashboard development**.

---

## Dashboard Overview

The report contains three analytical pages:

1. **Executive Sales Overview** — overall sales and pipeline performance
2. **Sales Team Performance** — agent and manager effectiveness
3. **Customer & Product Insights** — customer, sector, and product contribution

---

## Key Business KPIs

| KPI | Value |
|---|---:|
| Total Won Revenue | **$10.01M** |
| Won Deals | **4,238** |
| Win Rate | **63.2%** |
| Average Deal Value | **$2.36K** |
| Open Opportunities | **2,089** |
| Unique Customers | **85** |
| Top Customer Revenue | **$341.46K** |
| Top Product Revenue | **$3.51M** |

> Values reflect the full dataset with dashboard slicers cleared.

---

## Dashboard Pages

### 1. Executive Sales Overview

Provides a high-level view of CRM and sales performance.

**Includes:**
- Won Revenue
- Won Deals
- Win Rate
- Average Deal Value
- Open Opportunities
- Total Opportunities
- Monthly Won Revenue trend
- Opportunities by Deal Stage
- Revenue by Product
- Revenue by Regional Office
- Region, Product, and Quarter slicers

![Executive Sales Overview](Screenshots/1_Executive%20Sales%20Overview.png)

---

### 2. Sales Team Performance

Analyzes individual sales agents and managers to identify top performers and areas for improvement.

**Includes:**
- Won Revenue
- Won Deals
- Win Rate
- Average Deal Value
- Average Sales Cycle
- Top 10 Sales Agents by Won Revenue
- Agent Performance: Win Rate vs Won Revenue
- Won Revenue by Manager
- Sales Agent leaderboard
- Conditional formatting for revenue, win rate, and sales cycle
- Region, Manager, and Quarter slicers

![Sales Team Performance](Screenshots/2_Sales%20Team%20Performance.png)

---

### 3. Customer & Product Insights

Focuses on the customer segments and products driving CRM performance.

**Includes:**
- Won Revenue
- Unique Customers
- Average Deal Value
- Top Customer Revenue
- Top Product Revenue
- Top 10 Customers by Won Revenue
- Won Revenue by Customer Sector
- Product Performance: Win Rate vs Won Revenue
- Customer performance table
- Sector, Product, Region, and Quarter slicers

![Customer & Product Insights](Screenshots/3_Customer%20&%20Product%20Insights.png)

---

## Key Insights

- **GTX Pro** is the highest-revenue product, generating approximately **$3.51M** in won revenue.
- **Kan-code** is the highest-value customer, contributing approximately **$341K** in won revenue.
- **Retail** is the strongest customer sector, contributing approximately **$1.87M** in won revenue.
- **Technology** and **Medical** are also major revenue-generating sectors, contributing approximately **$1.52M** and **$1.36M** respectively.
- The overall CRM pipeline achieved a **63.2% win rate** across closed opportunities.
- Sales performance varies meaningfully by agent and manager, making sales-force benchmarking and coaching opportunities visible through the leaderboard and scatter analysis.

---

## Data Model

The project uses a star-schema-style model with `sales_pipeline` as the central fact table.

```text
                     accounts
                        1
                        |
                        *
products  1 ------ * sales_pipeline * ------ 1 sales_teams
                        *
                        |
                        1
                       Date
```

### Tables

| Table | Purpose |
|---|---|
| `sales_pipeline` | Sales opportunities, deal stages, dates, and deal values |
| `accounts` | Customer/company attributes |
| `products` | Product information and pricing |
| `sales_teams` | Sales agents, managers, and regional offices |
| `Date` | Date dimension used for time-based analysis |

---

## Data Cleaning

Data preparation was completed using **Power Query**.

Key transformations included:

- Corrected data types for dates, numbers, and text fields
- Removed duplicate records where appropriate
- Preserved business-valid nulls instead of blindly deleting them
- Corrected category spelling inconsistencies
- Standardized product names to preserve table relationships
- Validated open opportunity records with blank close dates and values

One example was standardizing:

```text
GTXPro  ->  GTX Pro
```

This ensured product-level relationships and metrics remained accurate.

---

## DAX Measures

Core measures created for the dashboard include:

```DAX
Total Opportunities =
DISTINCTCOUNT('sales pipeline'[opportunity_id])
```

```DAX
Won Deals =
CALCULATE(
    [Total Opportunities],
    'sales pipeline'[deal_stage] = "Won"
)
```

```DAX
Lost Deals =
CALCULATE(
    [Total Opportunities],
    'sales pipeline'[deal_stage] = "Lost"
)
```

```DAX
Closed Deals =
[Won Deals] + [Lost Deals]
```

```DAX
Win Rate % =
DIVIDE(
    [Won Deals],
    [Closed Deals],
    0
)
```

```DAX
Won Revenue =
CALCULATE(
    SUM('sales pipeline'[close_value]),
    'sales pipeline'[deal_stage] = "Won"
)
```

```DAX
Average Deal Value =
DIVIDE(
    [Won Revenue],
    [Won Deals],
    0
)
```

```DAX
Open Opportunities =
CALCULATE(
    [Total Opportunities],
    'sales pipeline'[deal_stage] IN {"Prospecting", "Engaging"}
)
```

```DAX
Avg Sales Cycle Days =
AVERAGEX(
    FILTER(
        'sales pipeline',
        'sales pipeline'[deal_stage] IN {"Won", "Lost"}
            && NOT ISBLANK('sales pipeline'[engage_date])
            && NOT ISBLANK('sales pipeline'[close_date])
    ),
    DATEDIFF(
        'sales pipeline'[engage_date],
        'sales pipeline'[close_date],
        DAY
    )
)
```

```DAX
Unique Customers =
CALCULATE(
    DISTINCTCOUNT('sales pipeline'[account]),
    'sales pipeline'[deal_stage] = "Won",
    'sales pipeline'[account] <> BLANK()
)
```

```DAX
Top Customer Revenue =
MAXX(
    VALUES(accounts[account]),
    [Won Revenue]
)
```

```DAX
Top Product Revenue =
MAXX(
    VALUES(products[product]),
    [Won Revenue]
)
```

---

## Tools & Skills Demonstrated

- **Power BI Desktop**
- **Power Query**
- **DAX**
- **Data Cleaning**
- **Star Schema / Data Modeling**
- **CRM Analytics**
- **Sales Performance Analysis**
- **Customer Segmentation**
- **Product Performance Analysis**
- **KPI Design**
- **Interactive Slicers**
- **Top-N Analysis**
- **Conditional Formatting**
- **Scatter / Performance Analysis**
- **Dashboard Design & Storytelling**

---

## Project Structure

```text
Sales-Performance-CRM-Analytics/
|
|-- dataset/
|   |-- accounts.csv
|   |-- products.csv
|   |-- sales_pipeline.csv
|   |-- sales_teams.csv
|   `-- data_dictionary.csv
|
|-- Power BI/
|   `-- CRM_Sales_Analytics.pbix
|
|-- Screenshots/
|   |-- 01-executive-sales-overview.png
|   |-- 02-sales-team-performance.png
|   `-- 03-customer-product-insights.png
|
`-- README.md
```

---

## Business Questions Answered

This dashboard was designed to answer practical CRM questions such as:

- How much revenue has the sales team generated?
- What percentage of closed opportunities are won?
- Which products generate the most revenue?
- Which customer sectors are most valuable?
- Which accounts contribute the most revenue?
- Which sales agents and managers perform best?
- Which agents combine high win rates with high revenue?
- What is the average time required to close an opportunity?
- How does performance vary by region, product, manager, and quarter?

---

## Dataset

The project uses the **CRM Sales Opportunities** dataset, containing B2B sales pipeline, account, product, and sales-team information.

The central sales pipeline contains **8,800 sales opportunities**.

---

## Author

**Aadya Gupta**

Built as a portfolio project to demonstrate practical **Power BI, CRM analytics, sales reporting, and business intelligence** skills.
