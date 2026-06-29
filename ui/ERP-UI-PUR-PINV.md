# HappyERP Purchase Invoice (PI) UI/UX Documentation

This document gives UI details of all pages used in Purchase Invoice module.

# Pages

| S. No. | Name                        | Type  | Purpose                                                                                             |
| ------ | --------------------------- | ----- | --------------------------------------------------------------------------------------------------- |
| 1.     | Purchase Invoice List       | Page  | - View all Purchase Invoices<br/>- Search/filter invoices<br/>- Perform quick actions on invoice(s) |
| 2.     | Add/Modify Purchase Invoice | Page  | - View details of the invoice<br/>- Add/modify any data of current invoice                          |
| 3.     | Add/Modify Invoice Product  | Popup | - Add/modify invoice product details in current invoice                                             |

# 1. Purchase Invoice List Page UI

This page shows list of Purchase Invoices in tabular format. The Purchase Invoices appearing in the list depend upon current user's permission and applied filters.

## 1.1. Purchase Invoice List Data Table

### 1.1a. DataTable (Purchase Invoice List) - Columns

| Name           | Content                        | Type     | On Click                    | Card Placement | Tooltip / On Hover            |
|:-------------- |:------------------------------ |:-------- |:--------------------------- |:-------------- |:----------------------------- |
| Invoice Number | Unique Purchase Invoice Number | Text     | Open Invoice View/Edit Page | Title          | View Purchase Invoice Details |
| Invoice Date   | Vendor Invoice Date            | Date     | –                           | Subtitle       | Vendor Invoice Date           |
| Vendor         | Vendor Name                    | Text     | Open Vendor Profile         | Body           | View Vendor Details           |
| Reference Type | PO / GRN / Direct Purchase     | Badge    | Open Reference Details      | Body           | Source Of Purchase Invoice    |
| Total Quantity | Total Invoice Quantity         | Number   | –                           | Body           | Total Quantity In Invoice     |
| Net Amount     | Final Invoice Amount           | Currency | –                           | Body           | Total Invoice Value           |
| Due Date       | Payment Due Date               | Date     | –                           | Body           | Payment Due Date              |
| Stage          | Current Workflow Stage         | Status   | Open Workflow Timeline      | Trailing       | Current Invoice Status        |
| Created By     | User Who Created Invoice       | User     | Open User Profile           | Footer         | Created By User               |
| Updated At     | Last Updated Date & Time       | DateTime | –                           | Footer         | Last Modification Date        |

---

### 1.1b. DataTable (Purchase Invoice List) - Toolbar Config

| Feature          | Settings                                  | On Click                                   |
|:---------------- |:----------------------------------------- |:------------------------------------------ |
| Search           | Yes                                       | Search Purchase Invoice records            |
| View Toggle      | Yes                                       | Switch between available DataTable layouts |
| Column Selection | Yes                                       | Show/Hide columns                          |
| Group By         | Yes                                       | Group records by selected column           |
| Filter           | Yes                                       | Open filter panel                          |
| Export           | Yes<br/>Filename: `purchase-invoice-list` | Export Purchase Invoice list               |
| Share            | Yes                                       | Share current filtered view                |
| Full Screen      | Yes                                       | Toggle full-screen view                    |
| Add              | Yes<br/>Page to open: `purchase_invoice`  | Open Purchase Invoice page in create mode  |

---

### 1.1c. DataTable (Purchase Invoice List) - Config

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

### 1.1d. DataTable (Purchase Invoice List) - RowAction Menu

