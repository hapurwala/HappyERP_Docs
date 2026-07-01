# HappyERP Purchase Order (PO) UI/UX Documentation

This document gives UI details of all pages used in Purchase Order module.

# Related Reference files

specs file : [ERP-DES-PUR-PO](/specs/ERP-DES-PUR-PO.md)

---

# Pages

| S. No. | Name              | Type  | Purpose                                                                                                         |
| ------ | ----------------- | ----- | ---------------------------------------------------------------------------------------------------------------- |
| 1.     | PO List           | Page  | - View all purchase orders<br/>- Search/filter purchase orders<br/>- Perform quick actions on purchase order(s) |
| 2.     | Add/Modify PO     | Page  | - View details of the purchase order<br/>- Add/modify any data of current purchase order                        |
| 3.     | Add Product to PO | Popup | - Add/modify data (product, quantity, rate etc) of product added in purchase order                              |
| 4.     | Print Preview     | Page  | -                                                                                                                |

---

# 1. PO List Page UI

This page shows list of POs in tabular format. The POs appear in the list will depend upon current user's permission and applied filters.

## 1.1. KPI Cards

Summary cards are the widgets showing summary of the data on specific criteria. The numbers shown will depend upon current user's permission and applied filters. User has an option to select which cards he/she wants to see. Following are some example of cards:

| S. No. | Description        | Examples                                                                                |
| ------ | ------------------- | ---------------------------------------------------------------------------------------- |
| 1.     | Total POs          | -                                                                                        |
| 2.     | Stage wise cards   | - Pending Approval<br/>- Ordered<br/>- Partial Received<br/>- Completed<br/>- Cancelled |
| 3.     | Period wise cards  | - Today<br/>- This week<br/>- Last week<br/>- This month                                |
| 4.     | Vendor wise Cards  | - Highest Value POs (3 cards)                                                           |
| 5.     | Product wise Cards | - Highest Value POs (3 cards)                                                           |

---

## 1.2. PO List Data Table

**Component Type:** DataTable

### 1.2a. DataTable (PO List) - Columns

| Header           | Data Source       | Format       | On Click                 | Card Placement | Tooltip / On Hover                                       | Inline Edit Component |
| ----------------- | ------------------- | ------------- | --------------------------- | ----------------- | -------------------------------------------------------------- | ------------------------ |
| PO Number        | t_purchase_order.`po_number`         | String       | Open PO View / Edit Page  | Title             | -                                                                | -                        |
| PO Date          | t_purchase_order.`po_date`           | Date         | -                           | Body              | -                                                                | -                        |
| Vendor           | t_purchase_order.`vendor_id`         | String       | Open Vendor Profile        | Body              | -                                                                | -                        |
| Total Amount     | t_purchase_order.`total_amount`      | Currency     | -                           | Body              | Total Purchase Order Value Breakup                              | -                        |
| Stage            | t_purchase_order.stage.`name`        | Stage Badge  | Open Workflow Timeline     | Body              | Current PO Status with updatedBy, updatedAt and remarks         | -                        |
| Delivery Status  | t_purchase_order.`delivery_status`   | Status Badge | Open Delivery Details      | Body              | -                                                                | -                        |
| Payment Status   | t_purchase_order.`payment_status`    | Status Badge | Open Payment Summary       | Body              | -                                                                | -                        |
| Created By       | t_purchase_order.`created_by`        | String       | Open User Profile          | Body              | -                                                                | -                        |
| Updated At       | t_purchase_order.`updated_at`        | DateTime     | -                           | Body              | -                                                                | -                        |

---

### 1.2b. DataTable (PO List) - Toolbar Config

| Feature          | Settings                       | On Click                                                |
| ----------------- | -------------------------------- | ---------------------------------------------------------- |
| Search           | Yes                              | -                                                            |
| View Toggle      | Yes                              | -                                                            |
| Column Selection | Yes                              | -                                                            |
| Group By         | Yes                              | -                                                            |
| Filter           | Yes                              | -                                                            |
| Export           | Yes<br/>Filename: `po-list`     | Export displayed records using filename `po-list`.          |
| Share            | Yes                              | -                                                            |
| Full Screen      | Yes                              | -                                                            |
| Add              | Yes                              | Page to Open: `po`                                           |

---

### 1.2c. DataTable (PO List) - Table Config

