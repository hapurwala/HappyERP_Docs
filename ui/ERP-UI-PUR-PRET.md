# HappyERP Purchase Return (PRET) UI/UX Documentation

This document gives UI details of all pages used in Purchase Return module.

# Pages

| S. No. | Name                         | Type  | Purpose                                                                                        |
| ------ | ---------------------------- | ----- | ---------------------------------------------------------------------------------------------- |
| 1.     | Purchase Return List         | Page  | - View all Purchase Returns<br>- Search, Filter & Export Returns<br>- Perform Workflow Actions |
| 2.     | Add / Modify Purchase Return | Page  | - Return Products To Vendor<br>- Reduce Inventory<br>- Generate Return Documents               |
| 3.     | Add Return Products          | Popup | - Select Products To Return                                                                    |

---

# 1. Purchase Return List Page UI

This page displays all Purchase Returns created in the system.

---

## 1.1 Purchase Return List

### 1.1a. DataTable (Purchase Return List) - Columns

| Name            | Content                      | Type     | On Click                | Card Placement | Tooltip / On Hover   |
|:--------------- |:---------------------------- |:-------- |:----------------------- |:-------------- |:-------------------- |
| Return Number   | Purchase Return Number       | Text     | Open Purchase Return    | Title          | View Purchase Return |
| Return Date     | Return Date                  | Date     | –                       | Subtitle       | Return Date          |
| Vendor          | Vendor Name                  | Text     | Open Vendor Details     | Body           | View Vendor          |
| Return Type     | Full Return / Partial Return | Badge    | –                       | Body           | Return Type          |
| Reference       | GRN / Purchase Invoice       | Text     | Open Reference Document | Body           | View Source Document |
| Total Products  | Total Products               | Number   | –                       | Body           | Products Returned    |
| Return Quantity | Total Returned Quantity      | Number   | –                       | Body           | Quantity Returned    |
| Return Value    | Return Amount                | Currency | –                       | Body           | Total Return Value   |
| Status          | Workflow Stage               | Status   | Open Workflow Timeline  | Trailing       | Current Stage        |
| Created By      | User                         | User     | Open User Profile       | Footer         | Created By           |
| Updated At      | Date Time                    | DateTime | –                       | Footer         | Last Updated         |

---

### 1.1b. DataTable (Purchase Return List) - Toolbar Config

| Feature          | Settings                                 | On Click                               |
|:---------------- |:---------------------------------------- |:-------------------------------------- |
| Search           | Yes                                      | Search Purchase Returns                |
| View Toggle      | Yes                                      | Switch Between Available Views         |
| Column Selection | Yes                                      | Show/Hide Columns                      |
| Group By         | Yes                                      | Group By Vendor / Status / Return Type |
| Filter           | Yes                                      | Open Filter Panel                      |
| Export           | Yes<br/>Filename: `purchase-return-list` | Export Purchase Returns                |
| Share            | Yes                                      | Share Current View                     |
| Full Screen      | Yes                                      | Toggle Full Screen                     |
| Add              | Yes                                      | Open Page: `purchase_return`           |

---

### 1.1c. DataTable (Purchase Return List) - Config

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

### 1.1d. DataTable (Purchase Return List) - RowAction Menu

