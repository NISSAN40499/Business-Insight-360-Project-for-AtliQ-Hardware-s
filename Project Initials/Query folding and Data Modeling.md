## **Query Folding and Data Modeling in Power BI**

### **What is Query Folding?**

**Query folding** refers to the process where Power Query pushes transformations back to the data source. This means that rather than performing calculations or transformations within Power BI, the data source itself processes the data, making the load and refresh process faster and more efficient. This is essential because the more transformations are done in Power Query rather than in the data source, the heavier and slower the process becomes.

### **Disabling Unnecessary Columns for Data Model Optimization**

Before diving into the data model, we should reduce the size of our tables by disabling unnecessary columns. This helps streamline the data model, reduce file size, and boost performance.

#### **Step-by-Step Process**:

1. **Fact Sales Monthly Table**:  
   - Go to `Choose Columns` → Uncheck columns like:
     - `Channel`
     - `Platform`
     - `Market`
     - `Product_Name`
     - `Category`
     - `Division`

   Disabling these columns will make the table more focused and efficient.

2. **Fact Forecast Monthly Table**:  
   - Similarly, from the `fact_forecast_monthly` table, we untick:
     - `Division`
     - `Category`
     - `Product`
     - `Market`
     - `Platform`
     - `Customer_Name`
     - `Channel`

   By unchecking these columns, the table becomes lighter, improving performance.

3. **Fact Actual Estimate Table**:  
   - Since this table combines data from `fact_sales_monthly` and `fact_forecast_monthly`, removing the unnecessary columns from both source tables makes `fact_actual_estimates` more efficient too.

By doing this, we ensure that only relevant data remains in the model, making it more responsive and easier to work with.

---

### **Creating the Data Model**

After optimizing our tables, we move to the creation of the **data model**. Properly setting up relationships between tables is essential for efficient querying and reporting.

#### **Star Schema**

For an effective data model, we arrange our **fact tables** in the center of the model and surround them with **dimension tables**. This forms a **star schema**, which is simple and efficient for querying.

- **Fact Tables**: Placed at the center of the model
- **Dimension Tables**: Positioned around the fact tables

This layout ensures that we can easily navigate relationships and create reports based on the dimension attributes.

#### **Snowflake Schema**

Sometimes, we might see that a **dimension table** is linked to another dimension table, creating a **snowflake schema**. While the **star schema** is simpler, the **snowflake schema** may occur when normalization is necessary for the data model.

#### Here is the Data model for my project: https://drive.google.com/file/d/1oVZEgZRukYhRr4fOHl9AdS7WTt4wHUNE/view?usp=sharing
---

### **Handling Many-to-Many Relationships: Fiscal Year Column**

While building relationships between fact and dimension tables, we might run into an issue when connecting the `fiscal_year` column across tables. Power BI will throw an error stating **“Cannot establish many-to-many relationship”** if we try to directly link the `fiscal_year` column from one table to another.

#### **Solution: Creating a Dim Fiscal Year Table**

To address this, we create a separate **`dim_fiscal_year`** table. This table serves as a bridge between the fact and dimension tables, allowing us to filter data correctly.

- **DAX Formula to Create `dim_fiscal_year`:**
  ```DAX
  fiscal_year = ALLNOBLANKROWS(dim_date[fiscal_year])
  ```

Once created, the **`dim_fiscal_year`** table can be used as a **slicer** and can be connected to other fact tables to ensure proper filtering.

- **Why Dimension Tables Are Ideal for Slicers:**
  Dimension tables work well for slicers because they allow us to filter data across multiple tables while maintaining context. The **`dim_fiscal_year`** table acts as a **parent table**, and all other tables (fact and dimension) are **child tables**. This means the child tables always follow the filtering and context set by the parent table, ensuring consistency across the data model.

---

### **Summary**:

1. **Query Folding** helps optimize performance by offloading computations to the data source.
2. Disabling unnecessary columns reduces the size of tables and improves efficiency.
3. The **Star Schema** arranges fact tables in the center with dimension tables around them for a clear and efficient data model.
4. The **Snowflake Schema** arises when dimension tables are connected, offering a normalized structure.
5. Creating a **Dim Fiscal Year** table avoids many-to-many relationship issues and improves filtering in the model.

By following these steps, we can ensure our data model in Power BI is efficient, flexible, and optimized for performance.

---

This should now fit well into your GitHub repo! Would you like me to make any further changes before you upload it?
