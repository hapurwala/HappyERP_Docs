# HappyERP Sales Order UI/UX Documentation


This document gives basic design details of following module.

# Related Reference files

specs file : [ERP-DES-SAL-Order](ERP-DES-SAL-Order.md)

---

# Pages

| S. No. | Name                   | Type | Purpose                                                                                    |
| :----- | :--------------------- | :--- | :----------------------------------------------------------------------------------------- |
| 1.     | Sales Order List       | Page | - View all Sales Orders<br/>- Search/filter orders<br/>- Perform quick actions on order(s) |
| 2.     | Add/Modify Sales Order | Page | - View details of the order<br/>- Add/modify any data of current order                     |

---

# 1. Sales Order List Page UI

This page shows list of Sales Orders in tabular format. The orders appearing in the list depend on the current user's permissions and applied filters.

## 1.1. Sales Order List Data Table

### 1.1a. DataTable (Sales Order List) - Columns

| Header                 | Data Source       | Format                  | On Click               | Card Placement | Tooltip / On Hover         | Inline Edit Component |
| :--------------------- | :---------------- | :---------------------- | :--------------------- | :------------- | :------------------------- | :-------------------- |
| Order Number           | order_number      | String                  | Open View/Modify page  | Title          | View Sales Order Details   | –                     |
| Priority               | priority          | String                  | –                      | Subtitle       | Order Priority             | –                     |
| Order Date             | order_date        | Date                    | –                      | Details        | Order Date                 | –                     |
| Customer               | customer_name     | String                  | Open Customer Profile  | Details        | View Customer Details      | –                     |
| Agent                  | agent_name        | String                  | –                      | Details        | Agent for this order       | –                     |
| Expected Delivery Date | min_delivery_date | Date                    | –                      | Details        | Earliest Expected Delivery | –                     |
| Total Quantity         | total_quantity    | Integer (Comma: Y)      | –                      | Details        | Total Order Quantity       | –                     |
| Total Value            | net_amount        | Decimal (2dp, Comma: Y) | –                      | Details        | Total Order Value          | –                     |
| Stage                  | stage             | String                  | Open Workflow Timeline | Details        | Current Order Stage        | –                     |

---

### 1.1b. DataTable (Sales Order List) - Toolbar Config

| Feature          | Settings                                     | On Click                                   |
| :--------------- | :------------------------------------------- | :----------------------------------------- |
| Search           | Yes                                          | Search Sales Order records                 |
| View Toggle      | Yes                                          | Switch between available DataTable layouts |
| Column Selection | Yes                                          | Show/Hide columns                          |
| Group By         | Yes                                          | Group records by selected column           |
| Filter           | Yes                                          | Open filter panel                          |
| Export           | Yes<br/>Filename: `sales-order-list`         | Export Sales Order list                    |
| Share            | Yes                                          | Share current filtered view                |
| Full Screen      | Yes                                          | Toggle full-screen view                    |
| Add              | when  s_app_role_permission.`can_add = true` | Page to Open: `Sales order`                |

---

### 1.1c. DataTable (Sales Order List) - Config

| Feature        | Settings              |
| :------------- | :-------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

---

### 1.1d. DataTable (Sales Order List) - RowAction Menu

| Name             | Action                          | Visibility Criteria                     | Icon               | Tooltip                |
| :--------------- | :------------------------------ | :-------------------------------------- | :----------------- | :--------------------- |
| View                | Open page: `sales_order`                          | View Permission                         | eye                | View Order             |
| Modify              | Open page: `sales_order`                          | Modify Permission + Stage Allows Edit   | pencil             | Modify Order           |
| Modify Date/Number  | Open popup to change order date or order number   | Modify Permission                       | calendar           | Change Date or Number  |
| Set as \<Stage\> | Set stage to next allowed stage | Allowed stage based on current stage    | arrow-right-circle | Change Workflow Stage  |
| Undo Stage       | Move back to last stage         | Rollback Permission                     | undo-2             | Rollback Current Stage |
| Print            | Download PDF of the order       | View Permission                         | printer            | Download Order PDF     |
| Cancel           | Show confirmation and cancel    | Cancel Permission + Stage Allows Cancel | x-circle           | Cancel Order           |
| Delete           | Show confirmation and delete    | Delete Permission + Stage Allows Delete | trash-2            | Delete Order           |

---

### 1.1e. DataTable (Sales Order List) - Filter Fields

