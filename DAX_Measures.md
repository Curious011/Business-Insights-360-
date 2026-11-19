# 📐 DAX Calculations & Measures Documentation

This documentation provides the complete catalog of **Data Analysis Expressions (DAX)** implemented within the **Business Insights 360 Enterprise Analytics Dashboard** for **AtliQ Hardware**. 

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
* **Purpose**: Computes total gross revenue before any pre-invoice or post-invoice deductions.
* **Format**: Currency (`$#,##0` / `₹#,##0`)

---

### Pre-Invoice Deduction
```dax
Pre Invoice Deduction = 
SUM(fact_actuals_estimates[pre_invoice_discount_amount])
```
* **Purpose**: Aggregates agreed upfront contract discounts provided directly on the invoice.
* **Format**: Currency

---

### Net Invoice Sales
```dax
Net Invoice Sales = 
[Gross Sales] - [Pre Invoice Deduction]
```
* **Purpose**: Calculates invoiced revenue before post-invoice customer deductions and promotional rebates.
* **Format**: Currency

---

### Total Post-Invoice Deductions
```dax
Total Post Invoice Deduction = 
SUM(fact_actuals_estimates[post_invoice_deductions_amount])
```
* **Purpose**: Sums promotional allowances, performance rebates, and seasonal return deductions.
* **Format**: Currency

---

### Net Sales
```dax
Net sales = 
SUM(fact_actuals_estimates[net_sales_amt.])
```
* **Alternative Formula**: `[Net Invoice Sales] - [Total Post Invoice Deduction]`
* **Purpose**: Primary top-line revenue metric evaluating actual realized sales across all channels and markets.
* **FY 2021 Model Value**: **$823.85 Million**

---

### Total COGS (Cost of Goods Sold)
```dax
Total COGS = 
SUM(fact_actuals_estimates[Total_cogs_amt])
```
* **Component Breakdown**:
  * `[Manufacturing Cost]` = `SUM(fact_actuals_estimates[manufacturing_cost_amt])`
  * `[Freight Cost]` = `SUM(fact_actuals_estimates[freight_cost_amt])`
  * `[Other Cost]` = `SUM(fact_actuals_estimates[other_cost_amt])`
* **Formula**: `[Manufacturing Cost] + [Freight Cost] + [Other Cost]`
* **FY 2021 Model Value**: **$523.22 Million**

---

### Gross Margin (GM)
```dax
Gross Margin = 
[Net sales] - [Total COGS]
```
* **Purpose**: Measures gross profit generated after direct production and logistics fulfillment costs.
* **FY 2021 Model Value**: **$300.63 Million**

---

### Gross Margin % (GM %)
```dax
GM % = 
DIVIDE([Gross Margin], [Net sales], 0)
```
* **Purpose**: Evaluates gross profitability percentage per dollar of Net Sales.
* **FY 2021 Model Value**: **36.49%** (Benchmark FY20: **37.10%**)
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
* **FY 2021 Model Value**: **$355.28 Million** (a **+249.3% surge** from $101.71M in FY20)

---

### Net Profit
```dax
Net Profit = 
[Gross Margin] - [Operational Expense]
```
* **Purpose**: Bottom-line operating profit after all cost components and operational expenses.
* **FY 2021 Model Value**: **-$54.65 Million** (Operating Loss)

---

### Net Profit %
```dax
Net Profit % = 
DIVIDE([Net Profit], [Net sales], 0)
```
* **Purpose**: Bottom-line net profit margin efficiency.
* **FY 2021 Model Value**: **-6.63%**

---

## 2. Dynamic P&L Modeling & Matrix Formatting

To present a traditional accounting Profit & Loss statement inside a single Power BI Matrix visual without creating separate tables, disconnected supporting tables (`P&L Rows` and `P & L Columns`) are utilized with dynamic DAX switching logic.

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
* **FY 2021 Model Value**: **$267.98 Million** (Reflecting FY20 Net Sales)

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
* **FY 2021 Model Total**: **49,412,868 units**

---

### Absolute Error (Abs Error)
```dax
abs error = 
SUM(fact_forecast_monthly[abs_error])
```
* **Formula Logic**: `SUMX(fact_forecast_monthly, ABS(fact_forecast_monthly[forecast_quantity] - fact_forecast_monthly[actual_quantity]))`
* **Purpose**: Quantifies total magnitude of forecast variance regardless of direction (under or over).
* **FY 2021 Model Total**: **9,780,738 units** (+70.3% YoY increase)

