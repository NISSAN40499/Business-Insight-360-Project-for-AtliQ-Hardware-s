### **Creating a Dim_Date Table**  

1. **Open Power Query Editor:**  
   - Click **Transform Data** → **Power Query Editor**.  
   - Select **New Source** → **Blank Query**.  

2. **Generate the Date List using M Language:**  
   - Open **Advanced Editor** and paste the following M-code:  
     ```m
     ={Number.From(#date(2017,9,1))..Number.From(#date(2022,12,31))}
     ```
   - This creates a date list from **September 1, 2017**, to **December 31, 2022**, based on sales data.  

3. **Convert and Rename Columns:**  
   - Convert to **Table** (`To Table` option).  
   - Rename the column to **Date**.  
   - Rename the query to **Dim_Date**.  
   - Change the data type to **Date**.  

4. **Add Month Column:**  
   - Click **Add Column** → **Date** → **Month** → **Start of the Month**.  
   - Rename the new column to **Month**.  

5. **Add Fiscal Year Column:**  
   - Click **Add Column** → **Custom Column**.  
   - Name it **Fiscal_Year**, and use the formula:  
     ```m
     Date.Year(Date.AddMonths([Month], 4))
     ```
   - Since AtliQ’s fiscal year starts in **September**, adding **4 months** adjusts the fiscal year accordingly.  
   - Change the **Fiscal_Year** column type to **Text**.  

The **Dim_Date** table is now ready to be used as a slicer for controlling other tables in the report.
