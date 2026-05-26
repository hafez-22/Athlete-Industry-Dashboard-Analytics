# 📈 Measures

This document lists each measure in the model with a concise, business-friendly description and the DAX formula.

---

## Table of Contents

- ColorCost
- Revenue
- Cost
- Profit
- #orders
- #customers
- Quantity
- RevenueLY
- RevenueVarfromLY
- RevenueVarFromLY%
- CostLY
- ProfitLY
- AOVVar
- ProfitVarfromLY
- CostVarFromLY%
- ProfitVarFromLY%
- ArrowRev
- ArrowProfit
- ArrowCost
- ColorRev
- ColorProfit
- RevVar
- CostVar
- ProfitVar
- AOV
- #Customers without orders
- FirstYear
- ReturnQTY
- ReturnAmount
- ReturnRate
- #CustomerReturn
- #ordersReturn
- QTYReturn%
- #CustomerReturn%
- #OrderReturn%
- Cost%
- GM%
- customersLY
- CustomerVar
- OrdersLY
- OrdersVar
- AOVLY
- CostVarfromLY
- TopCustomers
- TitleLocation
- RevenueYTD
- ProfitYTD
- CostYTD
- YTD
- YTDTitle
- MOMRevenue%
- MOMProfit%
- MOMCost%
- MOM%
- VarFromSameMonthLY%
- Varcolor
- ColorVarLY
- ColorMOM
- COLORRevForchart
- QTYVar
- ColorQTY
- QTYLY
- ArrowQTY
- Top3ReturnAmount
- Top3Customers
- Top Customers Value

---

### ColorCost
- Business logic: Color code (1/2) indicating whether Cost increased vs same period last year.
- DAX:
```dax
IF([Cost]>[CostLY],1,2)
```

### Revenue
- Business logic: Total sales revenue.
- DAX:
```dax
SUM ( SalesDetails[Sales] )
```

### Cost
- Business logic: Total cost of goods sold (unit cost * quantity per line).
- DAX:
```dax
SUMX ( SalesDetails, SalesDetails[UnitCost] * SalesDetails[OrderQuantity] )
```

### Profit
- Business logic: Gross profit = Revenue minus Cost.
- DAX:
```dax
[Revenue] - [Cost]
```

### #orders
- Business logic: Number of distinct orders.
- DAX:
```dax
DISTINCTCOUNT ( SalesDetails[SalesOrderNumber] )
```

### #customers
- Business logic: Number of unique customers who placed orders.
- DAX:
```dax
DISTINCTCOUNT ( SalesDetails[CustomerKey] )
```

### Quantity
- Business logic: Total units sold.
- DAX:
```dax
SUM ( SalesDetails[OrderQuantity] )
```

### RevenueLY
- Business logic: Revenue for the same period last year.
- DAX:
```dax
CALCULATE ( [Revenue], SAMEPERIODLASTYEAR ('Date'[Date] ) )
```

### RevenueVarfromLY
- Business logic: Absolute revenue change vs same period last year.
- DAX:
```dax
[Revenue] - [RevenueLY]
```

### RevenueVarFromLY%
- Business logic: Revenue percent change vs same period last year.
- DAX:
```dax
DIVIDE ( [RevenueVarfromLY], [RevenueLY] )
```

### CostLY
- Business logic: Cost for the same period last year.
- DAX:
```dax
CALCULATE ( [Cost], SAMEPERIODLASTYEAR ('Date'[Date] ) )
```

### ProfitLY
- Business logic: Profit for the same period last year.
- DAX:
```dax
CALCULATE ( [Profit], SAMEPERIODLASTYEAR ('Date'[Date] ) )
```

### AOVVar
- Business logic: Absolute change in Average Order Value vs last year.
- DAX:
```dax
[AOV] - [AOVLY]
```

### ProfitVarfromLY
- Business logic: Absolute profit change vs same period last year.
- DAX:
```dax
[Profit] - [ProfitLY]
```

### CostVarFromLY%
- Business logic: Cost percent change vs same period last year.
- DAX:
```dax
DIVIDE ( [CostVarfromLY], [CostLY] )
```

### ProfitVarFromLY%
- Business logic: Profit percent change vs same period last year.
- DAX:
```dax
DIVIDE ( [ProfitVarfromLY], [ProfitLY] )
```