| Name                      | Action                                                                                                                                                                    | Visibility Criteria                 | Icon               | Tooltip                   |
|:------------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |:----------------------------------- |:------------------ |:------------------------- |
| View                      | Open Page: `purchase_return`                                                                                                                                              | View Permission                     | eye                | View Purchase Return      |
| Modify                    | Open Page: `purchase_return`                                                                                                                                              | Modify Permission + Editable Stage  | pencil             | Modify Purchase Return    |
| Create Debit Note         | Open Page: `debit_note`                                                                                                                                                   | Finance Permission + Approved Stage | receipt            | Create Debit Note         |
| Create Payment Adjustment | Open Page: `payment_adjustment`                                                                                                                                           | Finance Permission                  | wallet             | Adjust Vendor Settlement  |
| Set As <Stage_1>          | Change Workflow Stage                                                                                                                                                     | Allowed Stage                       | arrow-right-circle | Change Workflow Stage     |
| Set As <Stage_2>          | Change Workflow Stage                                                                                                                                                     | Allowed Stage                       | arrow-right-circle | Change Workflow Stage     |
| Undo Stage                | Rollback Workflow                                                                                                                                                         | Rollback Permission                 | undo-2             | Rollback Stage            |
| Duplicate                 | Create Duplicate Return.<br/>Copies Vendor, References, Products, Return Charges & General Information.<br/>Resets Return Number, Workflow, Timeline & Audit Information. | Add Permission                      | copy               | Duplicate Purchase Return |
| Cancel                    | Cancel Purchase Return                                                                                                                                                    | Cancel Permission                   | x-circle           | Cancel Purchase Return    |
| Delete                    | Delete Purchase Return                                                                                                                                                    | Delete Permission                   | trash-2            | Delete Purchase Return    |

---

### 1.1e. DataTable (Purchase Return List) - Filters Fields

| Name           | Component         | Depends On   | Possible Values                                                    | Default Values | Output            | Tooltip                   |
|:-------------- |:----------------- |:------------ |:------------------------------------------------------------------ |:-------------- |:----------------- |:------------------------- |
| Date Range     | Date Range Picker | –            | Any Date Range                                                     | Today          | from_date,to_date | Filter By Date            |
| Organisation   | Multi Select      | –            | Allowed Organisations                                              | All            | organisation_ids  | Filter By Organisation    |
| Branch         | Multi Select      | Organisation | Active Branches                                                    | All            | branch_ids        | Filter By Branch          |
| Vendor         | Multi Select      | –            | Active Vendors                                                     | All            | vendor_ids        | Filter By Vendor          |
| Return Type    | Multi Select      | –            | Full Return, Partial Return                                        | All            | return_types      | Filter By Return Type     |
| Reference Type | Multi Select      | –            | GRN, Purchase Invoice, Mixed                                       | All            | reference_types   | Filter By Source Document |
| Status         | Multi Select      | –            | Draft, Submitted, Verified, Approved, Completed, Closed, Cancelled | Open Stages    | stage_ids         | Filter By Workflow Stage  |
| Created By     | User Lookup       | –            | Active Users                                                       | Current User   | user_ids          | Filter By Creator         |

---

# 2. Purchase Return Entry Page UI

This page is used to return products back to Vendor and update inventory, vendor ledger and financial records.

---

## 2.1 Left Section

### 2.1.1. Section (General Information)

This section captures basic information about current Purchase Return.

Business Rules:

- Purchase Return can be created against one or multiple GRNs.
- Purchase Return can be created against one or multiple Purchase Invoices.
- Mixed references (GRN + Purchase Invoice) are allowed.
- All references must belong to same Vendor.
- Purchase Return can be created before or after Vendor Payment.
- Partial return is allowed.
- Full return is allowed.

