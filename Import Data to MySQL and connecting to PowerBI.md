### **Importing Data into SQL Workbench**

1. I received two `.sql.gz` files: **gdb041** and **gdb056**.
2. Moved them to the **C:\dump** folder and uncompressed using **WinRAR**.
3. In **MySQL Workbench**, connected to **localhost**.
4. Imported each `.sql` file via **Server > Data Import** by selecting **Import from Self-Contained File** and browsing the files from **C:\dump**.
5. Verified the import by checking the tables in MySQL Workbench.

### **Connecting MySQL to Power BI**  

1. Open **Power BI** and select **Get Data**.  
2. Choose **MySQL Database** as the data source.  
3. Enter **Server** → `localhost` and **Database Name** → `gdb041`. Click **OK**.  
4. If prompted, authenticate:  
   - **Database authentication** → Enter **Username: root** and **Password**.  
   - Choose the level to apply authentication → **localhost-gdb041**. Click **OK**.  
5. The connection to MySQL is now established in Power BI.
6. Do the same for `gdb056` dataset.
