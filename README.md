# ARTHADṚṢṬI

## Amazon Sales Intelligence & Business Performance System

> **Transforming transactional data into actionable business intelligence.**

**ARTHADṚṢṬI** *(अर्थदृष्टि)* is an interactive Business Intelligence system built with **Microsoft Power BI** to analyze Amazon sales performance, order behavior, product performance, fulfilment operations, and geographic distribution.

The project was designed with a **client-facing BI and consulting mindset**, focusing not only on visualization, but also on data quality, analytical modeling, KPI design, operational analysis, and decision-oriented reporting.

---

##  Dashboard Preview

# Page 1
<img width="1307" height="735" alt="Screenshot 2026-09-14 105106" src="https://github.com/user-attachments/assets/9f22de4a-8634-4a71-a7d6-8755fdd07488" />
# Page 2
<img width="1307" height="733" alt="Screenshot 2026-09-14 105118" src="https://github.com/user-attachments/assets/b1d8c436-681d-4d42-859b-b44ceba3cfd3" />
# Page 3
<img width="1321" height="738" alt="Screenshot 2026-09-14 105135" src="https://github.com/user-attachments/assets/3bfb3a46-0a5b-4ab1-8857-9d24ebf8a049" />
# Page 4
<img width="1308" height="737" alt="Screenshot 2026-09-14 105149" src="https://github.com/user-attachments/assets/b3ad49f1-e385-435e-9d4a-7cf280707701" />

---

##  Project Objective

The objective of ARTHADṚṢṬI is to provide a centralized analytical view of e-commerce performance and help business stakeholders answer questions such as:

* How are overall sales and orders performing?
* Which product categories and SKUs contribute most to sales?
* Which states generate the highest sales?
* How are orders distributed across fulfilment channels?
* What proportion of orders are cancelled, delivered, returned, or affected by delivery issues?
* How does product performance vary across styles, SKUs, and sizes?
* Where are the major operational risks concentrated?
* How can business users interactively explore performance using filters and drill-down analysis?

---

# 🧭 Business Areas Covered

ARTHADṚṢṬI focuses on four major analytical areas:

### 1. Executive Performance

Provides a high-level overview of:

* Total Orders
* Gross Sales
* Units Sold
* Average Order Value
* Cancellation Rate
* Sales trends
* Category performance
* Fulfilment performance
* Geographic performance

### 2. Sales & Order Performance

Analyzes:

* Sales trends
* Order trends
* Sales by category
* Orders by category
* State-level sales
* Promotion performance
* Fulfilment distribution

### 3. Product Intelligence

Provides deeper product-level analysis across:

* Categories
* Styles
* SKUs
* ASINs
* Sizes
* Units
* Sales
* Orders

A hierarchical product analysis is supported through:

**Category → Style → SKU → Size**

### 4. Fulfilment & Operations

Evaluates operational performance using:

* Cancellation Rate
* Delivery Rate
* Return Rate
* Delivery Issue Rate
* Fulfilment method
* Courier status
* Operational status
* State-level operational performance

---

#  Dashboard Pages

| Page                          | Purpose                                             |
| ----------------------------- | --------------------------------------------------- |
| **Executive Overview**        | Overall business and sales performance              |
| **Sales & Order Performance** | Sales, orders, categories, geography and fulfilment |
| **Product Intelligence**      | Category, style, SKU and size performance           |
| **Fulfilment & Operations**   | Operational efficiency and risk analysis            |

---

#  Solution Architecture

The project follows a structured Business Intelligence workflow:

```text
                   RAW DATA
                      │
                      ▼
             DATA QUALITY AUDIT
                      │
                      ▼
              POWER QUERY ETL
                      │
          ┌───────────┴───────────┐
          │                       │
      CLEANING               STANDARDIZATION
          │                       │
          └───────────┬───────────┘
                      ▼
               DATA MODELING
                      │
                      ▼
                 DAX LAYER
                      │
                      ▼
              POWER BI REPORT
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
       SALES       PRODUCT     OPERATIONS
          │           │           │
          └───────────┼───────────┘
                      ▼
             BUSINESS INSIGHTS
```

---

# Data Model

The analytical model uses an order-line-level fact table supported by a dedicated date dimension and centralized measure layer.

```text
                  ┌─────────────────┐
                  │    Dim_Date     │
                  │                 │
                  │ Date            │
                  │ Year            │
                  │ Month           │
                  │ Quarter         │
                  │ Week            │
                  └────────┬────────┘
                           │
                         1 : *
                           │
                           ▼
              ┌─────────────────────────┐
              │   Fact_Amazon_Sales     │
              │                         │
              │ Order ID                │
              │ Date                    │
              │ Status                  │
              │ Category                │
              │ Style                   │
              │ SKU                     │
              │ Size                    │
              │ Qty                     │
              │ Amount                  │
              │ Fulfilment              │
              │ Geography               │
              │ Promotion               │
              └─────────────────────────┘


              ┌─────────────────────────┐
              │       _Measures         │
              │                         │
              │ DAX Measure Layer       │
              └─────────────────────────┘
```

### Data Grain

The dataset is maintained at **order-line/item level**.

This distinction is important because one `Order ID` can contain multiple rows.

Therefore:

* **Total Orders** → `DISTINCTCOUNT(Order ID)`
* **Order Lines** → `COUNTROWS(Fact_Amazon_Sales)`
* **Units Sold** → `SUM(Qty)`