| Name/Label    | Data Source     | Type/Component | Component Specific Information                                                                        | Required | Read Only | Validations           | On Change           | Description            | Tooltip                 |
|:------------- |:--------------- |:-------------- |:----------------------------------------------------------------------------------------------------- |:-------- |:--------- |:--------------------- |:------------------- |:---------------------- |:----------------------- |
| Return Number | return_number   | Text           | Prefix: PRET, Auto Generated                                                                          | Yes      | Yes       | Must Be Unique        | Auto Generate       | Purchase Return Number | System Generated Number |
| Return Date   | return_date     | Date Picker    | Single Date                                                                                           | Yes      | No        | Cannot Be Future Date | –                   | Return Date            | Date Of Return          |
| Organisation  | organisation_id | Lookup         | Active Organisations                                                                                  | Yes      | No        | Valid Organisation    | Load Branches       | Organisation           | Organisation            |
| Branch        | branch_id       | Lookup         | Filtered By Organisation                                                                              | Yes      | No        | Valid Branch          | –                   | Branch                 | Branch                  |
| Vendor        | vendor_id       | Lookup         | Searchable Async Lookup                                                                               | Yes      | No        | Vendor Must Be Active | Load References     | Vendor                 | Select Vendor           |
| Return Type   | return_type     | Select         | Full Return, Partial Return                                                                           | Yes      | No        | Valid Return Type     | Update Rules        | Return Type            | Type Of Return          |
| Return Reason | return_reason   | Select         | Damaged, Expired, Wrong Product, Wrong Quantity, Poor Quality, Vendor Recall, Price Difference, Other | Yes      | No        | Mandatory             | Update Summary      | Return Reason          | Reason For Return       |
| Currency      | currency_id     | Lookup         | Organisation Currency                                                                                 | Yes      | No        | Valid Currency        | Update Amounts      | Currency               | Transaction Currency    |
| Exchange Rate | exchange_rate   | Number Input   | Decimal Allowed                                                                                       | No       | No        | Greater Than Zero     | Recalculate Amounts | Exchange Rate          | Currency Conversion     |
| Remarks       | remarks         | Text Area      | Multi Line                                                                                            | No       | No        | –                     | –                   | Remarks                | Additional Notes        |

---

## 2.1.2. Section (Reference Documents)

This section links all source documents against which products are being returned.

Supported Documents:

- Goods Receipt Note (GRN)
- Purchase Invoice (PINV)
- Mixed References (GRN + Purchase Invoice)

Business Rules:

- Multiple documents are allowed.
- All selected documents must belong to same Vendor.
- Only Approved/Completed documents are available.
- Closed or Fully Returned documents are hidden.
- Products are loaded automatically after selecting references.

| Name/Label          | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations                    | On Change     | Description      | Tooltip                                   |
|:------------------- |:----------- |:-------------- |:------------------------------ |:-------- |:--------- |:------------------------------ |:------------- |:---------------- |:----------------------------------------- |
| Reference Documents | references  | DataTable      | Auto Loaded                    | Yes      | No        | Minimum One Reference Required | Load Products | Source Documents | Documents Against Which Return Is Created |

---

### 2.1.2a. DataTable (Reference Documents) - Columns

| Name/Label         | Data Source               | Type/Component | Component Specific Information | Required | Read Only | Validations         | On Change     | Description        | Tooltip                       |
|:------------------ |:------------------------- |:-------------- |:------------------------------ |:-------- |:--------- |:------------------- |:------------- |:------------------ |:----------------------------- |
| Reference Type     | reference_type            | Badge          | GRN, Purchase Invoice          | Yes      | Yes       | –                   | –             | Reference Type     | Source Document Type          |
| Document Number    | document_number           | Lookup         | Open Source Document           | Yes      | Yes       | Document Must Exist | View Document | Reference Number   | Open Document                 |
| Document Date      | document_date             | Date           | Auto Filled                    | Yes      | Yes       | –                   | –             | Document Date      | Reference Date                |
| Total Quantity     | total_quantity            | Number         | Auto Filled                    | Yes      | Yes       | –                   | –             | Total Quantity     | Original Quantity             |
| Already Returned   | already_returned_quantity | Number         | Auto Filled                    | Yes      | Yes       | –                   | –             | Returned Quantity  | Previous Returns              |
| Available Quantity | available_quantity        | Number         | Auto Filled                    | Yes      | Yes       | Cannot Be Negative  | –             | Available Quantity | Quantity Available For Return |
| Total Value        | total_value               | Currency       | Auto Filled                    | Yes      | Yes       | –                   | –             | Document Value     | Total Document Value          |
| Action             | –                         | Action Menu    | View / Remove                  | No       | No        | –                   | –             | Actions            | Available Actions             |

---

### 2.1.2b. DataTable (Reference Documents) - Toolbar Config