| Name                   | Action                                                                                                                                                                                                                          | Visibility Criteria                               | Icon               | Tooltip                  |
|:---------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |:------------------------------------------------- |:------------------ |:------------------------ |
| View                   | Open page: `purchase_invoice`                                                                                                                                                                                                   | View Permission                                   | eye                | View Invoice             |
| Modify                 | Open page: `purchase_invoice`                                                                                                                                                                                                   | Modify Permission + Stage Allows Edit             | pencil             | Modify Invoice           |
| Create Vendor Payment  | Open page: `vendor_payment` with current invoice selected                                                                                                                                                                       | Create Vendor Payment Permission + Approved Stage | wallet             | Create Vendor Payment    |
| Create Purchase Return | Open page: `purchase_return` with current invoice selected                                                                                                                                                                      | Create Purchase Return Permission                 | rotate-ccw         | Create Purchase Return   |
| Set as <Stage_1>       | Set stage to Stage_1                                                                                                                                                                                                            | Allowed stage based on current stage              | arrow-right-circle | Change Workflow Stage    |
| Set as <Stage_2>       | Set stage to Stage_2                                                                                                                                                                                                            | Allowed stage based on current stage              | arrow-right-circle | Change Workflow Stage    |
| Undo Stage             | Move back to last stage                                                                                                                                                                                                         | Permission to rollback                            | undo-2             | Rollback Current Stage   |
| Duplicate              | Create copy of current invoice.<br/>Copies: Vendor, PO/GRN References, Products, Taxes, Payment Terms, Remarks.<br/>Resets: Invoice Number, Vendor Invoice Number, Payments, Audit Logs, Stage History, Workflow Stage (Draft). | Add Permission                                    | copy               | Create Duplicate Invoice |
| Delete                 | Show confirmation and delete invoice                                                                                                                                                                                            | Delete Permission + Allowed Stage                 | trash-2            | Delete Invoice           |

---

### 1.1e. DataTable (Purchase Invoice List) - Filters Fields

| Name            | Component         | Depends On   | Possible Values                                       | Default Values | Output            | Tooltip                      |
|:--------------- |:----------------- |:------------ |:----------------------------------------------------- |:-------------- |:----------------- |:---------------------------- |
| Date Range      | Date Range Picker | –            | Any Date Range                                        | Today          | from_date,to_date | Filter Invoices By Date      |
| Organisation    | Multi Select      | –            | Allowed Organisations                                 | All            | organisation_ids  | –                            |
| Branch          | Multi Select      | Organisation | Active Branches                                       | All            | branch_ids        | –                            |
| Vendor          | Multi Select      | –            | Active Vendors                                        | All            | vendor_ids        | Filter By Vendor             |
| Product         | Multi Select      | –            | Active Products                                       | All            | product_ids       | Filter By Purchased Products |
| Product Variety | Multi Select      | Product      | Product Varieties                                     | All            | variety_ids       | Filter By Product Variety    |
| Reference Type  | Multi Select      | –            | PO, GRN, Direct Purchase, Mixed                       | All            | reference_types   | Invoice Source Type          |
| Stage           | Multi Select      | –            | Draft, Submitted, Verified, Approved, Paid, Cancelled | Open Stages    | stage_ids         | Filter By Workflow Stage     |
| Payment Status  | Multi Select      | –            | Pending, Partial, Paid                                | All            | payment_statuses  | Filter By Payment Status     |
| Created By      | User Lookup       | –            | Active Users                                          | Current User   | user_ids          | Filter By Creator            |

# 2. Purchase Invoice Entry Page UI

Fields in the main form are grouped into different sections.

```text
2.1. Left: Main Form

- General Information
- Reference Documents
- Invoice Product Details
- Tax Details
- Payment Terms
- Attachments

2.2. Right: Summary and Timeline

- Invoice Summary
- Workflow Timeline
- Payment Summary
```

## 2.1. Left Part

### 2.1.1. Section (General Information)

