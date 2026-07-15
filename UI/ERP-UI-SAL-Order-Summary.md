# HappyERP Sales Order Summary Report UI/UX Documentation

This document gives UI details of the following report.


---

# Pages

| S. No. | Name                       | Type | Purpose                                           |
| :----- | :------------------------- | :--- | :------------------------------------------------ |
| 1.     | Sales Order Summary Report | Page | View line-wise sales order data with filters      |

---

# 1. Sales Order Summary Report Page UI

Line-wise report showing all sales order lines with product, quantity, rate, and value. Each row represents one line item from a sales order.

## 1.1. KPI Cards

Summary cards showing aggregated numbers for the current filtered view. All values update dynamically when filters are applied.

| S. No. | KPI Card        | Data Source                    | Format                  | Tooltip                                    |
| :----- | :-------------- | :----------------------------- | :---------------------- | :----------------------------------------- |
| 1.     | Total Orders    | count(distinct order_number)   | Integer (Comma: Y)      | Total unique orders in current view        |
| 2.     | Total Customers | count(distinct customer_id)    | Integer (Comma: Y)      | Total unique customers in current view     |
| 3.     | Total Quantity  | sum(quantity)                  | Integer (Comma: Y)      | Total ordered quantity in current view     |
| 4.     | Total Value     | sum(line_total)                | Decimal (2dp, Comma: Y) | Total order value in current view          |

---

## 1.2. Report Data Table

### 1.2a. DataTable (Sales Order Summary) - Columns

| Header        | Data Source   | Format                  | On Click         | Card Placement | Tooltip / On Hover         | Inline Edit Component |
| :------------ | :------------ | :---------------------- | :--------------- | :------------- | :------------------------- | :-------------------- |
| Product Image | product_image | Image                   | –                | –              | Product image              | –                     |
| Date          | order_date    | Date                    | –                | Title          | Sales order date           | –                     |
| Order Number  | order_number  | String                  | Open Sales Order | Subtitle       | Open this sales order      | –                     |
| Party         | customer_name | String                  | –                | Details        | Customer placing the order | –                     |
| Product       | product_name  | String                  | –                | Details        | Product name               | –                     |
| Pack          | pack_name     | String                  | –                | Details        | Pack type                  | –                     |
| Quantity      | quantity      | Integer (Comma: Y)      | –                | Details        | Ordered quantity           | –                     |
| Rate          | rate          | Decimal (2dp, Comma: Y) | –                | Details        | Rate per unit              | –                     |
| Value         | line_total    | Decimal (2dp, Comma: Y) | –                | Details        | Line total value           | –                     |

---

### 1.2b. DataTable (Sales Order Summary) - Toolbar Config

| Feature          | Settings                                          | On Click                          |
| :--------------- | :------------------------------------------------ | :-------------------------------- |
| Search           | Yes                                               | Search report records             |
| View Toggle      | Yes                                               | Switch between available layouts  |
| Column Selection | Yes                                               | Show/Hide columns                 |
| Group By         | Yes                                               | Group records by selected column  |
| Filter           | Yes                                               | Open filter panel                 |
| Export           | Yes<br/>Filename: `sales-order-summary`           | Export report data                |
| Share            | Yes                                               | Share current filtered view       |
| Full Screen      | Yes                                               | Toggle full-screen view           |

---

### 1.2c. DataTable (Sales Order Summary) - Config

| Feature        | Settings               |
| :------------- | :--------------------- |
| Row Selection  | No                     |
| Bulk Actions   | No                     |
| Sticky Header  | Yes                    |
| Column Resize  | Yes                    |
| Column Pinning | Yes                    |
| Sorting        | Yes                    |
| Pagination     | Yes<br/>Page Size: 50  |

---

### 1.2e. DataTable (Sales Order Summary) - Filter Fields

| Name       | Component         | Depends On | Possible Values                                                                                              | Default Values | Output             | Required | Tooltip               |
| :--------- | :---------------- | :--------- | :----------------------------------------------------------------------------------------------------------- | :------------- | :----------------- | :------- | :-------------------- |
| Date Range | Date Range Picker | –          | Any date range                                                                                               | Current Month  | from_date, to_date | No       | Filter by Order Date  |
| Party      | Multi Select      | –          | All active customers                                                                                         | All            | customer_ids       | No       | Filter by Customer    |
| Product    | Multi Select      | –          | All active products                                                                                          | All            | product_ids        | No       | Filter by Product     |
| Pack       | Multi Select      | –          | All active packs                                                                                             | All            | pack_ids           | No       | Filter by Pack        |
| Status     | Multi Select      | –          | Draft, Submitted, Approved, Partial Dispatched, Fully Dispatched, Completed, Cancelled, Rejected             | All            | stage_ids          | No       | Filter by Order Stage |
| Agent      | Multi Select      | –          | All active agents                                                                                            | All            | agent_ids          | No       | Filter by Agent       |

---