| Name           | Component              | Depends On     | Possible Values                                                                                             | Default Values | Output             | Required | Tooltip                   |
| :------------- | :--------------------- | :------------- | :---------------------------------------------------------------------------------------------------------- | :------------- | :----------------- | :------- | :------------------------ |
| Financial Year | Single Select Dropdown | –              | All financial years since start of business                                                                 | Current FY     | financial_year_id  | No       | Filter by Financial Year  |
| Period         | Single Select Dropdown | Financial Year | Last 7 days, Last 30 days, This Month, Last Month, Last 3 Months, Last 6 Months, Financial Year, Date Range | Last 7 Days    | period             | No       | Filter by Period          |
| Date Range     | Date Range Picker      | Period         | Any date range (enabled when Date Range is selected in Period)                                              | –              | from_date, to_date | No       | Filter by Order Date      |
| Stage          | Multi Select           | –              | Draft, Submitted, Approved, Partial Dispatched, Fully Dispatched, Completed, Cancelled, Rejected            | Open Stages    | stage_ids          | No       | Filter by Workflow Stage  |
| Customer       | Multi Select           | –              | All active customers                                                                                        | All            | customer_ids       | No       | Filter by Customer        |
| Agent          | Multi Select           | –              | All active agents                                                                                           | All            | agent_ids          | No       | Filter by Agent           |
| Product        | Multi Select           | –              | All active products                                                                                         | All            | product_ids        | No       | Filter by Product         |
| Priority       | Multi Select           | –              | High, Medium, Low                                                                                           | All            | priority_ids       | No       | Filter by Priority        |

---

# 2. Sales Order Entry Page UI

Fields in the main form are grouped into different sections.

```
2.1. Left: Main Form

- Basic Details
- Order Details (DataTable)
- Attachments

2.2. Right: Summary and Timeline

- Order Summary
- Customer Summary
- Workflow Timeline
```

## 2.1. Left Part

### 2.1.1. Section (Basic Details)

| Name               | Data Source  | Component   | Component Specific Information                                          | Required       | Read Only                    | Validations                    | On Change                      | Description                        | Tooltip                             |
| :----------------- | :----------- | :---------- | :---------------------------------------------------------------------- | :------------- | :--------------------------- | :----------------------------- | :----------------------------- | :--------------------------------- | :---------------------------------- |
| Order Number       | order_number | Text        | Auto-generated after first save.                                        | not applicable | Yes                          | Must be unique                 | Auto Generated                 | Unique order number                | System generated order number       |
| Order Date         | order_date   | DatePicker  | Single Date Picker. Defaults to current date.                           | Yes            | No (locked after first save) | Cannot be a future date        | –                              | Date of the sales order            | Order date                          |
| Customer           | customer_id  | Select      | Searchable async lookup. Active customers only.                         | Yes            | No (locked after first save) | Customer must be active        | Load agent, placed by defaults | Customer placing the order         | Select customer                     |
| Agent              | agent_id     | Select      | Searchable async lookup. Defaults to agent linked to selected customer. | No             | No                           | –                              | –                              | Agent for this order               | Select agent                        |
| Placed By          | placed_by_id | Select      | Searchable async lookup. Active sales executives.                       | Yes            | No                           | Must be active sales executive | –                              | Sales executive handling the order | Select sales executive              |
| Currency           | currency_id  | Select      | Defaults to customer's default currency**                               | No             | No (locked when lines exist) | Valid currency required        | Update rate fields             | Currency for this order            | Select currency                     |
| Priority           | priority     | Select      | High, Medium, Low                                                       | Yes            | No                           | Valid priority required        | –                              | Order priority                     | Set order priority                  |
| Remarks            | remarks      | RichTextBox | –                                                                       | No             | No                           | –                              | –                              | Internal remarks                   | Additional notes                    |
| Terms & Conditions | terms        | RichTextBox | –                                                                       | No             | No                           | –                              | –                              | Order terms and conditions         | Terms and conditions for this order |



---

### 2.1.2. Section (Order Details)

Component Type: DataTable

---

### 2.1.2a. DataTable (Order Details) - Columns

