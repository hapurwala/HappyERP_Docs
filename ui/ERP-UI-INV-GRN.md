# HappyERP Goods Receipt Note (GRN) UI/UX Documentation

This document gives UI details of all pages used in Goods Receipt Note (GRN) module.

# Pages

| S. No. | Name                   | Type  | Purpose                                                                        |
| ------ | ---------------------- | ----- | ------------------------------------------------------------------------------ |
| 1.     | GRN List               | Page  | - View all GRNs<br/>- Search/filter GRNs<br/>- Perform quick actions on GRN(s) |
| 2.     | Add/Modify GRN         | Page  | - View details of the GRN<br/>- Add/modify any data of current GRN             |
| 3.     | Add/Modify GRN Product | Popup | - Add/modify received product details in current GRN                           |

# 1. GRN List Page UI

This page shows list of GRNs in tabular format. The GRNs appearing in the list depend upon current user's permission and applied filters.

## 1.1. GRN List Data Table

### 1.2a. DataTable (GRN List) - Columns

| Name           | Content                          | Type     | On Click                | Card Placement | Tooltip / On Hover              |
|:-------------- |:-------------------------------- |:-------- |:----------------------- |:-------------- |:------------------------------- |
| GRN Number     | Unique GRN Number                | Text     | Open GRN View/Edit Page | Title          | View Goods Receipt Note Details |
| GRN Date       | Goods Receipt Date               | Date     | –                       | Subtitle       | Goods Receipt Date              |
| Vendor         | Vendor Name                      | Text     | Open Vendor Profile     | Body           | View Vendor Details             |
| PO Count       | Number Of Linked Purchase Orders | Number   | Open PO References      | Body           | Linked Purchase Orders          |
| Total Quantity | Total Received Quantity          | Number   | –                       | Body           | Total Quantity Received         |
| Total Amount   | Total Receipt Value              | Currency | –                       | Body           | Total Receipt Value             |
| Stage          | Current Workflow Stage           | Status   | Open Workflow Timeline  | Trailing       | Current GRN Status              |
| Created By     | User Who Created GRN             | User     | Open User Profile       | Footer         | Created By User                 |
| Updated At     | Last Updated Date & Time         | DateTime | –                       | Footer         | Last Modification Date          |

### 1.2b. DataTable (GRN List) - Toolbar Config

| Feature          | Settings                     | On Click                                       |
|:---------------- |:---------------------------- |:---------------------------------------------- |
| Search           | Yes                          | Search GRN records based on entered keyword(s) |
| View Toggle      | Yes                          | Switch between available DataTable layouts     |
| Column Selection | Yes                          | Show/Hide columns                              |
| Group By         | Yes                          | Group records by selected column               |
| Filter           | Yes                          | Open filter panel                              |
| Export           | Yes<br/>Filename: `grn-list` | Export GRN list                                |
| Share            | Yes                          | Share current filtered view                    |
| Full Screen      | Yes                          | Toggle full-screen view                        |
| Add              | Yes<br/>Page to open: `grn`  | Open GRN page in create mode                   |

### 1.2c. DataTable (GRN List) - Config

| Feature        | Settings              |
|:-------------- |:--------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

### 1.2d. DataTable (GRN List) - RowAction Menu

| Name                    | Action                                                  | Visibility Criteria                                          | Icon               | Tooltip                 |
|:----------------------- |:------------------------------------------------------- |:------------------------------------------------------------ |:------------------ |:----------------------- |
| View                    | Open page: `grn`                                        | View Permission                                              | eye                | View GRN                |
| Modify                  | Open page: `grn`                                        | Modify Permission + Stage Allows Edit                        | pencil             | Modify GRN              |
| Print                   | Download GRN PDF                                        | Print Permission                                             | printer            | Print GRN               |
| Create Purchase Invoice | Open page: `purchase_invoice` with current GRN selected | Create Purchase Invoice Permission<br/>GRN Stage = Completed | receipt            | Create Purchase Invoice |
| Set as <Stage_1>        | Set stage to Stage_1                                    | Allowed stage based on current stage                         | arrow-right-circle | Change Workflow Stage   |
| Set as <Stage_2>        | Set stage to Stage_2                                    | Allowed stage based on current stage                         | arrow-right-circle | Change Workflow Stage   |
| Undo Stage              | Move back to last stage                                 | Permission to rollback                                       | undo-2             | Rollback Current Stage  |
| Duplicate               | Create copy of current GRN                              | Add Permission                                               | copy               | Create Duplicate GRN    |
| Cancel                  | Show confirmation and cancel GRN                        | Cancel Permission + Allowed Stage                            | share-2            | Cancel GRN              |
| Delete                  | Show confirmation and delete GRN                        | Delete Permission + Allowed Stage                            | trash-2            | Delete GRN              |

