# HappyERP Purchase Order (PO) UI/UX Documentation

This document gives UI details of all pages used in Purchase Order module.

# Pages

| S. No. | Name              | Type  | Purpose                                                                                                         |
| ------ | ----------------- | ----- | --------------------------------------------------------------------------------------------------------------- |
| 1.     | PO List           | Page  | - View all purchase orders<br/>- Search/filter purchase orders<br/>- Perform quick actions on purchase order(s) |
| 2.     | Add/Modify PO     | Page  | - View details of the purchase order<br/>- Add/modify any data of current purchase order                        |
| 3.     | Add Product to PO | Popup | - Add/modify data (product, quantity, rate etc) of product added in purchase order                              |
| 4.     | Print Preview     | Page  |                                                                                                                 |

# 1. PO List Page UI

This page shows list of POs in tabular format. The POs appear in the list will depend upon current user's permission and applied filters.

## 1.1. KPI Cards

Summary cards are the widgets showing summary of the data on specific criteria. The numbers shown will depend upon current user's permission and applied filters. User has an option to select which cards he/she wants to see. Following are some example of cards:

| S. No. | Description        | Examples                                                                                |
| ------ | ------------------ | --------------------------------------------------------------------------------------- |
| 1.     | Total POs          |                                                                                         |
| 2.     | Stage wise cards   | - Pending Approval<br/>- Ordered<br/>- Partial Received<br/>- Completed<br/>- Cancelled |
| 3.     | Period wise cards  | - Today<br/>- This week<br/>- Last week<br/>- This month                                |
| 4.     | Vendor wise Cards  | - Highest Value POs (3 cards)                                                           |
| 5.     | Product wise Cards | - Highest Value POs (3 cards)                                                           |

## 1.2. PO List Data Table

#### 1.2a. DataTable - Columns

| Name            | Content                       | Type     | On Click                 | Card Placement | Tooltip / On Hover                                      |
|:--------------- |:----------------------------- |:-------- |:------------------------ | -------------- |:------------------------------------------------------- |
| PO Number       | Unique Purchase Order Number  | Text     | Open PO View / Edit Page |                | View Purchase Order Details                             |
| PO Date         | Purchase Order Creation Date  | Date     | –                        |                | Purchase Order Date                                     |
| Vendor          | Vendor Name                   | Text     | Open Vendor Profile      |                | View Vendor Details                                     |
| Total Amount    | Total PO Amount Including Tax | Currency | –                        |                | Total Purchase Order Value Breakup                      |
| Stage           | Current Workflow Stage        | Status   | Open Workflow Timeline   |                | Current PO Status with updatedBy, updatedAt and remarks |
| Delivery Status | Delivery Progress Status      | Status   | Open Delivery Details    |                | View Delivery Progress                                  |
| Payment Status  | Payment Progress Status       | Status   | Open Payment Summary     |                | View Payment Progress                                   |
| Created By      | User Who Created PO           | User     | Open User Profile        |                | Created By User                                         |
| Updated At      | Last Updated Date & Time      | DateTime | –                        |                | Last Modification Date                                  |

### 1.2b. DataTable -  Toolbar Config

| Feature          | Settings                    |
|:---------------- |:--------------------------- |
| Search           | Yes                         |
| View Toggle      | Yes                         |
| Column Selection | Yes                         |
| Group by         | Yes                         |
| Filter           | Yes                         |
| Export           | Yes<br/>Filename: `po-list` |
| Share            | Yes                         |
| Full Screen      | Yes                         |
| Add              | Yes<br/>Page to open: `po`  |

### 1.2c. DataTable - Config

| Feature        | Settings              |
|:-------------- |:--------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

### 1.2d. DataTable - RowAction Menu

