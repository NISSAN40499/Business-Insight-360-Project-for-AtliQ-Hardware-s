# **Implementing Dynamic Targets & Switching Between Target and Last Year (LY) in Power BI**  

## **Feature Request & Business Need**  

Our **Product Owner, Nick Puri**, requested a new feature in our **Power BI report**: the ability to **see targets alongside Last Year (LY) performance**. Additionally, **Thor Hathodwala, the Supply Chain Master**, wanted a way to **switch between LY and Target** dynamically.  

To support this, Thor provided an **Excel file** containing **market-specific targets** for **Net Sales (NS)**, **Gross Margin (GM)**, and **Net Profit (NP)**.  

---

## **Step 1: Loading & Connecting Target Data**  

1. **Load the Excel file**:  
   - Go to **Get Data → Excel → Select `NsGmTarget.xlsx` → Load**  

2. **Integrate it into the data model**:  
   - Connect `dim_market[market]` to `NsGmTarget[market]`  
   - Connect `dim_date[date]` to `NsGmTarget[month]`  

### **Why Do We Need Target Data?**  

Having only **LY comparisons** can be misleading. Example:  

- **2021 Net Sales:** **$823M**  
- **2022 Net Sales:** **$3.74B**  
- **Growth:** **353%** 🚀  

At first glance, this seems like massive success, but **why did it happen?** Did a new technology disrupt the market? Are competitors also growing?  

To evaluate **real performance**, we must compare actuals against **market-specific targets** rather than just LY figures.  

---

## **Step 2: Creating Target Measures**  

Now, we need to create measures for **Net Sales (NS), Gross Margin (GM), and Net Profit (NP) targets**:  

```DAX
GM $ Target = SUM(NsGmTarget[gm_target])

NP Target = SUM(NsGmTarget[np_target])

NS $ Target = 
VAR tgt = SUM(NsGmTarget[ns_target])
RETURN IF([Customer / Product Filter Check], BLANK(), tgt)
```

### **Limitations of Target Data**  

1. **Target data is available only for 2022** → No values for previous years  
2. **Targets are market-specific** → No customer-level granularity  
3. **Cross-filtering by customer/product still displays market-level targets**  

To **fix this**, we need additional logic to handle cross-filtering.  

---

## **Step 3: Handling Cross-Filtering Issues**  

We only need to fix cross-filtering issues for **NS $ Target** because:  

- **GM % and NP % use ratios**, so customer filtering does not affect them.  

### **Creating a Cross-Filter Check Measure**  

```DAX
Customer / Product Filter Check = 
ISCROSSFILTERED(dim_product[product]) || ISFILTERED(dim_customer[customer])
```

- `ISFILTERED(dim_customer[customer])` → Returns **TRUE** if any customer filter is applied  
- `ISCROSSFILTERED(dim_product[product])` → Returns **TRUE** if **direct or indirect** filtering occurs  

### **Updating NS $ Target to Handle Filters**  

```DAX
NS $ Target = 
VAR tgt = SUM(NsGmTarget[ns_target])
RETURN IF([Customer / Product Filter Check], BLANK(), tgt)
```

---

## **Step 4: Creating a Dynamic Slicer for LY vs Target**  

To switch between **LY and Target**, we create a **static table**:  

1. **Go to Home → Enter Data**  
2. **Create a table named `BM`** with the following values:  

| Benchmark | ID |
|-----------|----|
| vs LY     | 1  |
| vs Target | 2  |

3. **Create a slicer using the `BM` table**  

---

## **Step 5: Creating Switchable Measures**  

Now, we create measures to dynamically switch between **LY and Target** based on slicer selection.  

### **Net Sales BM (LY vs Target)**  

```DAX
NS BM $ = 
SWITCH(
    TRUE(),
    SELECTEDVALUE('BM'[ID])=1, [NS $ LY],
    SELECTEDVALUE('BM'[ID])=2, [NS $ Target],
    NOT ISFILTERED(BM[ID]), [NS $ LY]
)
```

### **Gross Margin % BM (LY vs Target)**  

```DAX
GM % BM = 
SWITCH(
    TRUE(),
    SELECTEDVALUE('BM'[ID])=1, [GM % LY],
    SELECTEDVALUE('BM'[ID])=2, [GM % Target],
    NOT ISFILTERED(BM[ID]), [GM % LY]
)
```

