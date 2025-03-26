# Finance View - Power BI Implementation

## 1. Creating Metrics Table
To manage our measures efficiently, we create a new table where we will put all of our measures:
1. Navigate to **Home** > **New Table**
2. Name the table: **Key Measures**
3. Right-click on **Key Measures** and select **New Measure**, then create the following:

```DAX
GS $ = SUM(Fact_Actual_Estimates[gross_sales_amount])
NIS $ = SUM(Fact_Actual_Estimates[net_invoice_sales_amount])
Pre_Invoice_deduction % = [GS $] - [NIS $]
Post_invoice_deduction $ = SUM(Fact_Actual_Estimates[post_invoice_deduction_amount])
Post_invoice_other_deduction $ = SUM(Fact_Actual_Estimates[post_other_discount_amount])
Total_post_invoice_deduction $ = [NIS $] + [Post_invoice_other_deduction $]
NS $ = SUM(Fact_Actual_Estimates[net_sales])
Manufacturing_amount $ = SUM(Fact_Actual_Estimates[manufacturing_cost])
Freight_Cost $ = SUM(Fact_Actual_Estimates[freight_cost])
Other_Cost $ = SUM(Fact_Actual_Estimates[other_cost])
Total_Cogs $ = [Manufacturing_amount $] + [Freight_Cost $] + [Other_Cost $]
GM $ = [NS $] - [Total_Cogs $]
GM % = DIVIDE([GM $], [NS $], 0)
GM/Unit = DIVIDE([GM $], [Quantity], 0)
Quantity = SUM(Fact_Actual_Estimates[Qty])
```

## 2. Creating the P&L Static Table
We create a **static table** for Profit & Loss (P&L) visualization:
1. Navigate to **Home** > **Enter Data**
2. Paste the following data and rename it **P&L Rows**:

| Description                     | Order | Line Item                     |
|---------------------------------|-------|------------------------------|
| Gross Sales                     | 1     | Gross Sales                   |
| Pre Invoice Deduction           | 2     | Pre Invoice Deduction         |
| Net Invoice Sales               | 3     | Net Invoice Sales             |
| Post Invoice Discount           | 4     | Post Discounts                |
| Post Invoice Other Deduction    | 5     | - Post Deductions             |
| Total Post Invoice Deduction    | 6     | Total Post Invoice Deduction  |
| Net Sales                       | 7     | Net Sales                     |
| Manufacturing Cost              | 8     | - Manufacturing Cost          |
| Freight Cost                    | 9     | Freight Cost                  |
| Other Cost                      | 10    | Other Cost                    |
| Total COGS                      | 11    | Total COGS                    |
| Gross Margin                    | 12    | Gross Margin                  |
| Gross Margin %                  | 13    | Gross Margin %                |
| GM / Unit                       | 14    | GM / Unit                     |

## 3. Create a Matrix:
   - Add **Line Item** to rows so that we can put values against it.
   - Add a new measure **P&L Values** to display financials:

```DAX
P & L Values =
VAR x =
SWITCH(TRUE(),
    MAX('P&L Rows'[Order]) = 1, [GS $] / 1000000,
    MAX('P&L Rows'[Order]) = 2, [Pre_Invoice_deduction %] / 1000000,
    MAX('P&L Rows'[Order]) = 3, [NIS $] / 1000000,
    MAX('P&L Rows'[Order]) = 4, [Post_invoice_deduction $] / 1000000,
    MAX('P&L Rows'[Order]) = 5, [Post_invoice_other_deduction $] / 1000000,
    MAX('P&L Rows'[Order]) = 6, [Total_post_invoice_deduction $] / 1000000,
    MAX('P&L Rows'[Order]) = 7, [NS $] / 1000000,
    MAX('P&L Rows'[Order]) = 8, [Manufacturing_amount $] / 1000000,
    MAX('P&L Rows'[Order]) = 9, [Freight_Cost $] / 1000000,
    MAX('P&L Rows'[Order]) = 10, [Other_Cost $] / 1000000,
    MAX('P&L Rows'[Order]) = 11, [Total_Cogs $] / 1000000,
    MAX('P&L Rows'[Order]) = 12, [GM $] / 1000000,
    MAX('P&L Rows'[Order]) = 13, [GM %] * 100,
    MAX('P&L Rows'[Order]) = 14, [GM/Unit]
)
RETURN
IF(HASONEVALUE('P&L Rows'[Description]), x, [NS $])
```

## 4. Adding Net Profit
After CEO Stan's interest, and as he wanted to have net profit level insight from this report we had to extend our analysis by integrating **operational expenses**. The additional data was given by Nick Puri in a XLSX file:
1. Import the **Operational Expense** XLSX file.
2. Connect it to the **data model** as a fact table.
3. Create two calculated columns:

```DAX
ads_promotions =
VAR pct = CALCULATE(MAX('Operational Expense'[ads_promotions_pct]), RELATEDTABLE('Operational Expense'))
RETURN pct * Fact_Actual_Estimates[net_sales]

other_operational_exp =  
VAR pct = CALCULATE(MAX('Operational Expense'[other_operational_expense_pct]), RELATEDTABLE('Operational Expense'))
RETURN pct * Fact_Actual_Estimates[net_sales]
```

4. Create operational measures into the Key Measure table:

```DAX
adds_&_promo = SUM(Fact_Actual_Estimates[ads_promotions])
other_operational_expense = SUM(Fact_Actual_Estimates[other_operational_exp])
Operational_exp = ([adds_&_promo] + [other_operational_expense]) * -1
Net Profit = [GM $] + [Operational_exp]
Net Profit % = DIVIDE([Net Profit], [NS $], 0)
```

5. **Update P&L Static Table: because Stan wanted to see Operational expense, Net Profit & Net Profit %**

| Description         | Order | Line Item         |
|---------------------|-------|------------------|
| Operational Expense | 15    | Operational Exp  |
| Net Profit         | 16    | Net Profit       |
| Net Profit %       | 17    | Net Profit %     |

6. **Modify `P&L Values` formula** to include Operational Expense, Net Profit & Net Profit %.

## 5. Year-over-Year (YoY) Analysis
1. Create **Last Year (LY) column**:

```DAX
P & L LY = CALCULATE([P & L Values], SAMEPERIODLASTYEAR(dim_date[date]))
```

2. Create **YoY & YoY% measures**:

```DAX
P & L YOY = [P & L Values] - [P & L LY]
P & L YOY % = DIVIDE([P & L YOY], [P & L LY], 0)
```

## 6. Dynamically Column name Change in the P & L chart based on the Fiscal_year Slicer Selection
1. **Create a dynamic table for selection:**

```DAX
P & L Columns = // we are creating a dynamic table
VAR X = ALLNOBLANKROW(fiscal_year[FY_ description]) // we want all the rows from Fy_description
RETURN
UNION(  // also we want to see BM, YOY and YOY %
    ROW("Col Header", "BM"),// col names
    ROW("Col Header", "YOY"),
    ROW("Col Header", "YOY %"),
    X
)
```

2. **Modify measure to reflect selections: Now we will create P & L Final value measure as the previous P & L value measure does'nt show only the selected Fiscal_year data in the P & L Chart**

```DAX
P & L Final value = SWITCH(TRUE(),
    SELECTEDVALUE(fiscal_year[FY_ description]) = MAX('P & L Columns'[Col Header]), [P & L Values], // if selected year from slicer matches CoL header from P & L columns, then show [P & L Values]
    MAX('P & L Columns'[Col Header]) = "LY", [P & L LY],
    MAX('P & L Columns'[Col Header]) = "YOY", [P & L YOY],
    MAX('P & L Columns'[Col Header]) = "YOY %", [P & L YOY %])
```

This ensures dynamic year selection within the P&L Matrix. 🚀

Here's your rewritten version with clear explanations:  

---

## 7. Creating Quarters and YTD/YTG Slicers  

### **Creating the Fiscal Month Number Column**  
1. Go to `dim_date` in Power BI (not Power Query).  
2. Create a new column called `fy_month_num` using the following formula:  

   ```DAX
   fy_month_num = MONTH(DATE(YEAR(dim_date[date]), MONTH(dim_date[date]) + 4, 1))
   ```
   **Explanation:**  
   - We take the **year** from `dim_date[date]`.  
   - We add **4 months** to adjust for AtliQ’s fiscal year, which starts in **September**.  
   - The **DAY is set to 1** to ensure a valid date format.  
   - This shifts September to **FY month 1**, October to **FY month 2**, and so on.  

---

## 8. **Creating a Quarter Column for Slicer**  
1. Create a new column named `Quarters`:  

   ```DAX
   Quarters = "Q" & ROUNDUP(dim_date[fy_month_num] / 3, 0)
   ```
   **Explanation:**  
   - We divide `fy_month_num` by **3** (since a quarter consists of 3 months).  
   - Using `ROUNDUP`, we ensure that:  
     - Months **1, 2, 3** → **Q1**  
     - Months **4, 5, 6** → **Q2**  
     - Months **7, 8, 9** → **Q3**  
     - Months **10, 11, 12** → **Q4**  
   - Adding `"Q"` before the number creates a proper quarter label (e.g., `"Q1"`, `"Q2"`).  

---