| Name               | Action                                                      | Visibility Criteria                                                 | Icon               | Tooltip |
| ------------------ | ----------------------------------------------------------- | ------------------------------------------------------------------- | ------------------ | ------- |
| View               | Open page: `po`                                             | - View Permission<br/>- No Modify Permission (Due to Role or Stage) | eye                | -       |
| Modify             | Open page: `po`                                             | - Modify permission<br/>- Modify allowed on stage                   | pencil             | –       |
| Modify Date/Number | Open page: `modifyDocNumber`                                | - Modify permission<br/>- Modify allowed on stage                   | calendar-edit      | –       |
| Set as <Stage_1>   | Set stage to Stage_1                                        | Allowed stage based on current stage                                | arrow-right-circle | –       |
| Set as <Stage_2>   | Set stage to Stage_2                                        | –                                                                   | arrow-right-circle |         |
| Undo Stage         | Move back to last stage                                     | - Permission to rollback from current stage                         | undo-2             | –       |
| Print              | Downloads PDF of the order in given format                  | - Print permission                                                  | printer            | –       |
| Duplicate          | Create a copy of the PO. Stage will be set to `Draft`.      | - Add Permission                                                    |                    | -       |
| Cancel             | - Show confirmation message<br/>- On Yes, mark as cancelled | - Cancel permission<br/>- Cancel allowed on current stage           | share-2            | -       |
| Delete             | - Show confirmation message<br/>- On Yes, delete brand      | - Delete permission<br/>- Delete allowed on current stage           | trash-2            | –       |

### 1.2e. DataTable - Filters Fields

| Name           | Component               | Depends on   | Possible Values                                                                                       | Default Values   | Output             | Tooltip                        |
|:-------------- |:----------------------- |:------------ |:----------------------------------------------------------------------------------------------------- |:---------------- |:------------------ |:------------------------------ |
| Date Range     | Date Range Picker       | -            | Any Date Range                                                                                        | Today            | from_date, to_date | Filter Purchase Orders By Date |
| Organisation   | Multi Select            | -            | Allowed organisations for current user                                                                | <All>            | organisation_ids   | -                              |
| Branch         | Multi Select            | Organisation | Active Branches of selected organisations                                                             | <All>            | branch_ids         | -                              |
| Vendor Type    | Multi Select            | -            | Manufacturer, Distributor, Wholesaler, Importer, Service Provider                                     | <All>            | vendor_type_ids    | -                              |
| Vendor         | Multi Select            | Vendor Type  | Active Vendors Based On Selected Vendor Type                                                          | <All>            | vendor_ids         | -                              |
| Stage          | Multi Select            | –            | Draft, Submitted, Approved, Ordered, Partial Received, Fully Received, Completed, Cancelled, Rejected | Draft, Submitted | stage_ids          | -                              |
| Warehouse      | Multi Select            | –            | Active Warehouses                                                                                     | All              | warehouse_ids      | -                              |
| Payment Status | Dropdown (Multi Select) | –            | Pending, Partially Paid, Paid, Overdue, Cancelled                                                     | All              | payment_status_ids | -                              |
| Created By     | User Lookup             | –            | Active System Users                                                                                   | Current User     | user_ids           | -                              |

# 2. PO Entry Page UI

Fields in the main form are grouped in different sections.

```
2.1. Left: Main Form
- General Information
- Product Details
- Delivery Schedule
- Payment Terms
- Attachment/Media

2.2. Right: Summary and Timeline
- Summary panel
- Workflow timeline
- Activity log 
```

## 2.1. Left Part

### 2.1.1. Section: General Information