### ArrowRev
- Business logic: Visual up/down arrow for revenue change vs last year (used in labels).
- DAX:
```dax
IF (
    ISFILTERED ( 'Date'[Year] )
        && SELECTEDVALUE ( 'Date'[Year] ) <> 2010
        && [RevenueVarfromLY] > 0,
    UNICHAR ( 129033 ),
    IF (
        ISFILTERED ( 'Date'[Year] )
            && SELECTEDVALUE ( 'Date'[Year] ) <> 2010
            && [RevenueVarfromLY] < 0,
        UNICHAR ( 129035 ),
        ""
    )
)
```

### ArrowProfit
- Business logic: Visual up/down arrow for profit change vs last year.
- DAX:
```dax
IF (
    ISFILTERED ( 'Date'[Year] )
        && SELECTEDVALUE ( 'Date'[Year] ) <> 2010
        && [ProfitVarfromLY] > 0,
    UNICHAR ( 129033 ),
    IF (
        ISFILTERED ( 'Date'[Year] )
            && SELECTEDVALUE ( 'Date'[Year] ) <> 2010
            && [ProfitVarfromLY] < 0,
        UNICHAR ( 129035 ),
        ""
    )
)
```

### ArrowCost
- Business logic: Visual up/down arrow for cost-related variance (AOVVar used here).
- DAX:
```dax
IF (
    ISFILTERED ( 'Date'[Year] )
        && SELECTEDVALUE ( 'Date'[Year] ) <> 2010
        && [AOVVar] > 0,
    UNICHAR ( 129033 ),
    IF (
        ISFILTERED ( 'Date'[Year] )
            && SELECTEDVALUE ( 'Date'[Year] ) <> 2010
            && [AOVVar] < 0,
        UNICHAR ( 129035 ),
        ""
    )
)
```

### ColorRev
- Business logic: Color code (1/2) indicating whether Revenue increased vs last year.
- DAX:
```dax
IF([Revenue]>[RevenueLY],1,2)
```

### ColorProfit
- Business logic: Color code (1/2) indicating whether Profit increased vs last year.
- DAX:
```dax
IF([Profit]>[ProfitLY],1,2)
```

### RevVar
- Business logic: Formatted string combining revenue absolute and percent variance vs LY for display.
- DAX:
```dax
IF (
    AND ( ISFILTERED ( 'Date'[Year] ), SELECTEDVALUE ( 'Date'[Year] ) <> 2010 ),
    FORMAT ( [RevenueVarfromLY], "$#,0" ) & "    "
        & FORMAT ( [RevenueVarFromLY%], "#0.0%" ),
    ""
)
```

### CostVar
- Business logic: Formatted string combining cost absolute and percent variance vs LY for display.
- DAX:
```dax
IF (
    AND ( ISFILTERED ( 'Date'[Year] ), SELECTEDVALUE ( 'Date'[Year] ) <> 2010 ),
    FORMAT ( [CostVarfromLY], "$#,0" ) & "    "
        & FORMAT ( [CostVarFromLY%], "#0.0%"),
    ""
)
```

### ProfitVar
- Business logic: Formatted string combining profit absolute and percent variance vs LY for display.
- DAX:
```dax
IF (
    AND ( ISFILTERED ( 'Date'[Year] ), SELECTEDVALUE ( 'Date'[Year] ) <> 2010 ),
    FORMAT ( [ProfitVarfromLY], "$#,0" ) & "    "
        & FORMAT ( [ProfitVarFromLY%], "#0.0%"),
    ""
)
```

### AOV
- Business logic: Average Order Value = Revenue divided by number of orders.
- DAX:
```dax
DIVIDE([Revenue],[#orders])
```

### #Customers without orders
- Business logic: Count of customers with no recorded orders.
- DAX:
```dax
COUNTBLANK(Customers[NO.Orders])
```

### FirstYear
- Business logic: Year of the earliest order in the dataset.
- DAX:
```dax
YEAR(CALCULATE(MIN('SalesDetails'[OrderDate])))
```

### ReturnQTY
- Business logic: Total returned quantity.
- DAX:
```dax
SUM(Returns[ReturnQuantity])
```

### ReturnAmount
- Business logic: Total value of returns.
- DAX:
```dax
SUM(Returns[ReturnAmount])
```

### ReturnRate
- Business logic: Return amount as a percent of revenue.
- DAX:
```dax
DIVIDE([ReturnAmount],[Revenue])
```

