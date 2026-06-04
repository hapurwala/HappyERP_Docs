# Happy ERP Purchase Order (PO) UI/UX Documentation

# Page Types

## 1. PO List Page

Purpose:

* View all purchase orders
* Search/filter orders
* Perform quick actions

---

## 2. Create/Edit PO Page

Purpose:

* Create purchase order
* Edit draft/submitted PO
* Manage line items
* Configure delivery/payment terms

---

# Navigation Flow

```text
PO List
↓
Create PO
↓
Submit
↓
Approval
↓
Ordered
↓
GRN
↓
Invoice
↓
Vendor Payment
```

---

# PO List Page UI

## Header Section

Components:

* Page title
* Breadcrumb
* Create PO button
* Import button
* Export dropdown

---

Summary Cards

Cards:

- Total POs
- Pending Approval
- Ordered
- Partial Received
- Completed
- Cancelled

---

## PO List Data Table

| Name            | Content                       | Type     | On Click                 | Card Placement | Tooltip / On Hover                                      |
|:--------------- |:----------------------------- |:-------- |:------------------------ | -------------- |:------------------------------------------------------- |
| PO Number       | Unique Purchase Order Number  | Text     | Open PO View / Edit Page |                | View Purchase Order Details                             |
| PO Date         | Purchase Order Creation Date  | Date     | –                        |                | Purchase Order Date                                     |
| Vendor          | Vendor Name                   | Text     | Open Vendor Profile      |                | View Vendor Details                                     |
| Total Amount    | Total PO Amount Including Tax | Currency | –                        |                | Total Purchase Order Value Breackup                     |
| Stage           | Current Workflow Stage        | Status   | Open Workflow Timeline   |                | Current PO Status with updatedBy, updatedAt and remarks |
| Delivery Status | Delivery Progress Status      | Status   | Open Delivery Details    |                | View Delivery Progress                                  |
| Payment Status  | Payment Progress Status       | Status   | Open Payment Summary     |                | View Payment Progress                                   |
| Created By      | User Who Created PO           | User     | Open User Profile        |                | Created By User                                         |
| Updated At      | Last Updated Date & Time      | DateTime | –                        |                | Last Modification Date                                  |

---

## PO List Table Features

| Feature        | Settings           |
|:-------------- |:------------------ |
| Column Resize  | Yes                |
| Column Pinning | Tes                |
| Sorting        | Yes                |
| Pagination     | Yes; Page Size: 20 |
| Row Selection  | No                 |
| Bulk Actions   | No                 |
| Export         | Yes                |
| Sticky Header  | Yes                |

---

#### Row Action Menu

| Name | Action | Visibility Criteria | Icon |

| Name               | Action                                                 | Visibility Criteria                                                 | Icon               | Tooltip |
| ------------------ | ------------------------------------------------------ | ------------------------------------------------------------------- | ------------------ | ------- |
| View               | Open View/Modify page                                  | - Vew Permission-<br/>- No Modify Permission (Due to Role or Stage) | eye                | -       |
| Modify             | Open View/Modify page                                  | - Modify permission- Modify allowed on stage                        | pencil             | –       |
| Modify Date/Number | Open page where user can change the date/number        | - Modify permission- Modify allowed on stage                        | calendar-edit      | –       |
| Status_1           | Set status to Status_1                                 | Allowed stage based on current stage                                | arrow-right-circle | –       |
| Status_2           | Set status to Status_2                                 | –                                                                   | arrow-right-circle |         |
| Undo Status        | Move back to last status                               | - Permission to set current status                                  | undo-2             | –       |
| Print              | Downloads PDF of the order in given format             | - Undo is allowed at this status                                    | printer            | –       |
| Cancel             | - Show confirmation message- On Yes, mark as cancelled | - Cancel permission- Cancel allowed on status                       | share-2            | <br>    |
| Delete             | - Show confirmation message- On Yes, delete brand      | - Delete permission- Delete allowed on status                       | trash-2            | –       |

---

## ## PO List Filters

