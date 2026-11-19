# 📐 DAX Calculations & Measures Documentation

This documentation provides the complete catalog of **Data Analysis Expressions (DAX)** implemented within the **Business Insights 360 Enterprise Analytics Dashboard** for **AtliQ Hardware**. 

All monetary values are denominated in **Indian Rupees (₹)**.

Measures are categorized into six functional analytical tiers:
1. **Core Financial & Profitability Measures**
2. **Dynamic P&L Modeling & Matrix Formatting**
3. **Time Intelligence & YoY Benchmarks**
4. **Supply Chain & Demand Forecasting Metrics**
5. **Customer & Segment Performance Ratios**
6. **Market Share & Executive KPIs**

---

## 1. Core Financial & Profitability Measures

These core calculations form the financial backbone of the reporting system, tracking the step-by-step journey from top-line Gross Sales down to bottom-line Net Profit.

### Gross Sales
```dax
Gross Sales = 
SUM(fact_actuals_estimates[gross_sales_amount])
```
* **Purpose**: Computes total gross revenue before any pre-invoice discounts or post-invoice deductions.
* **FY 2021 Model Value**: **₹ 1,664.64 Million** (Benchmark FY20: **₹ 535.95 Million**, +210.60% YoY)
* **Format**: Currency (`₹#,##0`)

---

### Pre-Invoice Deduction
```dax
Pre Invoice Deduction = 
SUM(fact_actuals_estimates[pre_invoice_discount_amount])
```
* **Purpose**: Aggregates agreed upfront contract discounts provided directly on the invoice.
* **FY 2021 Model Value**: **₹ 392.50 Million** (Benchmark FY20: **₹ 124.69 Million**, +214.77% YoY)
* **Format**: Currency (`₹#,##0`)

---

### Net Invoice Sales
```dax
Net Invoice Sales = 
[Gross Sales] - [Pre Invoice Deduction]
```
* **Purpose**: Calculates invoiced revenue before post-invoice customer deductions and promotional rebates.
* **FY 2021 Model Value**: **₹ 1,272.13 Million** (Benchmark FY20: **₹ 411.25 Million**, +209.33% YoY)
* **Format**: Currency (`₹#,##0`)

---

### Post Discounts
```dax
Post Discounts = 
SUM(fact_actuals_estimates[post_invoice_deductions_pct])
```
* **Purpose**: Tracks promotional and volume-based discounts settled post-invoicing.
* **FY 2021 Model Value**: **₹ 281.64 Million** (Benchmark FY20: **₹ 95.85 Million**)
* **Format**: Currency (`₹#,##0`)

---

### Post Deductions
```dax
Post Deductions = 
SUM(fact_actuals_estimates[post_invoice_otherdeductions_pct])
```
* **Purpose**: Captures performance rebates, damaged goods write-offs, and customer allowances.
* **FY 2021 Model Value**: **₹ 166.65 Million** (Benchmark FY20: **₹ 47.43 Million**)
* **Format**: Currency (`₹#,##0`)

---

### Total Post-Invoice Deductions
```dax
Total Post Invoice Deduction = 
[Post Discounts] + [Post Deductions]
```
* **Purpose**: Sums promotional allowances, performance rebates, and seasonal return deductions.
* **FY 2021 Model Value**: **₹ 448.29 Million** (Benchmark FY20: **₹ 143.27 Million**, +212.89% YoY)
* **Format**: Currency (`₹#,##0`)

---

### Net Sales
```dax
Net sales = 
SUM(fact_actuals_estimates[net_sales_amt.])
```
* **Alternative Formula**: `[Net Invoice Sales] - [Total Post Invoice Deduction]`
* **Purpose**: Primary top-line revenue metric evaluating actual realized sales across all channels and markets.
* **FY 2021 Model Value**: **₹ 823.85 Million** (Benchmark FY20: **₹ 267.98 Million**, **+207.43% YoY**)
* **Format**: Currency (`₹#,##0`)

---

### Manufacturing Cost
```dax
Manufacturing Cost = 
SUM(fact_actuals_estimates[manufacturing_cost_amt])
```
* **Purpose**: Direct production and assembly costs of hardware products.
* **FY 2021 Model Value**: **₹ 497.78 Million** (Benchmark FY20: **₹ 160.30 Million**, +210.53% YoY)
* **Format**: Currency (`₹#,##0`)

---

