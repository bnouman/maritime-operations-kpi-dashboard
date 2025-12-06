# Maritime Operations KPI Dashboard (Power BI)

A simple but complete **Power BI dashboard** to monitor maritime / shipping operations:

- Total shipments & total volume (TEU)
- On-time vs delayed shipments
- On-time rate %
- Performance by **port** and by **month**

Built entirely in **Power BI Service (web)** as a learning / portfolio project.

---

## 📊 Dashboard Overview

The dashboard answers questions like:

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

In a real scenario this could be replaced by a database or API export.

---

## 🧮 DAX Measures & Calculated Columns

### Calculated columns

```DAX
MonthYear =
FORMAT(Shipments[PlannedArrivalDate], "MMM yyyy")

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