| Feature          | Settings | On Click                           |
|:---------------- |:-------- |:---------------------------------- |
| Search           | Yes      | Search Reference Documents         |
| View Toggle      | No       | –                                  |
| Column Selection | Yes      | Show/Hide Columns                  |
| Group By         | Yes      | Group By Reference Type            |
| Filter           | Yes      | Open Filter Panel                  |
| Export           | No       | –                                  |
| Share            | No       | –                                  |
| Full Screen      | Yes      | Toggle Full Screen                 |
| Add              | Yes      | Open Popup: `add_return_reference` |

---

### 2.1.2c. DataTable (Reference Documents) - Config

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

### 2.1.2d. DataTable (Reference Documents) - RowAction Menu

| Name   | Action               | Visibility Criteria | Icon    | Tooltip          |
|:------ |:-------------------- |:------------------- |:------- |:---------------- |
| View   | Open Source Document | View Permission     | eye     | View Document    |
| Remove | Remove Reference     | Modify Permission   | trash-2 | Remove Reference |

---

### 2.1.2e. DataTable (Reference Documents) - Filters Fields

| Name               | Component         | Depends On | Possible Values       | Default Values | Output             | Tooltip                      |
|:------------------ |:----------------- |:---------- |:--------------------- |:-------------- |:------------------ |:---------------------------- |
| Reference Type     | Multi Select      | –          | GRN, Purchase Invoice | All            | reference_types    | Filter By Reference Type     |
| Date Range         | Date Range Picker | –          | Any Date Range        | All            | from_date,to_date  | Filter By Date               |
| Document Number    | Text              | –          | –                     | –              | document_number    | Search Document              |
| Available Quantity | Range             | –          | Numeric               | All            | available_quantity | Filter By Available Quantity |

### 2.1.3. Section (Return Products)

This section displays all products selected for return.

Products can be loaded from:

- One Or Multiple GRNs
- One Or Multiple Purchase Invoices
- Mixed References (GRN + Purchase Invoice)

Business Rules:

- Only products available in selected reference documents are shown.
- Return Quantity cannot exceed Available Return Quantity.
- Partial return is allowed.
- Full return is allowed.
- Batch-wise return is supported.
- Multiple batches of same product can be returned.
- Quality inspection result can be recorded.
- Stock is reduced only after Purchase Return approval.

| Name/Label      | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations                  | On Change      | Description            | Tooltip              |
|:--------------- |:----------- |:-------------- |:------------------------------ |:-------- |:--------- |:---------------------------- |:-------------- |:---------------------- |:-------------------- |
| Barcode Scan    | –           | Input          | Barcode / QR Scanner           | No       | No        | Valid Product                | Load Product   | Barcode Scanner        | Scan Product Barcode |
| Return Products | products    | DataTable      | Auto Loaded                    | Yes      | No        | Minimum One Product Required | Update Summary | Return Product Details | Products To Return   |

---

### 2.1.3a. DataTable (Return Products) - Columns