### 1.2e. DataTable (GRN List) - Filters Fields

| Name           | Component         | Depends On   | Possible Values                                  | Default Values | Output            | Tooltip                |
|:-------------- |:----------------- |:------------ |:------------------------------------------------ |:-------------- |:----------------- |:---------------------- |
| Date Range     | Date Range Picker | –            | Any Date Range                                   | Today          | from_date,to_date | Filter GRNs By Date    |
| Organisation   | Multi Select      | –            | Allowed Organisations                            | All            | organisation_ids  | –                      |
| Branch         | Multi Select      | Organisation | Active Branches                                  | All            | branch_ids        | –                      |
| Vendor         | Multi Select      | –            | Active Vendors                                   | All            | vendor_ids        | –                      |
| Purchase Order | Multi Select      | Vendor       | Active Purchase Orders                           | All            | po_ids            | Linked Purchase Orders |
| Stage          | Multi Select      | –            | Draft, Submitted, Verified, Completed, Cancelled | Open Stages    | stage_ids         | –                      |
| Receipt Type   | Multi Select      | –            | Full Receipt, Partial Receipt                    | All            | receipt_type_ids  | –                      |
| Created By     | User Lookup       | –            | Active Users                                     | Current User   | user_ids          | –                      |

# 2. GRN Entry Page UI

Fields in the main form are grouped into different sections.

```
 2.1. Left: Main Form

- General Information
- Purchase Order References
- Product Receipt Details
- Batch / Lot Details
- Quality Inspection
- Attachments

2.2. Right: Summary and Timeline

- Summary Panel
- Workflow Timeline
- Receipt Summary 
```

## 2.1. Left Part

### 2.1.1. Section (General Information)

| Name/Label       | Data Source           | Type/Component | Component Specific Information                                        | Required | Read Only | Validations              | On Change            | Description           | Tooltip                          |
|:---------------- |:--------------------- |:-------------- |:--------------------------------------------------------------------- |:-------- |:--------- |:------------------------ |:-------------------- |:--------------------- |:-------------------------------- |
| GRN Number       | grn_number            | Text           | Prefix=GRN, Auto Increment, Format=GRN-YYYY-000001                    | Yes      | Yes       | Must Be Unique           | Auto Generated       | Unique GRN Number     | System Generated GRN Number      |
| GRN Date         | grn_date              | DatePicker     | Single Date Picker                                                    | Yes      | No        | Cannot Be Future Date    | –                    | Goods Receipt Date    | Date Of Material Receipt         |
| Receipt Type     | receipt_type          | Select         | Regular Receipt, Returnable Receipt, Sample Receipt, Transfer Receipt | Yes      | No        | Valid Receipt Type       | Update Receipt Rules | Type Of Receipt       | Receipt Classification           |
| Vendor           | vendor_id             | Select         | Single Select, Searchable, Async Lookup                               | Yes      | No        | Vendor Must Be Active    | Load Vendor Details  | Vendor Selection      | Select Vendor                    |
| Vehicle Number   | vehicle_number        | Text           | Max Length 20                                                         | No       | No        | –                        | –                    | Vehicle Number        | Transport Vehicle Number         |
| LR Number        | lr_number             | Text           | Max Length 50                                                         | No       | No        | –                        | –                    | Lorry Receipt Number  | Transport LR Number              |
| Transporter Name | transporter_name      | Text           | Max Length 100                                                        | No       | No        | –                        | –                    | Transport Company     | Transporter Details              |
| Invoice Number   | vendor_invoice_number | Text           | Max Length 50                                                         | No       | No        | –                        | –                    | Vendor Invoice Number | Vendor Reference Invoice         |
| Invoice Date     | vendor_invoice_date   | DatePicker     | Single Date Picker                                                    | No       | No        | Invoice Date <= GRN Date | –                    | Vendor Invoice Date   | Invoice Date Mentioned By Vendor |
| Remarks          | remarks               | RichTextBox    | –                                                                     | No       | No        | –                        | –                    | Remarks               | Additional Notes                 |

