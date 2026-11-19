# Business Insights 360 – Enterprise Power BI Analytics

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Data_Analysis_Expressions-blue?style=for-the-badge)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Git LFS](https://img.shields.io/badge/Git_LFS-Supported-orange?style=for-the-badge)

An end-to-end multi-departmental Business Intelligence solution built for **AtliQ Hardware**—a global consumer electronics manufacturer operating across 27 markets. This project centralizes **1.86M+ transactional records** from disparate MySQL databases and Excel files into an interactive 5-view Power BI dashboard, replacing legacy spreadsheet silos and delivering real-time actionable insights for C-suite decision-makers.

---

## 📌 Executive Summary & Business Problem

AtliQ Hardware experienced rapid worldwide expansion across **APAC, EU, North America, and LATAM**. However, management faced significant challenges:
1. **Reporting Silos:** Finance, Sales, Marketing, and Supply Chain operated on disconnected, static Excel files, resulting in conflicting numbers and multi-day reporting lag.
2. **Hidden Losses Despite Explosive Sales:** While top-line Net Sales tripled (+207% YoY), aggressive retail discounting, surging freight rates, and escalating regional operating expenses caused a severe net profit collapse.
3. **Supply Chain Mismatches:** Fluctuating demand forecasts led to stockouts of top-selling PC lines alongside costly excess inventory of peripheral accessories.

To solve this, **Business Insights 360** was developed to provide a unified, transparent view of business performance across all departments.

---

## 📊 Key Business Insights (FY 2020 vs. FY 2021)

* **Top-Line Growth vs. Bottom-Line Reality (Finance View):**  
  While Net Sales grew **207.4% YoY** (surging from **$268.0M in FY20 to $823.8M in FY21**), the company suffered a **$54.7M net loss** (-6.6% margin) as operational expenses surged by **249.3%** to support regional expansion.
* **The Supply Chain Paradox (Supply Chain View):**  
  While Forecast Accuracy improved by **7.2% YoY (73.0% to 80.2%)**, surging shipment volumes drove a **70.3% spike in Absolute Unit Error (5.7M to 9.8M units)**, significantly elevating physical warehouse holding costs and inventory risks across 74 accounts.
* **Customer Margin Disparity (Executive View):**  
  Top-volume retail customer **Amazon** generated $109.0M in sales but operated at an **-8.9% net margin loss** (35.4% GM). In contrast, direct-to-consumer **AtliQ Exclusive stores** delivered a healthy **43.7% Gross Margin** and remained net profitable (+1.8%).
* **Channel Distribution Dynamics:**  
  Retailers contributed **71.9% of total revenue ($592.0M)**, but ran at a -11.0% net loss due to heavy promotional allowances. The Direct channel delivered a **6.5% higher Gross Margin** (41.8% vs. 35.3%) by eliminating intermediary cuts.

---

## 🖥️ Dashboard Views Walkthrough

The dashboard is structured into 5 specialized departmental views connected via an intuitive landing page:

| View | Purpose & Key Visuals |
| :--- | :--- |
| **🏠 Home View** | Department navigation hub with interactive icons and links to user documentation. |
| **📈 Finance View** | Dynamic P&L Statement with drill-down across FY, Quarters, Markets, and Product Categories; Monthly P&L Trend vs. Last Year (LY); Segment & Regional P&L tables. |
| **🤝 Sales View** | Customer Performance Matrix (Scatter chart: Net Sales vs. Gross Margin %); Customer Statement table; pre/post-invoice deduction breakdowns. |
| **📢 Marketing View** | Regional Statement (Net Sales, GM%, Net Profit%); Product Segment statement; Waterfall margin erosion chart tracking margin drop from Gross Sales to Net Profit. |
| **📦 Supply Chain View** | Forecast Accuracy % and Net Error monthly combo chart; Customer & Product risk profiling (categorizing accounts into *Out of Stock Risk* vs. *Excess Inventory Risk*). |
| **👔 Executive View** | C-Suite strategic cockpit: Top 5 Customers & Products by revenue; Revenue by Division (P&A, PC, N&S) & Channel (Retailer, Direct, Distributor); Sub-zone performance table; PC Market Share trend against industry competitors. |

---

## 🏗️ Data Architecture & Star Schema Model

The data model is architected as an optimized **Star Schema** handling **1,858,329 fact records**:

* **Fact Tables:**
  * `fact_actuals_estimates`: Granular transactional sales actuals and operational estimates.
  * `fact_forecast_monthly`: Monthly demand forecasts by customer and product.
* **Dimension Tables:**
  * `dim_customer`: Customer metadata, market, platform, and distribution channel.
  * `dim_product`: Product hierarchy (Division, Category, Segment, Product).
  * `dim_market`: Geographical segmentation (Market, Sub-zone, Region).
  * `dim_date`: Custom fiscal calendar (Fiscal Year running from September to August).
* **Supporting / Disconnected Tables:**
  * `P&L Rows` & `P & L Columns`: Supporting structural tables for dynamic matrix P&L formatting.
  * `Operational Expense`, `freight_cost`, `gross_price`, `manufacturing_cost`, `marketshare`.

---

## 🧠 Key DAX Measures Implemented

The project contains **30+ custom DAX measures** leveraging advanced modeling techniques:

### 1. Dynamic P&L Metric Switcher
```dax
P&L Values = 
VAR _SelectedLineItem = SELECTEDVALUE('P&L Rows'[Line Item])
RETURN
SWITCH(
    TRUE(),
    _SelectedLineItem = "Gross Sales", [Gross Sales],
    _SelectedLineItem = "Pre Invoice Deduction", [Pre Invoice Deduction],
    _SelectedLineItem = "Net Invoice Sales", [Net Invoice Sales],
    _SelectedLineItem = "Net Sales", [Net sales],
    _SelectedLineItem = "Total COGS", [Total COGS],
    _SelectedLineItem = "Gross Margin", [Gross Margin],
    _SelectedLineItem = "Gross Margin %", [GM %],
    _SelectedLineItem = "Operational Expense", [Operational Expense],
    _SelectedLineItem = "Net Profit", [Net Profit],
    _SelectedLineItem = "Net Profit%", [Net Profit %],
    [Net sales]
)
```

### 2. Time Intelligence (Fiscal Year Last Year Benchmark)
```dax
Net Sales LY = 
CALCULATE(
    [Net sales],
    SAMEPERIODLASTYEAR(dim_date[date])
)
```

### 3. Supply Chain Risk Classification
```dax
Risk = 
SWITCH(
    TRUE(),
    [Net Error] > 0, "Out of Stock",
    [Net Error] < 0, "Excess Inventory",
    "Balanced"
)
```

---

## 🚀 How to Run the Project

### Prerequisites
* **Power BI Desktop** (August 2023 or newer recommended).
* **Git & Git LFS** (required to clone large files over 100 MB).

### Steps
1. Clone the repository using Git LFS:
   ```bash
   git lfs install
   git clone https://github.com/<your-username>/<your-repo-name>.git
   cd <your-repo-name>
   ```
2. Double-click **`Business_Insights_360.pbix`** to open it directly in Power BI Desktop.
3. Explore the dashboard using the interactive navigation buttons on the **Home** page.

> **Note on File Size:** Because the `.pbix` file is ~320 MB (containing VertiPaq tabular model cache for 1.86M+ rows), it is tracked via **Git LFS**. Alternatively, a direct cloud download link (Google Drive / OneDrive) can be provided upon request.

---

## 👨‍💻 Author
* **Portfolio / LinkedIn:** [Your LinkedIn Profile URL](https://www.linkedin.com)
* **GitHub:** [@yourusername](https://github.com)

*Feedback and suggestions are warmly welcomed! Feel free to star ⭐ this repository if you found it insightful!*
