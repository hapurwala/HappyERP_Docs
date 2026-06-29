# HappyERP Purchase Order (PO) UI/UX Documentation

This document gives UI details of all pages used in Purchase Order module.

# Pages

| S. No. | Name                  | Type  | Purpose                                                                                                         |
| ------ | --------------------- | ----- | --------------------------------------------------------------------------------------------------------------- |
| 1.     | PO List               | Page  | - View all purchase orders<br/>- Search/filter purchase orders<br/>- Perform quick actions on purchase order(s) |
| 2.     | Add/Modify PO         | Page  | - View details of the purchase order<br/>- Add/modify any data of current purchase order                        |
| 3.     | Add/Modify PO Product | Popup | - Add/modify data (product, quantity, rate etc) of product added in purchase order                              |
| 4.     | Print Preview         | Page  |                                                                                                                 |

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

#### 1.2a. DataTable (PO List) - Columns

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

### 1.2b. DataTable (PO List) - Toolbar Config

| Feature          | Settings                    | On Click                                                                               |
|:---------------- |:--------------------------- |:-------------------------------------------------------------------------------------- |
| Search           | Yes                         | Search PO records based on the entered keyword(s).                                     |
| View Toggle      | Yes                         | Switch between available DataTable view layouts.                                       |
| Column Selection | Yes                         | Open column selector to show or hide DataTable columns.                                |
| Group by         | Yes                         | Group DataTable records based on the selected column.                                  |
| Filter           | Yes                         | Open filter panel to apply additional filtering criteria.                              |
| Export           | Yes<br/>Filename: `po-list` | Export the displayed DataTable records using the configured filename `po-list`.        |
| Share            | Yes                         | Generate and share the current DataTable view, including applied filters and settings. |
| Full Screen      | Yes                         | Toggle the DataTable between normal and full-screen view.                              |
| Add              | Yes<br/>Page to open: `po`  | Open the `po` page in create mode to add a new Purchase Order.                         |

### 1.2c. DataTable (PO List) - Config

| Feature        | Settings              |
|:-------------- |:--------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

### 1.2d. DataTable (PO List) - RowAction Menu

| Name               | Action                                                      | Visibility Criteria                                                                            | Icon               | Tooltip                                               |
| ------------------ | ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------- | ------------------ | ----------------------------------------------------- |
| View               | Open page: `po`                                             | - View Permission<br/>- No Modify Permission (Due to Role or Stage)                            | eye                | View Purchase Order                                   |
| Modify             | Open page: `po`                                             | - Modify permission<br/>- Modify allowed on stage                                              | pencil             | Modify Purchase Order                                 |
| Modify Date/Number | Open page: `modifyDocNumber`                                | - Modify permission<br/>- Modify allowed on stage                                              | calendar-edit      | Modify PO Date or Number                              |
| Create GRN         | Open page: `grn` with current PO pre-selected               | - Create GRN Permission<br/>- PO Stage = Ordered / Partial Received<br/>- Pending Quantity > 0 | package-check      | Create Goods Receipt Note against this Purchase Order |
| Set as <Stage_1>   | Set stage to Stage_1                                        | Allowed stage based on current stage                                                           | arrow-right-circle | Change Workflow Stage                                 |
| Set as <Stage_2>   | Set stage to Stage_2                                        | –                                                                                              | arrow-right-circle | Change Workflow Stage                                 |
| Undo Stage         | Move back to last stage                                     | - Permission to rollback from current status                                                   | undo-2             | Rollback Current Stage                                |
| Print              | Downloads PDF of the order in given format                  | - Print permission                                                                             | printer            | Print Purchase Order                                  |
| Duplicate          | Create a copy of the PO. Stage will be set to `Draft`.      | - Add Permission                                                                               | copy               | Create Duplicate Purchase Order                       |
| Cancel             | - Show confirmation message<br/>- On Yes, mark as cancelled | - Cancel permission<br/>- Cancel allowed on current stage                                      | share-2            | Cancel Purchase Order                                 |
| Delete             | - Show confirmation message<br/>- On Yes, delete PO         | - Delete permission<br/>- Delete allowed on current stage                                      | trash-2            | Delete Purchase Order                                 |