| Name/Label            | Data Source           | Type/Component | Component Specific Information                       | Required | Read Only | Validations               | On Change                | Description            | Tooltip                          |
|:--------------------- |:--------------------- |:-------------- |:---------------------------------------------------- |:-------- |:--------- |:------------------------- |:------------------------ |:---------------------- |:-------------------------------- |
| Invoice Number        | invoice_number        | Text           | Prefix=PINV, Auto Increment, Format=PINV-YYYY-000001 | Yes      | Yes       | Must Be Unique            | Auto Generated           | Unique Invoice Number  | System Generated Invoice Number  |
| Invoice Date          | invoice_date          | DatePicker     | Single Date Picker                                   | Yes      | No        | Cannot Be Future Date     | –                        | Purchase Invoice Date  | Internal Invoice Date            |
| Vendor Invoice Number | vendor_invoice_number | Text           | Max Length 50                                        | Yes      | No        | Unique Per Vendor         | –                        | Vendor Invoice Number  | Vendor Provided Invoice Number   |
| Vendor Invoice Date   | vendor_invoice_date   | DatePicker     | Single Date Picker                                   | Yes      | No        | Cannot Be Future Date     | Update Due Date          | Vendor Invoice Date    | Date Mentioned On Vendor Invoice |
| Reference Type        | reference_type        | Select         | PO, GRN, Direct Purchase, Mixed                      | Yes      | No        | Valid Reference Type      | Load References          | Invoice Source         | Source Of Purchase Invoice       |
| Vendor                | vendor_id             | Select         | Searchable Async Lookup                              | Yes      | No        | Vendor Must Be Active     | Load Vendor Details      | Vendor Selection       | Select Vendor                    |
| Currency              | currency_id           | Select         | Auto Filled From Reference Or Organisation Default   | Yes      | No        | Valid Currency            | Update Amount Fields     | Transaction Currency   | Currency Used In Invoice         |
| Exchange Rate         | exchange_rate         | Number Input   | Decimal Allowed                                      | No       | No        | Must Be Greater Than Zero | Update Converted Amounts | Currency Exchange Rate | Exchange Rate To Base Currency   |
| Remarks               | remarks               | RichTextBox    | –                                                    | No       | No        | –                         | –                        | General Remarks        | Additional Notes                 |

---

### 2.1.2. Section (Reference Documents)

This section allows linking Purchase Invoice with one or multiple Purchase Orders, Goods Receipt Notes, or Direct Purchases.

Supported Cases:

- Purchase Invoice with PO without GRN
- Purchase Invoice with GRN without PO
- Direct Purchase Invoice (Without PO & without GRN)
- Mixed Invoice (PO + GRN + Direct Products)

Business Rules:

- Multiple POs are allowed.
- Multiple GRNs are allowed.
- All selected references must belong to the same Vendor.
- Direct Purchase products require approval before payment.
- Mixed mode invoices are supported.

| Name/Label            | Data Source           | Type/Component | Component Specific Information                | Required | Read Only | Validations                            | On Change            | Description            | Tooltip                            |
|:--------------------- |:--------------------- |:-------------- |:--------------------------------------------- |:-------- |:--------- |:-------------------------------------- |:-------------------- |:---------------------- |:---------------------------------- |
| Purchase Orders       | po_ids                | Multi Select   | Searchable, Async Lookup                      | No       | No        | All POs Must Belong To Same Vendor     | Load PO Products     | Linked Purchase Orders | Select One Or Multiple POs         |
| Goods Receipt Notes   | grn_ids               | Multi Select   | Searchable, Async Lookup                      | No       | No        | All GRNs Must Belong To Same Vendor    | Load GRN Products    | Linked GRNs            | Select One Or Multiple GRNs        |
| Allow Direct Purchase | allow_direct_purchase | Toggle         | Yes / No                                      | Yes      | No        | Purchase Manager Permission Required   | Show Direct Products | Allow Direct Products  | Add Products Without PO Or GRN     |
| Vendor                | vendor_id             | Auto Filled    | Loaded From PO/GRN Or Manual Selection        | Yes      | No        | Vendor Must Match All References       | Auto Populate        | Vendor Information     | Vendor For Current Invoice         |
| Currency              | currency_id           | Auto Filled    | Loaded From Reference Or Organisation Default | Yes      | Yes       | All References Must Have Same Currency | Auto Populate        | Transaction Currency   | Currency Used In Invoice           |
| Total PO Value        | total_po_value        | Currency       | Calculated Field                              | No       | Yes       | –                                      | Auto Calculate       | Total PO Value         | Sum Of Selected Purchase Orders    |
| Total GRN Value       | total_grn_value       | Currency       | Calculated Field                              | No       | Yes       | –                                      | Auto Calculate       | Total GRN Value        | Sum Of Selected GRNs               |
| Total Direct Value    | total_direct_value    | Currency       | Calculated Field                              | No       | Yes       | –                                      | Auto Calculate       | Direct Purchase Value  | Sum Of Direct Purchase Products    |
| Direct Purchase Stage | direct_purchase_stage | Status         | Pending, Approved, Rejected                   | No       | Yes       | Required If Direct Products Exist      | Auto Update          | Direct Purchase Status | Approval Status Of Direct Products |

