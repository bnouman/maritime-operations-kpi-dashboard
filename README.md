# Maritime Operations KPI Dashboard (Power BI)

A simple but complete **Power BI dashboard** to monitor maritime / shipping operations:

- Total shipments & total volume (TEU)
- On-time vs delayed shipments
- On-time rate %
- Performance by **port** and by **month**

Built entirely in **Power BI Service (web)** as a learning / portfolio project.

---

## 📊 Dashboard Overview

The dashboard is designed to answer questions like:

- How many shipments are we handling and what is our total volume (TEU)?
- What percentage of shipments arrive **on time** vs **delayed**?
- Which **ports** are causing more delays?
- How does performance change by **month**?

**Key features:**

- KPI cards for:
  - Total Shipments  
  - Total Volume (TEU)  
  - On-Time Shipments  
  - Delayed Shipments  
  - On-Time Rate % (with color rules)
- Bar charts:
  - Shipments by **Port & Status** (On Time / Delayed)
  - Shipments by **Month & Status**
- Slicer to filter everything by **Port**

---

## 🧾 Data Model

The data comes from a simple **Excel file** with one table: `Shipments`.

**Columns:**

- `ShipmentID` – unique ID per shipment (e.g. SHP001)
- `VesselName` – vessel / ship name
- `Port` – e.g. Hamburg, Rotterdam, Antwerp
- `PlannedArrivalDate` – scheduled arrival date
- `ActualArrivalDate` – actual arrival date
- `VolumeTEU` – shipment volume in TEU
- `Status` – `On Time` or `Delayed`

In a real scenario this could be replaced by a database or API export from a TMS / ERP system.

---

## 🧮 DAX Measures & Calculated Columns

### ✅ Calculated column

Used for grouping by month on the X-axis:

    MonthYear =
        FORMAT(Shipments[PlannedArrivalDate], "MMM yyyy")

### ✅ Measures

Core KPI measures used in the cards and visuals:

    Total Shipments =
        COUNTROWS(Shipments)

    On-Time Shipments =
        CALCULATE(
            COUNTROWS(Shipments),
            Shipments[Status] = "On Time"
        )

    On-Time Rate % =
        DIVIDE(
            [On-Time Shipments],
            [Total Shipments]
        )

    Delayed Shipments =
        [Total Shipments] - [On-Time Shipments]

These measures drive the **KPI cards** and the **On-Time Rate %** logic.

---

## 📈 Visuals in the Report

The dashboard includes:

### KPI Cards

- **Total Shipments** – `Total Shipments`  
- **Total Volume (TEU)** – Sum of `VolumeTEU`  
- **On-Time Shipments** – `On-Time Shipments`  
- **Delayed Shipments** – `Delayed Shipments`  
- **On-Time Rate %** – `On-Time Rate %` (formatted as Percentage)

### Charts

- **Shipments by Port & Status**
  - Visual: Clustered column chart  
  - X-axis: `Port`  
  - Y-axis: Count of `ShipmentID`  
  - Legend: `Status` (On Time / Delayed)

- **Shipments by Month & Status**
  - Visual: Clustered column chart  
  - X-axis: `MonthYear`  
  - Y-axis: Count of `ShipmentID`  
  - Legend: `Status` (On Time / Delayed)

### Filters

- **Port slicer**
  - Field: `Port`  
  - Filters all KPI cards and charts by port.

---

## 🎨 Conditional Formatting (On-Time Rate %)

The **On-Time Rate %** KPI card uses **rules-based conditional formatting** on the font color:

- **Red** if On-Time Rate < 60%
- **Orange** if 60% ≤ On-Time Rate < 80%
- **Green** if On-Time Rate ≥ 80%

This makes it easy to quickly see whether performance is good or bad without reading the exact number.

---

## 🧭 How to Recreate This Project

You can reproduce this dashboard in your own Power BI environment:

1. **Create the Excel file**
   - Name it e.g. `MaritimeOperationsData.xlsx`
   - Add a worksheet called `Shipments`
   - Include the columns listed in the **Data Model** section above.

2. **Load into Power BI**
   - In **Power BI Desktop** or **Power BI Service (web)**:
     - Get data → Excel → select `MaritimeOperationsData.xlsx`
     - Load the `Shipments` sheet.

3. **Create the calculated column**
   - Add the `MonthYear` column in DAX:
     - `MonthYear = FORMAT(Shipments[PlannedArrivalDate], "MMM yyyy")`

4. **Create the measures**
   - Add the DAX measures:
     - `Total Shipments`
     - `On-Time Shipments`
     - `On-Time Rate %`
     - `Delayed Shipments`

5. **Build the visuals**
   - KPI cards for each measure (and Total Volume TEU as a sum of `VolumeTEU`).
   - Clustered column chart:
     - Shipments by `Port` and `Status`.
   - Clustered column chart:
     - Shipments by `MonthYear` and `Status`.
   - Slicer for `Port`.

6. **Apply conditional formatting**
   - On the **On-Time Rate %** card:
     - Use **Rules** on the **Callout value** color:
       - 0 to 0.6 → Red  
       - 0.6 to 0.8 → Orange  
       - 0.8 to 1 → Green  

---

## 🖼 Screenshots

> After uploading your screenshot to this repo, update the file name below.

Example:

![Maritime Operations KPI Dashboard](https://github.com/bnouman/maritime-operations-kpi-dashboard/blob/main/dashboard.png)

---

## 🚀 Future Improvements

Some ideas to extend this project:

- Add a **DelayDays** column:
  - Difference between `ActualArrivalDate` and `PlannedArrivalDate`.
- Calculate **average delay per port**.
- Highlight **worst-performing ports** vs **best-performing ports**.
- Add a **drill-through page** for detailed shipment-level analysis.
- Connect to a **real data source** (SQL, API, CSV exports) instead of sample Excel.

---

## 🧑‍💻 About

This project was built as a practice / portfolio piece to improve skills in:

- Power BI (web)
- Data modeling and DAX
- KPI and operations dashboard design
- Visual communication for logistics / maritime operations

Feel free to fork, open issues, or suggest improvements 🙌
