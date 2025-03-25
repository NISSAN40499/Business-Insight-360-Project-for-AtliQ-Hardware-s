## P & L Calculation Using DAX 

### Explanation for Using MAX in DAX: 

In DAX, we use **`MAX`** (or **`MIN`** if preferred) in situations like this because it's often necessary to ensure that the DAX expression evaluates to a single value. When working with columns that might have multiple related values (like `post_invoice_deduction[discount_pct]`), using **`MAX`** ensures that you get the highest (or lowest, if you used `MIN`) value from the related rows for the calculation.

Without it, DAX might return multiple values for a single calculation, causing errors or inconsistent results. By using **`MAX`** (or **`MIN`**), we make sure that only one value is used for the calculation.

### Creating Remaining Visuals for P&L in DAX

#### **1. Create `post_invoice_deduction` Column**:
```DAX
post_invoice_deduction_col = 
VAR res = CALCULATE(MAX(post_invoice_deduction[discount_pct]), RELATEDTABLE(post_invoice_deduction))
RETURN res * fact_actual_estimates[net_invoice_sales_amt]
```

Explanation:
- **MAX** is used here because we want to ensure we're getting a single value from the related rows of the `post_invoice_deduction` table.
- This value is then multiplied by `fact_actual_estimates[net_invoice_sales_amt]` to calculate the post-invoice deduction amount.

#### **2. Create `post_invoice_other_deduction` Column**:
```DAX
post_invoice_other_deduction_col = 
VAR res = CALCULATE(MAX(post_invoice_deduction[other_discount_pct]), RELATEDTABLE(post_invoice_deduction))
RETURN res * fact_actual_estimates[net_invoice_sales_amt]
```

This is similar to the previous column, but here, we're using a different discount percentage (`other_discount_pct`).

#### **3. Change Type for Both Columns to Currency**:
Once the columns are created, change their data type to **Currency** in the **Model** view to represent the values as monetary amounts.

---

#### **4. Create `net_sales` Column**:
Right-click on a column and select "New Column," then paste the following code:
```DAX
net_sales_amount = net_invoice_sales_amount - post_invoice_deduction_amt - post_invoice_other_amt
```

Here, we're calculating the **net sales amount** by subtracting the deductions from the invoice amount.

---

#### **5. Create `cost_of_goods_sold` Column**:
```DAX
manufacturing_cost = 
VAR res = CALCULATE(MAX(manufacturing_cost[manufacturing_cost]), RELATEDTABLE(manufacturing_cost))
RETURN res * fact_actual_estimates[qty]
```

Explanation:
- We're calculating the **manufacturing cost** for each item by using the related **`manufacturing_cost`** value and multiplying it by the quantity (`qty`) from the `fact_actual_estimates` table.

---

#### **6. Create `freight_cost` Column**:
```DAX
freight_cost = 
VAR res = CALCULATE(MAX(freight_cost[freight_cost]), RELATEDTABLE(freight_cost))
RETURN res * fact_actual_estimates[net_sales_amount]
```

This is similar to the **manufacturing cost** column but with the **freight cost**.

---

#### **7. Create `other_cost` Column**:
```DAX
other_cost = 
VAR res = CALCULATE(MAX(freight_cost[other_cost_pct]), RELATEDTABLE(freight_cost))
RETURN res * fact_actual_estimates[net_sales_amount]
```

Here, we're calculating **other costs** as a percentage of the net sales amount.

---

#### **8. Create `total_cogs_amount`**:
```DAX
total_cogs_amount = fact_actual_estimates[manufacturing_cost] + fact_actual_estimates[freight_cost] + fact_actual_estimates[other_cost]
```

We now sum up the **manufacturing cost**, **freight cost**, and **other cost** to get the **total cost of goods sold (COGS)**.

---

#### **9. Create `gross_margin_amount`**:
```DAX
gross_margin_amount = fact_actual_estimates[net_sales_amount] - fact_actual_estimates[total_cogs_amount]
```

Finally, we calculate the **gross margin** by subtracting **total COGS** from the **net sales amount**.

---

These formulas together will help build out your P&L statement in Power BI using DAX. Let me know if you'd like further clarification or adjustments!
