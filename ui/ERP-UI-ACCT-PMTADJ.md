# HappyERP Payment Adjustment (PMTADJ) UI/UX Documentation

This document gives UI details of all pages used in Payment Adjustment module.

# Pages

| S. No. | Name                            | Type  | Purpose                                                                                    |
| ------ | ------------------------------- | ----- | ------------------------------------------------------------------------------------------ |
| 1.     | Payment Adjustment List         | Page  | - View all Payment Adjustments<br/>- Search/filter adjustments<br/>- Perform quick actions |
| 2.     | Add/Modify Payment Adjustment   | Page  | - View adjustment details<br/>- Add/modify payment adjustment                              |
| 3.     | Add/Modify Adjustment Reference | Popup | - Add/modify payment adjustment reference                                                  |

---

# 1. Payment Adjustment List Page UI

This page shows list of Payment Adjustments in tabular format. The Payment Adjustments appearing in the list depend upon current user's permissions and applied filters.

---

## 1.1. Payment Adjustment List Data Table

### 1.1a. DataTable (Payment Adjustment List) - Columns

| Name              | Content                   | Type     | On Click                               | Card Placement | Tooltip / On Hover             |
|:----------------- |:------------------------- |:-------- |:-------------------------------------- |:-------------- |:------------------------------ |
| Adjustment Number | Payment Adjustment Number | Text     | Open Payment Adjustment View/Edit Page | Title          | View Payment Adjustment        |
| Adjustment Date   | Adjustment Date           | Date     | –                                      | Subtitle       | Date Of Adjustment             |
| Vendor            | Vendor Name               | Text     | Open Vendor Details                    | Body           | View Vendor                    |
| Adjustment Type   | Adjustment Type           | Badge    | –                                      | Body           | Type Of Adjustment             |
| Payment Number    | Vendor Payment Number     | Text     | Open Vendor Payment                    | Body           | Linked Vendor Payment          |
| Invoice Count     | Linked Purchase Invoices  | Number   | Open References                        | Body           | Total Linked Purchase Invoices |
| Adjustment Amount | Adjustment Amount         | Currency | –                                      | Body           | Total Adjustment Value         |
| Status            | Current Status            | Status   | Open Workflow Timeline                 | Trailing       | Current Workflow Status        |
| Created By        | User Name                 | User     | Open User Profile                      | Footer         | Created By                     |
| Updated At        | Last Updated Date Time    | DateTime | –                                      | Footer         | Last Updated                   |

---

### 1.1b. DataTable (Payment Adjustment List) - Toolbar Config

| Feature          | Settings                                    | On Click                        |
|:---------------- |:------------------------------------------- |:------------------------------- |
| Search           | Yes                                         | Search Payment Adjustments      |
| View Toggle      | Yes                                         | Switch Between Available Views  |
| Column Selection | Yes                                         | Show/Hide Columns               |
| Group By         | Yes                                         | Group By Vendor / Type / Status |
| Filter           | Yes                                         | Open Filter Panel               |
| Export           | Yes<br/>Filename: `payment-adjustment-list` | Export Payment Adjustments      |
| Share            | Yes                                         | Share Current View              |
| Full Screen      | Yes                                         | Toggle Full Screen              |
| Add              | Yes<br/>Page to open: `payment_adjustment`  | Open Payment Adjustment         |

---

### 1.1c. DataTable (Payment Adjustment List) - Config

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

### 1.1d. DataTable (Payment Adjustment List) - RowAction Menu