### 1.2e. DataTable (PO List) - Filters Fields

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

### 2.1.1. Section (General Information)

| Name/Label   | Data Source  | Type/Component | Component Specific Information                                                  | Required | Read Only | Validations    | On Change                     | Description                  | Tooltip                    |
|:------------ |:------------ |:-------------- |:------------------------------------------------------------------------------- |:-------- |:--------- |:-------------- |:----------------------------- |:---------------------------- |:-------------------------- |
| PO Number    | po_number    | Text           | Prefix=PO, Auto Increment, Format=PO-YYYY-000001                                | Yes      | Yes       | Must Be Unique | Auto Generated                | Unique Purchase Order Number | System Generated PO Number |
| Vendor       | vendor_id    | Select         | Single Select, Searchable, Async Lookup, Source=active vendors                  | Yes      | No        | -              | Load Vendor Details: currency | Vendor Selection             | -                          |
| PO Date      | po_date      | DatePicker     | Single Date Picker                                                              | Yes      | No        | -              | -                             | Purchase Order Date          | -                          |
| Currency     | currency_id  | Select         | Single Select, Searchable, Source=vendors active currencies                     | Yes      | No        | -              | Recalculate Amounts           | Transaction Currency         | -                          |
| Freight Type | freight_type | Select         | Single Select, Options=Company Paid, Vendor Paid, Included In Rate, Third Party | Yes      | No        | -              | Update Freight Rules          | Freight Responsibility       | -                          |
| Remarks      | remarks      | RichTextBox    |                                                                                 | No       | No        | -              | -                             | Remarks                      | -                          |

### 2.1.2. Section (Product Details)

This section displays list of all products given in current PO.

| Name/Label      | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations | On Change | Description | Tooltip |
|:--------------- |:----------- |:-------------- |:------------------------------ |:-------- |:--------- |:----------- |:--------- |:----------- |:------- |
| Barcode Scan    | -           | Input          |                                | No       | No        |             | -         |             |         |
| Product Details | product     | DataTable      |                                | Yes      | No        |             |           |             |         |

### 2.1.2a. DataTable (Product Details) - Columns

| Name/Label                     | Data Source                                                     | Type/Component | Component Specific Information                          | Required | Read Only | Validations                                    | On Change            | Description              | Tooltip                                                     |
|:------------------------------ |:--------------------------------------------------------------- |:-------------- |:------------------------------------------------------- |:-------- |:--------- |:---------------------------------------------- |:-------------------- |:------------------------ |:----------------------------------------------------------- |
| Product                        | product_id                                                      | Select         | Single Select, Searchable, Async Lookup                 | Yes      | No        | Product Must Be Active                         | Load Product Details | Product Selection        | Select Product                                              |
| Variety                        | variety_id                                                      | Select         | Single Select, Filtered By Product                      | No       | No        | -                                              | Load Variety Details | Product Variety          | Select Product Variety                                      |
| Quantity                       | quantity + uom_id                                               | Input          | Suffix: uom_short_name; Decimal Allowed, Min=0          | Yes      | No        | Quantity Must Be Greater Than Zero             | Recalculate Amounts  | Ordered Quantity         | -                                                           |
| Rate                           | rate                                                            | Input          | Decimal Currency Input                                  | Yes      | No        | Rate Must Be Greater Than Zero                 | Recalculate Amounts  | Purchase Rate            | -                                                           |
| Discount                       | discount                                                        | Input          | Fixed Amount Or Percentage Or Unit                      | No       | No        | Discount Cannot Exceed Line Amount             | Recalculate Amounts  | Line Discount            | -                                                           |
| Discount Type                  | discount_type                                                   | Select         | Single Select, Possible Values: Unit, Percentage, Fixed | Yes      | No        |                                                | Recalculate Amounts  |                          | -                                                           |
| Product  Amount After Discount | product_amount = product_value_before_discount - discount_value | Text           | -                                                       | Yes      | Yes       | product_value_before_discount - discount_value | Recalculate Amounts  | Calculate after discount | Discounted Value                                            |
| Tax                            | tax_amount <br/>= SUM (taxes[].`tax_value`)                     | Text           | -                                                       | No       | No        | Valid Tax Configuration Required               | Recalculate Amounts  | Tax Amount               | Tax Breckup<br/>Tax 1<br/>Tax 2<br/>...                     |
| Total                          | line_total<br/>product_value_after_discount + tax_value         | Text           | -                                                       | Yes      | Yes       | Formula Validation                             | Auto Calculate       | Total Amount             | Amount Breckup <br/>Product Amount<br/>Tax Amount<br/>Total |