| Name/Label   | Data Source  | Type/Component | Component Specific Information                                                  | Required | Read Only | Validations    | On Change                     | Description                  | Tooltip                    |
|:------------ |:------------ |:-------------- |:------------------------------------------------------------------------------- |:-------- |:--------- |:-------------- |:----------------------------- |:---------------------------- |:-------------------------- |
| PO Number    | po_number    | Text           | Prefix=PO, Auto Increment, Format=PO-YYYY-000001                                | Yes      | Yes       | Must Be Unique | Auto Generated                | Unique Purchase Order Number | System Generated PO Number |
| Vendor       | vendor_id    | Select         | Single Select, Searchable, Async Lookup, Source=active vendors                  | Yes      | No        | -              | Load Vendor Details: currency | Vendor Selection             | -                          |
| PO Date      | po_date      | DatePicker     | Single Date Picker                                                              | Yes      | No        | -              | -                             | Purchase Order Date          | -                          |
| Currency     | currency_id  | Select         | Single Select, Searchable, Source=vendors active currencies                     | Yes      | No        | -              | Recalculate Amounts           | Transaction Currency         | -                          |
| Freight Type | freight_type | Select         | Single Select, Options=Company Paid, Vendor Paid, Included In Rate, Third Party | Yes      | No        | -              | Update Freight Rules          | Freight Responsibility       | -                          |
| Remarks      | remarks      | RichTextBox    |                                                                                 | No       | No        | -              | -                             | Remarks                      | -                          |

### 2.1.2. Section: Product Details

This section displays list of all products given in current PO.

| Name/Label      | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations | On Change | Description | Tooltip |
|:--------------- |:----------- |:-------------- |:------------------------------ |:-------- |:--------- |:----------- |:--------- |:----------- |:------- |
| Barcode Scan    | -           | Input          |                                | No       | No        |             | -         |             |         |
| Product Details | product     | DataTable      |                                | Yes      | No        |             |           |             |         |

### 2.1.2a. DataTable: Columns

| Name/Label     | Data Source       | Type/Component | Component Specific Information                          | Required | Read Only | Validations                        | On Change                  | Description               | Tooltip                                                     |
|:-------------- |:----------------- |:-------------- |:------------------------------------------------------- |:-------- |:--------- |:---------------------------------- |:-------------------------- |:------------------------- |:----------------------------------------------------------- |
| Product        | product_id        | Select         | Single Select, Searchable, Async Lookup                 | Yes      | No        | Product Must Be Active             | Load Product Details       | Product Selection         | Select Product                                              |
| Variety        | variety_id        | Select         | Single Select, Filtered By Product                      | No       | No        | -                                  | Load Variety Details       | Product Variety           | Select Product Variety                                      |
| Pack           | pack_id           | Select         | Single Select, Filtered By Product                      | No       | No        | Must Belong To Selected Product    | Update Quantity Conversion | Product Packaging         | -                                                           |
| Quantity       | quantity + uom_id | Input          | Suffix: uom_short_name; Decimal Allowed, Min=0          | Yes      | No        | Quantity Must Be Greater Than Zero | Recalculate Amounts        | Ordered Quantity          | -                                                           |
| Rate           | rate              | Input          | Decimal Currency Input                                  | Yes      | No        | Rate Must Be Greater Than Zero     | Recalculate Amounts        | Purchase Rate             | -                                                           |
| Discount       | discount          | Input          | Fixed Amount Or Percentage Or Unit                      | No       | No        | Discount Cannot Exceed Line Amount | Recalculate Amounts        | Line Discount             | -                                                           |
| Discount Type  | discount_type     | Select         | Single Select, Possible Values: Unit, Percentage, Fixed | Yes      | No        |                                    | Recalculate Amounts        |                           | -                                                           |
| Product Amount | product_amount    | Text           | -                                                       | Yes      | Yes       |                                    |                            |                           |                                                             |
| Tax            | tax_amount        | Text           | -                                                       | No       | No        | Valid Tax Configuration Required   | Recalculate Amounts        | Tax Amount                | Tax Breckup<br/>Tax 1<br/>Tax 2<br/>...                     |
| Total          | line_total        | Text           | -                                                       | Yes      | Yes       | Formula Validation                 | Auto Calculate             | Total Amount              | Amount Breckup <br/>Product Amount<br/>Tax Amount<br/>Total |
| Actions        | -                 | AppActionMenu  | Edit, Duplicate, Delete Row                             | No       | No        | -                                  | Execute Selected Action    | Delivery Schedule Actions | -                                                           |

### 2.1.2b. DataTable -  Toolbar Config

