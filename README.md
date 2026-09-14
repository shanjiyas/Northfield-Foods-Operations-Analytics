# Northfield Foods — Operations Analytics & Production Planning Project

## Project Overview

Northfield Foods is a fictional UK chilled-food manufacturer created for this portfolio project. The project demonstrates an end-to-end Operations Analyst workflow covering demand forecasting, production planning, finished-goods inventory, raw-material requirements, manufacturing capacity and operational exception management.

The solution uses synthetic 2026 operational data and combines Excel, Power Query and Power BI to transform weekly operational records into planning KPIs, risk alerts and management dashboards.

## Business Problem

Manufacturing operations require demand, inventory, materials, production and capacity information to be considered together. Looking at these areas independently can hide operational risks such as stock shortages, material constraints, production underperformance or overloaded production lines.

The business problem addressed in this project is how an Operations Analyst can identify operational risks early and provide decision-support information for production planning and cross-functional teams.

## Project Objectives

- Compare forecast demand with actual customer orders.
- Calculate weekly production requirements using demand and inventory positions.
- Monitor production plan versus actual production.
- Calculate BOM-driven raw-material requirements.
- Identify material shortages and receipt-performance risks.
- Monitor production-line utilisation and capacity gaps.
- Measure finished-goods stock cover and stockout risk.
- Automatically classify operational exceptions by severity.
- Link exceptions to root causes, recommended actions and responsible functions.
- Present the results through an interactive Power BI report.

## Operational Workflow

Demand  
↓  
Forecast  
↓  
Actual Orders  
↓  
Finished-Goods Inventory  
↓  
Production Requirement  
↓  
Production Plan & Actual Output  
↓  
BOM / Material Requirements  
↓  
Material Availability  
↓  
Production-Line Capacity  
↓  
Exception Detection  
↓  
Recommended Operational Action

## Tools & Technologies

### Excel
- Synthetic operational data generation
- Production and inventory calculations
- Interactive Operations Planner
- KPI analysis
- Scenario modelling

### Power Query
- Staging layer
- Data cleaning and transformation
- Referential-integrity checks
- QA queries
- Fact and dimension preparation

### Power BI
- Star-schema semantic model
- DAX measures
- KPI reporting
- Operational dashboards
- Exception and action reporting

## Dataset & Scope

The project uses a fully synthetic dataset covering 52 weeks of 2026.

The model includes:

- 5 finished products
- 4 supermarket customers
- 8 raw and packaging materials
- 3 production lines
- 52 weekly periods
- Demand and forecast records at Week × Customer × Product level
- Operations records at Week × Product level
- Material records at Week × Material level
- Capacity records at Week × Production Line level

All company names, products, customers and operational scenarios in this project are fictional. No confidential or employer data was used.

## Data Model

The Power BI solution uses a star-schema design with dimension tables filtering operational fact tables through one-to-many relationships and single-direction filtering.

### Dimension Tables

| Table | Purpose |
|---|---|
| `dim_Week` | Weekly calendar used for time-based filtering and analysis |
| `dim_Product` | Product master including product name, category, pack size and shelf life |
| `dim_Customer` | Customer master for supermarket-level demand analysis |
| `dim_Material` | Raw-material and packaging master including unit of measure |
| `dim_ProductionLine` | Production-line master containing available hours and production rates |

### Fact Tables

| Table | Grain | Purpose |
|---|---|---|
| `fact_Demand` | Week × Customer × Product | Forecast and actual customer demand analysis |
| `fact_WeeklyOperations` | Week × Product | Finished-goods inventory, production planning and actual production |
| `fact_MaterialRequirements` | Week × Product × Material | BOM-driven material requirements |
| `fact_MaterialInventory` | Week × Material | Material inventory, receipts and shortages |
| `fact_LineCapacity` | Week × Production Line | Required hours, available capacity, utilisation and capacity gaps |
| `fact_Exceptions` | Week × Product | Operational exception scoring, severity, root cause and recommended action |