### Freight Cost
```dax
Freight Cost = 
SUM(fact_actuals_estimates[freight_cost_amt])
```
* **Purpose**: Logistics, ocean shipping, and regional freight fulfillment costs.
* **FY 2021 Model Value**: **₹ 22.05 Million** (Benchmark FY20: **₹ 7.16 Million**, +207.98% YoY)
* **Format**: Currency (`₹#,##0`)

---

### Other Cost
```dax
Other Cost = 
SUM(fact_actuals_estimates[other_cost_amt])
```
* **Purpose**: Miscellaneous production and warehousing surcharge costs.
* **FY 2021 Model Value**: **₹ 3.39 Million** (Benchmark FY20: **₹ 1.10 Million**)
* **Format**: Currency (`₹#,##0`)

---

### Total COGS (Cost of Goods Sold)
```dax
Total COGS = 
SUM(fact_actuals_estimates[Total_cogs_amt])
```
* **Component Breakdown**: `[Manufacturing Cost] + [Freight Cost] + [Other Cost]`
* **Purpose**: Aggregates total cost of goods sold across manufacturing, shipping, and fulfillment.
* **FY 2021 Model Value**: **₹ 523.22 Million** (Benchmark FY20: **₹ 168.56 Million**, +210.41% YoY)
* **Format**: Currency (`₹#,##0`)

---

### Gross Margin (GM)
```dax
Gross Margin = 
[Net sales] - [Total COGS]
```
* **Purpose**: Measures gross profit generated after direct production and logistics fulfillment costs.
* **FY 2021 Model Value**: **₹ 300.63 Million** (Benchmark FY20: **₹ 99.42 Million**, **+202.37% YoY**)
* **Format**: Currency (`₹#,##0`)

---

### Gross Margin % (GM %)
```dax
GM % = 
DIVIDE([Gross Margin], [Net sales], 0)
```
* **Purpose**: Evaluates gross profitability percentage per rupee of Net Sales.
* **FY 2021 Model Value**: **36.49%** (Benchmark FY20: **37.10%**, -0.61% YoY / -1.65% variance)
* **Format**: Percentage (`0.00%`)

---

### Operational Expenses (OPEX)
```dax
Operational Expense = 
CALCULATE(
    SUM('Operational Expense'[operational_expense])
)
```
* **Purpose**: Aggregates commercial sales overhead, marketing campaigns, and regional operational administration fees.
* **FY 2021 Model Value**: **₹ 355.28 Million** (Benchmark FY20: **₹ 101.71 Million**, a **+249.30% surge**)
* **Format**: Currency (`₹#,##0`)

---

### Net Profit
```dax
Net Profit = 
[Gross Margin] - [Operational Expense]
```
* **Purpose**: Bottom-line operating profit after direct fulfillment costs and operational expenses.
* **FY 2021 Model Value**: **-₹ 54.65 Million** (Operating Loss widened from -₹ 2.29 Million in FY20)
* **Format**: Currency (`₹#,##0`)

---

### Net Profit %
```dax
Net Profit % = 
DIVIDE([Net Profit], [Net sales], 0)
```
* **Purpose**: Bottom-line net profit margin efficiency.
* **FY 2021 Model Value**: **-6.63%** (Benchmark FY20: **-0.85%**, -5.78% YoY variance)
* **Format**: Percentage (`0.00%`)

---

## 2. Dynamic P&L Modeling & Matrix Formatting

To present a traditional accounting Profit & Loss statement inside a single Power BI Matrix visual without creating separate physical tables, disconnected supporting tables (`P&L Rows` and `P & L Columns`) are utilized with dynamic DAX switching logic.

### Dynamic P&L Measure Switcher
```dax
P&L Values = 
VAR _SelectedLineItem = SELECTEDVALUE('P&L Rows'[Line Item])
RETURN
SWITCH(
    TRUE(),
    _SelectedLineItem = "Gross Sales", [Gross Sales],
    _SelectedLineItem = "Pre Invoice Deduction", [Pre Invoice Deduction],
    _SelectedLineItem = "Net Invoice Sales", [Net Invoice Sales],
    _SelectedLineItem = "Post Discounts", [Post Discounts],
    _SelectedLineItem = "Post Deductions", [Post Deductions],
    _SelectedLineItem = "Total Post Invoice Deduction", [Total Post Invoice Deduction],
    _SelectedLineItem = "Net Sales", [Net sales],
    _SelectedLineItem = "Manufacturing Cost", [Manufacturing Cost],
    _SelectedLineItem = "Freight Cost", [Freight Cost],
    _SelectedLineItem = "Other Cost", [Other Cost],
    _SelectedLineItem = "Total COGS", [Total COGS],
    _SelectedLineItem = "Gross Margin", [Gross Margin],
    _SelectedLineItem = "Gross Margin %", [GM %],
    _SelectedLineItem = "Operational Expense", [Operational Expense],
    _SelectedLineItem = "Net Profit", [Net Profit],
    _SelectedLineItem = "Net Profit%", [Net Profit %],
    [Net sales] -- Default fallback
)
```