### #CustomerReturn
- Business logic: Number of customers who made returns.
- DAX:
```dax
DISTINCTCOUNT(Returns[CustomerKey])
```

### #ordersReturn
- Business logic: Number of orders that had returns.
- DAX:
```dax
DISTINCTCOUNT(Returns[SalesOrderNumber])
```

### QTYReturn%
- Business logic: Returned quantity as percent of total sold quantity.
- DAX:
```dax
DIVIDE([ReturnQTY],[Quantity])
```

### #CustomerReturn%
- Business logic: Percent of customers who returned items.
- DAX:
```dax
DIVIDE([#CustomerReturn],[#customers])
```

### #OrderReturn%
- Business logic: Percent of orders that included returns.
- DAX:
```dax
DIVIDE([#ordersReturn],[#orders])
```

### Cost%
- Business logic: Cost as percent of revenue.
- DAX:
```dax
DIVIDE([Cost],[Revenue])
```

### GM%
- Business logic: Gross margin = Profit as percent of revenue.
- DAX:
```dax
DIVIDE([Profit],[Revenue])
```

### customersLY
- Business logic: Number of customers in the same period last year.
- DAX:
```dax
CALCULATE ( [#customers], SAMEPERIODLASTYEAR ('Date'[Date] ) )
```

### CustomerVar
- Business logic: Absolute change in customers vs last year.
- DAX:
```dax
[#customers]-[customersLY]
```

### OrdersLY
- Business logic: Number of orders in the same period last year.
- DAX:
```dax
CALCULATE ( [#orders], SAMEPERIODLASTYEAR ('Date'[Date] ) )
```

### OrdersVar
- Business logic: Absolute change in orders vs last year.
- DAX:
```dax
[#orders]-[OrdersLY]
```

### AOVLY
- Business logic: Average Order Value in the same period last year.
- DAX:
```dax
CALCULATE ( [AOV], SAMEPERIODLASTYEAR ('Date'[Date] ) )
```

### CostVarfromLY
- Business logic: Absolute change in cost vs last year.
- DAX:
```dax
[Cost] - [CostLY]
```

### TopCustomers
- Business logic: Revenue for customers ranked within the selected Top Customers value (dynamic Top N).
- DAX:
```dax
IF(RANKX(ALL(Customers[Customer]),[Revenue],,DESC,Dense) <= 'Top Customers'[Top Customers Value],[Revenue])
```

### TitleLocation
- Business logic: Dynamic title string like "Details For <Country> in <Year>".
- DAX:
```dax
"Details For " & SELECTEDVALUE(Customers[CountryName]) & " in " & SELECTEDVALUE('Date'[Year])
```

### RevenueYTD
- Business logic: Year-to-date revenue.
- DAX:
```dax
CALCULATE([Revenue],DATESYTD('Date'[Date]))
```

### ProfitYTD
- Business logic: Year-to-date profit.
- DAX:
```dax
CALCULATE([Profit],DATESYTD('Date'[Date]))
```

### CostYTD
- Business logic: Year-to-date cost.
- DAX:
```dax
CALCULATE([Cost],DATESYTD('Date'[Date]))
```

### YTD
- Business logic: Switchable YTD metric (Revenue/Profit/Cost) depending on user selection.
- DAX:
```dax
IF(SELECTEDVALUE('Time analysis'[YTD Order])=0,[RevenueYTD],IF(SELECTEDVALUE('Time analysis'[YTD Order])=1,[ProfitYTD],[CostYTD]))
```

### YTDTitle
- Business logic: Dynamic YTD title based on selected metric.
- DAX:
```dax
IF(SELECTEDVALUE('Time analysis'[YTD Order])=0,"Revenue YTD",IF(SELECTEDVALUE('Time analysis'[YTD Order])=1,"Profit YTD","Cost YTD"))
```

### MOMRevenue%
- Business logic: Month-over-month percent change in revenue.
- DAX:
```dax
VAR revenuelm =
    CALCULATE ( [Revenue], DATEADD ( 'Date'[Date], -1, MONTH ) )
VAR diffFromLM = [Revenue] - revenuelm
RETURN
    DIVIDE ( diffFromLM, revenuelm )
```