### 2.1.2b. DataTable (Product Details) - Toolbar Config

| Feature          | Settings                           | On Click                                                               |
|:---------------- |:---------------------------------- |:---------------------------------------------------------------------- |
| Search           | No                                 | N/A                                                                    |
| View Toggle      | No                                 | N/A                                                                    |
| Column Selection | No                                 | N/A                                                                    |
| Group by         | No                                 | N/A                                                                    |
| Filter           | No                                 | N/A                                                                    |
| Export           | No                                 | N/A                                                                    |
| Share            | No                                 | N/A                                                                    |
| Full Screen      | Yes                                | Toggle the DataTable between normal and full-screen view.              |
| Add              | Yes<br/>Page to open: `po_product` | Open the `po_product` page in create mode to add a new product record. |

### 2.1.2c. DataTable (Product Details) - Config

| Feature        | Settings              |
|:-------------- |:--------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

### 2.1.2d. DataTable (Product Details) - RowAction Menu

| Name             | Action                                                      | Visibility Criteria                                       | Icon               | Tooltip |
| ---------------- | ----------------------------------------------------------- | --------------------------------------------------------- | ------------------ | ------- |
| Modify           | Open page: `po_product`                                     | - Modify permission<br/>- Modify allowed on stage         | pencil             | –       |
| Set as <Stage_1> | Set stage to Stage_1                                        | Allowed stage based on current stage                      | arrow-right-circle | –       |
| Set as <Stage_2> | Set stage to Stage_2                                        | –                                                         | arrow-right-circle |         |
| Undo Stage       | Move back to last stage                                     | - Permission to rollback from current status              | undo-2             | –       |
| Duplicate        | Create a copy of the PO. Stage will be set to `Draft`.      | - Add Permission                                          |                    | -       |
| Cancel           | - Show confirmation message<br/>- On Yes, mark as cancelled | - Cancel permission<br/>- Cancel allowed on current stage | share-2            | -       |
| Delete           | - Show confirmation message<br/>- On Yes, delete brand      | - Delete permission<br/>- Delete allowed on current stage | trash-2            | –       |

### 2.1.3a. DataTable (Delivery Schedule) - Columns