---

### 2.1.2. Section (Purchase Order References)

This section allows linking one or multiple Purchase Orders with current GRN.

Business Rules:

- Single PO GRN is allowed.
- Multiple PO GRN is allowed.
- Direct Receipt (Without PO) is allowed.
- Mixed GRN (PO + Direct Receipt) is allowed.
- All selected POs and Direct Receipt products must belong to the same Vendor.
- Direct Receipt products require Purchase Manager approval before stock posting.
- Currency of all selected POs must be same.

| Name/Label             | Data Source            | Type/Component | Component Specific Information         | Required | Read Only | Validations                                 | On Change        | Description            | Tooltip                                |
|:---------------------- |:---------------------- |:-------------- |:-------------------------------------- |:-------- |:--------- |:------------------------------------------- |:---------------- |:---------------------- |:-------------------------------------- |
| Purchase Orders        | po_ids                 | Multi Select   | Searchable, Async Lookup               | No       | No        | All Selected POs Must Belong To Same Vendor | Load PO Products | Linked Purchase Orders | Select One Or Multiple Purchase Orders |
| Vendor                 | vendor_id              | Auto Filled    | Loaded From PO Or Manual Selection     | Yes      | No        | Vendor Must Match All Products              | Auto Populate    | Vendor Information     | Vendor For Current GRN                 |
| Currency               | currency_id            | Auto Filled    | Loaded From PO Or Organisation Default | Yes      | Yes       | All POs Must Have Same Currency             | Auto Populate    | Transaction Currency   | Currency Used In GRN                   |
| Total PO Quantity      | total_po_quantity      | Text           | Calculated Field                       | No       | Yes       | –                                           | Auto Calculate   | Total Ordered Quantity | Sum Of Selected PO Quantities          |
| Total Pending Quantity | total_pending_quantity | Text           | Calculated Field                       | No       | Yes       | –                                           | Auto Calculate   | Total Pending Quantity | Pending Receipt Quantity               |
| Total Receipt Quantity | total_receipt_quantity | Text           | Calculated Field                       | No       | Yes       | –                                           | Auto Calculate   | Total Current Receipt  | Total Quantity In Current GRN          |
| Total PO Value         | total_po_value         | Currency       | Calculated Field                       | No       | Yes       | –                                           | Auto Calculate   | Total PO Value         | Sum Of Selected PO Values              |

---

### 2.1.2a. DataTable (PO References) - Columns

| Name         | Content               | Type   | On Click      | Tooltip                      |
|:------------ |:--------------------- |:------ |:------------- |:---------------------------- |
| PO Number    | Purchase Order Number | Text   | Open PO       | View Purchase Order          |
| PO Date      | Purchase Order Date   | Date   | –             | PO Creation Date             |
| Vendor       | Vendor Name           | Text   | Open Vendor   | Vendor Details               |
| Ordered Qty  | Ordered Quantity      | Number | –             | Total Ordered Quantity       |
| Received Qty | Received Quantity     | Number | –             | Quantity Already Received    |
| Pending Qty  | Pending Quantity      | Number | –             | Quantity Pending For Receipt |
| Stage        | Current PO Stage      | Status | Open Timeline | Current Workflow Stage       |

---

### 2.1.2b. DataTable (PO References) - Toolbar Config

| Feature          | Settings | On Click           |
|:---------------- |:-------- |:------------------ |
| Search           | No       | N/A                |
| View Toggle      | No       | N/A                |
| Column Selection | No       | N/A                |
| Group By         | No       | N/A                |
| Filter           | No       | N/A                |
| Export           | No       | N/A                |
| Share            | No       | N/A                |
| Full Screen      | Yes      | Toggle Full Screen |
| Add              | No       | N/A                |