| Name           | Type                    | Depends on  | Possible Values                                                                                       | Default Values   | Output             | Tooltip                             |
|:-------------- |:----------------------- |:----------- |:----------------------------------------------------------------------------------------------------- |:---------------- |:------------------ |:----------------------------------- |
| PO Number      | Text Search             | –           | Any PO Number                                                                                         | Blank            | po_number          | Search Purchase Order Number        |
| Vendor Type    | Lookup                  | –           | Manufacturer, Distributor, Wholesaler, Importer, Service Provider                                     | All              | vendor_type_ids    | Filter Vendors By Type              |
| Vendor         | Lookup                  | Vendor Type | Active Vendors Based On Selected Vendor Type                                                          | All              | vendor_ids         | Filter Purchase Orders By Vendor    |
| Date Range     | Date Range Picker       | –           | Any Date Range                                                                                        | Today            | from_date, to_date | Filter Purchase Orders By Date      |
| Stage          | Dropdown (Multi Select) | –           | Draft, Submitted, Approved, Ordered, Partial Received, Fully Received, Completed, Cancelled, Rejected | Draft, Submitted | stage_ids          | Filter By Workflow Stage            |
| Warehouse      | Lookup                  | –           | Active Warehouses                                                                                     | All              | warehouse_ids      | Filter Purchase Orders By Warehouse |
| Branch         | Lookup                  | –           | Active Branches                                                                                       | User Branch      | branch_ids         | Filter Purchase Orders By Branch    |
| Payment Status | Dropdown (Multi Select) | –           | Pending, Partially Paid, Paid, Overdue, Cancelled                                                     | All              | payment_status_ids | Filter By Payment Status            |
| Created By     | User Lookup             | –           | Active System Users                                                                                   | Current User     | user_ids           | Filter Records Created By User      |

# Create/Edit PO Page

# Page Layout

```text
Left:
- Main form

Right:
- Summary panel
- Workflow timeline
- Activity log
```

---

# Sticky Header Actions

Buttons:

* Save Draft
* Submit
* Approve
* Order
* Cancel
* Rollback
* Print
* Share PDF

---

## General Information

| Name/Label   | Data (Collection Name > Doc > Field Name) | Type/Component  | Component Specific Information                                                  | Required | Read Only | Validations    | On Change                     | Description                  | Tooltip                    |
|:------------ |:----------------------------------------- |:--------------- |:------------------------------------------------------------------------------- |:-------- |:--------- |:-------------- |:----------------------------- |:---------------------------- |:-------------------------- |
| PO Number    | purchase_orders>{po_id}>po_number         | AutoNumberInput | Prefix=PO, Auto Increment, Format=PO-YYYY-000001                                | Yes      | Yes       | Must Be Unique | Auto Generated                | Unique Purchase Order Number | System Generated PO Number |
| Vendor       | purchase_orders>{po_id}>vendor_id         | AppLookup       | Single Select, Searchable, Async Lookup, Source=active vendors                  | Yes      | No        | -              | Load Vendor Details: currency | Vendor Selection             | -                          |
| PO Date      | purchase_orders>{po_id}>po_date           | AppDatePicker   | Single Date Picker                                                              | Yes      | No        | -              | -                             | Purchase Order Date          | -                          |
| Currency     | purchase_orders>{po_id}>currency_id       | AppLookup       | Single Select, Searchable, Source=vendors active currencies                     | Yes      | No        | -              | Recalculate Amounts           | Transaction Currency         | -                          |
| Freight Type | purchase_orders>{po_id}>freight_type      | AppSelect       | Single Select, Options=Company Paid, Vendor Paid, Included In Rate, Third Party | Yes      | No        | -              | Update Freight Rules          | Freight Responsibility       | -                          |

## 

#### Line Items Features - Table

| Name/Label     | Data (Collection Name > Doc > Field Name) | Type/Component     | Component Specific Information                          | Required | Read Only | Validations                        | On Change                  | Description               | Tooltip                                                     |
|:-------------- |:----------------------------------------- |:------------------ |:------------------------------------------------------- |:-------- |:--------- |:---------------------------------- |:-------------------------- |:------------------------- |:----------------------------------------------------------- |
| Product        | purchase_order_items>{item_id}>product_id | AppLookup          | Single Select, Searchable, Async Lookup                 | Yes      | No        | Product Must Be Active             | Load Product Details       | Product Selection         | Select Product                                              |
| Variety        | variety_id                                | AppLookup          | Single Select, Filtered By Product                      | No       | No        | -                                  | Load Variety Details       | Product Variety           | Select Product Variety                                      |
| Pack           | pack_id                                   | AppLookup          | Single Select, Filtered By Product                      | No       | No        | Must Belong To Selected Product    | Update Quantity Conversion | Product Packaging         | -                                                           |
| Quantity       | quantity                                  | AppNumberInput     | Decimal Allowed, Min=0                                  | Yes      | No        | Quantity Must Be Greater Than Zero | Recalculate Amounts        | Ordered Quantity          | -                                                           |
| UOM            | uom_id                                    | Text               | Single Select, Auto Filled From Product                 | Yes      | Yes       | Valid UOM Required                 | Update Conversion Rules    | Unit Of Measurement       | -                                                           |
| Rate           | rate                                      | AppCurrencyInput   | Decimal Currency Input                                  | Yes      | No        | Rate Must Be Greater Than Zero     | Recalculate Amounts        | Purchase Rate             | -                                                           |
| Discount       | discount_amount                           | AppCurrencyInput   | Fixed Amount Or Percentage Or Unit                      | No       | No        | Discount Cannot Exceed Line Amount | Recalculate Amounts        | Line Discount             | -                                                           |
| Discount Type  | discount_type                             | AppSelect          | Single Select, Possible Values: Unit, Percentage, Fixed | Yes      | No        |                                    | Recalculate Amounts        |                           | -                                                           |
| Product Amount | product_amount                            | Text               |                                                         | Yes      | Yes       |                                    |                            |                           |                                                             |
| Tax            | tax_amount                                | AppTaxSelector     | Single/Multiple Tax Slabs                               | No       | No        | Valid Tax Configuration Required   | Recalculate Amounts        | Tax Amount                | Tax Breckup<br/>Tax 1<br/>Tax 2<br/>...                     |
| Total          | line_total                                | AppCurrencyDisplay | Auto Calculated Field                                   | Yes      | Yes       | Formula Validation                 | Auto Calculate             | Total Amount              | Amount Breckup <br/>Product Amount<br/>Tax Amount<br/>Total |
| Actions        | -                                         | AppActionMenu      | Edit, Duplicate, Delete Row                             | No       | No        | -                                  | Execute Selected Action    | Delivery Schedule Actions | -                                                           |