| Feature          | Settings                           |
|:---------------- |:---------------------------------- |
| Search           | No                                 |
| View Toggle      | No                                 |
| Column Selection | No                                 |
| Group by         | No                                 |
| Filter           | No                                 |
| Export           | No                                 |
| Share            | No                                 |
| Full Screen      | Yes                                |
| Add              | Yes<br/>Page to open: `po_product` |

### 2.1.2c. DataTable - Config

| Feature        | Settings              |
|:-------------- |:--------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

### 2.1.2d. DataTable - RowAction Menu

| Name             | Action                                                      | Visibility Criteria                                       | Icon               | Tooltip |
| ---------------- | ----------------------------------------------------------- | --------------------------------------------------------- | ------------------ | ------- |
| Modify           | Open page: `po`                                             | - Modify permission<br/>- Modify allowed on stage         | pencil             | –       |
| Set as <Stage_1> | Set stage to Stage_1                                        | Allowed stage based on current stage                      | arrow-right-circle | –       |
| Set as <Stage_2> | Set stage to Stage_2                                        | –                                                         | arrow-right-circle |         |
| Undo Stage       | Move back to last stage                                     | - Permission to rollback from current stage              | undo-2             | –       |
| Duplicate        | Create a copy of the PO. Stage will be set to `Draft`.      | - Add Permission                                          |                    | -       |
| Cancel           | - Show confirmation message<br/>- On Yes, mark as cancelled | - Cancel permission<br/>- Cancel allowed on current stage | share-2            | -       |
| Delete           | - Show confirmation message<br/>- On Yes, delete brand      | - Delete permission<br/>- Delete allowed on current stage | trash-2            | –       |

### 2.1.3. Section: Delivery Schedule

| Name/Label             | Data Source                                                                                              | Type/ Component  | Component Specific Information               | Required | Read Only | Validations                                                                        | On Change                    | Description               | Tooltip                         |
|:---------------------- |:-------------------------------------------------------------------------------------------------------- |:---------------- |:-------------------------------------------- |:-------- |:--------- |:---------------------------------------------------------------------------------- |:---------------------------- |:------------------------- |:------------------------------- |
| Warehouse              | warehouse_id                                                                                             | Select           | Single Select, Searchable, Source=warehouses | Yes      | No        | Must Be Active Warehouse                                                           | Load Warehouse Locations     | Receiving Warehouse       | Select Delivery Warehouse       |
| Location               | location_id                                                                                              | Select           | Single Select, Filtered By Warehouse         | Yes      | No        | Must Belong To Selected Warehouse                                                  | Load Location Details        | Warehouse Location        | Select Storage Location         |
| Quantity               | quantity                                                                                                 | Input            | Decimal Allowed, Min=0                       | Yes      | No        | Quantity Must Be Greater Than Zero, Total Delivery Quantity Must Match PO Quantity | Recalculate Pending Quantity | Delivery Quantity         | Quantity Planned For Delivery   |
| Expected Delivery Date | purchase_order_delivery>{delivery_id}>expected_delivery_date                                             | DatePicker       | Single Date Picker                           | Yes      | No        | Must Be Greater Than Or Equal To PO Date                                           | Update Delivery Schedule     | Planned Delivery Date     | Expected Goods Arrival Date     |
| Delivery Status        | If received_quantity = 0 => **Pending**<br/>If pending_quantity = 0 => **Complete**<br/>Else **Partial** | AppStatusBadge   | Auto Updated By GRN                          | Yes      | Yes       | Valid Workflow Status                                                              | Auto Update                  | Delivery Progress Status  | Pending / Partial / Completed   |
| Received Quantity      | purchase_order_delivery>{delivery_id}>received_quantity                                                  | AppNumberDisplay | Auto Updated From GRN                        | Yes      | Yes       | Cannot Exceed Planned Quantity                                                     | Auto Update                  | Quantity Received         | Goods Received Against Delivery |
| Pending Quantity       | purchase_order_delivery>{delivery_id}>pending_quantity                                                   | AppNumberDisplay | Auto Calculated                              | Yes      | Yes       | Formula Validation                                                                 | Auto Calculate               | Remaining Quantity        | Quantity Yet To Be Received     |
| Priority               | purchase_order_delivery>{delivery_id}>priority                                                           | AppSelect        | Single Select (High, Medium, Low)            | No       | No        | Valid Priority Required                                                            | Update Delivery Rules        | Delivery Priority         | Delivery Importance Level       |
| Remarks                | purchase_order_delivery>{delivery_id}>remarks                                                            | AppTextArea      | Multi Line Text Area                         | No       | No        | Max 500 Characters                                                                 | Save Remarks                 | Additional Delivery Notes | Delivery Related Remarks        |
| Actions                | -                                                                                                        | AppActionMenu    | Edit, Duplicate, Delete Row                  | No       | No        | -                                                                                  | Execute Selected Action      | Delivery Schedule Actions | Manage Delivery Schedule Row    |