| Name                  | Action                                                                                                                                                                      | Visibility Criteria                   | Icon               | Tooltip                   |
|:--------------------- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |:------------------------------------- |:------------------ |:------------------------- |
| View                  | Open page: `payment_adjustment`                                                                                                                                             | View Permission                       | eye                | View Payment Adjustment   |
| Modify                | Open page: `payment_adjustment`                                                                                                                                             | Modify Permission + Stage Allows Edit | pencil             | Modify Payment Adjustment |
| Create Vendor Payment | Open page: `vendor_payment`                                                                                                                                                 | Create Vendor Payment Permission      | wallet             | Create Vendor Payment     |
| Set as <Stage_1>      | Set stage to Stage_1                                                                                                                                                        | Allowed Stage Based On Current Stage  | arrow-right-circle | Change Workflow Stage     |
| Set as <Stage_2>      | Set stage to Stage_2                                                                                                                                                        | Allowed Stage Based On Current Stage  | arrow-right-circle | Change Workflow Stage     |
| Undo Stage            | Move back to last stage                                                                                                                                                     | Permission To Rollback                | undo-2             | Rollback Current Stage    |
| Duplicate             | Create copy of current adjustment.<br/>Copies: Vendor, References, Adjustment Details, Remarks.<br/>Resets: Adjustment Number, Workflow Stage, Timeline, Audit Information. | Add Permission                        | copy               | Duplicate Adjustment      |
| Cancel                | Show confirmation and cancel adjustment                                                                                                                                     | Cancel Permission + Allowed Stage     | share-2            | Cancel Adjustment         |
| Delete                | Show confirmation and delete adjustment                                                                                                                                     | Delete Permission + Allowed Stage     | trash-2            | Delete Adjustment         |

---

### 1.1e. DataTable (Payment Adjustment List) - Filters Fields

| Name            | Component         | Depends On   | Possible Values                                                                            | Default Values | Output            | Tooltip                   |
|:--------------- |:----------------- |:------------ |:------------------------------------------------------------------------------------------ |:-------------- |:----------------- |:------------------------- |
| Date Range      | Date Range Picker | –            | Any Date Range                                                                             | Today          | from_date,to_date | Filter By Date            |
| Organisation    | Multi Select      | –            | Allowed Organisations                                                                      | All            | organisation_ids  | Filter By Organisation    |
| Branch          | Multi Select      | Organisation | Active Branches                                                                            | All            | branch_ids        | Filter By Branch          |
| Vendor          | Multi Select      | –            | Active Vendors                                                                             | All            | vendor_ids        | Filter By Vendor          |
| Adjustment Type | Multi Select      | –            | Excess Payment, Short Payment, Credit Note, Debit Note, Purchase Return, Manual, Round Off | All            | adjustment_types  | Filter By Adjustment Type |
| Payment Number  | Multi Select      | Vendor       | Vendor Payments                                                                            | All            | payment_ids       | Filter By Vendor Payment  |
| Stage           | Multi Select      | –            | Draft, Submitted, Verified, Approved, Completed, Cancelled                                 | Open Stages    | stage_ids         | Filter By Workflow Stage  |
| Created By      | User Lookup       | –            | Active Users                                                                               | Current User   | user_ids          | Filter By Creator         |

---

# 2. Payment Adjustment Entry Page UI

Fields in the main form are grouped in different sections.

```text
2.1 Left: Main Form

- General Information
- Payment References
- Adjustment Details
- Attachments

2.2 Right: Widgets

- Adjustment Summary
- Vendor Summary
- Workflow Timeline
```

## 2.1. Left Part

### 2.1.1. Section (General Information)