## Delivery Schedule Features - Table

| Name/Label             | Data (Collection Name > Doc > Field Name)                    | Type/Component   | Component Specific Information               | Required | Read Only | Validations                                                                        | On Change                    | Description               | Tooltip                         |
|:---------------------- |:------------------------------------------------------------ |:---------------- |:-------------------------------------------- |:-------- |:--------- |:---------------------------------------------------------------------------------- |:---------------------------- |:------------------------- |:------------------------------- |
| Warehouse              | purchase_order_delivery>{delivery_id}>warehouse_id           | AppLookup        | Single Select, Searchable, Source=warehouses | Yes      | No        | Must Be Active Warehouse                                                           | Load Warehouse Locations     | Receiving Warehouse       | Select Delivery Warehouse       |
| Location               | purchase_order_delivery>{delivery_id}>location_id            | AppLookup        | Single Select, Filtered By Warehouse         | Yes      | No        | Must Belong To Selected Warehouse                                                  | Load Location Details        | Warehouse Location        | Select Storage Location         |
| Quantity               | purchase_order_delivery>{delivery_id}>quantity               | AppNumberInput   | Decimal Allowed, Min=0                       | Yes      | No        | Quantity Must Be Greater Than Zero, Total Delivery Quantity Must Match PO Quantity | Recalculate Pending Quantity | Delivery Quantity         | Quantity Planned For Delivery   |
| Expected Delivery Date | purchase_order_delivery>{delivery_id}>expected_delivery_date | AppDatePicker    | Single Date Picker                           | Yes      | No        | Must Be Greater Than Or Equal To PO Date                                           | Update Delivery Schedule     | Planned Delivery Date     | Expected Goods Arrival Date     |
| Delivery Status        | purchase_order_delivery>{delivery_id}>delivery_status        | AppStatusBadge   | Auto Updated By GRN                          | Yes      | Yes       | Valid Workflow Status                                                              | Auto Update                  | Delivery Progress Status  | Pending / Partial / Completed   |
| Received Quantity      | purchase_order_delivery>{delivery_id}>received_quantity      | AppNumberDisplay | Auto Updated From GRN                        | Yes      | Yes       | Cannot Exceed Planned Quantity                                                     | Auto Update                  | Quantity Received         | Goods Received Against Delivery |
| Pending Quantity       | purchase_order_delivery>{delivery_id}>pending_quantity       | AppNumberDisplay | Auto Calculated                              | Yes      | Yes       | Formula Validation                                                                 | Auto Calculate               | Remaining Quantity        | Quantity Yet To Be Received     |
| Priority               | purchase_order_delivery>{delivery_id}>priority               | AppSelect        | Single Select (High, Medium, Low)            | No       | No        | Valid Priority Required                                                            | Update Delivery Rules        | Delivery Priority         | Delivery Importance Level       |
| Remarks                | purchase_order_delivery>{delivery_id}>remarks                | AppTextArea      | Multi Line Text Area                         | No       | No        | Max 500 Characters                                                                 | Save Remarks                 | Additional Delivery Notes | Delivery Related Remarks        |
| Actions                | -                                                            | AppActionMenu    | Edit, Duplicate, Delete Row                  | No       | No        | -                                                                                  | Execute Selected Action      | Delivery Schedule Actions | Manage Delivery Schedule Row    |