### 2.1.4. Section: Payment Terms

| Name/Label             | Data (Collection Name > Doc > Field Name)              | Type/Component     | Component Specific Information                        | Required | Read Only | Validations                        | On Change                 | Description                       | Tooltip                           |
|:---------------------- |:------------------------------------------------------ |:------------------ |:----------------------------------------------------- |:-------- |:--------- |:---------------------------------- |:------------------------- |:--------------------------------- |:--------------------------------- |
| Payment Terms Date     | purchase_orders>{po_id}>payment_terms_date             | AppDatePicker      | Single Date Picker                                    | Yes      | No        | Cannot Be Blank                    | Update Due Date           | Base Date For Payment Calculation | Payment Terms Effective Date      |
| Payment Basis          | purchase_orders>{po_id}>payment_basis                  | AppSelect          | Single Select (Invoice Date, GRN Date, Delivery Date) | Yes      | No        | Valid Option Required              | Recalculate Due Date      | Basis For Payment Calculation     | Select Payment Reference Date     |
| Credit Days            | purchase_orders>{po_id}>credit_days                    | AppNumberInput     | Integer Only                                          | Yes      | No        | Must Be Greater Than Or Equal To 0 | Recalculate Due Date      | Credit Period In Days             | Vendor Credit Period              |
| Advance Payment        | purchase_orders>{po_id}>advance_payment_percentage     | AppPercentageInput | Percentage Value                                      | No       | No        | 0 - 100%                           | Recalculate Payment Terms | Advance Payment Percentage        | Amount Payable In Advance         |
| Grace Days             | purchase_orders>{po_id}>grace_days                     | AppNumberInput     | Integer Only                                          | No       | No        | Must Be Greater Than Or Equal To 0 | Recalculate Due Date      | Additional Grace Period           | Extra Days Allowed After Due Date |
| Payment Mode           | purchase_orders>{po_id}>payment_mode                   | AppMultiSelect     | Cash, Bank Transfer, UPI, Cheque, NEFT, RTGS          | No       | No        | Valid Payment Mode Required        | Update Payment Rules      | Allowed Payment Methods           | Accepted Payment Modes            |
| TDS Applicable         | purchase_orders>{po_id}>tds_applicable                 | AppToggle          | Yes / No                                              | No       | No        | -                                  | Show/Hide TDS Fields      | Indicates TDS Deduction           | Is TDS Applicable?                |
| TDS Category           | purchase_orders>{po_id}>tds_category_id                | AppLookup          | Single Select, Source=TDS Master                      | No       | No        | Required If TDS Applicable         | Load TDS Percentage       | Applicable TDS Category           | Select TDS Category               |
| TDS Percentage         | purchase_orders>{po_id}>tds_percentage                 | AppPercentageInput | Auto Filled From TDS Category                         | No       | Yes       | 0 - 100%                           | Recalculate TDS Amount    | TDS Deduction Percentage          | Applicable TDS Rate               |
| Payment Due Date       | purchase_orders>{po_id}>payment_due_date               | AppDateDisplay     | Auto Calculated Field                                 | Yes      | Yes       | Formula Validation                 | Auto Calculate            | Final Payment Due Date            | Calculated Due Date               |
| Early Payment Discount | purchase_orders>{po_id}>early_payment_discount_enabled | AppToggle          | Yes / No                                              | No       | No        | -                                  | Show/Hide Discount Rules  | Enable Early Payment Discount     | Discount For Early Payment        |
| Late Payment Penalty   | purchase_orders>{po_id}>late_payment_penalty_enabled   | AppToggle          | Yes / No                                              | No       | No        | -                                  | Show/Hide Penalty Rules   | Enable Late Payment Penalty       | Penalty For Late Payment          |