| Name/Label                    | Data Source                   | Type/Component | Component Specific Information                                                                        | Required | Read Only | Validations                      | On Change           | Description         | Tooltip                       |
|:----------------------------- |:----------------------------- |:-------------- |:----------------------------------------------------------------------------------------------------- |:-------- |:--------- |:-------------------------------- |:------------------- |:------------------- |:----------------------------- |
| Reference Type                | reference_type                | Badge          | GRN / Purchase Invoice                                                                                | Yes      | Yes       | –                                | –                   | Source Type         | Product Source                |
| Reference Number              | reference_number              | Lookup         | Open Reference Document                                                                               | Yes      | Yes       | –                                | View Document       | Source Document     | GRN / Purchase Invoice        |
| Product                       | product_id                    | Lookup         | Searchable                                                                                            | Yes      | Yes       | Active Product                   | –                   | Product             | Product Name                  |
| Variety                       | variety_id                    | Lookup         | Auto Filled                                                                                           | No       | Yes       | –                                | –                   | Variety             | Product Variety               |
| Batch / Lot                   | batch_number                  | Lookup         | Batch Wise Selection                                                                                  | No       | No        | Valid Batch                      | Load Batch          | Batch Number        | Batch Information             |
| Ordered Quantity              | ordered_quantity              | Number         | Auto Filled                                                                                           | Yes      | Yes       | –                                | –                   | Ordered Quantity    | Original Ordered Quantity     |
| Received Quantity             | received_quantity             | Number         | Auto Filled                                                                                           | Yes      | Yes       | –                                | –                   | Received Quantity   | Quantity Received In GRN      |
| Previous Returned Quantity    | previous_returned_quantity    | Number         | Auto Filled                                                                                           | Yes      | Yes       | –                                | –                   | Previous Return     | Already Returned Quantity     |
| Available Return Quantity     | available_return_quantity     | Number         | Auto Filled                                                                                           | Yes      | Yes       | Cannot Be Negative               | –                   | Available Quantity  | Quantity Available For Return |
| Return Quantity               | return_quantity               | Number Input   | Decimal Allowed                                                                                       | Yes      | No        | Cannot Exceed Available Quantity | Recalculate Summary | Return Quantity     | Quantity To Return            |
| UOM                           | uom_id                        | Lookup         | Auto Filled                                                                                           | Yes      | Yes       | –                                | –                   | Unit Of Measure     | Product UOM                   |
| Rate                          | rate                          | Currency       | Auto Filled                                                                                           | Yes      | Yes       | –                                | –                   | Purchase Rate       | Product Rate                  |
| Product Value Before Discount | product_value_before_discount | Currency       | Auto Calculated                                                                                       | Yes      | Yes       | Formula Validation               | Auto Calculate      | Product Amount      | Before Discount               |
| Discount Type                 | discount_type                 | Select         | Unit / Percentage / Fixed                                                                             | No       | No        | Valid Discount                   | Recalculate Amount  | Discount Type       | Discount Method               |
| Discount Rate                 | discount_rate                 | Number Input   | Decimal Allowed                                                                                       | No       | No        | Greater Than Or Equal To Zero    | Recalculate Amount  | Discount Rate       | Discount Value                |
| Discount Value                | discount_value                | Currency       | Auto Calculated                                                                                       | No       | Yes       | Formula Validation               | Auto Calculate      | Discount Amount     | Total Discount                |
| Product Value After Discount  | product_value_after_discount  | Currency       | Auto Calculated                                                                                       | Yes      | Yes       | Formula Validation               | Auto Calculate      | Discounted Amount   | Product Value                 |
| Tax                           | tax_value                     | Currency       | Auto Calculated                                                                                       | No       | Yes       | Valid Tax Configuration          | Auto Calculate      | Tax Amount          | Total Tax                     |
| Net Value                     | net_value                     | Currency       | Auto Calculated                                                                                       | Yes      | Yes       | Formula Validation               | Auto Calculate      | Net Amount          | Final Return Amount           |
| Return Reason                 | return_reason                 | Select         | Damaged, Expired, Wrong Product, Wrong Quantity, Poor Quality, Vendor Recall, Price Difference, Other | Yes      | No        | Mandatory                        | Update Summary      | Return Reason       | Reason For Return             |
| Replacement Required          | replacement_required          | Toggle         | Yes / No                                                                                              | No       | No        | –                                | Update Workflow     | Replacement Request | Request Replacement Material  |
| QC Status                     | qc_status                     | Badge          | Pending, Passed, Failed                                                                               | No       | Yes       | –                                | –                   | QC Result           | Inspection Result             |
| Remarks                       | remarks                       | Text Area      | Multi Line                                                                                            | No       | No        | –                                | –                   | Product Remarks     | Additional Remarks            |

---

### 2.1.3b. DataTable (Return Products) - Toolbar Config

