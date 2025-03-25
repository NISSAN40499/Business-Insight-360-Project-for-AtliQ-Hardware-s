## Report Optimization in Power BI Using DAX Studio

When working with large Power BI files, performance and file size can become a concern. In this section, we'll explore how to reduce the file size using **DAX Studio** and best practices for column management in Power BI.

#### **Identifying and Reducing File Size**
Before optimization, the Power BI file size was around **122 MB**, but after adding certain calculations, it grew to **263 MB**. We aim to reduce the file size back by following these steps:

1. **Using DAX Studio to Analyze Size:**
   - From Power BI, open the **Navigation Panel** and click on **External Tools**.
   - After installing DAX Studio, click on it to launch it.
   - Once DAX Studio is open, select the specific Power BI file and go to the **Advanced** tab.
   - Click on **View Metrics** to view the **VertiPaq Analyzer**, where you can see the size of various columns in the data model.

2. **Identifying Problematic Columns:**
   - In the VertiPaq Analyzer, look for columns that are marked in **red** as they occupy a lot of space. Some of these columns include:
     - `total_cogs_amount`
     - `gross_margin_amount`
     - `net_sales_amount`
     - `post_invoice_deduction`
     - `gross_sales_amount`
   
   These large columns are likely contributing to the increase in file size.

3. **Removing Redundant Columns in Power Query:**
   - To reduce the file size, we'll remove these columns from Power Query.
   - In Power Query, these columns are cached, taking up unnecessary space in the PBIX file. By removing them from Power Query and creating dynamic measures during visualization instead, we can save space without affecting the results.

4. **Deleting Columns:**
   - Go to the `fact_actual_estimates` table in Power Query and remove the `gross_sales` column.
   - This removal creates a **new step** in the query pipeline (this is Power Query's "data pipeline" feature).
   - Similarly, remove the columns `pre_invoice_discount_pct` and `pre_invoice_discount_amount`.

5. **Dynamic Creation of Measures:**
   - Instead of keeping the `total_cogs_amount` and `gross_margin_amount` columns in Power Query, we can create them dynamically using **measures** when needed in the report visualization.
   
   - Removing these columns will not break the data model because Power BI is **dynamically fetching data** from the source (e.g., a MySQL server). This means the calculations are still based on the actual data without storing them in the report itself.

6. **Optimizing File Size:**
   - After removing the columns, we see that the file size reduces from **263 MB** to **193 MB**.
   - This means we've saved **70 MB** by removing unnecessary columns, making the Power BI file lighter and more efficient.

---

By following this approach, you're not only optimizing the file size but also improving the report performance. The key is to remove heavy, unnecessary columns in Power Query and use dynamic DAX measures for calculations in the report visuals.
