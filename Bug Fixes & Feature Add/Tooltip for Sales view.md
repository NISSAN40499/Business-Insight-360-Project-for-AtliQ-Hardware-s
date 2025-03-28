## **Creating a Tooltip for Sales View (Customer NS$, GM, GM%) Chart**  

The **Sales View Customer NS$ GM GM% Chart** needs a **tooltip** to provide additional insights when hovering over data points. This was requested by **Director Bruce Hariwali**.  

---

## **Step 1: Create the Tooltip Page**  
1. **Create a New Page:**  
   - Click on the **"+" icon** to add a new page.  
   - Open **Page Information** and rename it to **Sales Trend**.  

2. **Set the Page as a Tooltip:**  
   - Go to **Canvas Settings**.  
   - Set **Page Size** to **Tooltip**.  
   - Set **Page View** to **Fit to Page**.  
   - Set **Vertical Alignment** to **Middle**.  

---

## **Step 2: Create the Line Chart for the Tooltip**  
1. Add a **Line Chart** from the **Visualizations Pane**.  
2. Configure the fields:  
   - **X-Axis:** **Month**  
   - **Y-Axis:** **Net Sales (NS$)**  
   - **Secondary Y-Axis:** **Gross Margin % (GM%)**  
3. Adjust formatting as needed to ensure clarity.  

---

## **Step 3: Link the Tooltip to Sales View**  
1. Go to the **Sales View Page**.  
2. Select the **NS$ GM GM% Chart**.  
3. Open the **Format Pane** → Go to **General** → **Tooltip**.  
4. Change **Tooltip Type** to **Report Page**.  
5. Set **Page Tooltip** to **Sales Trend**.  

---

### **Final Result**  
Now, when users hover over a data point in the **Sales View Customer NS$ GM GM% Chart**, a tooltip will appear showing the **trend of NS$ and GM% over time**. 🎯