| Feature          | Settings                                     | On Click                                 |
|:---------------- |:-------------------------------------------- |:---------------------------------------- |
| Search           | Yes                                          | Search Products                          |
| View Toggle      | Yes                                          | Switch Between Available Views           |
| Column Selection | Yes                                          | Show/Hide Columns                        |
| Group By         | Yes                                          | Group By Product / Batch / Return Reason |
| Filter           | Yes                                          | Open Filter Panel                        |
| Export           | Yes<br/>Filename: `purchase-return-products` | Export Product Details                   |
| Share            | No                                           | –                                        |
| Full Screen      | Yes                                          | Toggle Full Screen                       |
| Add              | Yes                                          | Open Popup: `add_return_products`        |

---

### 2.1.3c. DataTable (Return Products) - Config

| Feature        | Settings |
|:-------------- |:-------- |
| Row Selection  | Yes      |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | Yes      |
| Pagination     | No       |

---

### 2.1.3d. DataTable (Return Products) - RowAction Menu

| Name           | Action                      | Visibility Criteria | Icon    | Tooltip                 |
|:-------------- |:--------------------------- |:------------------- |:------- |:----------------------- |
| View Reference | Open GRN / Purchase Invoice | View Permission     | eye     | View Reference Document |
| View Batch     | Open Batch Details          | View Permission     | package | View Batch              |
| Duplicate      | Duplicate Product Row       | Modify Permission   | copy    | Duplicate Product       |
| Delete         | Remove Product              | Modify Permission   | trash-2 | Remove Product          |

---

### 2.1.3e. DataTable (Return Products) - Filters Fields

| Name                 | Component    | Depends On | Possible Values                                                                                       | Default Values | Output               | Tooltip              |
|:-------------------- |:------------ |:---------- |:----------------------------------------------------------------------------------------------------- |:-------------- |:-------------------- |:-------------------- |
| Product              | Multi Select | –          | Active Products                                                                                       | All            | product_ids          | Filter Products      |
| Variety              | Multi Select | Product    | Product Varieties                                                                                     | All            | variety_ids          | Filter Variety       |
| Batch                | Multi Select | Product    | Available Batches                                                                                     | All            | batch_ids            | Filter Batch         |
| Return Reason        | Multi Select | –          | Damaged, Expired, Wrong Product, Wrong Quantity, Poor Quality, Vendor Recall, Price Difference, Other | All            | return_reasons       | Filter Return Reason |
| Replacement Required | Multi Select | –          | Yes, No                                                                                               | All            | replacement_required | Filter Replacement   |
| QC Status            | Multi Select | –          | Pending, Passed, Failed                                                                               | All            | qc_status            | Filter QC Status     |

---

### 2.1.4. Section (Return Charges)

This section records additional financial adjustments related to Purchase Return.

Business Rules:

- Charges can be positive or negative.
- Charges participate in final return amount calculation.
- Multiple charge rows are supported.

| Name/Label     | Data Source    | Type/Component | Component Specific Information | Required | Read Only | Validations | On Change           | Description        | Tooltip                |
|:-------------- |:-------------- |:-------------- |:------------------------------ |:-------- |:--------- |:----------- |:------------------- |:------------------ |:---------------------- |
| Return Charges | return_charges | DataTable      | Multiple Rows Supported        | No       | No        | –           | Recalculate Summary | Additional Charges | Return Related Charges |

### 2.1.4a. DataTable (Return Charges) - Columns