---

### 2.1.3. Section (Invoice Product Details)

Component Type: DataTable

This section captures all products included in the Purchase Invoice.

Products may come from:

- Purchase Orders
- Goods Receipt Notes
- Direct Purchase Entries
- Mixed Sources

---

### 2.1.3a. DataTable (Invoice Product Details) - Columns

| Name/Label                    | Data Source                                                     | Type/Component | Component Specific Information                           | Required | Read Only | Validations                                    | On Change            | Description              | Tooltip                                                    |
|:----------------------------- |:--------------------------------------------------------------- |:-------------- |:-------------------------------------------------------- |:-------- |:--------- |:---------------------------------------------- |:-------------------- |:------------------------ |:---------------------------------------------------------- |
| Receipt Source                | receipt_source                                                  | Select         | Single Select, Possible Values: PO, GRN, Direct Purchase | Yes      | No        | Valid Source Required                          | Load Reference Data  | Product Source           | Source Of Product                                          |
| Reference Number              | reference_number                                                | Lookup         | Based On Receipt Source                                  | No       | Yes       | Must Exist In Selected Reference               | Load Product Details | Source Document Number   | PO/GRN Reference                                           |
| Product                       | product_id                                                      | Select         | Single Select, Searchable, Async Lookup                  | Yes      | No        | Product Must Be Active                         | Load Product Details | Product Selection        | Select Product                                             |
| Variety                       | variety_id                                                      | Select         | Single Select, Filtered By Product                       | No       | No        | –                                              | Load Variety Details | Product Variety          | Select Product Variety                                     |
| Pack                          | pack_id                                                         | Select         | Single Select, Filtered By Product                       | No       | No        | –                                              | Load Pack Details    | Product Pack             | Select Product Packaging                                   |
| Quantity                      | quantity + uom_id                                               | Input          | Suffix: uom_short_name; Decimal Allowed, Min=0           | Yes      | No        | Quantity Must Be Greater Than Zero             | Recalculate Amounts  | Invoice Quantity         | Quantity Included In Invoice                               |
| Rate                          | rate                                                            | Input          | Decimal Currency Input                                   | Yes      | No        | Rate Must Be Greater Than Zero                 | Recalculate Amounts  | Purchase Rate            | Product Purchase Rate                                      |
| Discount                      | discount                                                        | Input          | Fixed Amount Or Percentage Or Unit                       | No       | No        | Discount Cannot Exceed Line Amount             | Recalculate Amounts  | Line Discount            | Product Level Discount                                     |
| Discount Type                 | discount_type                                                   | Select         | Single Select, Possible Values: Unit, Percentage, Fixed  | Yes      | No        | Valid Discount Type                            | Recalculate Amounts  | Discount Type            | Discount Calculation Method                                |
| Product Amount After Discount | product_amount = product_value_before_discount - discount_value | Text           | –                                                        | Yes      | Yes       | product_value_before_discount - discount_value | Recalculate Amounts  | Calculate After Discount | Discounted Product Value                                   |
| Tax                           | tax_amount <br/>= SUM(taxes[].tax_value)                        | Text           | –                                                        | No       | Yes       | Valid Tax Configuration Required               | Recalculate Amounts  | Tax Amount               | Tax Breakup<br/>CGST<br/>SGST<br/>IGST<br/>TDS             |
| Total                         | line_total = product_amount + tax_amount                        | Text           | –                                                        | Yes      | Yes       | Formula Validation                             | Auto Calculate       | Total Amount             | Amount Breakup<br/>Product Amount<br/>Tax Amount<br/>Total |

