# Executive View

The product owner, Nick Puri, requested an **Executive View** to provide high-level insights into the project's key performance indicators. To accomplish this, we created additional measures and designed various visualizations to effectively present the data.

---
## Custom Measures
To support the executive view, the following measures were created:

- **Market Share %**  
  ```DAX
  Market_share % = DIVIDE (SUM(marketshare [sales_$]), SUM(marketshare [total_market_sales_$]), 0)
  ```

- **AtliQ MS %** (AtliQ's market share percentage)  
  ```DAX
  AtliQ MS % = CALCULATE('Key Measures'[Market_share %], marketshare[Manufacturar]="AtliQ")
  ```

- **RC %** (Revenue Contribution %)  
  ```DAX
  RC % = DIVIDE([NS $], CALCULATE([NS $], ALL(dim_market), ALL(dim_customer), ALL(dim_product)))
  ```

---
## Yearly Trend Analysis
Nick Puri wanted to visualize the **yearly trend of GM%, NP%, and PC%**. To achieve this, we created the following visualization:

### **Visualization: Line and Clustered Column Chart**
- **X-Axis:** Fiscal Year Description (`fy_description`)
- **Column Y-Axis:** Net Sales (`NS$`)
- **Line Y-Axis:**
  - Gross Margin (`GM%`)
  - Net Profit (`NP%`)
  - AtliQ Market Share (`AtliQ MS%`)

---
## Matrix Reports
To provide a detailed breakdown, three matrix reports were created:

### **1. Key Insights by Sub-Zone**
**Rows:** Sub-Zone  
**Values:**
- Net Sales (`NS$`)
- Revenue Contribution (`RC%`)
- Gross Margin (`GM%`)
- Net Profit (`NP%`)
- Net Error (`NE%`)
- AtliQ Market Share (`AtliQ MS%`)
- Risk

### **2. Top 5 Products by Revenue**
**Rows:** Product  
**Values:**
- Revenue Contribution (`RC%`)
- Gross Margin (`GM%`)

### **3. Top 5 Customers by Revenue**
**Rows:** Customer  
**Values:**
- Revenue Contribution (`RC%`)
- Gross Margin (`GM%`)

---
## Market Share by Manufacturer
Since market share data was not previously available, **Nick Puri provided an Excel file containing market share data for Fiscal Years 2018-2022**. This data was imported into the data model as a fact table.

### **Visualization: Market Share by Manufacturer**
- **X-Axis:** Fiscal Year Description (`fy_description`)
- **Y-Axis:** Market Share %
- **Legend:** Manufacturer
- **Filters:** Excluded "Others" category (not primary competitors)

---
## Donut Charts
Two **donut charts** were created to visualize revenue distribution:

1. **Revenue by Division**
   - **Legend:** Division
   - **Values:** Net Sales (`NS$`)

2. **Revenue by Channel**
   - **Legend:** Channel
   - **Values:** Net Sales (`NS$`)

---
## Slicers & Filters
To enhance interactivity, the following slicers were implemented:

- **Fiscal Year Slicer**
- **Quarter Slicer**
- **YTD/YTG (Year-To-Date / Year-To-Go) Slicer**
- **Bookmark "More Filters" Option** containing:
  - Market
  - Customer
  - Segment

This ensures flexibility in data exploration while keeping the executive dashboard clean and insightful.

---
## Conclusion
The **Executive View** provides a high-level summary of the business performance, covering market share, revenue contribution, profitability trends, and risk analysis. It includes both numerical and visual insights, enabling leadership to make data-driven decisions efficiently.

