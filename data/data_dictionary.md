# 📘 Data Dictionary — Interiva Furniture (Fictional Dataset)

This data dictionary documents the fields used in the **Interiva Furniture** Power BI portfolio project, completed as part of a **college assessment** using a fictional company and dataset.

The purpose of this document is to clearly describe the structure, meaning, and usage of each field used in the analysis and reporting.

---

## 🗂️ Tables Overview

| Table | Type | Description |
|------|------|-------------|
| `fact_sales` | Fact | Transaction-level sales and customer data (cleaned in Power Query) |
| `dim_delivery` | Dimension | Delivery-related attributes used for operational performance analysis |
| `dim_date` | Dimension (DAX-generated) | Calendar table created in Power BI using DAX for time intelligence |

> Notes:  
> - Only `fact_sales` required cleaning and transformation in Power Query.  
> - `dim_delivery` was renamed for clarity but otherwise used as provided.  
> - `dim_date` was generated later in Power BI using DAX.

---

## 🧾 `fact_sales`

**Source file:** `Home Furniture DIRTY.csv`  
**Grain:** One row per order  
**Cleaning:** Yes (Power Query)

| Column Name | Data Type | Description | Example |
|------------|----------|-------------|---------|
| `Order_ID` | Whole number | Unique identifier for each order | `104382` |
| `First_Name` | Text | Customer first name | `Sarah` |
| `Last_Name` | Text | Customer last name | `Williams` |
| `Gender` | Text | Customer gender | `Female` |
| `Age` | Whole number | Customer age (nulls handled in Power Query) | `42` |
| `City` | Text | Customer city | `Liverpool` |
| `Channel` | Text | Sales channel | `Online` |
| `Store_Location` | Text | Physical store location (if applicable) | `Manchester` |
| `Customer_Segment` | Text | Customer segment classification | `Family` |
| `Order_Date` | Date | Date the order was placed | `2025-06-18` |
| `Product_Name` | Text | Name of the product purchased | `Oak Bed Frame` |
| `Product_Category` | Text | Product category | `Bedroom` |
| `Quantity` | Whole number | Number of units purchased | `1` |
| `Unit_Cost_GBP` | Decimal | Cost per unit (£) | `240.00` |
| `Unit_Price_GBP` | Decimal | Price per unit (£) | `399.00` |
| `Order_Cost_GBP` | Decimal | Total cost for the order (£) | `240.00` |
| `Order_Revenue_GBP` | Decimal | Total revenue for the order (£) | `399.00` |
| `Order_Profit_GBP` | Decimal | Profit for the order (£) | `159.00` |
| `Payment_Method` | Text | Payment method used | `Card` |
| `Source` | Text | Acquisition source | `Paid Ads` |
| `Device` | Text | Device used to place the order | `Mobile` |
| `Email_Opt_In` | Text / Boolean | Indicates whether customer opted into emails | `Yes` |
| `Delivery_Type` | Text | Type of delivery selected | `Standard` |
| `Delivery_Days` | Whole number | Number of days taken to deliver | `4` |
| `Review_Rating` | Decimal | Customer product rating | `4.6` |
| `Return` | Text / Boolean | Indicates if the order was returned | `No` |

> **Power Query notes:**  
> - `First_Name` and `Last_Name` were combined into a single customer name field for reporting.  
> - Missing `Age` values were replaced with the dataset average.  
> - Redundant columns and duplicate rows were removed.  
> - Data types were explicitly set to ensure reliable aggregation.

---

## 🚚 `dim_delivery`

**Source file:** `Home Furniture Table 2 CLEAN.csv`  
**Cleaning:** No (renamed only)

| Column Name | Data Type | Description | Example |
|------------|----------|-------------|---------|
| `Delivery_Type` | Text | Delivery method | `Standard` |
| `Courier` | Text | Courier or delivery provider | `Royal Mail` |
| `Fulfilment_Centre` | Text | Fulfilment or distribution centre | `London` |
| `Delivery_Status` | Text | Delivery outcome | `On Time` / `Late` |

> This table is related to `fact_sales` using the `Delivery_Type` field to support delivery performance analysis.

---

## 📅 `dim_date` (DAX-Generated)

**Creation method:** DAX (Power BI)  
**Purpose:** Enables consistent time filtering and time intelligence calculations.

| Column Name | Data Type | Description | Example |
|------------|----------|-------------|---------|
| `Date` | Date | Calendar date | `2025-06-18` |
| `Year` | Whole number | Year | `2025` |
| `Month_Number` | Whole number | Month number (1–12) | `6` |
| `Month_Name` | Text | Month name | `June` |
| `Year_Month` | Text | Year–month label | `2025-06` |
| `Quarter` | Text | Calendar quarter | `Q2` |

---

## 🧠 Metric Definitions (High-Level)

| Metric | Definition |
|------|------------|
| **Total Revenue** | Sum of `Order_Revenue_GBP` |
| **Total Profit** | Sum of `Order_Profit_GBP` |
| **Orders** | Count of `Order_ID` |
| **Average Order Value (AOV)** | Total Revenue ÷ Orders |
| **Profit Margin %** | Total Profit ÷ Total Revenue |
| **Late Delivery %** | Late deliveries ÷ total deliveries |

---

## ⚠️ Assumptions & Notes

- This dataset represents a fictional retailer and was used for a college assessment project.
- Column names reflect the final cleaned schema used in Power BI.
- All analysis and dashboards are based on the cleaned `fact_sales` table and supporting dimensions.