This prevents order counts from being incorrectly calculated using row count.

---

#  Data Preparation & Quality

The dataset underwent structured data-quality assessment and transformation before being used for reporting.

### Key preparation activities

* Data-type validation
* Missing-value analysis
* Financial field audit
* Status standardization
* Category normalization
* City cleaning
* State normalization
* Promotion classification
* Operational status grouping
* Courier status grouping
* Duplicate/relationship validation
* Order-grain validation
* Date validation
* Removal of unnecessary index field

### Derived analytical fields

Several business-friendly fields were created during preparation:

* `Promotion Status`
* `Order Status Group`
* `Courier Status Group`
* `Category Clean`
* `City Clean`
* `State Clean`
* `Status Group Sort`

The original source fields were retained where appropriate to preserve traceability.

---

#  KPI Framework

ARTHADṚṢṬI uses a structured DAX measure layer.

### Core KPIs

| KPI                         | Definition                   |
| --------------------------- | ---------------------------- |
| **Total Orders**            | Distinct Order IDs           |
| **Order Lines**             | Number of order-line records |
| **Units Sold**              | Sum of Qty                   |
| **Gross Sales Amount**      | Sum of Amount                |
| **Average Order Value**     | Gross Sales / Total Orders   |
| **Average Units per Order** | Units / Total Orders         |

### Product KPIs

* Unique Categories
* Unique Styles
* Unique SKUs
* Unique ASINs

### Operational KPIs

* Cancelled Orders
* Pending Orders
* Delivered Orders
* Returned Orders
* Delivery Issue Orders
* Cancellation Rate
* Delivery Rate
* Return Rate
* Delivery Issue Rate

### Commercial KPIs

* Sales per Order
* Sales per Unit
* Promotion Orders
* Non-Promotion Orders
* Promotion Order Rate
* Promotion Sales
* Non-Promotion Sales
* Amazon Fulfilment Orders
* Merchant Fulfilment Orders
* Amazon Fulfilment Rate
* Merchant Fulfilment Rate
* Average Lines per Order

---

#  Analytical Capabilities

ARTHADṚṢṬI provides interactive analytical capabilities including:

* Interactive date filtering
* Category filtering
* Fulfilment filtering
* Cross-filtering between visuals
* Drill-down product analysis
* Top-N analysis
* Dynamic visual interaction
* Product hierarchy analysis
* Geographic analysis
* Operational risk analysis
* KPI-driven performance monitoring

The dashboard is designed to allow stakeholders to move from **high-level performance → underlying drivers → detailed analysis**.

---


#  Technology Stack

### Business Intelligence

* **Microsoft Power BI**

### Data Transformation

* **Power Query**

### Analytics & Calculations

* **DAX**

### Data Documentation

* Markdown
* Excel

### Core Concepts

* Data Cleaning
* ETL
* Data Modeling
* Dimensional Modeling
* KPI Design
* Business Intelligence
* Data Visualization
* Operational Analytics

---
---

#  Important Analytical Assumptions

### Order Definition

An order is identified using the unique `Order ID`.

### Order-Line Definition

Each row represents an order-line/item-level record.

### Sales Definition

`Gross Sales Amount` represents the sum of the available `Amount` field in the source data.

### Units Definition

`Units Sold` represents the sum of the source `Qty` field.

### Cancellation Definition

Cancellation is based on the standardized `Order Status Group`.

### Delivery Definition

Delivery performance is based on the standardized operational status classification.

---

#  Data Limitations

The dashboard should be interpreted within the limitations of the underlying dataset.

### Time Coverage

The dataset covers approximately:

**31 March 2022 → 29 June 2022**

Therefore, the project does **not** make annual or year-over-year performance claims.

### Financial Data

Some records contain missing financial values in the source data. These values were investigated and retained rather than artificially replacing them.

### Profitability

The dataset does not provide sufficient information to calculate reliable:

* Profit
* Profit Margin
* ROI
* Contribution Margin

Therefore, these metrics are intentionally excluded.

### Customer Analytics

The available structure does not support robust customer lifetime value or customer profitability analysis.

---

#  Future Enhancements

Potential future extensions include:

* Automated data refresh pipeline
* Real-time or scheduled reporting
* Customer segmentation
* Profitability analysis with cost data
* Anomaly detection
* Advanced product recommendation systems
* Row-level security
* Cloud-based data warehouse integration

---

#  Business Value

ARTHADṚṢṬI demonstrates how raw transactional data can be transformed into a structured decision-support system.

The solution enables stakeholders to move through the analytical process:

```text
DATA
  ↓
INFORMATION
  ↓
PERFORMANCE
  ↓
DRIVERS
  ↓
RISKS
  ↓
BUSINESS INSIGHTS
```

Rather than presenting isolated charts, the dashboard connects **sales, products, geography, fulfilment, and operational outcomes** into a single analytical environment.

---

#  Author

**Yadavalli Lokesh**

B.Tech Computer Science & Engineering
Artificial Intelligence & Machine Learning

### Areas of Interest

* Business Intelligence
* Data Analytics
* Machine Learning
* Data Visualization
---

#  Project

**ARTHADṚṢṬI**
*Amazon Sales Intelligence & Business Performance System*

Built with:

**Power BI • Power Query • DAX • Business Intelligence**

---

> **From data to दृष्टि — from transactions to decisions.**