| Name/Label        | Data Source       | Type/Component | Component Specific Information                                                                                    | Required | Read Only | Validations           | On Change           | Description               | Tooltip                             |
|:----------------- |:----------------- |:-------------- |:----------------------------------------------------------------------------------------------------------------- |:-------- |:--------- |:--------------------- |:------------------- |:------------------------- |:----------------------------------- |
| Adjustment Number | adjustment_number | Text           | Prefix=PMTADJ, Auto Generated                                                                                     | Yes      | Yes       | Must Be Unique        | Auto Generate       | Payment Adjustment Number | System Generated Adjustment Number  |
| Adjustment Date   | adjustment_date   | Date Picker    | Single Date                                                                                                       | Yes      | No        | Cannot Be Future Date | –                   | Adjustment Date           | Date Of Adjustment                  |
| Organisation      | organisation_id   | Lookup         | Active Organisations                                                                                              | Yes      | No        | Valid Organisation    | Load Branches       | Organisation              | Organisation For Current Adjustment |
| Branch            | branch_id         | Lookup         | Filtered By Organisation                                                                                          | Yes      | No        | Valid Branch          | –                   | Branch                    | Branch For Current Adjustment       |
| Vendor            | vendor_id         | Lookup         | Active Vendors                                                                                                    | Yes      | No        | Vendor Must Be Active | Load Vendor Data    | Vendor                    | Vendor For Current Adjustment       |
| Adjustment Type   | adjustment_type   | Select         | Excess Payment, Short Payment, Purchase Return, Credit Note, Debit Note, Manual, Round Off, Ledger Reconciliation | Yes      | No        | Valid Adjustment Type | Show Related Fields | Adjustment Type           | Type Of Payment Adjustment          |
| Currency          | currency_id       | Lookup         | Organisation Currency                                                                                             | Yes      | No        | Valid Currency        | Update Amounts      | Currency                  | Transaction Currency                |
| Exchange Rate     | exchange_rate     | Number Input   | Decimal Allowed                                                                                                   | No       | No        | Greater Than Zero     | Recalculate Values  | Exchange Rate             | Currency Exchange Rate              |
| Remarks           | remarks           | Text Area      | Multi Line                                                                                                        | No       | No        | –                     | –                   | General Remarks           | Additional Notes                    |

---

## 2.1.2. Section (Payment References)

This section allows linking one or multiple Vendor Payments with current Payment Adjustment.

| Name/Label               | Data Source               | Type/Component | Component Specific Information | Required | Read Only | Validations                 | On Change            | Description        | Tooltip                                 |
|:------------------------ |:------------------------- |:-------------- |:------------------------------ |:-------- |:--------- |:--------------------------- |:-------------------- |:------------------ |:--------------------------------------- |
| Vendor Payments          | payment_ids               | Multi Select   | Searchable Async Lookup        | No       | No        | Same Vendor Required        | Load Payment Details | Vendor Payments    | Select One Or More Vendor Payments      |
| Allow Manual Adjustment  | allow_manual_adjustment   | Toggle         | Yes / No                       | Yes      | No        | Finance Permission Required | Show Manual Fields   | Manual Adjustment  | Allow Adjustment Without Vendor Payment |
| Vendor                   | vendor_id                 | Auto Filled    | Loaded From Selected Payment   | Yes      | Yes       | Same Vendor                 | Auto Populate        | Vendor             | Current Vendor                          |
| Currency                 | currency_id               | Auto Filled    | Loaded From Payment            | Yes      | Yes       | Same Currency               | Auto Populate        | Currency           | Transaction Currency                    |
| Total Payment Value      | total_payment_value       | Currency       | Calculated                     | No       | Yes       | –                           | Auto Calculate       | Total Payment      | Sum Of Selected Payments                |
| Total Adjusted Value     | total_adjusted_value      | Currency       | Calculated                     | No       | Yes       | –                           | Auto Calculate       | Already Adjusted   | Previous Adjustments                    |
| Available Balance        | available_balance         | Currency       | Calculated                     | No       | Yes       | –                           | Auto Calculate       | Available Balance  | Remaining Adjustment Amount             |
| Current Adjustment Value | current_adjustment_value  | Currency       | Calculated                     | Yes      | Yes       | Formula Validation          | Auto Calculate       | Current Adjustment | Total Current Adjustment                |
| Payment References       | payment_reference_details | DataTable      | Auto Loaded                    | No       | No        | –                           | –                    | Payment Details    | Linked Vendor Payments                  |

---

### 2.1.2a. DataTable (Payment References) - Columns