### 2.1.2c. DataTable (PO References) - Config

| Feature        | Settings |
|:-------------- |:-------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | Yes      |
| Pagination     | No       |

### 2.1.2d. DataTable (PO References) - RowAction Menu

| Name | Action | Visibility Criteria | Icon | Tooltip |
| ---- | ------ | ------------------- | ---- | ------- |
| -    | -      | -                   | -    | -       |

### 2.1.2e. DataTable (PO References) - Filters Fields

| Name | Component | Depends on | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| -    | -         | -          | -               | -              | -      | -       |

---

### 2.1.3. Section (Product Receipt Details)

This section displays all products loaded from selected Purchase Orders.

| Name/Label      | Data Source | Type/Component | Component Specific Information  | Required | Read Only | Validations | On Change    | Description             | Tooltip                 |
|:--------------- |:----------- |:-------------- |:------------------------------- |:-------- |:--------- |:----------- |:------------ |:----------------------- |:----------------------- |
| Barcode Scan    | –           | Input          | Barcode Scanner Supported       | No       | No        | –           | Load Product | Scan Product Barcode    |                         |
| Product Details | products    | DataTable      | Auto Loaded From Selected PO(s) | Yes      | No        | –           | –            | Product Receipt Details | Products Being Received |

---

### 2.1.3a. DataTable (Product Receipt Details) - Columns

| Name/Label              | Data Source               | Type/Component | Component Specific Information  | Required | Read Only | Validations                       | On Change         | Description              | Tooltip                          |
|:----------------------- |:------------------------- |:-------------- |:------------------------------- |:-------- |:--------- |:--------------------------------- |:----------------- |:------------------------ |:-------------------------------- |
| Receipt Source          | receipt_source            | Select         | PO Receipt, Direct Receipt      | Yes      | No        | Valid Receipt Source              | Update Rules      | Receipt Type             | Source Of Current Product        |
| PO Number               | po_number                 | Lookup         | Visible Only For PO Receipt     | No       | Yes       | Product Must Exist In Selected PO | Load PO Product   | Source PO                | Purchase Order Reference         |
| Product                 | product_id                | Lookup         | Auto Loaded Or Manual Selection | Yes      | No        | Product Must Be Active            | Load Product Info | Product                  | Product Being Received           |
| Variety                 | variety_id                | Lookup         | Auto Filled From Product        | No       | Yes       | –                                 | –                 | Product Variety          | Variety Information              |
| Pack                    | pack_id                   | Lookup         | Auto Filled From Product        | No       | Yes       | –                                 | –                 | Product Pack             | Packaging Information            |
| Ordered Qty             | ordered_quantity          | Number Display | Visible Only For PO Receipt     | No       | Yes       | –                                 | –                 | Ordered Quantity         | Ordered Quantity In PO           |
| Previously Received Qty | previous_received_qty     | Number Display | Visible Only For PO Receipt     | No       | Yes       | –                                 | –                 | Previous Receipts        | Already Received Quantity        |
| Pending Qty             | pending_quantity          | Number Display | Visible Only For PO Receipt     | No       | Yes       | Cannot Be Negative                | –                 | Pending Quantity         | Remaining Quantity               |
| Received Qty            | current_received_quantity | Number Input   | Decimal Allowed                 | Yes      | No        | Must Be Greater Than Zero         | Update Totals     | Current Receipt Quantity | Quantity Received In Current GRN |
| Excess Qty              | excess_quantity           | Number Input   | Decimal Allowed                 | No       | No        | Permission Based                  | Update Totals     | Excess Receipt           | Quantity Beyond PO               |
| UOM                     | uom_id                    | Text           | Auto Filled                     | Yes      | Yes       | –                                 | –                 | Unit Of Measure          | Product UOM                      |
| Approval Status         | approval_status           | Status         | Pending, Approved, Rejected     | No       | Yes       | Required For Direct Receipt       | Auto Update       | Direct Receipt Approval  | Approval Status                  |
| Remarks                 | remarks                   | TextArea       | Multi Line                      | No       | No        | –                                 | –                 | Product Remarks          | Additional Notes                 |

---