---

### 2.1.3b. DataTable (Invoice Product Details) - Toolbar Config

| Feature     | Settings | On Click                      |
|:----------- |:-------- |:----------------------------- |
| Search      | Yes      | Search Invoice Products       |
| Filter      | No       | Open Product Filters          |
| Export      | No       | Export Product Details        |
| Full Screen | Yes      | Toggle Full Screen            |
| Add         | Yes      | Open Popup: `invoice_product` |

---

### 2.1.3c. DataTable (Invoice Product Details) - Config

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

### 2.1.3d. DataTable (Invoice Product Details) - RowAction Menu

| Name      | Action                        | Visibility Criteria | Icon   | Tooltip                |
|:--------- |:----------------------------- |:------------------- |:------ |:---------------------- |
| Modify    | Open Popup: `invoice_product` | Modify Permission   | pencil | Modify Product Details |
| Duplicate | Copy Product Row              | Add Permission      | copy   | Duplicate Product      |
| Delete    | Delete Product Row            | Delete Permission   | trash  | Remove Product         |

---

### 2.1.3e. DataTable (Invoice Product Details) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| -    | -         | –          | -               | -              | -      | -       |

---

### 2.1.4. Section (Tax Details)

Component Type: DataTable

This section captures all taxes applicable on Purchase Invoice products.

Taxes can be loaded from:

- Product Master
- Vendor Master
- Purchase Order
- GRN
- Manually Added Taxes

Business Rules:

- Multiple taxes are allowed.
- Tax values are auto-calculated.
- Tax amount contributes to Net Invoice Amount.
- Manual override requires permission.

---

### 2.1.4a. DataTable (Tax Details) - Columns

| Name/Label     | Data Source    | Type/Component | Component Specific Information | Required | Read Only | Validations               | On Change            | Description     | Tooltip                   |
|:-------------- |:-------------- |:-------------- |:------------------------------ |:-------- |:--------- |:------------------------- |:-------------------- |:--------------- |:------------------------- |
| Product        | product_id     | Lookup         | Linked Invoice Products        | Yes      | Yes       | Product Must Exist        | Load Tax Rules       | Invoice Product | Product For Current Tax   |
| Tax            | tax_id         | Lookup         | Active Tax Masters             | Yes      | No        | Valid Tax                 | Calculate Tax Amount | Applied Tax     | Tax Applied On Product    |
| Tax Type       | tax_type       | Badge          | CGST, SGST, IGST, TDS, CESS    | Yes      | Yes       | –                         | –                    | Tax Category    | Type Of Tax               |
| Taxable Amount | taxable_amount | Currency       | Auto Calculated                | Yes      | Yes       | Must Be Greater Than Zero | Auto Calculate       | Taxable Value   | Amount Before Tax         |
| Tax Percentage | tax_percentage | Number Input   | Decimal Allowed                | Yes      | No        | Between 0 To 100          | Calculate Tax Amount | Tax Rate        | Applicable Tax Percentage |
| Tax Amount     | tax_amount     | Currency       | Auto Calculated                | Yes      | Yes       | Formula Validation        | Auto Calculate       | Tax Value       | Calculated Tax Amount     |
| Remarks        | remarks        | Text Area      | Multi Line                     | No       | No        | –                         | –                    | Tax Remarks     | Additional Notes          |

