## **Implementing Target Gap Tolerance for Sales View (Identifying Low GM% Customers)**  

We are adding a feature to **identify customers whose Gross Margin % (GM%) is below the target** using a **What-If parameter**. This allows users to **set a tolerance level dynamically** and filter out underperforming customers.  

---

### **1. Creating the Target Gap Tolerance Parameter**  

#### **Method 1: Using the Built-in What-If Parameter**  
- Click **New Parameter** → Name it **Target Gap Tolerance**  
- **Data Type**: Decimal  
- **Minimum**: 0  
- **Maximum**: 0.2  
- **Increment**: 0.01  
- Click **OK** → This will generate a slicer to adjust the tolerance level dynamically.  

#### **Method 2: Using a DAX Table (Alternative Approach)**  
```DAX
Target Gap Tolerance = GENERATESERIES(0, 0.2, 0.01)
```
This creates a series from **0% to 20%**, increasing by **1%**. If the user sets the tolerance to **10%**, it means customers whose **GM% is 10% below target** will be highlighted.  

---

### **2. Calculating GM% Variance**  
Next, we calculate how much each customer's GM% deviates from the target.  

```DAX
GM % Variance = [GM % BM] - [GM %]
```
This subtracts the **actual GM%** from the **benchmark GM% (Target or LY, based on slicer selection)**.  

---

### **3. Creating a Filter to Identify Low GM% Customers**  
Now, we create a measure that checks whether the **GM% Variance is greater than the selected tolerance level**.  

```DAX
GM % Filter = 
IF([GM % Variance] >= SELECTEDVALUE('Target Gap Tolerance'[Target Gap Tolerance]), 1, 0)
```
- If the **GM% Variance** is **greater than or equal to the selected tolerance**, return **1** (customer underperforming).  
- Otherwise, return **0**.  

---

### **4. Applying the Filter in the Scatter Chart**  
- Go to the **Scatter Chart** in the Sales View.  
- Turn on **Filters**.  
- Add **GM % Filter** to the filter pane.  
- Set **Show items when value = 1** and apply the filter.  

---

### **5. How It Works**  
- Users adjust the **Target Gap Tolerance** slider.  
- The measure checks if the **GM% Variance** exceeds the selected tolerance.  
- Only **underperforming customers** (GM% below target by more than the selected tolerance) appear in the **Scatter Chart**.  

Now, with this feature in place, users can **easily spot customers with below-target GM%** and take action accordingly. 🚀