### 2.1.3b. DataTable (Product Receipt Details) - Toolbar Config

| Feature          | Settings | On Click                     |
|:---------------- |:-------- |:---------------------------- |
| Search           | No       | N/A                          |
| View Toggle      | No       | N/A                          |
| Column Selection | No       | N/A                          |
| Group By         | No       | N/A                          |
| Filter           | No       | N/A                          |
| Export           | No       | N/A                          |
| Share            | No       | N/A                          |
| Full Screen      | Yes      | Toggle Full Screen           |
| Add              | No       | Products Auto Loaded From PO |

---

### 2.1.3c. DataTable (Product Receipt Details) - Config

| Feature        | Settings              |
|:-------------- |:--------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

---

### 2.1.3d. DataTable (Product Receipt Details) - RowAction Menu

| Name          | Action                               | Visibility Criteria | Icon                    | Tooltip                |
|:------------- |:------------------------------------ |:------------------- |:----------------------- |:---------------------- |
| Modify        | Open Product Receipt Popup           | Modify Permission   | pencil                  | Modify Receipt Details |
| Split Receipt | Split Quantity Into Multiple Batches | Modify Permission   | split-square-horizontal | Split Product Receipt  |
| Delete        | Remove Product From GRN              | Draft Stage Only    | trash-2                 | Remove Product         |

### 2.1.3e. DataTable (Product Receipt Details) - Filters Fields

| Name | Component | Depends on | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| -    | -         | -          | -               | -              | -      | -       |

### 2.1.4. Section (Batch / Lot Details)

This section is used to capture batch-wise receipt information for traceability and inventory control.

| Name/Label    | Data Source   | Type/Component | Component Specific Information      | Required | Read Only | Validations | On Change | Description       | Tooltip               |
|:------------- |:------------- |:-------------- |:----------------------------------- |:-------- |:--------- |:----------- |:--------- |:----------------- |:--------------------- |
| Batch Details | batch_details | DataTable      | One Or Multiple Batches Per Product | No       | No        | –           | –         | Batch Information | Product Batch Details |

---

### 2.1.4a. DataTable (Batch Details) - Columns

| Name/Label         | Data Source        | Type/Component | Component Specific Information | Required | Read Only | Validations                             | On Change     | Description        | Tooltip                    |
|:------------------ |:------------------ |:-------------- |:------------------------------ |:-------- |:--------- |:--------------------------------------- |:------------- |:------------------ |:-------------------------- |
| Product            | product_id         | Auto Filled    | Loaded From Product Receipt    | Yes      | Yes       | –                                       | –             | Product            | Product Name               |
| Batch Number       | batch_number       | Text           | Max Length 100                 | No       | No        | Unique Within Product                   | –             | Batch Number       | Manufacturer Batch Number  |
| Manufacturing Date | manufacturing_date | Date Picker    | Single Date                    | No       | No        | Cannot Be Future Date                   | –             | Manufacturing Date | Product Manufacturing Date |
| Expiry Date        | expiry_date        | Date Picker    | Single Date                    | No       | No        | Must Be Greater Than Manufacturing Date | –             | Expiry Date        | Product Expiry Date        |
| Received Qty       | quantity           | Number Input   | Decimal Allowed                | Yes      | No        | Cannot Exceed Product Receipt Qty       | Update Totals | Batch Quantity     | Quantity In Batch          |
| Remarks            | remarks            | Text Area      | Multi Line                     | No       | No        | –                                       | –             | Remarks            | Batch Remarks              |

---

### 2.1.4b. DataTable (Batch Details) - Toolbar Config

| Feature     | Settings | On Click           |
|:----------- |:-------- |:------------------ |
| Search      | No       | N/A                |
| Filter      | No       | N/A                |
| Export      | No       | N/A                |
| Full Screen | Yes      | Toggle Full Screen |
| Add         | Yes      | Add New Batch      |

---

### 2.1.4c. DataTable (Batch Details) - Config

| Feature        | Settings |
|:-------------- |:-------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | Yes      |
| Pagination     | No       |

---

### 2.1.4c. DataTable (Batch Details) - RowAction Menu