| Name/Label         | Data Source         | Type/Component | Component Specific Information | Required | Read Only | Validations                     | On Change           | Description         | Tooltip                  |
|:------------------ |:------------------- |:-------------- |:------------------------------ |:-------- |:--------- |:------------------------------- |:------------------- |:------------------- |:------------------------ |
| Payment Number     | payment_number      | Lookup         | Active Vendor Payments         | Yes      | No        | Valid Payment                   | Load Details        | Vendor Payment      | Vendor Payment Number    |
| Payment Date       | payment_date        | Date           | Auto Filled                    | Yes      | Yes       | –                               | –                   | Payment Date        | Vendor Payment Date      |
| Payment Type       | payment_type        | Badge          | Auto Filled                    | Yes      | Yes       | –                               | –                   | Payment Type        | Payment Category         |
| Total Payment      | total_payment_value | Currency       | Auto Filled                    | Yes      | Yes       | –                               | –                   | Total Payment       | Payment Amount           |
| Already Adjusted   | adjusted_value      | Currency       | Auto Filled                    | Yes      | Yes       | –                               | –                   | Previous Adjustment | Already Adjusted Amount  |
| Available Balance  | available_balance   | Currency       | Auto Filled                    | Yes      | Yes       | Cannot Be Negative              | –                   | Available Balance   | Remaining Balance        |
| Current Adjustment | adjustment_value    | Currency       | Decimal Allowed                | Yes      | No        | Cannot Exceed Available Balance | Recalculate Summary | Adjustment Amount   | Current Adjustment Value |
| Remarks            | remarks             | Text Area      | Multi Line                     | No       | No        | –                               | –                   | Remarks             | Reference Remarks        |

---

### 2.1.2b. DataTable (Payment References) - Toolbar Config

| Feature          | Settings | On Click                        |
|:---------------- |:-------- |:------------------------------- |
| Search           | Yes      | Search Payment References       |
| View Toggle      | No       | –                               |
| Column Selection | No       | –                               |
| Group By         | No       | –                               |
| Filter           | No       | –                               |
| Export           | No       | –                               |
| Share            | No       | –                               |
| Full Screen      | Yes      | Toggle Full Screen              |
| Add              | Yes      | Open Popup: `payment_reference` |

---

### 2.1.2c. DataTable (Payment References) - Config

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

### 2.1.2d. DataTable (Payment References) - RowAction Menu

| Name      | Action                   | Visibility Criteria | Icon    | Tooltip             |
|:--------- |:------------------------ |:------------------- |:------- |:------------------- |
| Modify    | Edit Payment Reference   | Modify Permission   | pencil  | Modify Reference    |
| Duplicate | Copy Payment Reference   | Add Permission      | copy    | Duplicate Reference |
| Delete    | Delete Payment Reference | Delete Permission   | trash-2 | Remove Reference    |

---

### 2.1.2e. DataTable (Payment References) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| –    | –         | –          | –               | –              | –      | –       |

### 2.1.3. Section (Adjustment Details)

This section manages actual payment settlement and adjustment transactions.

| Name/Label         | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations              | On Change      | Description        | Tooltip                    |
|:------------------ |:----------- |:-------------- |:------------------------------ |:-------- |:--------- |:------------------------ |:-------------- |:------------------ |:-------------------------- |
| Adjustment Details | adjustments | DataTable      | Multiple Adjustment Entries    | Yes      | No        | Minimum One Row Required | Update Summary | Adjustment Details | Payment Adjustment Details |

---

### 2.1.3a. DataTable (Adjustment Details) - Columns