### **Net Profit % BM (LY vs Target)**  

```DAX
NP % BM = 
SWITCH(
    TRUE(),
    SELECTEDVALUE('BM'[ID])=1, [Net Profit % LY],
    SELECTEDVALUE('BM'[ID])=2, [NP % Target],
    NOT ISFILTERED(BM[ID]), [Net Profit % LY]
)
```

These measures ensure:  

- If `vs LY` is selected → Show **LY values**  
- If `vs Target` is selected → Show **Target values**  
- If **nothing is selected** → Default to LY  

---

## **Step 6: Handling Blank Values Gracefully**  

Blank values might **confuse users**, so we create a **message measure**:  

```DAX
BM Message = 
IF(
    [NS BM $] = BLANK() || [GM % BM] = BLANK() || [NP % BM] = BLANK(), 
    "BM Target(s) is not available for the selected filters", 
    ""
)
```

Now, if any target is missing, the report will **display a message** instead of just a blank card.

---

## **Step 7: Updating the P&L Chart for BM Selection**  

### **Creating a New P&L Target Measure**  

```DAX
P & L Target = 
VAR RES = 
    SWITCH(
        TRUE(),
        MAX('P&L Rows'[Order])=7, [NS $ Target]/1000000,
        MAX('P&L Rows'[Order])=12, [GM $ Target]/1000000,
        MAX('P&L Rows'[Order])=13, [GM % Target]*100,
        MAX('P&L Rows'[Order])=17, [NP % Target]*100
    )
RETURN
    IF(HASONEVALUE('P&L Rows'[Description]), RES, [NS $ Target]/1000000)
```

### **Creating a P&L BM Measure (LY vs Target Switch)**  

```DAX
P & L BM = 
SWITCH(
    TRUE(),
    SELECTEDVALUE('BM'[ID])=1, [P & L LY],
    SELECTEDVALUE('BM'[ID])=2, [P & L Target]
)
```

### **Updating P&L Final Value Calculation**  

```DAX
P & L Final value = 
SWITCH(
    TRUE(),
    SELECTEDVALUE(fiscal_year[FY_ description]) = MAX('P & L Columns'[Col Header]), [P & L Values],
    MAX('P & L Columns'[Col Header]) = "BM", [P & L BM],
    MAX('P & L Columns'[Col Header]) = "YOY", [P & L YOY],
    MAX('P & L Columns'[Col Header]) = "YOY %", [P & L YOY %]
)
```

### **Updating P&L Columns Table**  

```DAX
P & L Columns = 
VAR X = ALLNOBLANKROW(fiscal_year[FY_ description])
RETURN
    UNION( 
        ROW("Col Header", "BM"),
        ROW("Col Header", "YOY"),
        ROW("Col Header", "YOY %"),
        X
    )
```

---

## **Final Implementation**  

1. **Replace KPI cards** with `NS BM $`, `GM % BM`, and `NP % BM`  
2. **Update P&L Chart** to use `P & L BM`  
3. **Ensure cross-filters behave correctly**  
4. **Add BM Message to handle blank cases gracefully**  

Everything now **switches dynamically** between **LY and Target** based on user selection. ✅

---

## Now, for the **Performance Chart**, which currently compares **this year vs last year (LY)**, we need to incorporate **P & L BM** as a replacement for LY when the benchmark slicer is switched to **vs Target**. 

To do this, we modify the **values used in the Performance Chart** to reference **P & L BM** instead of the static LY values. This ensures that when users toggle between **vs LY** and **vs Target**, the chart dynamically updates.

### Steps:
1. **Update the measure used in the Performance Chart:**
   - Previously, the chart was using **P & L LY**.
   - Now, replace it with **P & L BM**.
  
2. **Modify the values to use BM-based calculations**:
   - Instead of directly referencing LY values, we now use **P & L BM**.
   - This ensures consistency across all visuals when switching between LY and Target.

3. **Verify that the slicer affects the Performance Chart correctly**:
   - When **vs LY** is selected → The chart should display **this year vs LY**.
   - When **vs Target** is selected → The chart should display **this year vs Target**.
  
With these changes, the **Performance Chart now fully adapts** to the benchmark selection, ensuring users get a seamless experience when analyzing either LY trends or progress toward targets.

---

Now, all visuals in the report—including KPIs, P&L tables, and the Performance Chart—respond dynamically to the **vs LY | vs Target** slicer.