| Name/Label             | Data Source                                                                                              | Type/Component   | Component Specific Information    | Required | Read Only | Validations                                                                        | On Change                           | Description               | Tooltip                         |
|:---------------------- |:-------------------------------------------------------------------------------------------------------- |:---------------- |:--------------------------------- |:-------- |:--------- |:---------------------------------------------------------------------------------- |:----------------------------------- |:------------------------- |:------------------------------- |
| Product Pack Name      | product_pack_id                                                                                          | Select           |                                   | Yes      | No        |                                                                                    |                                     |                           | Product Pack name               |
| Branch Name            | branch_id                                                                                                | Select           |                                   | No       | No        | if party selected so can't select branch                                           | on select party should be disabled  |                           | Branch Id                       |
| Party Name             | party_id                                                                                                 | Select           |                                   | No       | No        | if branch selected so can't select party                                           | on select branch should be disabled |                           | Party Id                        |
| Quantity               | quantity                                                                                                 | Input            | Decimal Allowed, Min=0            | Yes      | No        | Quantity Must Be Greater Than Zero, Total Delivery Quantity Must Match PO Quantity | Recalculate Pending Quantity        | Delivery Quantity         | Quantity Planned For Delivery   |
| Expected Delivery Date | expected_delivery_date                                                                                   | DatePicker       | Single Date Picker                | Yes      | No        | Must Be Greater Than Or Equal To PO Date                                           | Update Delivery Schedule            | Planned Delivery Date     | Expected Goods Arrival Date     |
| Delivery Status        | If received_quantity = 0 => **Pending**<br/>If pending_quantity = 0 => **Complete**<br/>Else **Partial** | AppStatusBadge   | Auto Updated By GRN               | Yes      | Yes       | Valid Workflow Status                                                              | Auto Update                         | Delivery Progress Status  | Pending / Partial / Completed   |
| Received Quantity      | received_quantity                                                                                        | AppNumberDisplay | Auto Updated From GRN             | Yes      | Yes       | Cannot Exceed Planned Quantity                                                     | Auto Update                         | Quantity Received         | Goods Received Against Delivery |
| Pending Quantity       | pending_quantity                                                                                         | AppNumberDisplay | Auto Calculated                   | Yes      | Yes       | Formula Validation                                                                 | Auto Calculate                      | Remaining Quantity        | Quantity Yet To Be Received     |
| Priority               | priority                                                                                                 | AppSelect        | Single Select (High, Medium, Low) | No       | No        | Valid Priority Required                                                            | Update Delivery Rules               | Delivery Priority         | Delivery Importance Level       |
| Remarks                | remarks                                                                                                  | AppTextArea      | Multi Line Text Area              | No       | No        | Max 500 Characters                                                                 | Save Remarks                        | Additional Delivery Notes | Delivery Related Remarks        |

### 2.1.3b. DataTable (Delivery Schedule) - Toolbar Config

| Feature          | Settings                                     | On Click                                                                                   |
|:---------------- |:-------------------------------------------- |:------------------------------------------------------------------------------------------ |
| Search           | No                                           | N/A                                                                                        |
| View Toggle      | No                                           | N/A                                                                                        |
| Column Selection | No                                           | N/A                                                                                        |
| Group by         | No                                           | N/A                                                                                        |
| Filter           | No                                           | N/A                                                                                        |
| Export           | No                                           | N/A                                                                                        |
| Share            | No                                           | N/A                                                                                        |
| Full Screen      | Yes                                          | Toggle the DataTable between normal and full-screen view.                                  |
| Add              | Yes<br/>Page to open: `po_delivery_schedule` | Open the `po_delivery_schedule` page in create mode to add a new delivery schedule record. |

### 2.1.3c. DataTable (Delivery Schedule) - Config

| Feature        | Settings              |
|:-------------- |:--------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

### 2.1.3d. DataTable (Delivery Schedule) - RowAction Menu

| Name      | Action                                               | Visibility Criteria                                                                    | Icon    | Tooltip |
|:--------- |:---------------------------------------------------- |:-------------------------------------------------------------------------------------- |:------- |:------- |
| Modify    | Open page: `po_delivery_schedule`                    | - Modify permission<br/>- Modify allowed on stage                                      | pencil  | -       |
| Duplicate | Create a copy of current delivery schedule row       | - Add Permission                                                                       | copy    | -       |
| Delete    | - Show confirmation message<br/>- On Yes, delete row | - Delete permission<br/>- Delete allowed on current stage<br/>- recieved_quantity == 0 | trash-2 | -       |

### 2.1.4. Section (Payment Terms)

