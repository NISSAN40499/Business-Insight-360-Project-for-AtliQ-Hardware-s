### **Data Warehousing at AtliQ**

AtliQ's data infrastructure is structured as a **scattered system** consisting of multiple **Database Management Systems (DBMS)**. These systems are managed by different teams across the organization. a sample example of AtliQ's Databases is:

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

#### AtliQ's Data Infrastructure looks like this: https://drive.google.com/file/d/1NH0heKcGcFz0fNBF5QeLHnJx5D-lGnQ5/view?usp=sharing