### 2.1.5. Section: Attachments

| Name/Label | Data (Collection Name > Doc > Field Name)                     | Type/Component | Component Specific Information            | Required | Read Only | Validations              | On Change      | Description               | Tooltip                   |
|:---------- |:------------------------------------------------------------- |:-------------- |:----------------------------------------- |:-------- |:--------- |:------------------------ |:-------------- |:------------------------- |:------------------------- |
| Preview    | -                                                             | AppFileViewer  | PDF, Image, DOC Preview                   | No       | Yes       | -                        | Open Preview   | Preview Uploaded Document | View File Before Download |
| File Name  | purchase_orders>{po_id}>attachments>{attachment_id}>file_name | AppTextDisplay | Auto Generated From Uploaded File         | Yes      | Yes       | Cannot Be Blank          | Auto Populate  | Uploaded File Name        | Name Of Uploaded File     |
| File Type  | purchase_orders>{po_id}>attachments>{attachment_id}>file_type | AppBadge       | Auto Detected (PDF, JPG, PNG, XLSX, DOCX) | Yes      | Yes       | Supported File Type Only | Auto Detect    | Uploaded File Type        | Document Format           |
| Actions    | -                                                             | AppActionMenu  | Preview, Download, Delete                 | No       | No        | Permission Based Actions | Execute Action | Attachment Actions        | Manage Uploaded File      |

---

## 2.2. Right Side Widgets

### 2.2.1. Summary

Component to use: `Summary Card`

| Name           | Content                              | On Click | Tooltip / On Hover     |
|:-------------- |:------------------------------------ |:-------- |:---------------------- |
| Total Items    | count(distinct product_pack_id)      |          | -                      |
| Total Quantity | sum(quantity)                        |          | -                      |
| Product Value  | sum(product_value)                   |          | -                      |
| Tax Breakup    | sum(tax_amount) for individual taxes |          | -                      |
| Discount       | discount                             |          | -                      |
| Total PO Value | Product Value + Tax - Discount       |          | -                      |
| Advance Amount | advance_amount                       |          | -                      |
| Balance To Pay | Total PO Value - Advance Amount      |          | -                      |
| Delivery Date  | min(delivery_date)                   |          | Earliest Delivery Date |

### 2.2.2. Workflow Timeline

Component to use: `StageHistoryViewer`

| Name/Label | Data Source | Component                                        | Tooltip   |
|:---------- |:----------- |:------------------------------------------------ |:--------- |
| Stage      | stage_name  | Text                                             | -         |
| Set At     | set_on      | Text                                             | -         |
| Set by     | set_by      | Avatar                                           | User name |
| Remarks    | remarks     | Text                                             | -         |
| Attachment | url         | Pin icon. On click show attachment in Zoom mode. | -         |

### 2.2.3. Post Order Summary

Component to use: `Summary Card`

| Name                    | Content               | On Click | Tooltip / On Hover |
|:----------------------- |:--------------------- |:-------- |:------------------ |
| Total Quantity Received | sum(grn.quantity)     |          | -                  |
| Product Value Invoiced  | sum(pi.product_value) |          | -                  |
| Total Payment Made      | sum(payment.amount)   |          | -                  |

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