| Name/Label       | Data Source      | Type/Component | Component Specific Information                                                           | Required | Read Only | Validations                   | On Change           | Description        | Tooltip                   |
|:---------------- |:---------------- |:-------------- |:---------------------------------------------------------------------------------------- |:-------- |:--------- |:----------------------------- |:------------------- |:------------------ |:------------------------- |
| Charge Type      | charge_type      | Select         | Freight, Handling, Restocking, Inspection, Packing, Insurance, Loading, Unloading, Other | Yes      | No        | Valid Charge Type             | Update Amount       | Charge Type        | Type Of Return Charge     |
| Charge Name      | charge_name      | Lookup         | Active Charge Master                                                                     | No       | No        | Active Charge                 | Load Charge Details | Charge Name        | Charge Master             |
| Calculation Type | calculation_type | Select         | Fixed, Percentage, Per Unit                                                              | Yes      | No        | Valid Calculation Type        | Recalculate         | Calculation Method | Charge Calculation Method |
| Rate             | rate             | Currency Input | Decimal Allowed                                                                          | Yes      | No        | Greater Than Or Equal To Zero | Recalculate         | Charge Rate        | Charge Rate               |
| Quantity         | quantity         | Number Input   | Decimal Allowed                                                                          | No       | No        | Greater Than Or Equal To Zero | Recalculate         | Quantity           | Charge Quantity           |
| Tax              | tax_value        | Currency       | Auto Calculated                                                                          | No       | Yes       | Formula Validation            | Auto Calculate      | Tax Amount         | Tax On Charge             |
| Net Value        | net_value        | Currency       | Auto Calculated                                                                          | Yes      | Yes       | Formula Validation            | Auto Calculate      | Charge Amount      | Final Charge Value        |
| Remarks          | remarks          | Text           | Single Line                                                                              | No       | No        | –                             | –                   | Remarks            | Charge Remarks            |

---

### 2.1.4b. DataTable (Return Charges) - Toolbar Config

| Feature          | Settings | On Click           |
|:---------------- |:-------- |:------------------ |
| Search           | No       | –                  |
| View Toggle      | No       | –                  |
| Column Selection | No       | –                  |
| Group By         | No       | –                  |
| Filter           | No       | –                  |
| Export           | No       | –                  |
| Share            | No       | –                  |
| Full Screen      | Yes      | Toggle Full Screen |
| Add              | Yes      | Add Charge Row     |

---

### 2.1.4c. DataTable (Return Charges) - Config

| Feature       | Settings |
|:------------- |:-------- |
| Row Selection | No       |
| Bulk Actions  | No       |
| Sticky Header | Yes      |
| Column Resize | Yes      |
| Sorting       | Yes      |
| Pagination    | No       |

---

### 2.1.4d. DataTable (Return Charges) - RowAction Menu

| Name      | Action           | Visibility Criteria | Icon    | Tooltip          |
|:--------- |:---------------- |:------------------- |:------- |:---------------- |
| Modify    | Edit Charge      | Modify Permission   | pencil  | Modify Charge    |
| Duplicate | Duplicate Charge | Modify Permission   | copy    | Duplicate Charge |
| Delete    | Remove Charge    | Modify Permission   | trash-2 | Delete Charge    |

---

### 2.1.4e. DataTable (Return Charges) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| –    | –         | –          | –               | –              | –      | –       |

---

## 2.1.5. Section (Attachments)

Supporting documents related to Purchase Return.

Examples:

- Damaged Product Images
- Vendor Approval Letter
- Courier Receipt
- Return Challan
- QC Report
- Other Supporting Documents

---

### 2.1.5a. DataTable (Attachments) - Columns

| Name        | Data Source | Type/Component | Description                 |
|:----------- |:----------- |:-------------- |:--------------------------- |
| Preview     | file_url    | Preview        | Preview File                |
| File Name   | file_name   | Text           | Uploaded File Name          |
| File Type   | file_type   | Badge          | PDF, JPG, PNG, XLSX, DOCX   |
| File Size   | file_size   | Text           | File Size                   |
| Uploaded By | uploaded_by | User           | Uploaded User               |
| Uploaded On | uploaded_at | DateTime       | Upload Date                 |
| Action      | –           | Action Menu    | Preview / Download / Delete |

---

### 2.1.5b. DataTable (Attachments) - Toolbar Config