## 9. **Creating YTD/YTG Column**  
1. Create a new column named `YTD_YTG`:  

   ```DAX
   YTD_YTG =
   VAR LASTSALESDATE = MAX(fact_sales_monthly[date])
   VAR FYMONTHNUM = MONTH(DATE(YEAR(LASTSALESDATE), MONTH(LASTSALESDATE) + 4, 1))  
   RETURN  
   IF(dim_date[fy_month_num] > FYMONTHNUM, "YTG", "YTD")
   ```
   **Explanation:**  
   - **`LASTSALESDATE`** → Finds the most recent sales date in `fact_sales_monthly`.  
   - **`FYMONTHNUM`** → Converts the last sales date into **fiscal month format** by adding 4 months.  
   - **Condition:**  
     - If `dim_date[fy_month_num]` is **greater** than `FYMONTHNUM` → `"YTG"` (Year-To-Go).  
     - Otherwise → `"YTD"` (Year-To-Date).  
   - This helps track whether a date falls **before or after** the latest recorded sales date.  

---

This ensures that your fiscal year setup aligns correctly with **AtliQ’s September start**, enabling dynamic time-based analysis. 🚀

# Creating a New Visual: **Performance Over Time**  

## **1. Setting Up the Ribbon Chart**  
- From the **Visualizations** pane, select the **Ribbon Chart**.  
- Set the **X-axis** to `dim_date[date]`.  
- To make the month names smaller:  
  - Go to `dim_date` → Select `date` → Change format to `"Mmm"` (e.g., January → Jan).  
- Change the **X-axis type** to **Categorical** to ensure proper alignment.  

---

## **2. Making the Chart Dynamic**  
- To ensure that selections in the **P&L Chart** reflect in the **Performance Over Time Chart**,  
  - Set the **Y-axis** to **P&L Values** and **P&L LY (Last Year)**.  

### **Fixing Cross-Highlighting Issue**  
**Problem:** When selecting a metric (e.g., GS $) in the P&L Chart, the Performance Over Time Chart displayed unrelated results.  
**Solution:**  
- Go to **File → Options → Current File → Report Settings**.  
- Change **Default Visual Settings** from **Cross-Highlighting** to **Cross-Filtering**.  

**Why?**  
- **Cross-Highlighting** only dims unrelated data, keeping everything visible.  
- **Cross-Filtering** removes unrelated data completely, ensuring only relevant results appear.  

---

## **3. Handling Default Display When No Selection is Made**  
**Problem:** When no selection is made in the P&L Chart, what should the Performance Over Time chart display?  
**Solution:** Modify the **P&L Values** formula:  

```DAX
P&L Values =  
VAR x = SWITCH(
    TRUE(),
    MAX('P&L Rows'[Order]) = 1, [GS $] / 1000000,  
    -----
    -----
    -----
    MAX('P&L Rows'[Order]) = 17, [Net Profit %]
)  
RETURN  
IF(HASONEVALUE('P&L Rows'[Description]), x, [NS $]) // meaning if one value is selected from description, show x else show [NS $]
```

### **How This Works:**  
- If **a specific P&L row is selected**, the corresponding value (`x`) is returned.  
- If **nothing is selected**, the default value `[NS $]` (Net Sales) is displayed.  

---

## **4. Creating a Dynamic Chart Title**  
**Problem:** The chart updates when selecting a metric, but the title remains unchanged.  
**Solution:** Create a measure for a **dynamic title**:  

```DAX
Selected_P&L_Values =  
IF(HASONEVALUE('P&L Rows'[Description]), SELECTEDVALUE('P&L Rows'[Description]), "Net Sales")
```

Now, modify it to include a descriptive title:  

```DAX
Performance_Visual_Title = [Selected_P&L_Values] & " Performance Over Time"
```

### **Applying the Dynamic Title:**  
- Go to the **Performance Over Time Chart**.  
- Select **Title** → Click **fx** → Choose `Performance_Visual_Title`.  
- Now, the title updates dynamically based on the selected metric.  

---

## **5. Renaming Measures for Better Clarity**  
- Rename **P&L Values** → `Selected Year`.  
- Rename **P&L LY** → `Selected Year - 1`.  

---

## **6. Creating Two Matrix Tables**  
1. **Top/Bottom by Net Sales:**  
   - **Rows:** `Market`.  
   - **Values:** `P&L` and `P&L YoY %`.  

2. **Segment & Category Breakdown:**  
   - **Rows:** `Segment` and `Category`.  
   - **Values:** `P&L` and `P&L YoY %`.  

---

### 🎯 **Final Outcome:**  
- **Performance Over Time Chart** dynamically updates based on P&L Chart selections.  
- **Default display** is `[NS $]` when nothing is selected.  
- **Title updates automatically** to match the selected metric.  
- **Two matrix tables** provide insights into market and segment performance.  

This ensures a **seamless, interactive analysis** experience in Power BI! 🚀