| Name/Label             | Data Source       | Type/Component     | Component Specific Information               | Required | Read Only | Validations                        | On Change              | Description                       | Tooltip                       |
|:---------------------- |:----------------- |:------------------ |:-------------------------------------------- |:-------- |:--------- |:---------------------------------- |:---------------------- |:--------------------------------- |:----------------------------- |
| Payment Mode           | payment_mode      | AppMultiSelect     | Cash, Bank Transfer, UPI, Cheque, NEFT, RTGS | No       | No        | Valid Payment Mode Required        | Update Payment Rules   | Allowed Payment Methods           | Accepted Payment Modes        |
| Base Date              | base_date         | AppSelect          | Invoice Date, Delivery Date                  | Yes      | No        | Valid Option Required              | Recalculate Due Date   | Base Date For Payment Calculation | Select Payment Reference Date |
| Credit Days            | credit_days       | AppNumberInput     | Integer Only                                 | Yes      | No        | Must Be Greater Than Or Equal To 0 | Recalculate Due Date   | Credit Period In Days             | Vendor Credit Period          |
| TDS Applicable         | tds_applicable    | AppToggle          | Yes / No                                     | No       | No        | -                                  | Show/Hide TDS Fields   | Indicates TDS Deduction           | Is TDS Applicable?            |
| TDS Category           | tds_category_id   | AppLookup          | Single Select, Source=TDS Master             | No       | No        | Required If TDS Applicable         | Load TDS Percentage    | Applicable TDS Category           | Select TDS Category           |
| TDS Category Name      | tds_category_name | AppLookup          |                                              | No       | No        | Required If TDS Applicable         |                        |                                   |                               |
| TDS Percentage         | tds_percentage    | AppPercentageInput | Auto Filled From TDS Category                | No       | Yes       | 0 - 100%                           | Recalculate TDS Amount | TDS Deduction Percentage          | Applicable TDS Rate           |
| Payment schedule       |                   | DataTable          |                                              |          |           |                                    |                        |                                   |                               |
| Early Payment Discount |                   | DataTable          |                                              |          |           |                                    |                        |                                   |                               |
| Late Payment Panelty   |                   | DataTable          |                                              |          |           |                                    |                        |                                   |                               |

### 2.1.4.1a. DataTable (Payment Schedule) - Columns

| Name/Label  | Data Source | Type/Component     | Component Specific Information          | Required | Read Only | Validations                        | On Change            | Description               | Tooltip |
|:----------- |:----------- |:------------------ |:--------------------------------------- |:-------- |:--------- |:---------------------------------- |:-------------------- |:------------------------- |:------- |
| Base Date   | base_date   | AppSelect          | Invoice Date, Delivery Date, Order Date | Yes      | No        | Valid Option Required              | Recalculate Due Date | Payment Reference Date    | -       |
| Days Within | days_within | AppNumberInput     | Integer Only                            | Yes      | No        | Must Be Greater Than Or Equal To 0 | Recalculate Due Date | Days From Base Date       | -       |
| Due Date    | due_date    | AppDateDisplay     | Auto Calculated                         | Yes      | Yes       | Formula Validation                 | Auto Calculate       | Payment Due Date          | -       |
| Percentage  | percent     | AppPercentageInput | Percentage Value                        | Yes      | No        | 0 - 100%                           | Recalculate Amount   | Percentage Of Order Value | -       |
| Amount      | amount      | AppCurrencyDisplay | Auto Calculated / Editable              | Yes      | No        | Must Be Greater Than Or Equal To 0 | Recalculate Schedule | Payment Amount            | -       |

### 2.1.4.1b. DataTable (Payment Schedule) - Toolbar Config

