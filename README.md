# Executive Retail Analytics Dashboard: Portfolio Documentation

## 1. Project Overview & Business Problem
* **Objective:** Provide retail executive leadership with a consolidated, interactive dashboard to monitor core business performance, track monthly revenue velocity, evaluate sales across product categories, and filter performance dynamically by store region.
* **Key Stakeholders:** Regional Operations Managers, Sales Directors, Executive Leadership.

---

## 2. Data Architecture & Star Schema
The semantic model is built on a standard Star Schema design ($1 : *$) centered around the core transactions table to maximize performance and simplify filter propagation:

* **Fact Table:** `fact_orders` (captures line-item sales metrics, order quantities, cost, and timestamps).
* **Dimension Tables:**
  * `dim_calendar` ($1 : *$) $\rightarrow$ Drives uniform time-series filtering and custom `Month Year` trend reporting.
  * `dim_products` ($1 : *$) $\rightarrow$ Categorizes SKUs, product families, and merchandise groups.
  * `dim_stores` ($1 : *$) $\rightarrow$ Captures regional store geography (*Central, East, South, West*).
  * `dim_shipment` ($1 : *$) $\rightarrow$ Houses fulfillment methods and log details.

---

## 3. Core DAX Measures (`_Measures`)
All key performance indicators were centralized in a dedicated measure table for clean maintenance and efficiency:

```dax
Total Revenue = SUM ( 'fact_orders'[Sales Amount] )

Total Profit = SUM ( 'fact_orders'[Profit Amount] )

Total Units Sold = SUM ( 'fact_orders'[Quantity] )

Total Orders = DISTINCTCOUNT ( 'fact_orders'[Order ID] )

Avg. Fulfillment Days = AVERAGE ( 'fact_orders'[Fulfillment Days] )

---

## ** 4.  Dashboard Architecture & User Experience **
Executive KPI Header: High-level cards across the top for immediate executive visibility into high-level performance metrics.

Interactive Control Panel: Top-right Region dropdown slicer powered by dim_stores, enabling cross-filtering across all canvas visuals.

Category Performance Breakdown: Horizontal bar chart comparing product performance to quickly highlight top-performing categories.

Sales Velocity Trend: Time-series line chart tracking revenue growth month-over-month using standardized Month Year formatting.