| Feature        | Settings              |
| --------------- | ---------------------- |
| Row Selection   | No                     |
| Bulk Actions    | No                     |
| Sticky Header   | Yes                    |
| Column Resize   | Yes                    |
| Column Pinning  | Yes                    |
| Sorting         | Yes                    |
| Pagination      | Yes<br/>Page Size: 20  |

---

### 1.2d. DataTable (PO List) - RowAction Menu

| Name               | Action                                                      | Visibility Criteria                                                 | Icon               | Tooltip |
| ------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------- | -------------------- | ------- |
| View               | Open page: `po`                                             | - View Permission<br/>- No Modify Permission (Due to Role or Stage) | eye                | -       |
| Modify             | Open page: `po`                                             | - Modify permission<br/>- Modify allowed on stage                   | pencil             | -       |
| Modify Date/Number | Open page: `modifyDocNumber`                                | - Modify permission<br/>- Modify allowed on stage                   | calendar-edit      | -       |
| Set as <Stage_1>   | Set stage to Stage_1                                        | Allowed stage based on current stage                                 | arrow-right-circle | -       |
| Set as <Stage_2>   | Set stage to Stage_2                                        | Allowed stage based on current stage                                 | arrow-right-circle | -       |
| Undo Stage         | Move back to last stage                                     | - Permission to rollback from current stage                          | undo-2             | -       |
| Print              | Downloads PDF of the order in given format                  | - Print permission                                                   | printer            | -       |
| Duplicate          | Create a copy of the PO. Stage will be set to `Draft`.      | - Add Permission                                                     | copy               | -       |
| Cancel             | - Show confirmation message<br/>- On Yes, mark as cancelled | - Cancel permission<br/>- Cancel allowed on current stage            | share-2            | -       |
| Delete             | - Show confirmation message<br/>- On Yes, delete            | - Delete permission<br/>- Delete allowed on current stage            | trash-2            | -       |

---

### 1.2e. DataTable (PO List) - Filters Fields

| Name           | Component               | Depends On   | Possible Values                                                                                       | Default Values   | Output             | Required | Tooltip                          |
| --------------- | ------------------------- | -------------- | ---------------------------------------------------------------------------------------------------------- | ------------------- | --------------------- | ---------- | ----------------------------------- |
| Date Range     | Date Range Picker       | -            | Any Date Range                                                                                        | Today             | from_date, to_date | -        | Filter Purchase Orders By Date   |
| Organisation   | Multi Select            | -            | Allowed organisations for current user                                                                | <All>              | organisation_ids   | -        | -                                 |
| Branch         | Multi Select            | Organisation | Active Branches of selected organisations                                                             | <All>              | branch_ids         | -        | -                                 |
| Vendor Type    | Multi Select            | -            | Manufacturer, Distributor, Wholesaler, Importer, Service Provider                                     | <All>              | vendor_type_ids    | -        | -                                 |
| Vendor         | Multi Select            | Vendor Type  | Active Vendors Based On Selected Vendor Type                                                          | <All>              | vendor_ids         | -        | -                                 |
| Stage          | Multi Select            | -            | Draft, Submitted, Approved, Ordered, Partial Received, Fully Received, Completed, Cancelled, Rejected | Draft, Submitted  | stage_ids          | -        | -                                 |
| Warehouse      | Multi Select            | -            | Active Warehouses                                                                                      | <All>              | warehouse_ids      | -        | -                                 |
| Payment Status | Dropdown (Multi Select) | -            | Pending, Partially Paid, Paid, Overdue, Cancelled                                                     | <All>              | payment_status_ids | -        | -                                 |
| Created By     | User Lookup             | -            | Active System Users                                                                                   | Current User       | user_ids           | -        | -                                 |

---

# 2. PO Entry Page UI

Fields in the main form are grouped in different sections.

## Main Form Sections

```
2.1. Left: Main Form
- General Information
- Product Details
- Delivery Schedule
- Payment Terms
- Attachments

2.2. Right: Summary, Timeline and System Information
- Summary
- Workflow Timeline
- Post Order Summary
- System Information
```

---

## 2.1 Left Section

### 2.1.1 General Information

