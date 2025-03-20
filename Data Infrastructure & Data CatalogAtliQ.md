### **Data Warehousing at AtliQ**

AtliQ's data infrastructure is structured as a **scattered system** consisting of multiple **Database Management Systems (DBMS)**. These systems are managed by different teams across the organization:

- **Software Engineers** manage the following:
  - Sales Software
  - CRM Software
  - Various survey data stored in **CSV files** in the cloud (from platforms like SurveyMonkey).

- **Data Engineers** are responsible for the **ETL (Extract, Transform, Load)** processes, which include:
  - Extracting data from the different sources.
  - Transforming the data to ensure that it fits specific formats (e.g., converting currency types, date formats, etc.).
  - Storing the transformed data in a **Data Warehouse** (which could be on platforms such as **MySQL**, **Snowflake**, etc.).

- **Data Analysts (DAs)** handle the **data analysis** portion. Our role involves:
  - Creating **reports** using the data stored in the Data Warehouse.
  - Using **Business Intelligence (BI) tools** like Power BI to create **dashboards** and visualizations that provide actionable insights for business decision-making.

#### **Why Use a Data Warehouse?**

As **Data Analysts**, we cannot directly perform analysis on data stored in **OLTP (Online Transaction Processing) systems**, which handle real-time data for customers. Performing analysis directly on these systems could **impact their performance**, potentially slowing down the service for end users. 

Instead, we use the **ETL process** to move data from the OLTP systems into a **Data Warehouse**, where it is optimized for analysis. This allows us to run **complex queries** without affecting the performance of customer-facing systems.

#### **Data Preparation in the Data Warehouse**

To perform effective data analysis, we may need to:
- **Clean the data**: Remove duplicates, errors, and blanks to ensure high-quality data.
- **Format the data**: This includes tasks like adjusting currency types, standardizing date formats, and creating **date tables** for time-based analysis.
- **Transform the data**: This may involve changing data structures or formats to make it easier to work with and analyze.

By having a dedicated **Data Warehouse**, AtliQ can handle large volumes of data from multiple sources while maintaining system performance and ensuring that the data is clean and ready for analysis.


### **AtliQ’s Data Catalog**

As part of the project’s data analysis process, **Tony Sharma (DA Lead)** provided a detailed **Data Catalog** to ensure we are working with the correct databases and datasets for the project. This catalog was delivered in the form of an Excel file, which includes:

- **DBMS Server Names**: The names of the database servers where the data is stored.
- **Table Names**: The specific tables in each DBMS that contain relevant data.
- **Table Descriptions**: Brief descriptions of what each table contains.
- **Contact Information for Data Engineering Support**: In case any issues arise while working with the databases.

Based on the needs of the **Business Insight 360** project, we have selected the following two data servers and datasets:

- **gdb041**
- **gdb056**

The next step in the process is for the **Data Engineering team** to grant us access to these databases. Once access is granted, we will proceed to **import the data into MySQL**, where we will set up the **Data Warehouse** for further analysis and reporting.

### AtliQ's Data infrastructure and Data Catelog PDF: https://drive.google.com/file/d/1-U9d7SHovw6CSTUdNe_20UQibj-j6-NP/view?usp=sharing
