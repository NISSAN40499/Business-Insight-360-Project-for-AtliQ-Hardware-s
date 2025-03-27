# **Automatic Data Refresh in Power BI**  

After publishing our **Power BI report** to **app.powerBI.com** under the **Data Analytics workspace**, we encountered issues while setting up **scheduled refresh**. Below is the structured approach we took to resolve these issues and ensure automatic data refresh.  

---

## **Step 1: Setting Up a Personal Gateway for Scheduled Refresh**  
Since **Power BI Online** requires a **personal data gateway** for scheduled refresh, we followed these steps:  

1. Navigate to **app.powerBI.com** → Locate the published dataset  
2. Click on the **three dots (⋮)** next to the dataset → Select **Settings**  
3. Under **Gateway and Cloud Connections**, we saw that a **gateway** was needed  
4. Clicked on the **Download** button (top-right corner) → **Data Gateway**  
5. The download page opened → Selected **Standard Mode** to download  
6. Installed the downloaded file (`gatewayinstall.exe`)  
7. Signed in using our **Microsoft account**  
8. Registered a new gateway:  
   - Named it **Nissan’s Gateway**  
   - Set up a **password** and configured it  

9. After installation, we returned to **Power BI Online**:  
   - Opened **Settings** → **Gateway and Cloud Connections**  
   - Found that the status was incorrect  
   - Clicked **Actions → Add Gateway**  
   - Provided **Connection Name, SQL Server username, and password**  
   - Clicked **OK**  

✅ **Repeated this process for all 5 databases**, ensuring all were properly added to the gateway.  
✅ Finally, mapped each dataset to the correct gateway in the **"Map to"** section.  

For a video reference: [YouTube Guide](https://youtu.be/i4OALAQ7qz4?si=Ay2NP8FqIs4q1mSV)  

---

## **Step 2: Moving Local Excel Files to SharePoint for Stability**  
We realized that **locally stored Excel files** (provided by Nick) posed a risk—if moved or deleted, the **entire Power BI dashboard** could break. To solve this:  

### **Moving Excel Files to SharePoint**  
1. Shared the Excel files via **Microsoft Teams**  
2. Created a **new channel** named **"BI 360 Files"**  
3. Navigated to **Files** → **Uploaded all required Excel files**  
4. Copied the **SharePoint URL** (up to the **team chat name**)  

### **Connecting SharePoint Files in Power BI**  
1. **Power Query Editor** → **New Source** → **SharePoint Folder**  
2. Pasted the **copied SharePoint URL** → **Connected**  
3. Retrieved all uploaded **Excel tables**  
4. Renamed this query to **Excel_Data_Source**  

---

## **Step 3: Updating Queries to Use SharePoint Files Instead of Local Files**  

### **Updating Market Share Data Source**  
1. Created a **blank query** → Renamed it **Market_Share_Source**  
2. Copied the **source code** from **Excel_Data_Source**  
3. Selected **Market Share file (Binary)**  
4. Chose the **Market Share table** (not Sheet1)  
5. Updated the **Market Share table source** from **local file** to **Market_Share_Source**  

### **Updating Other Data Sources**  
Similarly, updated:  
- **Target File:** Renamed as **Target_Source** → Updated the Target Table source  
- **Operational Expense File:** Renamed as **Operational_Source** → Updated the Operational Expense Table source  

---

## **Step 4: Publishing and Ensuring Data Refresh**  
1. **Saved & loaded** the updated Power BI file  
2. **Published** it to **app.powerBI.com**  
3. Verified the **scheduled refresh** was properly configured  

✅ Now, Power BI Online automatically refreshes data from SharePoint, ensuring no file path dependency on local machines. 🚀