| Name         | Data Source   | Component    | Component Specific Information                                                  | Required | Read Only | Validations    | On Change                     | Description                  | Tooltip |
| ------------- | -------------- | ------------- | ----------------------------------------------------------------------------------- | -------- | --------- | ---------------- | -------------------------------- | ------------------------------- | ------- |
| PO Number    | t_purchase_order.`po_number`    | Text         | Prefix=PO, Auto Increment, Format=PO-YYYY-000001                                | Yes      | Yes       | Must Be Unique | Auto Generated                | Unique Purchase Order Number | -       |
| Vendor       | t_purchase_order.`vendor_id`    | Select       | Single Select, Searchable, Async Lookup, Source=active vendors                  | Yes      | No        | -              | Load Vendor Details: currency | Vendor Selection             | -       |
| PO Date      | t_purchase_order.`po_date`      | DatePicker   | Single Date Picker                                                              | Yes      | No        | -              | -                              | Purchase Order Date          | -       |
| Currency     | t_purchase_order.`currency_id`  | Select       | Single Select, Searchable, Source=vendors active currencies                     | Yes      | No        | -              | Recalculate Amounts           | Transaction Currency         | -       |
| Freight Type | t_purchase_order.`freight_type` | Select       | Single Select, Options=Company Paid, Vendor Paid, Included In Rate, Third Party | Yes      | No        | -              | Update Freight Rules          | Freight Responsibility       | -       |
| Remarks      | t_purchase_order.`remarks`      | RichTextBox  | -                                                                                | No       | No        | -              | -                              | Remarks                      | -       |

---

### 2.1.2 Product Details

This section displays list of all products given in current PO.

| Name            | Data Source | Component  | Component Specific Information | Required | Read Only | Validations | On Change | Description                                            | Tooltip |
| ---------------- | ------------- | ----------- | ---------------------------------- | -------- | --------- | -------------- | ----------- | ----------------------------------------------------------- | ------- |
| Barcode Scan    | -           | Input      | -                              | No       | No        | -            | -         | Scan a product's barcode to add it to the table below | -       |
| Product Details | t_purchase_order.products()   | DataTable  | -                              | Yes      | No        | -            | -         | -                                                       | -       |

**Component Type:** DataTable

### 2.1.2a. DataTable (Product Details) - Columns

| Header         | Data Source        | Format   | On Click | Card Placement | Tooltip / On Hover                       | Inline Edit Component                                                                                                            |
| --------------- | --------------------- | --------- | ---------- | ----------------- | --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| Product        | t_purchase_order.products().`product_id`        | String   | -        | Title             | -                                              | Select<br/>Searchable, Async Lookup<br/>Mandatory<br/>Validation: Product Must Be Active<br/>On Change: Load Product Details          |
| Variety        | t_purchase_order.products().`variety_id`        | String   | -        | -                  | -                                              | Select<br/>Filtered By Product<br/>On Change: Load Variety Details                                                                     |
| Pack           | t_purchase_order.products().`pack_id`           | String   | -        | -                  | -                                              | Select<br/>Filtered By Product<br/>Validation: Must Belong To Selected Product<br/>On Change: Update Quantity Conversion              |
| Quantity       | t_purchase_order.products().`quantity`          | Number   | -        | -                  | -                                              | Input<br/>Suffix: `uom_short_name`, Decimal Allowed, Min=0<br/>Mandatory<br/>Validation: Quantity Must Be Greater Than Zero<br/>On Change: Recalculate Amounts |
| Rate           | t_purchase_order.products().`rate`              | Currency | -        | -                  | -                                              | Input<br/>Decimal Currency Input<br/>Mandatory<br/>Validation: Rate Must Be Greater Than Zero<br/>On Change: Recalculate Amounts        |
| Discount       | t_purchase_order.products().`discount`          | Number   | -        | -                  | -                                              | Input<br/>Fixed Amount Or Percentage Or Unit<br/>Validation: Discount Cannot Exceed Line Amount<br/>On Change: Recalculate Amounts      |
| Discount Type  | t_purchase_order.products().`discount_type`     | String   | -        | -                  | -                                              | Select<br/>Options: Unit, Percentage, Fixed<br/>Mandatory<br/>On Change: Recalculate Amounts                                            |
| Product Amount | t_purchase_order.products().`product_amount`    | Currency | -        | -                  | -                                              | -                                                                                                                                        |
| Tax            | t_purchase_order.products().`tax_amount`        | Currency | -        | -                  | Tax Breakup<br/>Tax 1<br/>Tax 2<br/>...        | -                                                                                                                                        |
| Total          | t_purchase_order.products().`line_total`        | Currency | -        | -                  | Amount Breakup<br/>Product Amount<br/>Tax Amount<br/>Total | -                                                                                                                            |
| Actions        | -                   | -        | -        | -                  | -                                              | AppActionMenu<br/>Edit, Duplicate, Delete Row                                                                                           |