| Name   | Action       | Visibility Criteria | Icon    | Tooltip              |
|:------ |:------------ |:------------------- |:------- |:-------------------- |
| Modify | Edit Batch   | Modify Permission   | pencil  | Modify Batch Details |
| Delete | Delete Batch | Draft Stage Only    | trash-2 | Delete Batch         |

---

### 2.1.4e. DataTable (Batch Details) - Filters Fields

| Name | Component | Depends on | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| -    | -         | -          | -               | -              | -      | -       |

## 2.1.5. Section (Quality Inspection)

This section captures product-wise quality inspection details before stock becomes available for consumption or sale.

**Component Type:** DataTable

### 2.1.5a. DataTable (Quality Inspection) - Columns

| Name/Label          | Data Source         | Type/Component | Component Specific Information       | Required | Read Only | Validations                              | On Change                   | Description            | Tooltip                             |
|:------------------- |:------------------- |:-------------- |:------------------------------------ |:-------- |:--------- |:---------------------------------------- |:--------------------------- |:---------------------- |:----------------------------------- |
| PO Number           | po_number           | Text           | Auto Filled From GRN Products        | Yes      | Yes       | –                                        | –                           | Purchase Order         | Source Purchase Order               |
| Product             | product_id          | Lookup         | Auto Filled From GRN Products        | Yes      | Yes       | Product Must Exist In Selected PO        | –                           | Product                | Product Under Inspection            |
| Variety             | variety_id          | Lookup         | Auto Filled                          | No       | Yes       | –                                        | –                           | Product Variety        | Product Variety                     |
| Batch Number        | batch_number        | Lookup         | Auto Filled From Batch Details       | No       | Yes       | –                                        | –                           | Batch Reference        | Batch Being Inspected               |
| Received Quantity   | received_quantity   | Number Display | Auto Filled                          | Yes      | Yes       | –                                        | –                           | Received Quantity      | Quantity Received In GRN            |
| Inspection Required | inspection_required | Toggle         | Yes / No                             | Yes      | No        | –                                        | Show/Hide Inspection Fields | Inspection Requirement | Whether Inspection Is Required      |
| Inspection Status   | inspection_status   | Select         | Pending, Approved, Rejected, Partial | Yes      | No        | Valid Status                             | Update Inspection Totals    | Inspection Status      | Current Inspection Result           |
| Inspected By        | inspected_by        | User Lookup    | Active Users                         | No       | No        | Valid User                               | –                           | Inspector              | Quality Inspector                   |
| Inspection Date     | inspection_date     | Date Picker    | Single Date                          | No       | No        | Must Be Greater Than GRN Date            | –                           | Inspection Date        | Date Of Inspection                  |
| Approved Quantity   | approved_quantity   | Number Input   | Decimal Allowed                      | Yes      | No        | Received Qty=Approved Qty + Rejected Qty | Update Totals               | Approved Quantity      | Accepted Quantity                   |
| Rejected Quantity   | rejected_quantity   | Number Input   | Decimal Allowed                      | Yes      | No        | Received Qty=Approved Qty + Rejected Qty | Update Totals               | Rejected Quantity      | Quantity Rejected During Inspection |
| Inspection Remarks  | inspection_remarks  | Text Area      | Multi Line                           | No       | No        | –                                        | –                           | Inspection Remarks     | Quality Notes                       |

---

### 2.1.5b. DataTable (Quality Inspection) - Toolbar Config

| Feature          | Settings                                   | On Click                                 |
|:---------------- |:------------------------------------------ |:---------------------------------------- |
| Search           | Yes                                        | Search Inspection Records                |
| View Toggle      | Yes                                        | Switch Between Available Views           |
| Column Selection | Yes                                        | Show/Hide Columns                        |
| Group By         | Yes                                        | Group By Product / PO / Status           |
| Filter           | Yes                                        | Open Filter Panel                        |
| Export           | Yes<br/>Filename: `grn-quality-inspection` | Export Inspection Data                   |
| Share            | No                                         | N/A                                      |
| Full Screen      | Yes                                        | Toggle Full Screen                       |
| Add              | No                                         | Records Auto Generated From GRN Products |

---

### 2.1.5c. DataTable (Quality Inspection) - Config