The model uses one-to-many relationships from dimensions to facts with single-direction cross-filtering to minimise ambiguity and maintain consistent KPI behaviour.

Staging and QA queries are retained in Power Query but are not loaded into the final reporting model.

## Key KPIs

The analytical model includes DAX measures covering demand, production, inventory, materials, capacity and operational risk.

| KPI | Definition / Business Use |
|---|---|
| Forecast Accuracy % | `1 - WAPE`; measures overall forecast performance |
| Forecast Bias % | Measures systematic over- or under-forecasting |
| WAPE % | Absolute forecast error divided by actual demand |
| Production Attainment % | Actual production divided by planned production |
| Production Variance Qty | Actual production minus planned production |
| Average Stock Cover Days | Closing finished-goods stock divided by average daily demand |
| FG Shortage Qty | Quantity of customer demand that cannot be covered by available stock and production |
| FG Shortage Events | Number of product-weeks containing a finished-goods shortage |
| Material Receipt Attainment % | Actual material receipts divided by planned receipts |
| Material Shortage Events | Number of material-weeks with insufficient available material |
| Weighted Capacity Utilisation % | Total required production hours divided by total effective available hours |
| Over-Capacity Line-Weeks | Number of production-line weeks where required output exceeds capacity |
| Total Capacity Gap Packs | Production volume exceeding available line capacity |
| Critical Exceptions | Product-week exceptions classified as Critical |
| Warning Exceptions | Product-week exceptions classified as Warning |
| Actionable Exceptions | Critical plus Warning product-week exceptions |

### KPI Validation

At full-year level, the model produces key results including:

- Forecast Accuracy: **96.6%**
- WAPE: **3.4%**
- Forecast Bias: approximately **-0.04%**
- Production Attainment: approximately **99.3%**
- Finished-Goods Shortage: **221 packs**
- Material Shortage Events: **1**
- Weighted Capacity Utilisation: **38.9%**
- Over-Capacity Line-Weeks: **1**
- Capacity Gap: **9,750 packs**
- Critical Exceptions: **4**
- Warning Exceptions: **129**

Material quantities are not aggregated into a single company-wide shortage quantity because materials use different units of measure, including kilograms and individual packaging units. Cross-material risk is therefore primarily reported using shortage-event counts rather than summed quantities.

## Exception Engine

An automated exception engine was developed at Week × Product level to convert operational KPIs into prioritised management actions.

The workflow is:

**Detect → Score → Prioritise → Explain → Recommend Action → Assign Owner**

### Exception Signals

The engine monitors:

- Finished-goods shortages
- Material shortages
- Production-line capacity constraints
- Production attainment
- Finished-goods stock cover
- Forecast variance
- High capacity utilisation

### Exception Scoring

Higher-risk operational conditions receive larger scores.

Examples include:

- Finished-goods shortage: +5
- Material shortage: +5
- Over-capacity production line: +5
- Severe production shortfall: +3
- Critical stock cover below one day: +3
- Forecast variance above defined thresholds: +1 or +2

### Severity Classification

Exceptions are classified into:

- **CRITICAL** — immediate operational review required
- **WARNING** — requires monitoring or investigation
- **OK** — no immediate action required

Critical operational conditions such as a finished-goods shortage, material shortage or over-capacity line are automatically escalated regardless of other KPI performance.

### Root Cause & Action Logic

Each exception is assigned:

- Primary exception
- Root-cause category
- Recommended operational response
- Responsible business function

Example:

**Capacity Constraint → Capacity → Review line loading and production schedule → Production / Planning**

This allows the report to move beyond identifying a problem and instead provide structured operational decision support.

### Exception Scenario Validation

The synthetic dataset contains deliberately designed operational stress scenarios to validate the exception logic.

