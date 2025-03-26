# Supply Chain View

## Overview
To build the **Supply Chain View**, we need to calculate custom measures and set up the required visualizations. Below is a structured breakdown of the process.

---

## **1. Understanding the Data**

### **Example Dataset (Demo)**

| Month | Product   | Forecast Qty | Actuals | Net Error | Abs Net Error | Net Error % | Abs Net Error % | Forecast Accuracy % |
|-------|----------|-------------|---------|-----------|---------------|-------------|-----------------|----------------------|
| Oct   | Keyboard | 80          | 50      | 30        | 30            | 10.6%       | 24.5%           | 89.4%                |
| Nov   | Keyboard | 75          | 85      | -10       | 10            | -           | -               | -                    |
| Dec   | Keyboard | 90          | 84      | 6         | 6             | -           | -               | -                    |

here is a better example in a pdf watch this: https://drive.google.com/file/d/1t6feUCbBFuR6RSMkZ_xy4t6EIGGbjeAQ/view?usp=sharing


This table represents **forecasted vs actual sales** data along with error calculations. Our goal is to calculate these values dynamically in Power BI.

---

## **2. Creating Custom Measures**

### **A. Sales Quantity Measure**
The `Fact_Actual_Estimates` table contains both actual and estimated sales. We need a new measure to extract only actual sales:

```DAX
Sales Qty =
CALCULATE(
    [Quantity],
    Fact_Actual_Estimates[date] <= MAX(Last_sales_Month[Last_sales_Month])
)
```

---

### **B. Forecast Quantity Measure**
This measure ensures we consider forecasted sales **only up to the last sales month**:

```DAX
Forecast_quantity =
VAR last_sales_date = MAX(Last_sales_Month[Last_sales_Month])
RETURN
CALCULATE(
    SUM(fact_forecast_monthly[forecast_quantity]),
    fact_forecast_monthly[date] <= last_sales_date
)
```

---

### **C. Net Error Calculation**

```DAX
Net Error = [Forecast_quantity] - [Sales Qty]
```

```DAX
Net Error % = DIVIDE([Net Error], [Forecast_quantity], 0)
```

---

### **D. Absolute Error Calculation**
Since simply taking `ABS([Net Error])` can break granularity, we use **SUMX** over distinct products and dates:

```DAX
ABS error =
SUMX(
    DISTINCT(dim_date[date]),
    SUMX(DISTINCT(dim_product[product_code]), ABS([Net Error]))
)
```

```DAX
ABS Error % = DIVIDE([ABS error], [Forecast_quantity], 0)
```

```DAX
ABS ERROR % LY = CALCULATE([ABS Error %], SAMEPERIODLASTYEAR(dim_date[date]))
```

---

### **E. Forecast Accuracy Calculation**

```DAX
Forecast Accuracy % =
IF([ABS Error %] <> BLANK(), 1 - [ABS Error %], BLANK())
```

```DAX
Forecast Accuracy % LY = CALCULATE([Forecast Accuracy %], SAMEPERIODLASTYEAR(dim_date[date]))
```

---

## **3. Visualizations**

### **A. Line and Clustered Column Chart**
- **X-axis** → `Month` (Categorical)
- **Column Y-axis** → `Net Error`
- **Line Y-axis** → `Forecast Accuracy %`, `Forecast Accuracy % LY`

---

### **B. Matrix 1: Customer-Level Analysis**
**Rows:** `Customer`

**Values:**
- Forecast Accuracy %
- Forecast Accuracy % LY
- Net Error
- Net Error %
- Absolute Error %
- **Risk (New Measure)**

```DAX
RISK =
IF([Net Error] > 0, "Excess",
IF([Net Error] < 0, "Stockout", BLANK()))
```

---

### **C. Matrix 2: Segment-Level Analysis**
**Rows:** `Segment`, `Category`, `Market`

**Values:**
- Forecast Accuracy %
- Forecast Accuracy % LY
- Absolute Error %
- Net Error
- Net Error %
- Risk

---

## **4. Additional Filters & Slicers**
- Fiscal Year
- Quarter
- YTD/YTG
- **Bookmark Slicer:** Customer, Market, Segment

---

## **5. Summary**
This implementation enables the product owner **Nick Puri** to visualize key supply chain metrics effectively. By combining **error calculations, accuracy metrics, and risk analysis**, we provide a comprehensive **forecast vs actual** comparison in Power BI.

**Next Steps:**
- Validate the measures with real data
- Optimize DAX performance
- Test slicers and bookmarks

---

### **End of Documentation**