| Name/Label         | Data Source       | Type/Component | Component Specific Information                                                                                    | Required | Read Only | Validations                     | On Change              | Description         | Tooltip                  |
|:------------------ |:----------------- |:-------------- |:----------------------------------------------------------------------------------------------------------------- |:-------- |:--------- |:------------------------------- |:---------------------- |:------------------- |:------------------------ |
| Adjustment Type    | adjustment_type   | Select         | Purchase Return, Credit Note, Debit Note, Excess Payment, Short Payment, Manual, Round Off, Ledger Reconciliation | Yes      | No        | Valid Adjustment Type           | Load Reference         | Adjustment Type     | Select Adjustment Type   |
| Reference Number   | reference_number  | Lookup         | Dynamic Based On Adjustment Type                                                                                  | No       | No        | Reference Must Exist            | Load Reference Details | Reference Document  | Linked Reference         |
| Reference Date     | reference_date    | Date           | Auto Filled                                                                                                       | No       | Yes       | –                               | –                      | Reference Date      | Document Date            |
| Original Amount    | original_amount   | Currency       | Auto Filled                                                                                                       | Yes      | Yes       | –                               | –                      | Original Amount     | Original Document Amount |
| Already Adjusted   | adjusted_amount   | Currency       | Auto Filled                                                                                                       | Yes      | Yes       | –                               | –                      | Previous Adjustment | Already Adjusted Amount  |
| Available Balance  | available_balance | Currency       | Auto Filled                                                                                                       | Yes      | Yes       | Cannot Be Negative              | –                      | Available Balance   | Remaining Balance        |
| Current Adjustment | adjustment_value  | Currency       | Decimal Allowed                                                                                                   | Yes      | No        | Cannot Exceed Available Balance | Recalculate Summary    | Current Adjustment  | Adjustment Amount        |
| Adjustment Reason  | adjustment_reason | Select         | Rate Difference, Quantity Difference, Damage, Duplicate Payment, Vendor Settlement, Finance Correction, Others    | Yes      | No        | Valid Reason                    | –                      | Adjustment Reason   | Reason Of Adjustment     |
| Remarks            | remarks           | Text Area      | Multi Line                                                                                                        | No       | No        | –                               | –                      | Remarks             | Additional Notes         |

---

### 2.1.3b. DataTable (Adjustment Details) - Toolbar Config

| Feature          | Settings | On Click                  |
|:---------------- |:-------- |:------------------------- |
| Search           | Yes      | Search Adjustment Details |
| View Toggle      | No       | –                         |
| Column Selection | No       | –                         |
| Group By         | No       | –                         |
| Filter           | No       | –                         |
| Export           | No       | –                         |
| Share            | No       | –                         |
| Full Screen      | Yes      | Toggle Full Screen        |
| Add              | Yes      | Add Adjustment Row        |

---

### 2.1.3c. DataTable (Adjustment Details) - Config

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

### 2.1.3d. DataTable (Adjustment Details) - RowAction Menu

| Name      | Action               | Visibility Criteria | Icon    | Tooltip              |
|:--------- |:-------------------- |:------------------- |:------- |:-------------------- |
| Modify    | Edit Adjustment      | Modify Permission   | pencil  | Modify Adjustment    |
| Duplicate | Duplicate Adjustment | Add Permission      | copy    | Duplicate Adjustment |
| Delete    | Delete Adjustment    | Delete Permission   | trash-2 | Remove Adjustment    |

---

### 2.1.3e. DataTable (Adjustment Details) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| –    | –         | –          | –               | –              | –      | –       |

---

## 2.1.4. Section (Attachments)

This section stores supporting documents related to Payment Adjustment.

| Name/Label  | Data Source                      | Type/Component   | Component Specific Information | Required | Read Only | Validations      | On Change | Description              | Tooltip |
|:----------- |:-------------------------------- |:---------------- |:------------------------------ |:-------- |:--------- |:---------------- |:--------- |:------------------------ |:------- |
| Attachments | payment_adjustment.`attachments` | AttachmentViewer | Multiple Files Allowed         | No       | No        | Valid File Types | –         | Payment Adjustment Media | -       |

---

### 2.1.4a. DataTable (Attachments) - Columns

| Name        | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations | On Change | Description       | Tooltip                |
|:----------- |:----------- |:-------------- |:------------------------------ |:-------- |:--------- |:----------- |:--------- |:----------------- |:---------------------- |
| Preview     | file_url    | File Preview   | Image, PDF, Excel, Docs        | No       | Yes       | –           | –         | File Preview      | Preview Attachment     |
| File Name   | file_name   | Text           | –                              | Yes      | Yes       | –           | –         | File Name         | Uploaded File Name     |
| File Type   | file_type   | Badge          | PDF, JPG, PNG, XLSX            | Yes      | Yes       | –           | –         | File Type         | Attachment Type        |
| Uploaded By | uploaded_by | User Lookup    | Auto Filled                    | Yes      | Yes       | –           | –         | Uploaded User     | User Who Uploaded File |
| Actions     | –           | Action Menu    | Preview, Download, Delete      | No       | No        | –           | –         | Available Actions | Attachment Operations  |