### MOMProfit%
- Business logic: Month-over-month percent change in profit.
- DAX:
```dax
VAR profitlm =
    CALCULATE ( [Profit], DATEADD ( 'Date'[Date], -1, MONTH ) )
VAR diffFromLM = [Profit] - profitlm
RETURN
    DIVIDE ( diffFromLM, profitlm )
```

### MOMCost%
- Business logic: Month-over-month percent change in cost.
- DAX:
```dax
VAR costlm =
    CALCULATE ( [Cost], DATEADD ( 'Date'[Date], -1, MONTH ) )
VAR diffFromLM = [Cost] - costlm
RETURN
    DIVIDE ( diffFromLM, costlm )
```

### MOM%
- Business logic: Switchable MOM metric percent based on user selection.
- DAX:
```dax
IF(SELECTEDVALUE('Time analysis'[YTD Order])=0,[MOMRevenue%],IF(SELECTEDVALUE('Time analysis'[YTD Order])=1,[MOMProfit%],[MOMCost%]))
```

### VarFromSameMonthLY%
- Business logic: Switchable percent variance vs same month last year (Revenue/Profit/Cost) based on user selection.
- DAX:
```dax
IF(SELECTEDVALUE('Time analysis'[YTD Order])=0,[RevenueVarFromLY%],IF(SELECTEDVALUE('Time analysis'[YTD Order])=1,[ProfitVarFromLY%],[CostVarFromLY%]))
```

### Varcolor
- Business logic: (No expression defined) Placeholder/flag for variant coloring.
- DAX:
```dax

```

### ColorVarLY
- Business logic: Color code based on positive/negative variance vs same month last year.
- DAX:
```dax
IF([VarFromSameMonthLY%]>0,1,IF([VarFromSameMonthLY%]<0,2,""))
```

### ColorMOM
- Business logic: Color code based on MOM% sign.
- DAX:
```dax
IF([MOM%]>0,1,IF([MOM%]<0,2,""))
```

### COLORRevForchart
- Business logic: Color code (1/2) for charting revenue vs last year (uses Revenue<RevenueLY condition).
- DAX:
```dax
IF([Revenue]<[RevenueLY],1,2)
```

### QTYVar
- Business logic: Formatted string showing quantity diff and percent vs same period last year for display.
- DAX:
```dax
VAR qtyly =
    CALCULATE ( [Quantity], SAMEPERIODLASTYEAR ( 'Date'[Date] ) )
VAR diff =
    [Quantity] - qtyly
VAR diffPerc =
    DIVIDE ( diff, qtyly )
RETURN
    IF (
        AND ( ISFILTERED ( 'Date'[Year] ), SELECTEDVALUE ( 'Date'[Year] ) <> 2010 ),
        FORMAT ( diff, "$#,0" ) & "    "
            & FORMAT ( diffPerc, "#0.0%" ),
        ""
    )

```

### ColorQTY
- Business logic: Color code (1/2) indicating whether Quantity increased vs last year.
- DAX:
```dax
IF([Quantity]>[QTYLY],1,2)
```

### QTYLY
- Business logic: Quantity for the same period last year.
- DAX:
```dax
CALCULATE([Quantity],SAMEPERIODLASTYEAR('Date'[Date]))
```

### ArrowQTY
- Business logic: Visual up/down arrow for Quantity change vs last year.
- DAX:
```dax
IF (
    ISFILTERED ( 'Date'[Year] )
        && SELECTEDVALUE ( 'Date'[Year] ) <> 2010
        && [Quantity] > [QTYLY],
    UNICHAR ( 129033 ),
    IF (
        ISFILTERED ( 'Date'[Year] )
            && SELECTEDVALUE ( 'Date'[Year] ) <> 2010
            && [Quantity] < [QTYLY],
        UNICHAR ( 129035 ),
        ""
    )
)
```

### Top3ReturnAmount
- Business logic: Return amount for the top 3 products by return amount (else blank).
- DAX:
```dax
VAR ReturnRevenueRank =
    RANKX(
        ALLSELECTED(Products[ProductName]),
        [ReturnAmount],
        ,
        DESC
    )
RETURN 
    IF(ReturnRevenueRank <= 3, [ReturnAmount])
```

### Top3Customers
- Business logic: Return quantity for the top 2 customers by return quantity (else blank).
- DAX:
```dax
VAR ReturnCustomersRank =
    RANKX(
        ALLSELECTED(Customers[Customer]),
        [ReturnQTY],
        ,
        DESC
    )
RETURN 
    IF(ReturnCustomersRank <= 2, [ReturnQTY])
```