---

### 2.1.4b. DataTable (Tax Details) - Toolbar Config

| Feature     | Settings | On Click           |
|:----------- |:-------- |:------------------ |
| Search      | No       | -                  |
| Filter      | No       | -                  |
| Export      | No       | -                  |
| Full Screen | Yes      | Toggle Full Screen |
| Add         | No       | -                  |

---

### 2.1.4c. DataTable (Tax Details) - Config

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

### 2.1.4d. DataTable (Tax Details) - RowAction Menu

| Name | Action | Visibility Criteria | Icon | Tooltip |
|:---- |:------ |:------------------- |:---- |:------- |
| -    | -      | -                   | -    | -       |

---

### 2.1.4e. DataTable (Tax Details) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| -    | -         | –          | -               | -              | -      | -       |

---

### 2.1.5. Section (Payment Terms)

This section captures payment terms agreed with Vendor.

Business Rules:

* Payment Terms can be inherited from Purchase Order.
* Supports Immediate Payment.
* Supports Credit Purchases.
* Supports Multiple Payment Schedules.
* Supports Early Payment Discounts.
* Supports Late Payment Penalties.
* Supports TDS Deduction.

| Name/Label             | Data Source       | Type/Component     | Component Specific Information               | Required | Read Only | Validations                        | On Change              | Description                       | Tooltip                       |
|:---------------------- |:----------------- |:------------------ |:-------------------------------------------- |:-------- |:--------- |:---------------------------------- |:---------------------- |:--------------------------------- |:----------------------------- |
| Payment Mode           | payment_mode      | AppMultiSelect     | Cash, Bank Transfer, UPI, Cheque, NEFT, RTGS | No       | No        | Valid Payment Mode Required        | Update Payment Rules   | Allowed Payment Methods           | Accepted Payment Modes        |
| Base Date              | base_date         | AppSelect          | Invoice Date, GRN Date, PO Date              | Yes      | No        | Valid Option Required              | Recalculate Due Date   | Base Date For Payment Calculation | Select Payment Reference Date |
| Credit Days            | credit_days       | AppNumberInput     | Integer Only                                 | Yes      | No        | Must Be Greater Than Or Equal To 0 | Recalculate Due Date   | Credit Period In Days             | Vendor Credit Period          |
| Due Date               | due_date          | AppDateDisplay     | Auto Calculated                              | Yes      | Yes       | Formula Validation                 | Auto Calculate         | Final Due Date                    | Payment Due Date              |
| TDS Applicable         | tds_applicable    | AppToggle          | Yes / No                                     | No       | No        | –                                  | Show/Hide TDS Fields   | Indicates TDS Deduction           | Is TDS Applicable?            |
| TDS Category           | tds_category_id   | AppLookup          | Single Select, Source=TDS Master             | No       | No        | Required If TDS Applicable         | Load TDS Percentage    | Applicable TDS Category           | Select TDS Category           |
| TDS Category Name      | tds_category_name | AppTextDisplay     | Auto Filled                                  | No       | Yes       | Required If TDS Applicable         | –                      | TDS Category Name                 | Selected TDS Category Name    |
| TDS Percentage         | tds_percentage    | AppPercentageInput | Auto Filled From TDS Category                | No       | Yes       | 0 - 100%                           | Recalculate TDS Amount | TDS Deduction Percentage          | Applicable TDS Rate           |
| TDS Amount             | tds_amount        | AppCurrencyDisplay | Auto Calculated                              | No       | Yes       | Formula Validation                 | Auto Calculate         | TDS Amount                        | Calculated TDS Deduction      |
| Payment Schedule       | –                 | DataTable          | Multiple Installments Supported              | No       | No        | Total Percentage Must Equal 100%   | Recalculate Due Dates  | Vendor Payment Schedule           | Payment Milestones            |
| Early Payment Discount | –                 | DataTable          | Multiple Discount Rules Supported            | No       | No        | Valid Discount Configuration       | Recalculate Net Amount | Early Payment Benefits            | Discount On Early Payments    |
| Late Payment Penalty   | –                 | DataTable          | Multiple Penalty Rules Supported             | No       | No        | Valid Penalty Configuration        | Recalculate Penalties  | Late Payment Charges              | Penalty For Delayed Payments  |

