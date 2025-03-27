## **Landing Estimates Table: Actual + Estimates**  

To address the limitation of missing actual sales data beyond December 2022, we use the **Landing Estimate Method**—a combination of actuals (till the latest available month) and forecast estimates (for future periods).  

Here is a Demo of the concept:
| Month   | Actuals | Forecast |Actual_Estimates |
|---------|---------|---------|------------------|
| Sep-21  | 22      | 39      | 22              |
| Oct-21  | 56      | 61      | 56              |
| Nov-21  | 34      | 54      | 34              |
| Dec-21  | 54      | 20      | 54              |
| Jan-22  | -       | 43      | 43              |
| Feb-22  | -       | 74      | 74              |
| Mar-22  | -       | 63      | 63              |
| Apr-22  | -       | 50      | 50              |
| May-22  | -       | 75      | 75              |
| Jun-22  | -       | 61      | 61              |
| Jul-22  | -       | 17      | 17              |
| Aug-22  | -       | 27      | 27              |

---

### **Creating the Fact_Actual_Estimates Table**  
This table merges actual sales data from `fact_sales_monthly` and forecasted data from `fact_forecast_monthly`.  

#### **Steps to Build the Table:**  

1. **Identify the Last Sales Month**  
   - Open **Power Query** and select `fact_sales_monthly`.  
   - Create a **reference table** (Right-click → Reference).  
   - Rename it to `last_sales_month`.  
   - Modify the **M-language formula** in the formula bar:  
     ```m
     = List.Max(#"gdb041 fact_sales_monthly"[date])
     ```
   - This extracts the latest available month from sales data.  

2. **Create the Remaining Forecast Table**  
   - Select `fact_forecast_monthly` and create a **reference table**.  
   - Rename it to `remaining_forecast`.  
   - Apply a filter to exclude past data and keep only future forecasts.  
   - Modify the **M-language formula**:  
     ```m
     = Table.SelectRows(Source, each [date] > last_sales_month)
     ```
   - This keeps only forecast data beyond the last actual sales month.  

3. **Column Naming Issue & Resolution**  
   - During the **Append Queries** process, we faced an issue where **quantity columns from both tables had different names** (`Sales_Qty` in one table and `Forecast_Qty` in the other).  
   - Since Power BI treats them as separate columns instead of merging them into one, we **renamed both columns to `QTY`** before appending.  
   - Now, all quantity data (both actuals and forecasts) flow into a **single `QTY` column** in the merged table.  

4. **Append the Two Tables**  
   - Navigate to **Append Queries → Append Queries as New**.  
   - Select:  
     - **First Table**: `fact_sales_monthly` (Actuals)  
     - **Second Table**: `remaining_forecast` (Forecasts)  
   - Rename the resulting table as **Fact_Actual_Estimates**.  

Now, **Fact_Actual_Estimates** serves as a single source for reporting, seamlessly blending historical actuals with future estimates without column mismatches. 🚀