### Top Customers Value
- Business logic: Selected Top N value (user parameter for TopCustomers measure).
- DAX:
```dax
SELECTEDVALUE('Top Customers'[Top Customers])
```

---

If you want these descriptions adjusted to match specific business terminology (e.g., "Net Sales" instead of "Revenue"), or if you want the document exported to a different location, tell me which terms and I will update the file.
# Measures Documentation — Sales Analysis

Generated from: Sales Analysis.pbix

This document lists each measure with a business-friendly name, a short description of the business logic, and the DAX expression.

---

## Revenue — Total Revenue
- Table: DAX measures
- Business description: Total sales value for the selected context (sum of sales amounts).

```dax
SUM('gold fact_sales'[sales_amount])
```

---

## Quantity — Total Quantity Sold
- Table: DAX measures
- Business description: Total number of items/units sold in the selected context.

```dax
SUM('gold fact_sales'[quantity])
```

---

## Cost — Total Cost
- Table: DAX measures
- Business description: Total cost of goods sold (COGS) for the selected context.

```dax
SUM('gold fact_sales'[cost])
```

---

## Profit — Gross Profit
- Table: DAX measures
- Business description: Simple profit metric calculated as Revenue less Cost for the selected context.

```dax
[Revenue] - [Cost]
```

---

## #Customers — Unique Customers
- Table: DAX measures
- Business description: Number of unique customers who made purchases in the selected context.

```dax
DISTINCTCOUNT('gold fact_sales'[customer_key])
```

---

## #Orders — Unique Orders
- Table: DAX measures
- Business description: Number of unique order identifiers in the selected context.

```dax
DISTINCTCOUNT('gold fact_sales'[order_number])
```

---

## rev_diff — Revenue Change vs Last Year
- Table: DAX measures
- Business description: Revenue change compared to the prior period (uses a helper function `diffFromLY`). Use for trend analysis year-over-year.

```dax
diffFromLY([Revenue])
```

---

## profit_diff — Profit Change vs Last Year
- Table: DAX measures
- Business description: Profit change compared to the prior period using the same `diffFromLY` helper.

```dax
diffFromLY([Profit])
```

---

## cost_diff — Cost Change vs Last Year
- Table: DAX measures
- Business description: Cost change compared to the prior period using the `diffFromLY` helper.

```dax
diffFromLY([Cost])
```

---

## rev_arrow — Revenue Direction Indicator
- Table: DAX measures
- Business description: Visual indicator (arrow) showing revenue direction compared to previous period; relies on `arrows` helper which returns an indicator value or symbol.

```dax
arrows([Revenue])
```

---

## rev_color — Revenue Color Indicator
- Table: DAX measures
- Business description: Color/indicator value derived from revenue performance (uses `color` helper). Useful for conditional formatting.

```dax
color([Revenue])
```

---

## profit_color — Profit Color Indicator
- Table: DAX measures
- Business description: Color/indicator value derived from profit performance.

```dax
color([Profit])
```

---

## cost_color — Cost Color Indicator
- Table: DAX measures
- Business description: Color/indicator value for cost performance (uses `color_cost` helper).

```dax
color_cost([Cost])
```

---

## profit_arrow — Profit Direction Indicator
- Table: DAX measures
- Business description: Arrow indicator for profit direction, uses `arrows` helper.

```dax
arrows([Profit])
```

---

## cost_arrow — Cost Direction Indicator
- Table: DAX measures
- Business description: Arrow indicator for cost direction, uses `arrows` helper.

```dax
arrows([Cost])
```

---

## qty_diff — Quantity Change vs Last Year
- Table: DAX measures
- Business description: Quantity (units) change vs prior period using `diffFromLY` helper.

```dax
diffFromLY([Quantity])
```

---

## qty_arrow — Quantity Direction Indicator
- Table: DAX measures
- Business description: Arrow indicator showing whether quantity increased or decreased vs prior period.

```dax
arrows([Quantity])
```

---

## qty_color — Quantity Color Indicator
- Table: DAX measures
- Business description: Color/indicator for quantity performance (uses `color` helper).

```dax
color([Quantity])
```

---

## first_order — Year of First Order
- Table: DAX measures
- Business description: Returns the year of the earliest order date in the current filter context (customer first order year).