| Scenario | Result |
|---|---|
| September material disruption | Chicken Meat (M002) shortage of 398 kg |
| November capacity reduction | L02 utilisation reached 161.9% with a 9,750-pack capacity gap |
| December production disruption | P005 production attainment fell to 70%, resulting in a 221-pack finished-goods shortage and zero closing stock cover |

These scenarios were intentionally simulated to test whether the analytical model correctly identifies, prioritises and explains operational risk.


## Power BI Dashboard

The Power BI report contains four interactive pages designed around different operational management questions.

### 1. Executive Overview

Provides a high-level view of operational performance across demand, production, inventory, materials and capacity.

Key elements include:

- Forecast Accuracy
- Production Attainment
- Average Stock Cover
- Critical Exceptions
- Material Shortage Events
- Over-Capacity Line-Weeks
- Forecast vs Actual Demand trend
- Planned vs Actual Production trend
- Actionable Exceptions by Type

This page is designed to answer:

**How is the operation performing overall, and where does management attention need to be focused?**

---

### 2. Demand & Production

Provides detailed analysis of forecast performance and production delivery.

Key elements include:

- Total Forecast Quantity
- Total Actual Order Quantity
- Forecast Variance
- Forecast Accuracy
- Production Attainment
- Production Variance
- Weekly Forecast vs Actual Demand
- WAPE by Product
- Weekly Planned vs Actual Production
- Production Attainment by Product

This page is designed to answer:

**Are demand and production aligned, and which products are contributing most to forecast or production variance?**

---

### 3. Materials & Capacity

Focuses on raw-material availability and manufacturing-line constraints.

Key elements include:

- Material Receipt Attainment
- Material Shortage Events
- Weighted Capacity Utilisation
- Over-Capacity Line-Weeks
- Capacity Gap Packs
- Material Receipt Attainment by Material
- Material Shortage Events by Material
- Weekly Capacity Utilisation by Production Line
- Capacity Gap by Production Line

This page is designed to answer:

**Where are material-supply or production-capacity constraints creating operational risk?**

---

### 4. Exceptions & Actions

Provides an operational risk-management view using the automated exception engine.

Key elements include:

- Critical Exceptions
- Warning Exceptions
- Actionable Exceptions
- Finished-Goods Shortage Product-Weeks
- Capacity-Exposed Product-Weeks
- Actionable Exceptions by Root Cause
- Monthly Exception Trend
- Operational Exception Action Log

The action log includes:

- Week
- Product
- Severity
- Exception Score
- Primary Exception
- Root-Cause Category
- Recommended Action
- Action Owner

This page is designed to answer:

**What requires attention, why is it happening, and what action should be considered next?**

## Key Business Insights

### 1. Overall forecasting performance was strong

Full-year Forecast Accuracy reached **96.6%**, with WAPE of **3.4%** and Forecast Bias of approximately **-0.04%**.

This indicates that total forecast demand was closely aligned with actual customer orders and that there was very little systematic over- or under-forecasting.

---

### 2. Product-level forecast performance still varied

Although overall forecast performance was strong, product-level WAPE showed differences between SKUs.

Veggie Breakfast Patties recorded approximately **3.9% WAPE**, compared with around **3.0%** for the strongest-performing product.

This demonstrates why product-level analysis is necessary even when overall forecasting KPIs appear healthy.

---

### 3. Production performance was strong overall, but individual disruptions created service risk

Overall Production Attainment was approximately **99.3%**.

However, the December P005 production disruption reduced weekly attainment to **70%**, resulting in:

- 221-pack finished-goods shortage
- Zero closing finished-goods stock
- Zero days of stock cover
- CRITICAL exception classification

This demonstrates how strong annual production performance can hide significant short-term operational failures.

---

### 4. Overall capacity appeared comfortable, but one production line experienced a severe bottleneck

Weighted annual capacity utilisation was approximately **38.9%**.

However, during the week of 02 November, production line L02 reached **161.9% utilisation**, creating a **9,750-pack capacity gap**.