---

### 2.1.2b. DataTable (Product Details) - Toolbar Config

| Feature          | Settings | On Click             |
| ----------------- | -------- | ----------------------- |
| Search           | No       | -                        |
| View Toggle      | No       | -                        |
| Column Selection | No       | -                        |
| Group By         | No       | -                        |
| Filter           | No       | -                        |
| Export           | No       | -                        |
| Share            | No       | -                        |
| Full Screen      | Yes      | -                        |
| Add              | Yes      | Page to Open: `po_product` |

---

### 2.1.2c. DataTable (Product Details) - Table Config

| Feature        | Settings              |
| --------------- | ---------------------- |
| Row Selection   | No                     |
| Bulk Actions    | No                     |
| Sticky Header   | Yes                    |
| Column Resize   | Yes                    |
| Column Pinning  | Yes                    |
| Sorting         | Yes                    |
| Pagination      | Yes<br/>Page Size: 20  |

---

### 2.1.2d. DataTable (Product Details) - RowAction Menu

| Name             | Action                                                      | Visibility Criteria                                       | Icon               | Tooltip |
| ----------------- | -------------------------------------------------------------- | -------------------------------------------------------------- | -------------------- | ------- |
| Modify           | Open page: `po`                                             | - Modify permission<br/>- Modify allowed on stage           | pencil             | -       |
| Set as <Stage_1> | Set stage to Stage_1                                        | Allowed stage based on current stage                         | arrow-right-circle | -       |
| Set as <Stage_2> | Set stage to Stage_2                                        | Allowed stage based on current stage                         | arrow-right-circle | -       |
| Undo Stage       | Move back to last stage                                     | - Permission to rollback from current stage                  | undo-2             | -       |
| Duplicate        | Create a copy of the PO. Stage will be set to `Draft`.      | - Add Permission                                              | copy               | -       |
| Cancel           | - Show confirmation message<br/>- On Yes, mark as cancelled | - Cancel permission<br/>- Cancel allowed on current stage     | share-2            | -       |
| Delete           | - Show confirmation message<br/>- On Yes, delete            | - Delete permission<br/>- Delete allowed on current stage     | trash-2            | -       |

---

### 2.1.3 Delivery Schedule

| Name                    | Data Source                                                                                              | Component          | Component Specific Information               | Required | Read Only | Validations                                                                        | On Change                    | Description                | Tooltip |
| ------------------------- | -------------------------------------------------------------------------------------------------------- | --------------------- | ------------------------------------------------ | -------- | --------- | --------------------------------------------------------------------------------------- | -------------------------------- | ------------------------------- | ------- |
| Warehouse              | t_purchase_order.deliveries().`warehouse_id`                                                              | Select              | Single Select, Searchable, Source=warehouses | Yes      | No        | Must Be Active Warehouse                                                           | Load Warehouse Locations     | Receiving Warehouse       | -       |
| Location               | t_purchase_order.deliveries().`location_id`                                                               | Select              | Single Select, Filtered By Warehouse         | Yes      | No        | Must Belong To Selected Warehouse                                                  | Load Location Details        | Warehouse Location        | -       |
| Quantity               | t_purchase_order.deliveries().`quantity`                                                                  | Input               | Decimal Allowed, Min=0                       | Yes      | No        | Quantity Must Be Greater Than Zero, Total Delivery Quantity Must Match PO Quantity | Recalculate Pending Quantity | Delivery Quantity         | -       |
| Expected Delivery Date | t_purchase_order.deliveries().`expected_delivery_date`                                                    | DatePicker          | Single Date Picker                           | Yes      | No        | Must Be Greater Than Or Equal To PO Date                                          | Update Delivery Schedule     | Planned Delivery Date     | -       |
| Delivery Status        | If received_quantity = 0 => **Pending**<br/>If pending_quantity = 0 => **Complete**<br/>Else **Partial** | AppStatusBadge      | Auto Updated By GRN                          | Yes      | Yes       | Valid Workflow Status                                                              | Auto Update                  | Delivery Progress Status  | Pending / Partial / Completed |
| Received Quantity      | t_purchase_order.deliveries().`received_quantity`                                                         | AppNumberDisplay    | Auto Updated From GRN                        | Yes      | Yes       | Cannot Exceed Planned Quantity                                                     | Auto Update                  | Quantity Received         | -       |
| Pending Quantity       | t_purchase_order.deliveries().`pending_quantity`                                                          | AppNumberDisplay    | Auto Calculated                              | Yes      | Yes       | Formula Validation                                                                 | Auto Calculate               | Remaining Quantity        | -       |
| Priority               | t_purchase_order.deliveries().`priority`                                                                  | AppSelect           | Single Select (High, Medium, Low)            | No       | No        | Valid Priority Required                                                           | Update Delivery Rules        | Delivery Priority         | -       |
| Remarks                | t_purchase_order.deliveries().`remarks`                                                                   | AppTextArea         | Multi Line Text Area                         | No       | No        | Max 500 Characters                                                                 | Save Remarks                 | Additional Delivery Notes | -       |
| Actions                | -                                                                                                          | AppActionMenu       | Edit, Duplicate, Delete Row                  | No       | No        | -                                                                                  | Execute Selected Action      | Delivery Schedule Actions | -       |

