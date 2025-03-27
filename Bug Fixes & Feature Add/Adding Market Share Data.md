### Market Share Data Import & Transformation in Power BI  

We have received a **market share file** in **XLSX format** from our stakeholders, which needs to be incorporated into our **Power BI system**. Below is the step-by-step process for importing, transforming, and integrating this data into our data model.  

---

### **Step 1: Importing the Market Share File**  
1. **Go to Power BI Home** → Click **Get Data**  
2. Select **Excel Workbook** → Choose the received market share file  
3. Click **Open** → Select the required sheet → Click **Load**  

---

### **Step 2: Data Transformation (Unpivoting Manufacturer Columns)**  
The provided dataset has manufacturer names as **column headers**, but we need them in **rows** with their respective sales amounts.  

1. Open **Power Query Editor**  
2. Select all manufacturing company columns (e.g., **atliQ_sales_$** to **others_sales_$**)  
3. Click **Unpivot Columns**  

   ✅ This converts the column headers (manufacturers) into **rows**, with their corresponding **sales_$** values.  

4. Rename:  
   - **Attribute** → `Manufacturer`  
   - **Value** → `Sales_$`  

---

### **Step 3: Cleaning the Manufacturer Column**  
To clean up the **Manufacturer** column:  
1. Select the `Manufacturer` column  
2. Click **Extract** → **Text Before Delimiter**  
3. Set **“_” (underscore)** as the delimiter  
4. Click **Close & Apply**  

✅ This removes any extra text after the underscore in manufacturer names.  

---

### **Step 4: Adding Market Share as a Fact Table**  
Now that the data is transformed, we add **Market Share** as a **fact table** in the Power BI **Data Model**.  

---

### **Step 5: Handling Many-to-Many Relationships**  
While establishing relationships, we found that **sub_zone** appears in multiple tables, causing a **many-to-many relationship issue**. To resolve this:  

1. **Create a Unique Sub-Zone Table**  
   - **Go to Modeling** → Click **New Table**  
   - Enter the following DAX formula:  
     ```DAX
     Subzone = ALLNOBLANKROW(dim_market[sub_zone])
     ```  
   - This creates a **deduplicated** list of sub-zones.  
   - Connect `dim_market[sub_zone]` to the new `Subzone` table and then link it to the Market Share table.  

2. **Create a Unique Category Table** (To resolve another many-to-many issue)  
   - **Go to Modeling** → Click **New Table**  
   - Enter the following DAX formula:  
     ```DAX
     Category = ALLNOBLANKROW(dim_product[category])
     ```  
   - This creates a **unique Category table**.  
   - Establish relationships:  
     - `dim_product[category]` → `Category table`  
     - `Category table` → `Market Share table[category]`  

✅ These steps resolve **many-to-many relationships**, ensuring smooth data connections.  

---

### **Conclusion**  
The Market Share data is now successfully:  
✅ Imported into Power BI  
✅ Unpivoted for proper row structure  
✅ Cleaned & transformed for consistency  
✅ Integrated into the data model with proper relationships  

This ensures **accurate** and **efficient** analysis within Power BI. 🚀
