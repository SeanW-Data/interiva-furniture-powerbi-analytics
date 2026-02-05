# 📄 DAX Measures — Interiva Furniture

This file contains all DAX measures used in the Power BI model.
Measures are grouped by their display folders as structured in Power BI.

---

## KPIs

### Total Revenue
```DAX
Total Revenue =
SUM ( fact_sales[Order_Revenue_GBP] )
```

### Total Expenses
```DAX
Total Expenses =
SUM ( fact_sales[Order_Cost_GBP] )
```

### Total Profit
```DAX
Total Profit =
[Total Revenue] - [Total Expenses]
```

### Total Orders
```DAX
Total Orders =
DISTINCTCOUNT ( fact_sales[Order_ID] )
```

### Total Units
```DAX
Total Units =
SUM ( fact_sales[Quantity] )
```

### Profit Margin %
```DAX
Profit Margin % =
DIVIDE ( [Total Profit], [Total Revenue], 0 )
```

### Avg Revenue per Order
```DAX
Avg Revenue per Order =
DIVIDE ( [Total Revenue], [Total Orders], 0 )
```

### Avg Rating
```DAX
Avg Rating =
AVERAGE ( fact_sales[Review_Rating] )
```

### Return Rate
```DAX
Return Rate =
DIVIDE ( [Total Returns], [Total Units] )
```

---

## Customers

### Unique Customers
```DAX
Unique Customers =
COUNTROWS (
    SUMMARIZE (
        fact_sales,
        fact_sales[Customer_Name],
        fact_sales[Generation],
        fact_sales[City],
        fact_sales[Gender]
    )
)
```

### Average Revenue per Customer
```DAX
Average_Revenue_Per_Customer =
DIVIDE ( [Total Revenue], [Unique Customers] )
```

---

## Deliveries

### Total Returns
```DAX
Total Returns =
CALCULATE (
    COUNTROWS ( fact_sales ),
    fact_sales[Return] = "Yes"
)
```

### Late Deliveries
```DAX
Late Deliveries =
CALCULATE (
    DISTINCTCOUNT ( fact_sales[Order_ID] ),
    dim_delivery[Delivery_Status] = "Delivered (late)"
)
```

### On-Time Deliveries
```DAX
On-Time Deliveries =
CALCULATE (
    DISTINCTCOUNT ( fact_sales[Order_ID] ),
    dim_delivery[Delivery_Status] = "Delivered"
)
```

### Total Delivered
```DAX
Total Delivered =
COUNTROWS ( dim_delivery )
```

### Late Deliveries (Dispatch Date)
```DAX
Late Deliveries (Dispatch Date) =
CALCULATE (
    [Late Deliveries],
    USERELATIONSHIP ( dim_date[Date], dim_delivery[Dispatch_Date] )
)
```

### Late Delivery Rate
```DAX
Late Delivery Rate =
DIVIDE (
    [Late Deliveries],
    [Late Deliveries] + [On-Time Deliveries],
    0
)
```

---

## Trends

### Revenue Current Year
```DAX
Revenue Current Year =
VAR y = MAX ( dim_date[Year] )
RETURN
CALCULATE (
    [Total Revenue],
    FILTER ( ALL ( dim_date ), dim_date[Year] = y )
)
```

### Revenue Previous Year
```DAX
Revenue Previous Year =
VAR y = MAX ( dim_date[Year] )
RETURN
IF (
    y = 2024,
    BLANK (),
    CALCULATE (
        [Total Revenue],
        FILTER ( ALL ( dim_date ), dim_date[Year] = y - 1 )
    )
)
```

### Revenue vs PY %
```DAX
Revenue vs PY % =
VAR CY = [Revenue Current Year]
VAR PY = [Revenue Previous Year]
RETURN
IF ( ISBLANK ( PY ) || PY = 0, BLANK (), DIVIDE ( CY - PY, PY ) )
```

### Revenue Current Month
```DAX
Revenue Current Month =
VAR d = [Last Revenue Date]
RETURN
CALCULATE (
    [Total Revenue],
    FILTER (
        ALL ( dim_date[Date] ),
        dim_date[Date] >= DATE ( YEAR ( d ), MONTH ( d ), 1 )
            && dim_date[Date] <= EOMONTH ( d, 0 )
    )
)
```