---

### 2.1.4 Payment Terms

| Name                    | Data Source                                              | Component           | Component Specific Information                        | Required | Read Only | Validations                        | On Change                  | Description                       | Tooltip |
| ------------------------- | ------------------------------------------------------------ | ---------------------- | ----------------------------------------------------------- | -------- | --------- | --------------------------------------- | ------------------------------ | ------------------------------------- | ------- |
| Payment Terms Date     | t_purchase_order.`payment_terms_date`             | AppDatePicker        | Single Date Picker                                    | Yes      | No        | Cannot Be Blank                    | Update Due Date           | Base Date For Payment Calculation | -       |
| Payment Basis          | t_purchase_order.`payment_basis`                  | AppSelect            | Single Select (Invoice Date, GRN Date, Delivery Date) | Yes      | No        | Valid Option Required              | Recalculate Due Date      | Basis For Payment Calculation     | -       |
| Credit Days            | t_purchase_order.`credit_days`                    | AppNumberInput       | Integer Only                                          | Yes      | No        | Must Be Greater Than Or Equal To 0 | Recalculate Due Date      | Credit Period In Days             | -       |
| Advance Payment        | t_purchase_order.`advance_payment_percentage`     | AppPercentageInput   | Percentage Value                                      | No       | No        | 0 - 100%                           | Recalculate Payment Terms | Advance Payment Percentage        | -       |
| Grace Days             | t_purchase_order.`grace_days`                     | AppNumberInput       | Integer Only                                          | No       | No        | Must Be Greater Than Or Equal To 0 | Recalculate Due Date      | Additional Grace Period           | -       |
| Payment Mode           | t_purchase_order.`payment_mode`                   | AppMultiSelect       | Cash, Bank Transfer, UPI, Cheque, NEFT, RTGS          | No       | No        | Valid Payment Mode Required        | Update Payment Rules      | Allowed Payment Methods           | -       |
| TDS Applicable         | t_purchase_order.`tds_applicable`                 | AppToggle            | Yes / No                                              | No       | No        | -                                   | Show/Hide TDS Fields       | Indicates TDS Deduction           | -       |
| TDS Category           | t_purchase_order.`tds_category_id`                | AppLookup            | Single Select, Source=TDS Master                      | No       | No        | Required If TDS Applicable         | Load TDS Percentage       | Applicable TDS Category           | -       |
| TDS Percentage         | t_purchase_order.`tds_percentage`                 | AppPercentageInput   | Auto Filled From TDS Category                         | No       | Yes       | 0 - 100%                           | Recalculate TDS Amount    | TDS Deduction Percentage          | -       |
| Payment Due Date       | t_purchase_order.`payment_due_date`               | AppDateDisplay       | Auto Calculated Field                                 | Yes      | Yes       | Formula Validation                 | Auto Calculate            | Final Payment Due Date            | -       |
| Early Payment Discount | t_purchase_order.`early_payment_discount_enabled` | AppToggle            | Yes / No                                              | No       | No        | -                                   | Show/Hide Discount Rules  | Enable Early Payment Discount     | -       |
| Late Payment Penalty   | t_purchase_order.`late_payment_penalty_enabled`   | AppToggle            | Yes / No                                              | No       | No        | -                                   | Show/Hide Penalty Rules   | Enable Late Payment Penalty       | -       |

---

### 2.1.5 Attachments