| Feature          | Settings | On Click           |
|:---------------- |:-------- |:------------------ |
| Search           | No       | –                  |
| View Toggle      | No       | –                  |
| Column Selection | No       | –                  |
| Group By         | No       | –                  |
| Filter           | No       | –                  |
| Export           | No       | –                  |
| Share            | No       | –                  |
| Full Screen      | Yes      | Toggle Full Screen |
| Add              | Yes      | Upload Attachment  |

---

### 2.1.5c. DataTable (Attachments) - Config

| Feature       | Settings |
|:------------- |:-------- |
| Row Selection | No       |
| Bulk Actions  | No       |
| Sticky Header | Yes      |
| Column Resize | Yes      |
| Sorting       | Yes      |
| Pagination    | No       |

---

### 2.1.5d. DataTable (Attachments) - RowAction Menu

| Name     | Action              | Visibility Criteria | Icon     | Tooltip            |
|:-------- |:------------------- |:------------------- |:-------- |:------------------ |
| Preview  | Preview Attachment  | View Permission     | eye      | Preview Attachment |
| Download | Download Attachment | View Permission     | download | Download File      |
| Delete   | Delete Attachment   | Delete Permission   | trash-2  | Delete Attachment  |

---

### 2.1.5e. DataTable (Attachments) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| –    | –         | –          | –               | –              | –      | –       |

---

# 2.2 Right Section

## 2.2.1. Section (Return Summary)

Component: **Summary Card**

| Name                                | Formula / Source                   | Description              |
|:----------------------------------- |:---------------------------------- |:------------------------ |
| Total Products                      | Count(Return Products)             | Total Products Selected  |
| Total Return Quantity               | Sum(Return Quantity)               | Quantity Being Returned  |
| Total Product Value Before Discount | Sum(Product Value Before Discount) | Gross Product Value      |
| Total Discount                      | Sum(Discount Value)                | Total Discount           |
| Total Product Value After Discount  | Sum(Product Value After Discount)  | Discounted Product Value |
| Total Tax                           | Sum(Tax Value)                     | Total Tax                |
| Total Return Charges                | Sum(Return Charges)                | Additional Charges       |
| Round Off                           | Manual / Auto                      | Round Off Difference     |
| Net Return Value                    | Final Calculated Amount            | Final Return Value       |

---

## 2.2.2. Section (Vendor Information)

Component: **Summary Card**

| Name                      | Source                 |
|:------------------------- |:---------------------- |
| Vendor Name               | Selected Vendor        |
| Vendor Code               | Vendor Master          |
| Vendor GSTIN              | Vendor Master          |
| Credit Days               | Vendor Master          |
| Credit Limit              | Vendor Master          |
| Outstanding Amount        | Vendor Ledger          |
| Total Purchase Returns    | Purchase Return Module |
| Last Purchase Return Date | Purchase Return Module |
| Last Vendor Payment       | Vendor Payment Module  |

---

## 2.2.3. Section (Workflow Timeline)

Component: **StageHistoryViewer**

| Name       | Content         |
|:---------- |:--------------- |
| Stage      | Workflow Stage  |
| Set At     | Date & Time     |
| Set By     | User            |
| Remarks    | Stage Remarks   |
| Attachment | Supporting File |

---

# 3. Add Return Products Popup

This popup allows user to select products from GRN and/or Purchase Invoice.

Business Rules:

- Products are filtered by selected Vendor.
- Only products having Available Return Quantity are displayed.
- Same product can appear multiple times if Batch is different.
- Multiple products can be selected together.

---

## 3.1 Search Filters

| Name               | Component         |
|:------------------ |:----------------- |
| Product            | Lookup            |
| Batch              | Lookup            |
| Reference Type     | Multi Select      |
| Reference Number   | Search            |
| Date Range         | Date Range Picker |
| Available Quantity | Range             |

---

## 3.2 Product List

### Columns

| Name                      |
|:------------------------- |
| Select                    |
| Product                   |
| Variety                   |
| Batch                     |
| Reference Type            |
| Reference Number          |
| Received Quantity         |
| Previously Returned       |
| Available Return Quantity |
| Purchase Rate             |

---