---

### Dynamic P&L Prior Year Value
```dax
P&L LY = 
CALCULATE(
    [P&L Values],
    SAMEPERIODLASTYEAR(dim_date[date])
)
```

---

### Dynamic P&L YoY Growth %
```dax
P&L YOY Chg% = 
VAR _Current = [P&L Values]
VAR _Previous = [P&L LY]
RETURN
DIVIDE(_Current - _Previous, _Previous, 0)
```

---

### Dynamic P&L Column Formatter (Matrix Matrix Header Evaluation)
```dax
P&L Final Value = 
VAR _ColHeader = SELECTEDVALUE('P & L Columns'[Col Header])
RETURN
SWITCH(
    TRUE(),
    _ColHeader = "2020", CALCULATE([P&L Values], dim_date[fiscal_year] = "2020"),
    _ColHeader = "2021", CALCULATE([P&L Values], dim_date[fiscal_year] = "2021"),
    _ColHeader = "BM₹" || _ColHeader = "BM$", [P&L LY],
    _ColHeader = "Chg", [P&L Values] - [P&L LY],
    _ColHeader = "Chg %", [P&L YOY Chg%],
    [P&L Values]
)
```

---

### Dynamic Visual Title
```dax
Performance Visual Title = 
VAR _Selected = SELECTEDVALUE('P&L Rows'[Line Item], "Net Sales")
RETURN
_Selected & " Performance Trend (vs. LY)"
```
* **Purpose**: Dynamically updates the line/area chart title in the Finance View to reflect whichever row the user clicks in the P&L statement.

---

## 3. Time Intelligence Measures

AtliQ Hardware operates on a custom **Fiscal Year running from September to August**. These time intelligence measures compute prior-year benchmarks dynamically based on slicer selection.

### Net Sales Last Year (LY)
```dax
Net Sales LY = 
CALCULATE(
    [Net sales],
    SAMEPERIODLASTYEAR(dim_date[date])
)
```
* **FY 2021 Model Value**: **₹ 267.98 Million** (Reflecting FY20 Net Sales)

---

### Gross Margin % Last Year (GM % LY)
```dax
GM % LY = 
CALCULATE(
    [GM %],
    SAMEPERIODLASTYEAR(dim_date[date])
)
```
* **FY 2021 Model Value**: **37.10%**

---

### Net Profit % Last Year (Net Profit % LY)
```dax
Net Profit % LY = 
CALCULATE(
    [Net Profit %],
    SAMEPERIODLASTYEAR(dim_date[date])
)
```
* **FY 2021 Model Value**: **-0.85%**

---

## 4. Supply Chain & Demand Forecasting Metrics

These measures track demand forecasting reliability, quantify unit forecast error, and segment accounts into operational risk tiers.

### Forecast Quantity
```dax
Forecast Quantity = 
SUM(fact_forecast_monthly[forecast_quantity])
```
* **Purpose**: Cumulative projected unit demand across products and customer accounts.
* **FY 2021 Model Total**: **49,412,868 units** (~49.41M units, +132.4% YoY)

---

### Absolute Error (abs error)
```dax
abs error = 
SUM(fact_forecast_monthly[abs_error])
```
* **Formula Logic**: `SUMX(fact_forecast_monthly, ABS(fact_forecast_monthly[forecast_quantity] - fact_forecast_monthly[actual_quantity]))`
* **Purpose**: Quantifies total magnitude of forecast variance regardless of direction (under or over).
* **FY 2021 Model Total**: **9,780,738 units** (~9.78M units, +70.30% YoY increase)

---

### Absolute Error Last Year (Abs Error LY)
```dax
Abs Error LY = 
CALCULATE(
    [abs error],
    SAMEPERIODLASTYEAR(dim_date[date])
)
```
* **FY 2021 Model Value**: **5,743,199 units** (~5.74M units)