---

## Payment Terms Features

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

---

## Attachments Features Table

| Name/Label | Data (Collection Name > Doc > Field Name)                     | Type/Component | Component Specific Information            | Required | Read Only | Validations              | On Change      | Description               | Tooltip                   |
|:---------- |:------------------------------------------------------------- |:-------------- |:----------------------------------------- |:-------- |:--------- |:------------------------ |:-------------- |:------------------------- |:------------------------- |
| Preview    | -                                                             | AppFileViewer  | PDF, Image, DOC Preview                   | No       | Yes       | -                        | Open Preview   | Preview Uploaded Document | View File Before Download |
| File Name  | purchase_orders>{po_id}>attachments>{attachment_id}>file_name | AppTextDisplay | Auto Generated From Uploaded File         | Yes      | Yes       | Cannot Be Blank          | Auto Populate  | Uploaded File Name        | Name Of Uploaded File     |
| File Type  | purchase_orders>{po_id}>attachments>{attachment_id}>file_type | AppBadge       | Auto Detected (PDF, JPG, PNG, XLSX, DOCX) | Yes      | Yes       | Supported File Type Only | Auto Detect    | Uploaded File Type        | Document Format           |
| Actions    | -                                                             | AppActionMenu  | Preview, Download, Delete                 | No       | No        | Permission Based Actions | Execute Action | Attachment Actions        | Manage Uploaded File      |

---

## Workflow Timeline Features

| Name/Label     | Data (Collection Name > Doc > Field Name) | Type/Component  | Component Specific Information   | Required | Read Only | Validations | On Change          | Description                     | Tooltip                |
|:-------------- |:----------------------------------------- |:--------------- |:-------------------------------- |:-------- |:--------- |:----------- |:------------------ |:------------------------------- |:---------------------- |
| Stage History  | workflow_logs>{log_id}                    | AppTimeline     | Vertical Timeline View           | No       | Yes       | -           | Refresh Timeline   | Complete Workflow Stage History | All Workflow Movements |
| Approval Logs  | workflow_logs>{log_id}                    | AppLogViewer    | Approval/Rejection Audit Records | No       | Yes       | -           | Refresh Logs       | Approval/Rejection Records      | Approval Tracking      |
| User Activity  | activity_logs>{log_id}                    | AppActivityFeed | User Activity Feed               | No       | Yes       | -           | Refresh Activities | User Action History             | User Audit Trail       |
| Audit Tracking | audit_logs>{log_id}                       | AppAuditLog     | System Audit Records             | No       | Yes       | -           | Refresh Audit Log  | System Audit Information        | Compliance Tracking    |

---

# Line Item Features

Features:

* Keyboard navigation
* Auto calculation
* Duplicate row
* Product quick preview
* Batch add
* Barcode scan
* Dynamic row validation

---

# Workflow Timeline UI

Timeline should show:

* Stage name
* User
* Date/time
* Remarks
* System action
* Approval notes

---

# Right Sidebar Widgets

Widgets:

* Vendor Balance
* Pending Deliveries
* Linked GRNs
* Linked Invoices
* Payment Summary
* Vendor Rating

---

# Notification UI

Notifications:

* Approval pending
* Delivery delayed
* Payment overdue
* GRN created
* Invoice created

---

# Validation UX

## Real-Time Validation

Validate:

* Duplicate product
* Quantity mismatch
* Tax mismatch
* Required fields
* Budget exceeded

---

# Error Handling

Show:

* Inline errors
* Toast notifications
* Modal confirmations

---

# Responsive Design

## Desktop

* Multi-column layout
* Side summary panel

---

## Tablet

* Collapsible side panels

---

## Mobile

* Tab accordion layout
* Floating action buttons

---

# Modal/Dialog Screens

Required Modals:

* Approve PO
* Reject PO
* Rollback PO
* Cancel PO
* Share PO
* Print Preview

---

# Reports Integration

Quick Links:

* PO Report
* Vendor Ledger
* Pending PO
* GRN Report

---

# Performance Guidelines

* Lazy load tables
* Virtual scrolling
* Pagination
* Cached lookups
* Debounced search

---

# Recommended UI Components

## Components

* DataGrid
* Drawer
* Tabs
* Stepper
* Timeline
* Summary Cards
* Lookup Dialog
* Attachment Viewer
* Activity Feed

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

# Final Recommendation

PO UI should behave like:

* workflow dashboard
* procurement workspace
* approval engine
* inventory coordination screen
* financial preparation module

not just a normal CRUD form.
