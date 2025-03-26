# **Creating a Sales View in Power BI**  

The product owner requested a **scatter chart** to analyze **Sales (NS$) and Gross Margin (GM$) distribution**. This will help identify customer trends and optimize sales strategies.  

---

## **1. Creating the Scatter Chart**  
1. Select **Scatter Chart** from the **Visualizations** panel.  
2. Configure the values:  
   - **X-axis:** `NS$` (Net Sales).  
   - **Y-axis:** `GM$` (Gross Margin).  
   - **Legend:** `Region`.  
   - **Size:** `NS$` (to represent sales volume).  
   - **Tooltip:** `GM%` (Gross Margin Percentage).  
3. **Purpose of the Chart:**  
   - Identifies **customers with high NS$ but low GM$**, indicating a need to **increase margins**.  
   - Highlights **customers with both low NS$ and GM$**, requiring sales team intervention.  
   - The **goal** is to shift all customers towards the **high NS$ and high GM$** area.  

---

## **2. Additional Sales Performance Visuals**  

### **Customer Sales & Margin Metrics (Matrix 1)**  
- **Rows:** `Customer`.  
- **Values:** `NS$`, `GM$`, `GM%`.  

### **Segment & Category Sales Breakdown (Matrix 2)**  
- **Rows:** `Segment`, `Category`.  
- **Values:** `NS$`, `NS$ LY`, `NS YoY%`, `GM$`, `GM%`.  

---

## **3. Donut Charts for P&L Breakdown**  

### **First Donut Chart:**  
- **Legend:** `Description`.  
- **Values:** `P&L Values`.  
- **Filter:** `Pre-Invoice Deduction`, `Total Post-Invoice Deduction`, `Net Sales`.  

### **Second Donut Chart:**  
- **Legend:** `Description`.  
- **Values:** `P&L Values`.  
- **Filter:** `Total COGS`, `Gross Margin`.  

---

## **4. Slicers for Interactive Filtering**  
- **Fiscal Year**  
- **Quarter**  
- **YTD/YTG**  
- **More Filters (Bookmark Slicer):**  
  - `Customer`  
  - `Region`  
  - `Segment`  

---

### 🎯 **Final Outcome:**  
- **Scatter chart** for sales and margin comparison across customers.  
- **Matrix tables** for detailed insights at customer and category levels.  
- **Donut charts** for P&L breakdown.  
- **Multiple slicers** for flexible filtering and deep dives into data.  

This setup provides a **clear, data-driven approach** to optimizing sales performance! 🚀