---

### 2.1.4b. DataTable (Attachments) - Toolbar Config

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

### 2.1.4c. DataTable (Attachments) - Config

| Feature        | Settings |
|:-------------- |:-------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | No       |
| Sorting        | Yes      |
| Pagination     | No       |

---

### 2.1.4d. DataTable (Attachments) - RowAction Menu

| Name     | Action              | Visibility Criteria | Icon     | Tooltip            |
|:-------- |:------------------- |:------------------- |:-------- |:------------------ |
| Preview  | Preview Attachment  | View Permission     | eye      | Preview Attachment |
| Download | Download Attachment | View Permission     | download | Download File      |
| Delete   | Delete Attachment   | Delete Permission   | trash-2  | Remove Attachment  |

---

### 2.1.4e. DataTable (Attachments) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| –    | –         | –          | –               | –              | –      | –       |

## 2.2. Right Side Widgets

### 2.2.1. Adjustment Summary

Component to use: `SummaryCard`

| Name                    | Content                           |
|:----------------------- |:--------------------------------- |
| Total Payment Value     | Sum(Selected Vendor Payments)     |
| Total Adjusted Value    | Sum(Current Adjustments)          |
| Total Available Balance | Remaining Balance                 |
| Total Purchase Returns  | Count(Purchase Return References) |
| Total Credit Notes      | Count(Credit Note References)     |
| Total Debit Notes       | Count(Debit Note References)      |
| Manual Adjustment       | Manual Adjustment Value           |
| Round Off               | Round Off Amount                  |
| Net Adjustment Value    | Final Adjustment Amount           |

---

### 2.2.2. Vendor Summary

Component to use: `SummaryCard`

| Name                      | Content                    |
|:------------------------- |:-------------------------- |
| Vendor Name               | Selected Vendor            |
| Outstanding Amount        | Current Outstanding Amount |
| Total Purchase Invoices   | Count(Purchase Invoices)   |
| Total Vendor Payments     | Count(Vendor Payments)     |
| Total Payment Adjustments | Count(Payment Adjustments) |
| Advance Balance           | Available Advance Amount   |
| Last Payment Date         | Last Vendor Payment Date   |
| Credit Days               | Vendor Credit Days         |

---

### 2.2.3. Workflow Timeline

Component to use: `StageHistoryViewer`

| Name       | Content               |
|:---------- |:--------------------- |
| Stage      | Workflow Stage        |
| Set At     | Date Time             |
| Set By     | User                  |
| Remarks    | Stage Remarks         |
| Attachment | Supporting Attachment |

---

# 3. Adjustment Reference Popup UI

---

## 3.1. Section (Reference Information)

| Name               | Type             | Validation                           |
|:------------------ |:---------------- |:------------------------------------ |
| Adjustment Type    | Select           | Mandatory                            |
| Reference Document | Lookup           | Mandatory For Non-Manual Adjustments |
| Reference Date     | Date             | Auto Filled                          |
| Original Amount    | Currency Display | Auto Filled                          |
| Already Adjusted   | Currency Display | Auto Filled                          |
| Available Balance  | Currency Display | Auto Filled                          |
| Current Adjustment | Currency Input   | Cannot Exceed Available Balance      |
| Adjustment Reason  | Select           | Mandatory                            |
| Remarks            | Text Area        | Optional                             |

---

## 3.2. Section (Settlement Information)

| Name              | Type             | Validation                |
|:----------------- |:---------------- |:------------------------- |
| Settlement Type   | Select           | Mandatory                 |
| Settlement Amount | Currency Input   | Greater Than Zero         |
| Round Off         | Currency Input   | Optional                  |
| Manual Adjustment | Currency Input   | Finance Approval Required |
| Net Adjustment    | Currency Display | Auto Calculated           |
| Remarks           | Text Area        | Optional                  |

---
