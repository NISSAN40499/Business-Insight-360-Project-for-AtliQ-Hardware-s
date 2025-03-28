## **Creating a Toggle Button to Switch Between NS$ vs GM$ and NP$ vs GM$**  

We are adding a **toggle button** to seamlessly switch between **Net Sales (NS$) vs GM$** and **Net Profit (NP$) vs GM$**. This will be achieved using **Bookmarks, a Bookmark Navigator button, and selection pane visibility control**.  

---

## **1. Create the Toggle Button Using Bookmarks**  

### **Step 1: Add Two Bookmarks**
1. Open the **View** tab and select **Bookmarks Pane**.  
2. Create a bookmark **Toggle On** (for NS$ vs GM$).  
3. Create another bookmark **Toggle Off** (for NP$ vs GM$).  
4. Group these bookmarks:  
   - Click **Toggle On**, then the **three dots (•••) → Group**.  
   - Rename the group to **Toggle Group**.  
   - Inside the group, rename **Toggle On** to **⚪** (Unicode white circle).  

---

### **Step 2: Insert and Configure the Toggle Button**  
1. Go to the **Insert** tab → Select **Button** → **Navigator** → **Bookmark Navigator**.  
2. This creates a **toggle button** for switching between the two states.  
3. Configure the bookmark navigator:  
   - In the **Bookmarks Pane**, select **Toggle Group**.  
   - Select the **toggle button** → Go to the **Format Pane**.  
   - Under **Bookmarks**, set **Allow Deselection = ON**.  
   - Set **Launch on Deselection** to **Toggle Off**.  

---

## **2. Styling the Toggle Button**  
Now, let's refine the button's appearance.  

- **Shape:** Rounded Rectangle  
- **Rounded Corners:** 40%  
- **Text:**  
  - Font: **Segoe UI, 14 px**  
  - Alignment: **Left (Default), Middle (Vertical)**  
  - Padding: **0 (Everywhere)**  

### **Colors Based on State**
| State       | Fill Color  | Border  | Text Alignment |
|------------|------------|---------|--------------|
| Default    | **#118DFF (Blue)** | **Black, Width 4** | **Left** |
| Hover      | **Same as Default** | **Same** | **Same** |
| Pressed    | **#D64550 (Red)** | **Same** | **Same** |
| Selected   | **#D64550 (Red)** | **Same** | **Right** |

---

## **3. Setting Up the Visuals for Switching**  

### **Step 1: Duplicate and Modify the Chart**
1. Select the **NS$ vs GM$ chart** and **duplicate** it.  
2. Modify the duplicate to show **NP$ vs GM$**.  

### **Step 2: Control Chart Visibility Using Bookmarks**  
1. Open the **Selection Pane**.  
2. For the **Toggle On** bookmark:  
   - Select the toggle button.  
   - **Hide NP$ vs GM$** chart.  
   - **Show NS$ vs GM$** chart.  
   - **Update** the **Toggle On** bookmark.  
3. For the **Toggle Off** bookmark:  
   - **Hide NS$ vs GM$** chart.  
   - **Show NP$ vs GM$** chart.  
   - **Update** the **Toggle Off** bookmark.  

---

## **Final Result**
With this setup, users can **click the toggle button to seamlessly switch** between NS$ vs GM$ and NP$ vs GM$. 
Here is a reference video to create a toggle button:https://youtu.be/5QMpc5fUV2I?si=DG-FMOvdL2ncUdFL
