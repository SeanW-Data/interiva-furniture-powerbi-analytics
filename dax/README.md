# 🧠 DAX Measures — Interiva Furniture

This folder documents the DAX measures created for the **Interiva Furniture** Power BI project (college assessment, fictional dataset).

The measures are organised into logical folders within Power BI to keep the model clean, reusable, and easy to maintain. This structure supports a recruiter-friendly review: high-level KPIs first, followed by time intelligence, customer analysis, delivery performance, and supporting/formatting measures used for visuals.

---

## 📌 Measure Grouping (as used in Power BI)

### **KPIs**
Core business performance metrics used throughout the report:
- Total Revenue, Total Profit, Total Expenses, Total Orders, Total Units
- Profit Margin %, Avg Revenue per Order, Avg Rating, Return Rate

### **Trends**
Time comparison measures used for KPI deltas and executive context:
- Revenue / Profit / Orders: Previous Year, vs PY %, Past Month, vs PM %

### **Customers**
Customer-level metrics supporting segmentation and value analysis:
- Unique Customers
- Average Revenue per Customer

### **Deliveries**
Operational performance measures used in the delivery analysis page:
- Late Deliveries, On-Time Deliveries, Late Delivery Rate
- Date-based delivery analysis using alternate relationships (Dispatch Date)

### **Supporting**
Helper measures used to power insight panels and narrative visuals:
- Current Month / Current Year measures
- Last available date measures
- Peak / Lowest month logic
- Segment identification (largest segment, highest value segment)

### **Formatting**
Measures used purely for display and conditional formatting:
- KPI arrows (vs last month / vs last year)
- Hex colour logic for positive/negative performance callouts

---

## 📄 Full Measure Definitions

All measures and their full DAX expressions are documented here:

➡️ **[measures.md](measures.md)**

---

## ✅ Notes for Reviewers

- Measures follow a consistent naming approach (e.g. `Revenue vs PY %`, `Profit Past Month`).
- Time-based measures reference the custom `dim_date` table created in Power BI using DAX.
- Formatting measures are separated to avoid mixing UI logic with core business logic.

