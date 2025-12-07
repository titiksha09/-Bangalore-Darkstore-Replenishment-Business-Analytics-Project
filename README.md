# 🔄 **Process Flow – My End-to-End Workflow**

This section outlines the complete analytical and operational workflow I followed to solve the Bangalore Darkstore Replenishment assignment.
It captures my thinking, decision-making, and technical approach from raw data to dashboards and final operational outputs.

---

## **1️⃣ Data Understanding & Requirement Breakdown**

* Reviewed the problem statement and identified four core deliverables:
  **SKU rationalization**, **daily replenishment planning**, **warehouse picklist generation**, and **inventory health analysis**.
* Mapped dependencies between datasets:
  **Sales → Demand Forecast → Replenishment → Picklist → Inventory Metrics**.
* Noted operational constraints:

  * 5,000-unit max storage per darkstore
  * FIFO picking at warehouse
  * Replenishment must not exceed MWH availability

---

## **2️⃣ Data Extraction & Cleaning**

* Loaded all datasets (Sales, Shelf-wise Inventory, In-Transit).
* Standardized column names, SKU IDs, and location IDs.
* Removed duplicates, fixed inconsistent SKU codes, and cleaned missing values.
* Merged datasets into a **master inventory view** for all 10 locations.

**Output:** Clean structured data ready for analysis.

---

## **3️⃣ SKU Rationalization (ABC + Velocity)**

* Calculated daily & weekly sales velocity for each SKU.
* Performed **ABC classification** based on contribution to total sales.
* Identified:

  * **A-class (fast moving)** → Must stock
  * **B-class (medium moving)** → Stock selectively
  * **C-class (slow moving)** → Remove or deprioritize
* Created a **final Keep List** of SKUs optimized for shelf space and demand.

**Output:** Rationalized SKU assortment.

---

## **4️⃣ Demand Forecasting**

* Derived forecast using a simple yet robust method:

  * **7-day moving average** for stable items
  * Weighted recent demand for fluctuating SKUs
* Ensured forecasts were realistic and avoided overfitting.

**Output:** Forecasted Daily Sales for all SKUs.

---

## **5️⃣ Replenishment Planning**

For each Darkstore × SKU:

```
Replenishment Qty = Forecasted Daily Sales 
                     – (Current Stock + In-Transit Stock)
```

Then applied constraints:

* If the value < 0 → set to 0
* Ensured total inventory after replenishment ≤ **5000 units** per DSK
* Checked MWH availability and reduced quantities where needed

**Output:** Consolidated Replenishment Plan (Darkstore-wise table).

---

## **6️⃣ Mother Warehouse Picklist Generation**

* Filtered MWH stock for only the SKUs required in the replenishment plan.
* Applied **FIFO rule** (pick oldest batch first).
* Sorted picklist by:

  1. **Shelf Location** → Minimizes picker walking path
  2. **Batch ID** → Ensures proper rotation
* Generated final operational-ready picklist including:
  SKU, Quantity, Shelf Location, Batch ID, Destination DSK.

**Output:** Warehouse Picklist (ready for dispatch operations).

---

## **7️⃣ Inventory Health Metrics (OOS & DOI)**

### **Out-of-Stock %**

* Calculated OOS at SKU level per location.
* Segmented by Brand-Class (A/B/C).

### **Days of Inventory (DOI)**

```
DOI = (Current Stock + In-Transit + Replenishment) / Forecasted Daily Sales
```

* Compared **Pre-transfer DOI vs Post-transfer DOI**.
* Identified critically low-stock stores.

**Output:** Dashboard visuals for network-wide inventory health.

---

## **8️⃣ Insights & Process Optimization**

* Identified SKUs causing repeated OOS issues.
* Suggested automating safety stock computation using demand variability.
* Proposed replenishment automation with Python + scheduled triggers.
* Highlighted need for better real-time inventory visibility.

---

## **9️⃣ Final Deliverables**

* ✔ SKU Keep List
* ✔ Daily Replenishment Plan
* ✔ MWH FIFO Picklist
* ✔ Inventory Health Reports (OOS & DOI)
* ✔ Notebook documenting full methodology