---

### Absolute Error Last Year (Abs Error LY)
```dax
Abs Error LY = 
CALCULATE(
    [abs error],
    SAMEPERIODLASTYEAR(dim_date[date])
)
```
* **FY 2021 Model Value**: **5,743,199 units**

---

### Net Forecast Error (Net Error)
```dax
Net Error = 
SUM(fact_forecast_monthly[net_error])
```
* **Formula Logic**: `SUMX(fact_forecast_monthly, fact_forecast_monthly[forecast_quantity] - fact_forecast_monthly[actual_quantity])`
* **Purpose**: Evaluates directional forecasting bias:
  * Positive Net Error $\rightarrow$ Over-forecasted demand.
  * Negative Net Error $\rightarrow$ Under-forecasted demand.
* **FY 2021 Model Value**: **-751,714 units** (shifted from +491.6K in FY20)

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
* **FY 2021 Model Value**: **80.21%** (Benchmark FY20: **72.99%**, +7.22% YoY gain)

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
  * **Out of Stock**: Projected demand was lower than customer actuals, risking stockouts.
  * **Excess Inventory**: Projected demand exceeded actual customer demand, risking high warehouse holding fees and obsolescence.

---

## 5. Market Share & Executive KPIs

Measures engineered for the Executive View to benchmark AtliQ's market position against major industry competitors (Dell, HP, Lenovo, Asus, Acer).

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
* **Purpose**: Tracks AtliQ's market share percentage in the personal computer (PC) division over fiscal years.
* **Model Benchmark (FY21)**: **7.60%** (target 5.90% in 2022Est across sub-zones).

---

## 📁 Summary Matrix of Calculated Measures

| Category | Measure Name | Base Formula / Source | Format |
| :--- | :--- | :--- | :--- |
| **Finance** | `Gross Sales` | `SUM(fact_actuals_estimates[gross_sales_amount])` | Currency |
| **Finance** | `Net Sales` | `SUM(fact_actuals_estimates[net_sales_amt.])` | Currency |
| **Finance** | `Total COGS` | `SUM(fact_actuals_estimates[Total_cogs_amt])` | Currency |
| **Finance** | `Gross Margin` | `[Net sales] - [Total COGS]` | Currency |
| **Finance** | `GM %` | `DIVIDE([Gross Margin], [Net sales], 0)` | Percentage |
| **Finance** | `Operational Expense` | `SUM('Operational Expense'[operational_expense])` | Currency |
| **Finance** | `Net Profit` | `[Gross Margin] - [Operational Expense]` | Currency |
| **Finance** | `Net Profit %` | `DIVIDE([Net Profit], [Net sales], 0)` | Percentage |
| **P&L Matrix** | `P&L Values` | Dynamic `SWITCH(SELECTEDVALUE('P&L Rows'[Line Item]))` | Currency / % |
| **P&L Matrix** | `P&L LY` | `CALCULATE([P&L Values], SAMEPERIODLASTYEAR(...))` | Currency / % |
| **P&L Matrix** | `P&L YOY Chg%` | `DIVIDE([P&L Values] - [P&L LY], [P&L LY], 0)` | Percentage |
| **Supply Chain** | `Forecast Quantity` | `SUM(fact_forecast_monthly[forecast_quantity])` | Integer (Units) |
| **Supply Chain** | `abs error` | `SUM(fact_forecast_monthly[abs_error])` | Integer (Units) |
| **Supply Chain** | `Net Error` | `SUM(fact_forecast_monthly[net_error])` | Integer (Units) |
| **Supply Chain** | `Forecast Accuracy %` | `1 - DIVIDE([abs error], [Forecast Quantity], 0)` | Percentage |
| **Supply Chain** | `Risk` | `SWITCH([Net Error] < 0, "Out of Stock", ...)` | Text |
| **Executive** | `Market Share%` | `DIVIDE(SUM(marketshare[sales_volume]), Total)` | Percentage |
| **Executive** | `Atliq MS %` | `CALCULATE([Market Share%], manufacturer = "atliq")` | Percentage |