```dax
YEAR(CALCULATE(MIN('gold fact_sales'[order_date])))
```

---

## AOV — Average Order Value
- Table: DAX measures
- Business description: Average revenue per order (Revenue divided by number of orders).

```dax
DIVIDE([Revenue],[#Orders])
```

---

## top_customers — Top Customers Revenue (thresholded)
- Table: DAX measures
- Business description: Returns revenue only for customers ranked in top N by revenue (N defined by `'NO.Customers'[#Customers Value]`). Useful to isolate top customer contribution.

```dax
IF( RANKX(ALL('gold dim_customers'[customer_name]),[Revenue],,DESC, Skip) <= 'NO.Customers'[#Customers Value],[Revenue])
```

---

## last_order — Year of Most Recent Order
- Table: DAX measures
- Business description: Returns the year of the most recent order date in the current selection.

```dax
YEAR(CALCULATE(MAX('gold fact_sales'[order_date])))
```

---

## no.cust_new_old — New vs Returning Customers (percentage text)
- Table: DAX measures
- Business description: Returns a formatted text showing percentage of new vs returning customers for the selected year (requires `date_table[year]`). Useful for customer composition reporting.

```dax
VAR new =
    COUNTX (
        FILTER (
            'gold dim_customers',
            'gold dim_customers'[First_order] = SELECTEDVALUE ( date_table[year] )
        ),
        'gold dim_customers'[customer_key]
    )
VAR perc_new =
    DIVIDE ( new, [#Customers] )*100
VAR old = [#Customers] - new
VAR perc_old =
    DIVIDE ( old, [#Customers] )*100
VAR space = UNICHAR(160) & UNICHAR(160) & UNICHAR(160) & UNICHAR(160)

RETURN
IF (
    ISFILTERED ( date_table[year] ),
    "New" & "   " & FORMAT(perc_new, "0.00") & " %" &
    space & "|" & space & "Old" & "   " & FORMAT(perc_old, "0.00") & " %",
    ""
    )
```

---

## no.ord_new_old — New vs Returning Orders (percentage text)
- Table: DAX measures
- Business description: Same idea as `no.cust_new_old` but using order counts to show percentage of orders from new vs returning customers.

```dax
VAR new =
    COUNTX (
        FILTER (
            'gold dim_customers',
            'gold dim_customers'[First_order] = SELECTEDVALUE ( date_table[year] )
        ),
        [#Orders]
    )
VAR perc_new =
    DIVIDE ( new, [#Orders] )*100
VAR old = [#Orders] - new
VAR perc_old =
    DIVIDE ( old, [#Orders] )*100
VAR space = UNICHAR(160) & UNICHAR(160) & UNICHAR(160) & UNICHAR(160)

RETURN
IF (
    ISFILTERED ( date_table[year] ),
    "New" & "   " & FORMAT(perc_new, "0.00") & " %" &
    space & "|" & space & "Old" & "   " & FORMAT(perc_old, "0.00") & " %",
    ""
    )
```

---

## rev_new_old — New vs Returning Revenue (percentage text)
- Table: DAX measures
- Business description: Shows revenue contribution from new vs returning customers as a formatted text for the selected year.

```dax
VAR new =
    SUMX (
        FILTER (
            'gold dim_customers',
            'gold dim_customers'[First_order] = SELECTEDVALUE ( date_table[year] )
        ),
        [Revenue]
    )
VAR perc_new =
    DIVIDE ( new, [Revenue] )*100
VAR old = [Revenue] - new
VAR perc_old =
    DIVIDE ( old, [Revenue] )*100
VAR space = UNICHAR(160) & UNICHAR(160) & UNICHAR(160) & UNICHAR(160)

RETURN
IF (
    ISFILTERED ( date_table[year] ),
    "New" & "   " & FORMAT(perc_new, "0.00") & " %" &
    space & "|" & space & "Old" & "   " & FORMAT(perc_old, "0.00") & " %",
    ""
    )
```

---

## profit% — Profit Percentage
- Table: DAX measures
- Business description: Profit as a percentage of revenue, formatted as text with a percent sign.

```dax
FORMAT( DIVIDE([Profit],[Revenue])*100,"0.00") & " %"
```

---

## ASP — Average Selling Price
- Table: DAX measures
- Business description: Revenue divided by quantity, indicating average price per unit sold.