| Feature        | Settings              |
|:-------------- |:--------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

---

### 2.1.5d. DataTable (Quality Inspection) - RowAction Menu

| Name            | Action                           | Visibility Criteria   | Icon         | Tooltip                   |
|:--------------- |:-------------------------------- |:--------------------- |:------------ |:------------------------- |
| View            | View Inspection Details          | View Permission       | eye          | View Inspection           |
| Modify          | Modify Inspection Details        | Modify Permission     | pencil       | Edit Inspection           |
| Approve         | Mark As Approved                 | Inspection Permission | check-circle | Approve Product           |
| Reject          | Mark As Rejected                 | Inspection Permission | x-circle     | Reject Product            |
| Partial Approve | Mark As Partial                  | Inspection Permission | alert-circle | Partially Approve Product |
| Duplicate       | Create Copy Of Inspection Record | Add Permission        | copy         | Duplicate Inspection      |
| Delete          | Remove Inspection Record         | Draft Stage Only      | trash-2      | Delete Inspection Record  |

---

### 2.1.5e. DataTable (Quality Inspection) - Filters Fields

| Name              | Component    | Depends On | Possible Values                      | Default Values | Output              | Tooltip                     |
|:----------------- |:------------ |:---------- |:------------------------------------ |:-------------- |:------------------- |:--------------------------- |
| PO Number         | Multi Select | –          | Linked PO Numbers                    | All            | po_ids              | Filter By Purchase Order    |
| Product           | Multi Select | –          | Products In GRN                      | All            | product_ids         | Filter By Product           |
| Batch Number      | Multi Select | Product    | Available Batches                    | All            | batch_numbers       | Filter By Batch             |
| Inspection Status | Multi Select | –          | Pending, Approved, Rejected, Partial | All            | inspection_statuses | Filter By Inspection Status |
| Inspector         | User Lookup  | –          | Active Users                         | All            | inspector_ids       | Filter By Inspector         |
| Inspection Date   | Date Range   | –          | Any Date Range                       | Today          | from_date,to_date   | Filter By Inspection Date   |

---

## 2.1.6. Section (Attachments)

| Name/Label | Data Source | Type/Component | Required | Read Only | Description                 | Tooltip                   |
|:---------- |:----------- |:-------------- |:-------- |:--------- |:--------------------------- |:------------------------- |
| Preview    |             | File Viewer    | No       | Yes       | Preview File                | View File Before Download |
| File Name  | file_name   | Text Display   | Yes      | Yes       | File Name                   | Uploaded File Name        |
| File Type  | file_type   | Badge          | Yes      | Yes       | File Type                   | PDF, JPG, PNG, XLSX, DOCX |
| Actions    | –           | Action Menu    | No       | No        | Preview / Download / Delete | Manage Attachment         |

---

## 2.2. Right Side Widgets

### 2.2.1. GRN Summary

Component to use: Summary Card

| Name                    | Content                         |
|:----------------------- |:------------------------------- |
| Total PO Count          | Count(Selected Purchase Orders) |
| Total Products          | Count(Products)                 |
| Total Received Quantity | Sum(Received Quantity)          |
| Total Receipt Value     | Sum(Product Value)              |
| Rejected Quantity       | Sum(Rejected Quantity)          |
| Approved Quantity       | Sum(Approved Quantity)          |
| Vendor                  | Selected Vendor                 |
| Receipt Type            | Current Receipt Type            |

---

### 2.2.2. Workflow Timeline

Component to use: StageHistoryViewer

| Name       | Content               |
|:---------- |:--------------------- |
| Stage      | Stage Name            |
| Set At     | Date Time             |
| Set By     | User                  |
| Remarks    | Stage Remarks         |
| Attachment | Supporting Attachment |

---

### 2.2.3. Purchase Order Receipt Summary

Component to use: Summary Card

| Name                    | Content                       |
|:----------------------- |:----------------------------- |
| Total Ordered Qty       | Sum(PO Ordered Qty)           |
| Previously Received Qty | Sum(PO Received Qty)          |
| Current Received Qty    | Sum(Current GRN Qty)          |
| Remaining Pending Qty   | Sum(PO Pending Qty After GRN) |