### Revenue Past Month
```DAX
Revenue Past Month =
VAR d = [Last Revenue Date]
RETURN
CALCULATE (
    [Total Revenue],
    FILTER (
        ALL ( dim_date[Date] ),
        dim_date[Date] >= EOMONTH ( d, -2 ) + 1
            && dim_date[Date] <= EOMONTH ( d, -1 )
    )
)
```

### Revenue vs PM %
```DAX
Revenue vs PM % =
VAR CM = [Revenue Current Month]
VAR PM = [Revenue Past Month]
RETURN
IF ( ISBLANK ( PM ) || PM = 0, BLANK (), DIVIDE ( CM - PM, PM ) )
```

### Profit Current Year
```DAX
Profit Current Year =
VAR y = MAX ( dim_date[Year] )
RETURN
CALCULATE (
    [Total Profit],
    FILTER ( ALL ( dim_date ), dim_date[Year] = y )
)
```

### Profit Previous Year
```DAX
Profit Previous Year =
VAR y = MAX ( dim_date[Year] )
RETURN
IF (
    y = 2024,
    BLANK (),
    CALCULATE (
        [Total Profit],
        FILTER ( ALL ( dim_date ), dim_date[Year] = y - 1 )
    )
)
```

### Profit vs PY %
```DAX
Profit vs PY % =
VAR CY = [Profit Current Year]
VAR PY = [Profit Previous Year]
RETURN
IF ( ISBLANK ( PY ) || PY = 0, BLANK (), DIVIDE ( CY - PY, PY ) )
```

### Profit Current Month
```DAX
Profit Current Month =
VAR d = [Last Profit Date]
RETURN
CALCULATE (
    [Total Profit],
    FILTER (
        ALL ( dim_date[Date] ),
        dim_date[Date] >= DATE ( YEAR ( d ), MONTH ( d ), 1 )
            && dim_date[Date] <= EOMONTH ( d, 0 )
    )
)
```

### Profit Past Month
```DAX
Profit Past Month =
VAR d = [Last Profit Date]
RETURN
CALCULATE (
    [Total Profit],
    FILTER (
        ALL ( dim_date[Date] ),
        dim_date[Date] >= EOMONTH ( d, -2 ) + 1
            && dim_date[Date] <= EOMONTH ( d, -1 )
    )
)
```

### Profit vs PM %
```DAX
Profit vs PM % =
VAR CM = [Profit Current Month]
VAR PM = [Profit Past Month]
RETURN
IF ( ISBLANK ( PM ) || PM = 0, BLANK (), DIVIDE ( CM - PM, PM ) )
```

---

## Supporting

### Last Revenue Date
```DAX
Last Revenue Date =
MAXX (
    FILTER (
        ALLSELECTED ( dim_date[Date] ),
        NOT ISBLANK ( CALCULATE ( [Total Revenue] ) )
    ),
    dim_date[Date]
)
```

### Last Profit Date
```DAX
Last Profit Date =
MAXX (
    FILTER (
        ALLSELECTED ( dim_date[Date] ),
        NOT ISBLANK ( CALCULATE ( [Total Profit] ) )
    ),
    dim_date[Date]
)
```

### Peak Month Orders
```DAX
Peak Month Orders =
VAR MonthlyOrders =
    SUMMARIZE (
        dim_date,
        dim_date[Year],
        dim_date[MonthNo],
        "Orders", [Total Orders]
    )
RETURN
MAXX ( MonthlyOrders, [Orders] )
```

---

## Formatting

### Profit vs PM Colour
```DAX
Profit vs PM Colour =
VAR p = [Profit vs PM %]
RETURN
SWITCH (
    TRUE (),
    ISBLANK ( p ), "#A0A0A0",
    p > 0, "#2E7D32",
    p < 0, "#C62828",
    "#808080"
)
```

### Highest Value Segment KPI
```DAX
Highest Value Segment KPI =
[Highest Value Segment] & " – " & FORMAT ( [Highest Value Segment Value], "£#,0" )
```

### Largest Segment KPI
```DAX
Largest Segment KPI =
[Largest Segment] & " – " & FORMAT ( [Largest Segment Value], "#,0" ) & " customers"
```