---

### Net Forecast Error (Net Error)
```dax
Net Error = 
SUM(fact_forecast_monthly[net_error])
```
* **Formula Logic**: `SUMX(fact_forecast_monthly, fact_forecast_monthly[forecast_quantity] - fact_forecast_monthly[actual_quantity])`
* **Purpose**: Evaluates directional forecasting bias:
  * Positive Net Error: Over-forecasted demand (inventory surplus risk).
  * Negative Net Error: Under-forecasted demand (out-of-stock risk).
* **FY 2021 Model Value**: **-751,714 units** (~ -751.71K units, shifted from +491.60K in FY20)

---

### Net Error Last Year (Net Error LY)
```dax
Net Error LY = 
CALCULATE(
    [Net Error],
    SAMEPERIODLASTYEAR(dim_date[date])
)
```
* **FY 2021 Model Value**: **+491,600 units** (+491.60K units)

---

### Net Error %
```dax
Net Error % = 
DIVIDE([Net Error], [Forecast Quantity], 0)
```
* **FY 2021 Model Value**: **-1.52%**

---

### Forecast Accuracy %
```dax
Forecast Accuracy % = 
IF(
    [abs error] > 0,
    1 - DIVIDE([abs error], [Forecast Quantity], 0),
    0
)
```
* **Purpose**: Key supply chain KPI measuring overall accuracy of monthly demand predictions.
* **FY 2021 Model Value**: **80.21%** (Benchmark FY20: **72.99%**, +7.22% absolute, +9.88% relative improvement)

---

### Forecast Accuracy Last Year % (Forecast Accuracy LY %)
```dax
Forecast Accuracy LY % = 
CALCULATE(
    [Forecast Accuracy %],
    SAMEPERIODLASTYEAR(dim_date[date])
)
```
* **FY 2021 Model Value**: **72.99%**

---

### Supply Chain Risk Profile Classification
```dax
Risk = 
SWITCH(
    TRUE(),
    [Net Error] < 0, "Out of Stock",
    [Net Error] > 0, "Excess Inventory",
    "Balanced"
)
```
* **Purpose**: Categorizes customer accounts and product categories into operational inventory risk buckets:
  * **Out of Stock**: Projected demand was lower than customer actuals (`Net Error < 0`), risking inventory depletion.
  * **Excess Inventory**: Projected demand exceeded actual customer demand (`Net Error > 0`), risking high warehouse holding fees and obsolescence.

---

## 5. Market Share & Executive KPIs

Measures engineered for the Executive View to benchmark AtliQ's market position against major industry competitors.

### Total Market Size Volume
```dax
Total Market Volume = 
CALCULATE(
    SUM(marketshare[sales_volume]),
    ALL(marketshare[manufacturer])
)
```

---

### Market Share %
```dax
Market Share% = 
DIVIDE(
    SUM(marketshare[sales_volume]),
    [Total Market Volume],
    0
)
```

---

### AtliQ Market Share % (Atliq MS %)
```dax
Atliq MS % = 
CALCULATE(
    [Market Share%],
    marketshare[manufacturer] = "atliq"
)
```
* **Purpose**: Tracks AtliQ's market share percentage over fiscal years.
* **Model Values**: **7.60%** in PC division (FY21), **1.06%** across all sub-zones.

---

## 📁 Summary Matrix of Calculated Measures