| Feature          | Settings                                    | On Click                                                                                 |
|:---------------- |:------------------------------------------- |:---------------------------------------------------------------------------------------- |
| Search           | No                                          | N/A                                                                                      |
| View Toggle      | No                                          | N/A                                                                                      |
| Column Selection | No                                          | N/A                                                                                      |
| Group by         | No                                          | N/A                                                                                      |
| Filter           | No                                          | N/A                                                                                      |
| Export           | No                                          | N/A                                                                                      |
| Share            | No                                          | N/A                                                                                      |
| Full Screen      | Yes                                         | Toggle the DataTable between normal and full-screen view.                                |
| Add              | Yes<br/>Page to open: `po_payment_schedule` | Open the `po_payment_schedule` page in create mode to add a new payment schedule record. |

### 2.1.4.1c. DataTable (Payment Schedule) - Config

| Feature        | Settings              |
|:-------------- |:--------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

### 2.1.4.1d. DataTable (Payment Schedule) - RowAction Menu

| Name      | Action                                | Visibility Criteria | Icon    | Tooltip |
|:--------- |:------------------------------------- |:------------------- |:------- |:------- |
| Modify    | Open page: `po_payment_schedule`      | Modify Permission   | pencil  | -       |
| Duplicate | Create a copy of payment schedule row | Add Permission      | copy    | -       |
| Delete    | Delete payment schedule row           | Delete Permission   | trash-2 | -       |

## 2.1.4.2. Early Payment Discount

### 2.1.4.2a. DataTable (Early Payment Discount) - Columns

| Name/Label         | Data Source | Type/Component   | Component Specific Information          | Required | Read Only | Validations                        | On Change                 | Description                             | Tooltip                     |
|:------------------ |:----------- |:---------------- |:--------------------------------------- |:-------- |:--------- |:---------------------------------- |:------------------------- |:--------------------------------------- |:--------------------------- |
| Base Date          | base_date   | AppSelect        | Invoice Date, Delivery Date, Order Date | Yes      | No        | Valid Option Required              | Recalculate Discount Date | Reference Date For Discount Calculation | Select Discount Base Date   |
| Days Within        | days_within | AppNumberInput   | Integer Only                            | Yes      | No        | Must Be Greater Than Or Equal To 0 | Recalculate Discount Date | Days Within Base Date                   | Discount Eligibility Period |
| Discount Type      | type        | AppSelect        | Per Unit, Percentage, Fixed             | Yes      | No        | Valid Option Required              | Recalculate Discount      | Discount Calculation Method             | Select Discount Type        |
| Discount Rate      | rate        | AppNumberInput   | Decimal Allowed                         | Yes      | No        | Must Be Greater Than Or Equal To 0 | Recalculate Discount      | Discount Value                          | Enter Discount Value        |
| Max Discount Value | max_value   | AppCurrencyInput | Decimal Currency Input                  | No       | No        | Must Be Greater Than Or Equal To 0 | Recalculate Discount      | Maximum Discount Allowed                | Upper Limit For Discount    |

### 2.1.4.2b. DataTable (Early Payment Discount) - Toolbar Config

| Feature          | Settings                                    | On Click                                                                                       |
|:---------------- |:------------------------------------------- |:---------------------------------------------------------------------------------------------- |
| Search           | No                                          | N/A                                                                                            |
| View Toggle      | No                                          | N/A                                                                                            |
| Column Selection | No                                          | N/A                                                                                            |
| Group by         | No                                          | N/A                                                                                            |
| Filter           | No                                          | N/A                                                                                            |
| Export           | No                                          | N/A                                                                                            |
| Share            | No                                          | N/A                                                                                            |
| Full Screen      | Yes                                         | Toggle the DataTable between normal and full-screen view.                                      |
| Add              | Yes<br/>Page to open: `po_payment_discount` | Open the `po_payment_discount` page in create mode to add a new early payment discount record. |

### 2.1.4.2c. DataTable (Early Payment Discount) - Config

| Feature        | Settings              |
|:-------------- |:--------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

### 2.1.4.2d. DataTable (Early Payment Discount) - RowAction Menu

