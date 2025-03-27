# **Adding a Dynamic Last Refresh Date & Time in Power BI**  

One of our stakeholders requested a feature to **dynamically display the last refresh date and time** on the **Power BI report**. This ensures users always know when the data was last updated. Here’s how we implemented it:  

---

## **Step 1: Generating the Last Refresh Date & Time**  

To create a **dynamic refresh timestamp**, we used **Power Query** to capture the **local system time** at the moment of the data refresh.  

1. **Open Power Query Editor**  
2. Click **New Source** → **Enter Data**  
3. In the new table, enter this formula:  

   ```powerquery
   = DateTime.LocalNow()
   ```

   This function returns the **current date and time** of the system when the dataset is refreshed.  

4. Convert it into a **table** and rename the column to **DateTime**  

---

## **Step 2: Extracting Date and Time Separately**  

To provide flexibility in reporting, we split the **DateTime** column into separate **Date** and **Time** fields:  

1. **Duplicate the `DateTime` column** → Rename it to **Date**  
2. **Duplicate the `Date` column** → Rename it to **Time**  
3. Extract only the time from the **Time** column using this transformation:  

   ```powerquery
   = Table.TransformColumns(#"Renamed Columns2", {{"Time", DateTime.Time, type time}})
   ```

   - This ensures the **Time column** contains only the time portion of the `DateTime` value.  

---

## **Step 3: Displaying the Last Refresh Timestamp in the Report**  

Now, we bring this into the Power BI report for visibility:  

1. **Create a Card Visual**  
   - Add the **DateTime** column to the **Card Visual**  
   - This will display the last refresh timestamp dynamically  

2. **Add a Text Box**  
   - Insert a **Text Box** on the report  
   - Type **"Last Update:"** next to the Card Visual  

This setup ensures the last refresh time updates **automatically** every time the dataset is refreshed. 🚀