---

### 2.1.5a. DataTable (Payment Schedule) - Columns

| Name/Label  | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations           | On Change          | Description        | Tooltip                      |
|:----------- |:----------- |:-------------- |:------------------------------ |:-------- |:--------- |:--------------------- |:------------------ |:------------------ |:---------------------------- |
| Base Date   | base_date   | Select         | Invoice Date, GRN Date         | Yes      | No        | Valid Base Date       | Calculate Due Date | Reference Date     | Base Date For Schedule       |
| Days Within | days_within | Number Input   | Integer Only                   | Yes      | No        | Cannot Be Negative    | Calculate Due Date | Credit Days        | Days From Base Date          |
| Due Date    | due_date    | Date Display   | Auto Calculated                | Yes      | Yes       | Formula Validation    | Auto Calculate     | Due Date           | Payment Due Date             |
| Percentage  | percentage  | Number Input   | Decimal Allowed                | Yes      | No        | Total Must Equal 100% | Calculate Amount   | Payment Percentage | Percentage Of Invoice Amount |
| Amount      | amount      | Currency       | Auto Calculated                | Yes      | Yes       | Formula Validation    | Auto Calculate     | Payment Amount     | Amount To Be Paid            |

---

### 2.1.5b. DataTable (Payment Schedule) - Toolbar Config

| Feature     | Settings | On Click             |
|:----------- |:-------- |:-------------------- |
| Search      | No       | –                    |
| Filter      | No       | –                    |
| Export      | No       | –                    |
| Full Screen | Yes      | Toggle Full Screen   |
| Add         | Yes      | Add Payment Schedule |

---

### 2.1.5c. DataTable (Payment Schedule) - Config

| Feature        | Settings |
|:-------------- |:-------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | No       |
| Pagination     | No       |

---

### 2.1.5d. DataTable (Payment Schedule) - RowAction Menu

| Name      | Action                  | Visibility Criteria | Icon   | Tooltip            |
|:--------- |:----------------------- |:------------------- |:------ |:------------------ |
| Modify    | Edit Payment Schedule   | Modify Permission   | pencil | Modify Schedule    |
| Duplicate | Copy Payment Schedule   | Add Permission      | copy   | Duplicate Schedule |
| Delete    | Delete Payment Schedule | Delete Permission   | trash  | Remove Schedule    |

---

### 2.1.5e. DataTable (Payment Schedule) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| –    | –         | –          | –               | –              | –      | –       |

---

### 2.1.6. Section (Attachments)

This section stores all supporting documents related to Purchase Invoice.

Examples:

- Vendor Invoice PDF
- E-Way Bill
- Transport Receipt
- Delivery Challan
- Tax Documents
- Other Supporting Documents

Business Rules:

- Multiple attachments are allowed.
- Maximum file size depends on Organisation Settings.
- Only authorized users can delete attachments.
- At least one Vendor Invoice attachment is recommended.

| Name/Label  | Data Source                    | Type/Component   | Component Specific Information                       | Required | Read Only | Validations | On Change | Description | Tooltip |
|:----------- |:------------------------------ |:---------------- |:---------------------------------------------------- |:-------- |:--------- |:----------- |:--------- |:----------- |:------- |
| Attachments | purchase_invoice.`attachments` | AttachmentViewer | Prefix=PINV, Auto Increment, Format=PINV-YYYY-000001 | No       | No        | -           | -         | PI Media    | -       |