Both Herb Chicken Bites and Smoky Beef Bites were exposed because they share the same production line.

This demonstrates why capacity must be analysed at line-and-week level rather than only through annual averages.

---

### 5. Material supply was highly reliable overall, but one shortage still created production risk

Annual Material Receipt Attainment was approximately **99.9%**.

Despite this, Chicken Meat (M002) experienced a **398 kg shortage** during the September stress scenario.

This shows that high annual supply performance does not eliminate short-term operational risk.

---

### 6. Most actionable exceptions were early inventory warnings rather than critical failures

The exception engine identified:

- **133 actionable product-weeks**
- **129 Warning**
- **4 Critical**

Most warning-level exceptions were driven by low finished-goods stock cover.

This demonstrates the value of exception-based monitoring as an early-warning system rather than only identifying failures after a stockout has already occurred.

## Management Recommendations

1. **Manage capacity at line-and-week level rather than relying only on annual averages.**  
   The L02 capacity event demonstrated that significant local constraints can exist even when overall factory utilisation appears comfortable.

2. **Use exception-based inventory monitoring to identify service risk early.**  
   Warning-level low-stock-cover events should be monitored before they develop into finished-goods shortages.

3. **Review forecast performance at SKU level as well as overall level.**  
   Product-level forecast differences can be hidden by strong company-wide forecast accuracy.

4. **Monitor material availability through weekly exception reporting.**  
   High annual receipt attainment should not prevent short-term material shortages from being escalated.

5. **Link operational exceptions directly to root cause, action and ownership.**  
   Structured exception management supports faster cross-functional decision-making and prioritisation.

## Project Limitations

This project is a portfolio simulation built using synthetic operational data rather than live manufacturing data.

The main limitations are:

- Demand, production, inventory and material behaviour are simulated rather than sourced from a live ERP or manufacturing system.
- Forecasting is based on controlled synthetic assumptions rather than a production forecasting model.
- Production planning is modelled at weekly product level rather than detailed shift, batch or sequencing level.
- Labour availability and workforce scheduling are outside the current scope.
- Supplier lead times, purchase orders and supplier performance are not modelled in detail.
- Transport, distribution and customer delivery scheduling are outside the scope.
- Quality-control losses, rejects and production yield variation are not modelled separately.
- Financial measures such as production cost, margin and waste cost are not included.
- Material quantities use different units of measure, so cross-material quantities are not aggregated into a single total.
- Operational scenarios were deliberately introduced to validate the exception engine and should not be interpreted as real company events.

The project therefore focuses primarily on the analytical workflow of an Operations Analyst:

**Demand → Inventory → Production → Materials → Capacity → Exceptions → Action**

## Repository Structure

## Repository Structure


Northfield-Foods-Operations-Analytics/
│
├── README.md
├── .gitignore
│
├── data/
│   └── .gitkeep
│
├── excel/
│   └── Northfield_Foods_Operations_Analytics.xlsx
│
├── powerbi/
│   └── Northfield_Foods_Operations_Analytics.pbix
│
├── documentation/
│   ├── 01_Key_Business_Insights.docx
│   ├── 02_Operational_Scenario_Deep_Dives.docx
│   └── 03_Management_Recommendations_and_Project_Summary.docx
│
└── images/
    ├── 01_Executive_Overview.png
    ├── 02_Demand_and_Production.png
    ├── 03_Materials_and_Capacity.png
    └── 04_Exceptions_and_Actions.png


The repository is organised so that the analytical model, documentation and dashboard outputs can be reviewed independently.

The repository is organised so that the data, analytical model, documentation and dashboard outputs can be reviewed independently.

## Author

**Shanjiyas K N**

MSc Business Analytics  
University of Stirling, Scotland

### Portfolio Focus

- Operations Analytics
- Business Intelligence
- Data Analysis
- Production Planning
- Supply Chain Analytics

### Tools Used

**Excel | Power Query | Power BI | DAX**