```dax
DIVIDE([Revenue],[Quantity])
```

---

## mom_rev — Month-over-Month Revenue Change
- Table: DAX measures
- Business description: Month-over-month change for revenue using `mom` helper function.

```dax
mom([Revenue])
```

---

## mom_cost — Month-over-Month Cost Change
- Table: DAX measures
- Business description: Month-over-month change for cost using `mom` helper.

```dax
mom([Cost])
```

---

## mom_profit — Month-over-Month Profit Change
- Table: DAX measures
- Business description: Month-over-month change for profit using `mom` helper.

```dax
mom([Profit])
```

---

## ytd_rev — Year-to-Date Revenue
- Table: DAX measures
- Business description: Running total of revenue for the year to date (uses `ytd_` helper).

```dax
ytd_([Revenue])
```

---

## ytd_cost — Year-to-Date Cost
- Table: DAX measures
- Business description: Running total of cost for the year to date (uses `ytd_` helper).

```dax
ytd_([Cost])
```

---

## ytd_profit — Year-to-Date Profit
- Table: DAX measures
- Business description: Running total of profit for the year to date (uses `ytd_` helper).

```dax
ytd_([Profit])
```

---

## YTD_title — Selected Measure YTD Title
- Table: DAX measures
- Business description: Returns a label string showing which YTD metric is selected (Revenue YTD / Profit YTD / Cost YTD) based on the `Measure` slicer selection.

```dax
IF(SELECTEDVALUE('Measure'[Measure Order])=0,"Revenue YTD",IF(SELECTEDVALUE('Measure'[Measure Order])=1 ,"Profit YTD","Cost YTD"))
```

---

## MOM — Selected Measure MOM Value
- Table: DAX measures
- Business description: Switches between month-over-month measures (`mom_rev`, `mom_profit`, `mom_cost`) depending on `Measure` selection.

```dax
IF(SELECTEDVALUE('Measure'[Measure Order])=0,[mom_rev],IF(SELECTEDVALUE('Measure'[Measure Order])=1 ,[mom_profit],[mom_cost]))
```

---

## ColorMOM — MOM Color Code
- Table: DAX measures
- Business description: Produces a numeric color code (1/2) depending on whether MOM is positive or negative; used for conditional formatting.

```dax
SWITCH(
    TRUE(),
    [MOM] > 0, 1,
    [MOM] < 0, 2
)
```

---

## YTD Revenue — Revenue YTD (context-sensitive)
- Table: DAX measures
- Business description: Returns revenue YTD, optionally filtered by `Measure` slicer.

```dax
VAR SelOrder = SELECTEDVALUE('Measure'[Measure Order])
RETURN
   IF(
        ISBLANK(SelOrder),          
        [ytd_rev],              
        IF(SelOrder = 0, [ytd_rev], BLANK())
    )
```

---

## YTD Profit — Profit YTD (context-sensitive)
- Table: DAX measures
- Business description: Returns profit YTD when `Measure` selection corresponds to profit.

```dax
VAR SelOrder = SELECTEDVALUE('Measure'[Measure Order])
RETURN
   IF(
        ISBLANK(SelOrder), 
        BLANK(),                   
        IF(SelOrder = 1, [ytd_profit], BLANK())
    )
```

---

## Selected Measure Line Color — Chart Line Color for Selected Measure
- Table: DAX measures
- Business description: Returns a hex color string to style chart lines depending on which measure is selected in `Measure` slicer.

```dax
SWITCH(
    SELECTEDVALUE('Measure'[Measure Order]),
    0, "#476D5B",  
    1, "#09124F",   
    2, "#EFB5B9"   
)
```

---

## YTD Cost — Cost YTD (context-sensitive)
- Table: DAX measures
- Business description: Returns cost YTD when `Measure` selection corresponds to cost.

```dax
VAR SelOrder = SELECTEDVALUE('Measure'[Measure Order])
RETURN
   IF(
        ISBLANK(SelOrder), 
        BLANK(),                   
        IF(SelOrder = 2, [ytd_cost], BLANK())
    )
```

---

## #Customers Value — Selected Customers Parameter
- Table: NO.Customers
- Business description: Returns the selected numeric value from the `NO.Customers` table used to parameterize top-customer thresholds.

```dax
SELECTEDVALUE('NO.Customers'[#Customers])
```

---