| Name       | Data Source                                                     | Component       | Component Specific Information            | Required | Read Only | Validations              | On Change      | Description               | Tooltip |
| ----------- | -------------------------------------------------------------------- | ------------------ | ------------------------------------------- | -------- | --------- | --------------------------- | ----------------- | ------------------------------ | ------- |
| Preview    | -                                                                | AppFileViewer     | PDF, Image, DOC Preview                   | No       | Yes       | -                        | Open Preview   | Preview Uploaded Document | -       |
| File Name  | t_purchase_order.attachments().`file_name`  | AppTextDisplay    | Auto Generated From Uploaded File         | Yes      | Yes       | Cannot Be Blank          | Auto Populate  | Uploaded File Name        | -       |
| File Type  | t_purchase_order.attachments().`file_type`  | AppBadge          | Auto Detected (PDF, JPG, PNG, XLSX, DOCX) | Yes      | Yes       | Supported File Type Only | Auto Detect    | Uploaded File Type        | -       |
| Actions    | -                                                                | AppActionMenu     | Preview, Download, Delete                 | No       | No        | Permission Based Actions | Execute Action | Attachment Actions        | -       |

---

## 2.2 Right Section

### 2.2.1 Summary

Component to use: `Summary Card`

| Name           | Data Source                          | Format   | On Click | Tooltip / On Hover     |
| --------------- | --------------------------------------- | --------- | ---------- | -------------------------- |
| Total Items    | count(distinct product_pack_id)      | Number   | -        | -                        |
| Total Quantity | sum(quantity)                        | Number   | -        | -                        |
| Product Value  | sum(product_value)                   | Currency | -        | -                        |
| Tax Breakup    | sum(tax_amount) for individual taxes | Currency | -        | -                        |
| Discount       | discount                             | Currency | -        | -                        |
| Total PO Value | Product Value + Tax - Discount       | Currency | -        | -                        |
| Advance Amount | advance_amount                       | Currency | -        | -                        |
| Balance To Pay | Total PO Value - Advance Amount      | Currency | -        | -                        |
| Delivery Date  | min(delivery_date)                   | Date     | -        | Earliest Delivery Date  |

---

### 2.2.2 Workflow Timeline

Component to use: `StageHistoryViewer`

| Name       | Data Source  | Component                                        | Tooltip   |
| ----------- | -------------- | ---------------------------------------------------- | ----------- |
| Stage      | t_purchase_order.stage_history().`stage_name`  | Text                                             | -         |
| Set At     | t_purchase_order.stage_history().`set_on`      | Text                                             | -         |
| Set By     | t_purchase_order.stage_history().`set_by`      | Avatar                                           | User name |
| Remarks    | t_purchase_order.stage_history().`remarks`     | Text                                             | -         |
| Attachment | t_purchase_order.stage_history().`url`         | Pin icon. On click show attachment in Zoom mode. | -         |

---

### 2.2.3 Post Order Summary

Component to use: `Summary Card`

| Name                    | Data Source            | Format   | On Click | Tooltip / On Hover |
| ------------------------- | -------------------------- | --------- | ---------- | --------------------- |
| Total Quantity Received | sum(grn.quantity)       | Number   | -        | -                   |
| Product Value Invoiced  | sum(pi.product_value)   | Currency | -        | -                   |
| Total Payment Made      | sum(payment.amount)     | Currency | -        | -                   |

---

### 2.2.4 System Information

| Name        | Data Source       | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                                                | Tooltip |
| ------------ | -------------------- | ----------- | ---------------------------------- | -------- | --------- | -------------- | --------- | ---------------------------------------------------------- | ------- |
| Created By  | System Generated   | Text      | -                                | No       | Yes       | -            | -         | User who created the Purchase Order                       | -       |
| Created On  | System Generated   | Date Time | DD/MM/YYYY HH:mm                | No       | Yes       | -            | -         | Date and Time when the Purchase Order was created          | -       |
| Modified By | System Generated   | Text      | -                                | No       | Yes       | -            | -         | User who last modified the Purchase Order                 | -       |
| Modified On | System Generated   | Date Time | DD/MM/YYYY HH:mm                | No       | Yes       | -            | -         | Date and Time when the Purchase Order was last modified    | -       |

---

# Recommended Tech Stack

Frontend:

* React
* MUI
* TanStack Table
* React Hook Form
* Zustand
* Firebase SDK

---

# Recommended Folder Structure

```text
/modules/purchase-order
    /pages
    /components
    /tables
    /forms
    /dialogs
    /workflow
    /services
    /hooks
```

---