| Name      | Action                                               | Visibility Criteria                                       | Icon    | Tooltip |
|:--------- |:---------------------------------------------------- |:--------------------------------------------------------- |:------- |:------- |
| Modify    | Open page: `po_payment_discount`                     | - Modify permission<br/>- Modify allowed on stage         | pencil  | -       |
| Duplicate | Create a copy of current discount rule               | - Add Permission                                          | copy    | -       |
| Delete    | - Show confirmation message<br/>- On Yes, delete row | - Delete permission<br/>- Delete allowed on current stage | trash-2 | -       |

---

## 2.1.4.3. Late Payment Penalty

### 2.1.4.3a. DataTable (Late Payment Penalty) - Columns

| Name/Label        | Data Source | Type/Component   | Component Specific Information                    | Required | Read Only | Validations                        | On Change                | Description                            | Tooltip                  |
|:----------------- |:----------- |:---------------- |:------------------------------------------------- |:-------- |:--------- |:---------------------------------- |:------------------------ |:-------------------------------------- |:------------------------ |
| Base Date         | base_date   | AppSelect        | Due Date, Invoice Date, Delivery Date, Order Date | Yes      | No        | Valid Option Required              | Recalculate Penalty Date | Reference Date For Penalty Calculation | Select Penalty Base Date |
| Days After        | days_after  | AppNumberInput   | Integer Only                                      | Yes      | No        | Must Be Greater Than Or Equal To 0 | Recalculate Penalty Date | Days After Base Date                   | Penalty Grace Period     |
| Penalty Type      | type        | AppSelect        | Per Unit, Percentage, Fixed                       | Yes      | No        | Valid Option Required              | Recalculate Penalty      | Penalty Calculation Method             | Select Penalty Type      |
| Penalty Rate      | rate        | AppNumberInput   | Decimal Allowed                                   | Yes      | No        | Must Be Greater Than Or Equal To 0 | Recalculate Penalty      | Penalty Value                          | Enter Penalty Value      |
| Max Penalty Value | max_value   | AppCurrencyInput | Decimal Currency Input                            | No       | No        | Must Be Greater Than Or Equal To 0 | Recalculate Penalty      | Maximum Penalty Allowed                | Upper Limit For Penalty  |

### 2.1.4.3b. DataTable (Late Payment Penalty) - Toolbar Config

| Feature          | Settings                                   | On Click                                                                                    |
|:---------------- |:------------------------------------------ |:------------------------------------------------------------------------------------------- |
| Search           | No                                         | N/A                                                                                         |
| View Toggle      | No                                         | N/A                                                                                         |
| Column Selection | No                                         | N/A                                                                                         |
| Group by         | No                                         | N/A                                                                                         |
| Filter           | No                                         | N/A                                                                                         |
| Export           | No                                         | N/A                                                                                         |
| Share            | No                                         | N/A                                                                                         |
| Full Screen      | Yes                                        | Toggle the DataTable between normal and full-screen view.                                   |
| Add              | Yes<br/>Page to open: `po_payment_penalty` | Open the `po_payment_penalty` page in create mode to add a new late payment penalty record. |

### 2.1.4.3c. DataTable (Late Payment Penalty) - Config

| Feature        | Settings              |
|:-------------- |:--------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

### 2.1.4.3d. DataTable (Late Payment Penalty) - RowAction Menu

| Name      | Action                                               | Visibility Criteria                                       | Icon    | Tooltip |
|:--------- |:---------------------------------------------------- |:--------------------------------------------------------- |:------- |:------- |
| Modify    | Open page: `po_payment_penalty`                      | - Modify permission<br/>- Modify allowed on stage         | pencil  | -       |
| Duplicate | Create a copy of current penalty rule                | - Add Permission                                          | copy    | -       |
| Delete    | - Show confirmation message<br/>- On Yes, delete row | - Delete permission<br/>- Delete allowed on current stage | trash-2 | -       |

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

-
