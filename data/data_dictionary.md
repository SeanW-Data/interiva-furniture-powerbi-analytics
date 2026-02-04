# 📘 Data Dictionary — Interiva Furniture (Fictional Dataset)

This data dictionary documents the fields used in the **Interiva Furniture** Power BI portfolio project (created for a **college assessment** using a fictional company and dataset).

It is intended to help reviewers quickly understand how the data is structured, how key metrics are derived, and how the model supports analysis.

---

## 🗂️ Tables Overview

| Table | Type | Description | Key |
|------|------|-------------|-----|
| `fact_sales` | Fact | Transaction-level sales data used for all revenue/profit/order analysis | `order_id` (or equivalent) |
| `dim_delivery` | Dimension | Delivery attributes used to analyse delivery performance (courier, fulfilment centre, delivery timing) | `delivery_id` (or equivalent) |
| `dim_date` | Dimension (DAX-generated) | Calendar table created in Power BI using DAX for time intelligence (YoY/MoM, trends, seasonality) | `date` / `date_id` |

> Notes:  
> - Only `fact_sales` required cleaning in Power Query.  
> - `dim_delivery` was included as a supporting table and renamed for clarity.  
> - `dim_date` was created later in Power BI using DAX.

---

## 🧾 `fact_sales` (Fact Table)

**Grain:** 1 row per order (or order line, depending on dataset design).

| Column | Data Type | Description | Example |
|--------|----------|-------------|---------|
| `order_id` | Whole number / Text | Unique identifier for each order | `102391` |
| `order_date` | Date | Date the order was placed | `2025-07-14` |
| `customer_id` | Whole number / Text | Customer identifier (if present) | `C00124` |
| `customer_name` | Text | Customer name (may be derived/combined in Power Query) | `Alex Turner` |
| `age` | Whole number | Customer age (nulls handled in Power Query) | `34` |
| `gender` | Text | Customer gender category | `Male` |
| `generation` | Text | Derived age group (e.g., Gen Z, Millennial, Gen X) | `Millennial` |
| `segment` | Text | Customer segment grouping | `Family` |
| `city` | Text | Customer city | `Liverpool` |
| `country` | Text | Country (if included in dataset) | `United Kingdom` |
| `channel` | Text | Sales channel | `Online` / `In-Store` |
| `source` | Text | Acquisition/source of sale | `Paid Ads`, `Organic Search`, `Direct` |
| `product_id` | Whole number / Text | Product identifier | `P0042` |
| `product_name` | Text | Product name | `Oak Bed Frame` |
| `category` | Text | Product category | `Bedroom` |
| `quantity` | Whole number | Units purchased | `1` |
| `unit_price` | Decimal number | Price per unit | `399.00` |
| `revenue` | Decimal number | Total revenue for the row/order | `399.00` |
| `cost` | Decimal number | Cost of goods sold (if available) | `240.00` |
| `profit` | Decimal number | Profit amount (may be calculated/derived) | `159.00` |
| `rating` | Whole/Decimal | Product rating (if available) | `4.6` |
| `payment_method` | Text | Payment method used | `Card`, `PayPal` |
| `device` | Text | Device type (if online) | `Mobile`, `Desktop` |
| `delivery_id` | Whole number / Text | Link key to delivery dimension | `D0192` |

---

## 🚚 `dim_delivery` (Dimension Table)

**Purpose:** Supports operational analysis of delivery performance by courier and fulfilment centre.

| Column | Data Type | Description | Example |
|--------|----------|-------------|---------|
| `delivery_id` | Whole number / Text | Unique delivery identifier | `D0192` |
| `courier` | Text | Courier/shipper name | `Royal Mail` |
| `fulfilment_centre` | Text | Distribution/fulfilment centre | `London` |
| `delivery_status` | Text | Delivery status category | `On Time`, `Late` |
| `delivery_days` | Whole number | Delivery duration in days (if available) | `4` |

---

## 📅 `dim_date` (DAX-Generated Date Table)

**Purpose:** Enables consistent time filtering and time intelligence calculations (YoY/MoM, running totals, seasonality).

| Column | Data Type | Description | Example |
|--------|----------|-------------|---------|
| `date` | Date | Calendar date | `2025-07-14` |
| `year` | Whole number | Year | `2025` |
| `month_number` | Whole number | Month number (1–12) | `7` |
| `month_name` | Text | Month name | `July` |
| `year_month` | Text | Year-month label | `2025-07` |
| `quarter` | Text / Whole number | Quarter | `Q3` |

---

## 🧠 Metric Definitions (High-Level)

| Metric | Definition |
|--------|------------|
| **Total Revenue** | Sum of `revenue` |
| **Total Profit** | Sum of `profit` (or `revenue - cost`) |
| **Orders** | Count of orders (`order_id`) |
| **Average Order Value (AOV)** | Total Revenue / Orders |
| **Profit Margin %** | Total Profit / Total Revenue |
| **On-time Delivery %** | On-time deliveries / total deliveries |
| **Late Delivery %** | Late deliveries / total deliveries |

---

## ⚠️ Assumptions & Notes

- This dataset represents a fictional retailer and was used for a college assessment project.
- Column names may vary slightly depending on how the CSVs are imported and renamed in Power BI.
- If your model uses different field names (e.g., `sales_amount` instead of `revenue`), update the dictionary to match the final Power BI schema.
