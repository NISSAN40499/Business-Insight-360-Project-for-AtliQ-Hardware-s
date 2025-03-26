# **Creating a Marketing View in Power BI**  

The **Marketing View** is designed similarly to the **Sales View**, but with a focus on **products** rather than customers. This is crucial for **AtliQ's marketing team**, as they need to analyze product performance, ad campaigns, and other relevant marketing metrics.  

---

## **1. Scatter Plot Chart for Marketing View**  
1. **Scatter Plot Chart:** From the **Visualizations** panel, select **Scatter Chart**.  
2. **Configure the chart:**  
   - **Values:** `Segment`, `Category`, `Product`  
   - **X-axis:** `NP%` (Net Profit Percentage)  
   - **Y-axis:** `GM%` (Gross Margin Percentage)  
   - **Legend:** `Division` (to differentiate product categories).  
   - **Size:** `NP%` (Bubble size based on profitability).  
   - **Tooltip:** `GM%` (Gross Margin Percentage).  

   This chart helps visualize how products are performing based on **Net Profit** and **Gross Margin**. A product with a high **NP%** and **GM%** is performing well, while one with low values in both indicates areas for improvement.  

---

## **2. Toggle to Show Alternative Scatter Plot**  
1. **Switching Chart:** Add a **toggle button** to switch between two scatter charts.  
2. **Second Scatter Chart:** When toggled, display the second chart:  
   - **Values:** `Segment`, `Category`, `Product`  
   - **X-axis:** `NS$` (Net Sales)  
   - **Y-axis:** `GM%` (Gross Margin Percentage)  
   - **Legend:** `Division`  
   - **Size:** `NS%` (Bubble size based on Net Sales Percentage).  
   - **Tooltip:** `GM%`  

   This allows comparing **Net Sales** vs **Gross Margin** for products across different segments and categories.  

---

## **3. Matrix Visuals for Further Insights**  
1. **Matrix 1:**  
   - **Rows:** `Segment`, `Category`  
   - **Values:** `NS$`, `GM$`, `GM%`, `NP`, `NP%`  

2. **Matrix 2:**  
   - **Rows:** `Market`, `Region`  
   - **Values:** `NS$`, `GM$`, `GM%`, `NP`, `NP%`  

---

## **4. Stacked Column Chart for COGS vs GM$**  
1. **Switching Chart:** Add another **switching chart** with two options: **COGS vs GM$** and **GM$**, **Profit**, and **OPEX**.  
2. **COGS vs GM$ Option:**  
   - Use a **Stacked Column Chart** to show the distribution.  
   - **X-axis:** `s.description` (concise descriptions from the **P&L Rows** table).  
   - **Y-axis:** `P&L Values 2`  

3. **P&L Values 2:** A new measure for cleaner visualization. We used this instead of `P&L Values` to avoid scaling issues (like billion-dollar margins turning into millions).  

   ```DAX
   P&L_values_2 = VAR x = 
   SWITCH(TRUE(),
   MAX('P&L Rows'[Order])=1,[GS $],
   MAX('P&L Rows'[Order])=2, [Pre_Invoice_deduction %],
   MAX('P&L Rows'[Order])=3,[NIS $],
   MAX('P&L Rows'[Order])=4,[Post_invoice_deduction $],
   MAX('P&L Rows'[Order])=5,[Post_invoice_other_deduction $],
   MAX('P&L Rows'[Order])=6,[Total_post_invoice_deduction $],
   MAX('P&L Rows'[Order])=7,[NS $],
   MAX('P&L Rows'[Order])=8,[Manufacturing_amount $],
   MAX('P&L Rows'[Order])=9,[Freight_Cost $],
   MAX('P&L Rows'[Order])=10,[Other_Cost $],
   MAX('P&L Rows'[Order])=11,[Total_Cogs $],
   MAX('P&L Rows'[Order])=12,[GM $],
   MAX('P&L Rows'[Order])=13,[GM %]*100,
   MAX('P&L Rows'[Order])=14,[GM/Unit],
   MAX('P&L Rows'[Order])=15,[Operational_exp],
   MAX('P&L Rows'[Order])=16,[Net Profit],
   MAX('P&L Rows'[Order])=17,[Net Profit %])
   RETURN
   IF(HASONEVALUE('P&L Rows'[Description]),X,[NS $])
   ```

4. **GM$ and OPEX Chart:** Use a **Waterfall Chart** for GM$, **Profit**, and **OPEX**.  
   - **Category:** `s.description`  
   - **Y-axis:** `P&L Values 2`  
   - **Filter:** Apply filters for GM$, **Profit**, and **OPEX** from the **P&L Values**  

---

## **5. Filters and Slicers for the Marketing View**  
1. **Fiscal Year Slicer:** Allow users to filter by **Fiscal Year**.  
2. **Quarter Slicer:** Allow filtering by **Quarter**.  
3. **YTD/YTG Slicer:** Filter by **Year-to-Date** or **Year-to-Go**.  
4. **Toggle Slicer:** For switching between **COGS vs GM$** or **GM$, Profit, and OPEX** charts.  
5. **Additional Filter Bookmark Slicer:** This slicer includes options for **Customer**, **Region**, and **Segment** to allow deeper segmentation of the data.  

---

By implementing this **Marketing View**, AtliQ's marketing team can effectively track **product performance**, identify **trends in profitability**, and make data-driven decisions.
