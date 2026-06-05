# Purchase Order Reports

## Purpose

Purchase Order Reports provide operational, analytical, financial, and management insights related to Purchase Orders.

Uses common functionality from:

```text
report-engine.md
```

---

# KPI Cards

| KPI              | Description               | Formula                       |
| ---------------- | ------------------------- | ----------------------------- |
| Total PO Count   | Total Purchase Orders     | Count(PO)                     |
| Total PO Value   | Total Purchase Amount     | Sum(Grand Total)              |
| Draft POs        | Draft Purchase Orders     | Count(Stage=Draft)            |
| Pending Approval | Waiting For Approval      | Count(Stage=Submitted)        |
| Approved POs     | Approved Purchase Orders  | Count(Stage=Approved)         |
| Ordered POs      | Orders Sent To Vendor     | Count(Stage=Ordered)          |
| Partial Received | Partially Received POs    | Count(Stage=Partial Received) |
| Completed POs    | Fully Completed POs       | Count(Stage=Completed)        |
| Cancelled POs    | Cancelled Purchase Orders | Count(Stage=Cancelled)        |
| Average PO Value | Average Purchase Value    | Total Value / Total POs       |

---

# Dashboard Charts

## PO Trend

Type:

```text
Line Chart
```

Shows:

```text
Daily / Weekly / Monthly PO Creation Trend
```

---

## Purchase Value Trend

Type:

```text
Area Chart
```

Shows:

```text
Purchase Value Over Time
```

---

## Stage Distribution

Type:

```text
Donut Chart
```

Shows:

```text
Draft
Submitted
Approved
Ordered
Partial Received
Completed
Cancelled
```

---

## Vendor Wise Purchase

Type:

```text
Bar Chart
```

Shows:

```text
Top Vendors By Purchase Value
```

---

## Branch Wise Purchase

Type:

```text
Stacked Bar Chart
```

Shows:

```text
Branch Purchase Comparison
```

---

## Warehouse Wise Purchase

Type:

```text
Bar Chart
```

Shows:

```text
Warehouse Purchase Comparison
```

---

# Report Filters

| Filter     | Type         |
| ---------- | ------------ |
| Date Range | Date Range   |
| Vendor     | Lookup       |
| Branch     | Lookup       |
| Warehouse  | Lookup       |
| Product    | Lookup       |
| Stage      | Multi Select |
| Created By | User Lookup  |

---

# Purchase Order Register

## Purpose

Master report for all Purchase Orders.

Columns:

- PO Number

- PO Date

- Vendor

- Branch

- Warehouse

- Stage

- Amount

- Created By

---

# Pending Approval Report

## Purpose

Purchase Orders waiting for approval.

Columns:

- PO Number

- Vendor

- Submitted Date

- Pending Since

- Approver

---

# Ordered PO Report

## Purpose

Purchase Orders already sent to vendors.

Columns:

- PO Number

- Vendor

- Ordered Date

- Expected Delivery Date

- Amount

---

# Partial Received Report

## Purpose

Track partially received orders.

Columns:

- PO Number

- Vendor

- Ordered Quantity

- Received Quantity

- Pending Quantity

---

# Completed PO Report

## Purpose

Fully completed Purchase Orders.

Columns:

- PO Number

- Vendor

- Completion Date

- Amount

---

# Vendor Purchase Analysis

## Purpose

Vendor-wise purchase performance.

Columns:

- Vendor

- PO Count

- Purchase Value

- Average PO Value

---

# Product Purchase Analysis

## Purpose

Product-wise purchase analysis.

Columns:

- Product

- Quantity

- Purchase Amount

- Average Rate

---

# Branch Purchase Analysis

## Purpose

Branch-wise purchase monitoring.

Columns:

- Branch

- PO Count

- Purchase Value

---

# Warehouse Purchase Analysis

## Purpose

Warehouse-wise purchase analysis.

Columns:

- Warehouse

- PO Count

- Purchase Value

---

# Delivery Performance Report

## Purpose

Monitor supplier delivery commitments.

Columns:

- Vendor

- Expected Delivery Date

- Actual Delivery Date

- Delay Days

---

# Aging Reports

## Pending Approval Aging

Buckets:

- 0-1 Days

- 2-5 Days

- 6-10 Days

- 10+ Days

---

## Pending Delivery Aging

Buckets:

- 0-7 Days

- 8-15 Days

- 16-30 Days

- 30+ Days

---

# Pivot Reports

## Purchase Analysis Pivot

Rows:

- Vendor

- Product

- Branch

- Warehouse

Columns:

- Month

- Quarter

- Year

Values:

- PO Count

- Quantity

- Purchase Amount

---

# Drilldown Paths

```text
Vendor
↓
Vendor Purchase Analysis
↓
Purchase Order Register
↓
Purchase Order Details
```

```text
Product
↓
Product Purchase Analysis
↓
Purchase Order Register
↓
Purchase Order Details
```

---

# Report Permissions

| Role               | View |
| ------------------ | ---- |
| Purchase Executive | Yes  |
| Purchase Manager   | Yes  |
| Procurement Head   | Yes  |
| Accounts Executive | Yes  |
| Finance Head       | Yes  |
| Auditor            | Yes  |
| System Admin       | Yes  |