| Category | Measure Name | Base Formula / Source | Format |
| :--- | :--- | :--- | :--- |
| **Finance** | `Gross Sales` | `SUM(fact_actuals_estimates[gross_sales_amount])` | Currency (`₹#,##0`) |
| **Finance** | `Pre Invoice Deduction` | `SUM(fact_actuals_estimates[pre_invoice_discount_amount])` | Currency (`₹#,##0`) |
| **Finance** | `Net Invoice Sales` | `[Gross Sales] - [Pre Invoice Deduction]` | Currency (`₹#,##0`) |
| **Finance** | `Post Discounts` | `SUM(fact_actuals_estimates[post_invoice_deductions_pct])` | Currency (`₹#,##0`) |
| **Finance** | `Post Deductions` | `SUM(fact_actuals_estimates[post_invoice_otherdeductions_pct])` | Currency (`₹#,##0`) |
| **Finance** | `Total Post Invoice Deduction` | `[Post Discounts] + [Post Deductions]` | Currency (`₹#,##0`) |
| **Finance** | `Net sales` | `SUM(fact_actuals_estimates[net_sales_amt.])` | Currency (`₹#,##0`) |
| **Finance** | `Manufacturing Cost` | `SUM(fact_actuals_estimates[manufacturing_cost_amt])` | Currency (`₹#,##0`) |
| **Finance** | `Freight Cost` | `SUM(fact_actuals_estimates[freight_cost_amt])` | Currency (`₹#,##0`) |
| **Finance** | `Other Cost` | `SUM(fact_actuals_estimates[other_cost_amt])` | Currency (`₹#,##0`) |
| **Finance** | `Total COGS` | `SUM(fact_actuals_estimates[Total_cogs_amt])` | Currency (`₹#,##0`) |
| **Finance** | `Gross Margin` | `[Net sales] - [Total COGS]` | Currency (`₹#,##0`) |
| **Finance** | `GM %` | `DIVIDE([Gross Margin], [Net sales], 0)` | Percentage (`0.00%`) |
| **Finance** | `Operational Expense` | `CALCULATE(SUM('Operational Expense'[operational_expense]))` | Currency (`₹#,##0`) |
| **Finance** | `Net Profit` | `[Gross Margin] - [Operational Expense]` | Currency (`₹#,##0`) |
| **Finance** | `Net Profit %` | `DIVIDE([Net Profit], [Net sales], 0)` | Percentage (`0.00%`) |
| **P&L Matrix** | `P&L Values` | Dynamic `SWITCH(SELECTEDVALUE('P&L Rows'[Line Item]))` | Currency / % |
| **P&L Matrix** | `P&L LY` | `CALCULATE([P&L Values], SAMEPERIODLASTYEAR(...))` | Currency / % |
| **P&L Matrix** | `P&L YOY Chg%` | `DIVIDE([P&L Values] - [P&L LY], [P&L LY], 0)` | Percentage |
| **P&L Matrix** | `P&L Final Value` | Dynamic `SWITCH(SELECTEDVALUE('P & L Columns'[Col Header]))` | Currency / % |
| **Time Intel** | `Net Sales LY` | `CALCULATE([Net sales], SAMEPERIODLASTYEAR(...))` | Currency (`₹#,##0`) |
| **Time Intel** | `GM % LY` | `CALCULATE([GM %], SAMEPERIODLASTYEAR(...))` | Percentage (`0.00%`) |
| **Time Intel** | `Net Profit % LY` | `CALCULATE([Net Profit %], SAMEPERIODLASTYEAR(...))` | Percentage (`0.00%`) |
| **Supply Chain** | `Forecast Quantity` | `SUM(fact_forecast_monthly[forecast_quantity])` | Integer (Units) |
| **Supply Chain** | `abs error` | `SUM(fact_forecast_monthly[abs_error])` | Integer (Units) |
| **Supply Chain** | `Abs Error LY` | `CALCULATE([abs error], SAMEPERIODLASTYEAR(...))` | Integer (Units) |
| **Supply Chain** | `Net Error` | `SUM(fact_forecast_monthly[net_error])` | Integer (Units) |
| **Supply Chain** | `Net Error LY` | `CALCULATE([Net Error], SAMEPERIODLASTYEAR(...))` | Integer (Units) |
| **Supply Chain** | `Net Error %` | `DIVIDE([Net Error], [Forecast Quantity], 0)` | Percentage (`0.00%`) |
| **Supply Chain** | `Forecast Accuracy %` | `IF([abs error] > 0, 1 - DIVIDE(...), 0)` | Percentage (`0.00%`) |
| **Supply Chain** | `Forecast Accuracy LY %` | `CALCULATE([Forecast Accuracy %], SAMEPERIODLASTYEAR(...))` | Percentage (`0.00%`) |
| **Supply Chain** | `Risk` | `SWITCH([Net Error] < 0, "Out of Stock", ...)` | Text |
| **Executive** | `Market Share%` | `DIVIDE(SUM(marketshare[sales_volume]), Total)` | Percentage (`0.00%`) |
| **Executive** | `Atliq MS %` | `CALCULATE([Market Share%], manufacturer = "atliq")` | Percentage (`0.00%`) |
| **Executive** | `Performance Visual Title` | `_Selected & " Performance Trend (vs. LY)"` | Text |