---

## 2.2. Right Side Widgets

### 2.2.1. Invoice Summary

Component to use: Summary Card

| Name               | Content                  |
|:------------------ |:------------------------ |
| Total Products     | Count(products)          |
| Total Quantity     | Sum(quantity)            |
| Gross Amount       | Sum(product_amount)      |
| Discount Amount    | Sum(discount_amount)     |
| Tax Amount         | Sum(tax_amount)          |
| TDS Amount         | payment_terms.tds_amount |
| Net Invoice Amount | Final Payable Amount     |
| Currency           | currency_short_name      |
| Payment Due Date   | payment_terms.due_date   |

---

### 2.2.2. Payment Summary

Component to use: Summary Card

| Name                 | Content                        |
|:-------------------- |:------------------------------ |
| Total Invoice Amount | Net Invoice Amount             |
| Total Paid Amount    | Sum(vendor_payments.amount)    |
| Remaining Amount     | Net Amount - Paid Amount       |
| Payment Status       | Pending / Partial / Paid       |
| Last Payment Date    | Latest Vendor Payment Date     |
| Next Due Date        | Next Payment Schedule Due Date |

---

### 2.2.3. Vendor Summary

Component to use: Summary Card

| Name                 | Content                    |
|:-------------------- |:-------------------------- |
| Vendor Name          | party_display_name         |
| Vendor Mobile        | party_mobile               |
| Vendor GSTIN         | party_gstin                |
| Vendor Outstanding   | Total Vendor Outstanding   |
| Last Purchase Date   | Last Purchase Invoice Date |
| Total Purchase Value | Lifetime Purchase Value    |

---

### 2.2.4. Workflow Timeline

Component to use: StageHistoryViewer

| Name       | Content               |
|:---------- |:--------------------- |
| Stage      | Current Stage Name    |
| Set At     | Date & Time           |
| Set By     | User Name             |
| Remarks    | Stage Remarks         |
| Attachment | Supporting Attachment |

---

# Purchase Invoice Business Scenarios

The following scenarios are supported:

```text
1. Purchase Invoice With Purchase Order
PO
↓
Purchase Invoice

2. Purchase Invoice With GRN
PO
↓
GRN
↓
Purchase Invoice

3. Purchase Invoice Without GRN
PO
↓
Purchase Invoice

4. Direct Purchase Invoice
No PO
No GRN
↓
Purchase Invoice

5. Mixed Purchase Invoice
PO Products
+
Direct Purchase Products
↓
Single Purchase Invoice

6. Multiple Purchase Orders
PO-001
PO-002
PO-003
↓
Single Purchase Invoice

7. Multiple GRNs
GRN-001
GRN-002
↓
Single Purchase Invoice

8. Partial Invoice
GRN Value = ₹100,000
Invoice Value = ₹60,000

9. Multiple Invoices Against Same PO
PO-001
↓
Invoice-001
Invoice-002

10. Credit/Debit Adjustments
Purchase Invoice
↓
Debit Note / Credit Note
```

---

# Purchase Invoice Workflow

```text
Draft
↓
Submitted
↓
Verified
↓
Approved
↓
Partially Paid
↓
Paid
```

Alternative Flows:

```text
Draft
↓
Cancelled
```

```text
Submitted
↓
Rejected
```

---

# Purchase Invoice Module Completion Status

```text
✓ PI List Page
✓ General Information
✓ Reference Documents
✓ Invoice Product Details
✓ Tax Details
✓ Payment Terms
✓ Attachments
✓ Invoice Summary
✓ Payment Summary
✓ Vendor Summary
✓ Workflow Timeline
✓ Direct Purchase Support
✓ Mixed Purchase Support
✓ Multiple PO Support
✓ Multiple GRN Support
✓ Partial Invoice Support
✓ Credit/Debit Adjustment Support
```
