# Merging Tables || Best Practices in Power BI

## Creating a Gross Price Column

1. **Go to Transform Data** → Power Query → `fact_actual_estimates` → Add Column → Custom Column
2. **Create a Fiscal Year Column:**
   ```
   fiscal year = Date.Year(Date.AddMonths([date], 4))
   ```
   - Format the column as text.
   - The fiscal year column is created because it is missing in `fact_actual_estimates` but exists in the `gross_price` table.

3. **Join `gross_price_table` to `fact_actual_estimates`**
   - Go to `Merge Queries`
   - Merge `product_code` (fact_actual_estimates) with `product_code` (gross_price_table)
   - Merge `fiscal_year` (fact_actual_estimates) with `fiscal_year` (gross_price_table)
   - Use **Left Outer Join** (default)
   - Expand the merged table and **select only `gross_price` column**
   - Change type to **Fixed Decimal**

4. **Calculate Gross Sales Amount:**
   - Add Column → Custom Column:
   ```
   gross_sales_amt = [gross_price] * [qty]
   ```
   - Change type to **Fixed Decimal**

## Merging `pre_invoice_deduction` Table

1. **Go to `fact_actual_estimates`** → Home → Merge Queries
2. **Merge `customer_code` and `fiscal_year` from both tables**
3. Expand the table and **select only `pre_invoice_deduction_percent` column**
4. Change type to **Percentage**

### Calculate `pre_invoice_deduction_amount`:
```
pre_invoice_deduction_amount = [gross_sales_amt] * [pre_invoice_deduction_pct]
```

### Calculate `net_invoice_sales_amount`:
``` 
net_invoice_sales_amount = [gross_sales_amount] - [pre_invoice_deduction_amount]
```

---
## Best Practices in Power BI

1. **Naming Query Steps Correctly**
   - Properly name each step to improve readability and maintainability.

2. **Grouping Tables**
   - Organize tables into folders for better navigation.

3. **Disable Load for Unnecessary Tables**
   - Reduce file size and improve performance.
   - Example: Disable load for `gross_price_table` and `pre_invoice_deduction_table` as they are merged into `fact_actual_estimates`.

4. **Keep Table Names Minimal**
   - Use short and meaningful names for tables and columns.

---
## Power Query or DAX? When to Use What?

### Why Not Merge Everything in Power Query?
- Using DAX reduces **file size** and improves **performance**.
- DAX loads faster compared to merging everything in Power Query.

### Why Not Use DAX for Everything?
- Using too much DAX can **increase file size** and **slow down performance**.
- Complex calculations can make the model difficult to manage.

### Decision Matrix for Choosing Power Query vs. DAX

| Criteria | Power Query | DAX Measure | DAX Calculated Column |
|----------|------------|-------------|----------------------|
| **Power BI File Size** | Low | Low | high |
| **System Memory Consumption** | Medium | High | Low |
| **Developer Waiting Time** | High | Low | Medium |
| **Skill Level Required** | Medium | High | Low |

#### Key Takeaways:
- Use **DAX Measures** whenever possible to keep file size low and ensure quick development.
- Power Query is easier for **merging and combining tables** but may slow down refresh times.
- Striking a balance between **DAX and Power Query** is essential for optimizing performance.

---