| Header        | Data Source      | Format                  | On Click | Card Placement | Tooltip / On Hover            | Inline Edit Component                                                                             |
| :------------ | :--------------- | :---------------------- | :------- | :------------- | :---------------------------- | :------------------------------------------------------------------------------------------------ |
| Barcode       | barcode          | String                  | –        | –              | Scan or enter product barcode | Barcode Input                                                                                     |
| Product       | product_name     | String                  | –        | Title          | Product being ordered         | Select                                                                                            |
| Variety       | variety_name     | String                  | –        | Subtitle       | Product variety               | Select                                                                                            |
| Pack          | pack_name        | String                  | –        | Details        | Product pack                  | Select                                                                                            |
| No. of Packs  | no_of_packs      | Integer (Comma: N)      | –        | Details        | Number of outer packs         | Number Input                                                                                      |
| Quantity      | quantity         | Integer (Comma: N)      | –        | Details        | Ordered quantity              | Number Input                                                                                      |
| UoM           | uom_name         | String                  | –        | Details        | Unit of measurement           | –                                                                                                 |
| Rate          | rate             | Decimal (2dp, Comma: Y) | –        | Details        | Rate per unit                 | Number Input                                                                                      |
| Discount %    | discount_percent | Decimal (2dp, Comma: N) | –        | Details        | Discount percentage           | Number Input (displays as `X% (₹Y)` — percentage editable, amount shown in brackets as read-only) |
| Tax           | tax_amount       | Decimal (2dp, Comma: Y) | –        | Details        | Tax applied on this line      | –                                                                                                 |
| Total         | total            | Decimal (2dp, Comma: Y) | –        | Details        | Total for this line item      | –                                                                                                 |
| Delivery Date | delivery_date    | Date                    | –        | Details        | Delivery date for this line   | DatePicker                                                                                        |
| Payment Terms | payment_terms_id | String                  | –        | Details        | Payment terms for this line   | Select (defaults from customer master)                                                            |
| Notes         | notes            | String                  | –        | Details        | Notes for this line item      | Text Area                                                                                         |
| Stage         | stage            | String                  | –        | Details        | Current line dispatch status  | –                                                                                                 |

---

### 2.1.2b. DataTable (Order Details) - Toolbar Config

| Feature          | Settings | On Click                                    |
| :--------------- | :------- | :------------------------------------------ |
| Search           | Yes      | Search order product rows                   |
| View Toggle      | No       | –                                           |
| Column Selection | No       | –                                           |
| Group By         | No       | –                                           |
| Filter           | No       | –                                           |
| Export           | No       | –                                           |
| Share            | No       | –                                           |
| Full Screen      | No       | -                                           |
| Add              | Yes      | Add blank row at bottom of table            |
| Save             | Yes      | Save all pending changes to the sales order |

---

### 2.1.2c. DataTable (Order Details) - Config

| Feature        | Settings |
| :------------- | :------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | Yes      |
| Pagination     | No       |

---

### 2.1.2d. DataTable (Order Details) - RowAction Menu

| Name      | Action                                                          | Visibility Criteria | Icon  | Tooltip               |
| :-------- | :-------------------------------------------------------------- | :------------------ | :---- | :-------------------- |
| Cancel    | Cancel this line item                                           | Cancel Permission   | x     | Cancel this line      |
| Delete    | Delete product row                                              | Delete Permission   | trash | Remove product row    |

---

### 2.1.2e. DataTable (Order Details) - Filter Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Required | Tooltip |
| :--- | :-------- | :--------- | :-------------- | :------------- | :----- | :------- | :------ |
| –    | –         | –          | –               | –              | –      | –        | –       |

---

### 2.1.3. Section (Attachments)

| Name        | Data Source               | Component        | Component Specific Information      | Required | Read Only | Validations | On Change | Description          | Tooltip |
| :---------- | :------------------------ | :--------------- | :---------------------------------- | :------- | :-------- | :---------- | :-------- | :------------------- | :------ |
| Attachments | sales_order.`attachments` | AttachmentViewer | PDF, Image, DOC Preview. Max 10 MB. | No       | No        | –           | –         | Supporting documents | –       |

---

## 2.2. Right Side Widgets

### 2.2.1. Order Summary

Component to use: Summary Card

*This section is fixed/sticky so it remains visible as the user scrolls.*

| Name               | Content                                    |
| :----------------- | :----------------------------------------- |
| Total Items        | Count of unique products in the order      |
| Total Packs        | Sum of no. of packs across all lines       |
| Total Quantity     | Sum of quantity across all lines           |
| Subtotal           | Sum of line totals before discount and tax |
| Total Discount     | Sum of discounts across all lines          |
| Total Tax          | Sum of tax across all lines                |
| Net Amount         | Final order value                          |
| Round Off          | Round off amount                           |
| Min. Delivery Date | Minimum delivery date across all lines     |

---

### 2.2.2. Customer Summary

Component to use: Summary Card

| Name                  | Content                               |
| :-------------------- | :------------------------------------ |
| Customer Name         | customer display name                 |
| Customer Mobile       | customer mobile number                |
| Customer GSTIN        | customer GSTIN                        |
| Customer Outstanding  | Total customer outstanding balance    |
| Last Order Date       | Date of last sales order              |
| Total Sales Value     | Lifetime sales value for this customer |

---

### 2.2.3. Workflow Timeline

Component to use: `StageHistoryViewer`

Data Source: `stageHistory` array

| Name/Label | Data Source             | Component | Tooltip   |
| :--------- | :---------------------- | :-------- | :-------- |
| Stage      | stage                   | Text      | –         |
| Set At     | actionAt                | Text      | –         |
| Set By     | actionBy / actionByName | Avatar    | User name |
| Remarks    | remarks                 | Text      | –         |

---
