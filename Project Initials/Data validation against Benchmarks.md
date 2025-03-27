## **Data Validation Against Benchmark**  

Ensuring data accuracy after importing it into Power BI is crucial before generating reports. This task was assigned via **Microsoft Teams Taskbot** by **Tony Sharma**, who also provided a **Word file** containing benchmark figures for validation.  

#### **Validation Process**  

1. **Setting Up Validation in Power BI:**  
   - Opened **Power BI**.  
   - Added **Fiscal_Year** as a row from the **Dim_Date** table.  
   - Created a table visualization to cross-check data against the provided benchmarks.
   - Used Data from Fact tables for validation.
    

2. **Benchmark Figures for Validation:**  

   **Total Sales Quantity (M)**  
   - **2018** → **3.45415M**  
   - **2019** → **10.78465M**  
   - **2020** → **20.7734M**  
   - **2021** → **50.16472M**  

   **Total Forecast Quantity (M)**  
   - **2022** → **86.82346M**  

   **Products Sold**  
   - **FY 2022** → **345**  

   **Active Customers**  
   - **FY 2022** → **209**  

   **Active Markets**  
   - **FY 2022** → **27**  

3. **Issue Identified & Resolution:**  
   - A significant **discrepancy** was found in the **Total Forecast Quantity for 2022**.  
   - Contacted the **Data Engineer**, who confirmed that the **product data was recently updated**.  
   - Reached out to **Product Owner Nick Puri**, who revealed that a **new product (AQ Marquee P4)** had recently been launched.  
   - The missing amount (**376K**) matched the discrepancy.  
   - The **Data Engineer updated the database**, and after revalidation, the **figures aligned correctly**.  

With the corrections in place, the dataset was now accurate and ready for reporting.
