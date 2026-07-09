# HappyERP Payment Adjustment (PMTADJ) UI/UX Documentation

This document gives UI details of all pages used in Payment Adjustment module.

# Pages

| S. No. | Name                            | Type  | Purpose                                                                                               |
| ------ | ------------------------------- | ----- | ----------------------------------------------------------------------------------------------------- |
| 1.     | Payment Adjustment List         | Page  | - View all Payment Adjustments<br>- Search, Filter & Export Adjustments<br>- Perform Workflow Actions |
| 2.     | Add / Modify Payment Adjustment | Page  | - Settle Vendor Outstanding Bills<br>- Adjust Vendor Credits<br>- Create Final Settlement             |
| 3.     | Add Bills                       | Popup | - Select Bills For Adjustment                                                                         |

---

# 1. Payment Adjustment List Page UI

This page displays all Payment Adjustments created in the system.

---

## 1.1. Payment Adjustment List

### 1.1a. DataTable (Payment Adjustment List) - Columns

| Name              | Content                 | Type     | On Click                | Card Placement | Tooltip / On Hover          |
|:----------------- |:----------------------- |:-------- |:----------------------- |:-------------- |:--------------------------- |
| Adjustment Number | PMTADJ Number           | Text     | Open Payment Adjustment | Title          | View Payment Adjustment     |
| Adjustment Date   | Adjustment Date         | Date     | –                       | Subtitle       | Adjustment Date             |
| Vendor            | Vendor Name             | Text     | Open Vendor Details     | Body           | View Vendor                 |
| Total Payable     | Total Outstanding Bills | Currency | –                       | Body           | Bills We Have To Pay Vendor |
| Vendor Credits    | Vendor Credits          | Currency | –                       | Body           | Bills Vendor Has To Pay Us  |
| Net Payable       | Final Settlement Amount | Currency | –                       | Body           | Net Amount To Be Paid       |
| Bills Used        | Selected Bills Count    | Number   | –                       | Body           | Bills Used In Adjustment    |
| Status            | Workflow Stage          | Status   | Open Workflow Timeline  | Trailing       | Current Workflow Stage      |
| Created By        | User                    | User     | Open User Profile       | Footer         | Created By                  |
| Updated At        | Date Time               | DateTime | –                       | Footer         | Last Updated                |

---

### 1.1b. DataTable (Payment Adjustment List) - Toolbar Config

| Feature          | Settings                                    | On Click                        |
|:---------------- |:------------------------------------------- |:------------------------------- |
| Search           | Yes                                         | Search Payment Adjustments      |
| View Toggle      | Yes                                         | Switch Between Available Views  |
| Column Selection | Yes                                         | Show/Hide Columns               |
| Group By         | Yes                                         | Group By Vendor / Status        |
| Filter           | Yes                                         | Open Filter Panel               |
| Export           | Yes<br/>Filename: `payment-adjustment-list` | Export Payment Adjustments      |
| Share            | Yes                                         | Share Current View              |
| Full Screen      | Yes                                         | Toggle Full Screen              |
| Add              | Yes                                         | Open Page: `payment_adjustment` |

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

| Name                  | Action                                                                                                                                                               | Visibility Criteria                | Icon               | Tooltip                   |
|:--------------------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------- |:---------------------------------- |:------------------ |:------------------------- |
| View                  | Open Page: `payment_adjustment`                                                                                                                                      | View Permission                    | eye                | View Payment Adjustment   |
| Modify                | Open Page: `payment_adjustment`                                                                                                                                      | Modify Permission + Editable Stage | pencil             | Modify Payment Adjustment |
| Create Vendor Payment | Open Page: `vendor_payment`                                                                                                                                          | Create Vendor Payment Permission   | wallet             | Create Vendor Payment     |
| Set As <Stage_1>      | Change Workflow Stage                                                                                                                                                | Allowed Stage                      | arrow-right-circle | Change Workflow Stage     |
| Set As <Stage_2>      | Change Workflow Stage                                                                                                                                                | Allowed Stage                      | arrow-right-circle | Change Workflow Stage     |
| Undo Stage            | Rollback Workflow                                                                                                                                                    | Rollback Permission                | undo-2             | Rollback Stage            |
| Duplicate             | Create Duplicate Adjustment.<br/>Copies Vendor, Selected Bills, Credits & General Information.<br/>Resets Adjustment Number, Workflow, Timeline & Audit Information. | Add Permission                     | copy               | Duplicate Adjustment      |
| Cancel                | Cancel Payment Adjustment                                                                                                                                            | Cancel Permission                  | x-circle           | Cancel Adjustment         |
| Delete                | Delete Payment Adjustment                                                                                                                                            | Delete Permission                  | trash-2            | Delete Adjustment         |

---

### 1.1e. DataTable (Payment Adjustment List) - Filters Fields

| Name         | Component         | Depends On   | Possible Values                                                    | Default Values | Output            | Tooltip                  |
|:------------ |:----------------- |:------------ |:------------------------------------------------------------------ |:-------------- |:----------------- |:------------------------ |
| Date Range   | Date Range Picker | –            | Any Date Range                                                     | Today          | from_date,to_date | Filter By Date           |
| Organisation | Multi Select      | –            | Allowed Organisations                                              | All            | organisation_ids  | Filter By Organisation   |
| Branch       | Multi Select      | Organisation | Active Branches                                                    | All            | branch_ids        | Filter By Branch         |
| Vendor       | Multi Select      | –            | Active Vendors                                                     | All            | vendor_ids        | Filter By Vendor         |
| Status       | Multi Select      | –            | Draft, Submitted, Verified, Approved, Completed, Closed, Cancelled | Open Stages    | stage_ids         | Filter By Workflow Stage |
| Created By   | User Lookup       | –            | Active Users                                                       | Current User   | user_ids          | Filter By Creator        |

---

# 2. Payment Adjustment Entry Page UI

This page is used to settle vendor outstanding bills against vendor credit documents and generate final payment amount.

---

## 2.1. Left Section

### 2.1.1. Section (Vendor Selection)

This section is used to select Vendor whose outstanding bills need to be settled.

Business Rules:

- Only one Vendor can be selected.
- After Vendor selection, all pending payable bills and vendor credits are loaded automatically.
- Only Open/Pending documents are shown.
- Bills belonging to other Vendors are never displayed.
- User cannot change Vendor after selecting any bill. User must clear selected bills first.
- Vendor Ledger button opens complete Vendor Ledger in read-only mode.

| Name/Label    | Data Source   | Type/Component   | Component Specific Information          | Required | Read Only | Validations            | On Change          | Description   | Tooltip                     |
|:------------- |:------------- |:---------------- |:--------------------------------------- |:-------- |:--------- |:---------------------- |:------------------ |:------------- |:--------------------------- |
| Vendor        | vendor_id     | Lookup           | Single Select, Searchable, Async Lookup | Yes      | No        | Active Vendor Required | Load Vendor Bills  | Vendor        | Select Vendor               |
| GSTIN         | vendor_gstin  | Text             | Auto Filled                             | Yes      | Yes       | –                      | –                  | GST Number    | Vendor GST Number           |
| Mobile        | vendor_mobile | Text             | Auto Filled                             | No       | Yes       | –                      | –                  | Mobile Number | Vendor Mobile               |
| Email         | vendor_email  | Text             | Auto Filled                             | No       | Yes       | –                      | –                  | Email Address | Vendor Email                |
| Credit Days   | credit_days   | Number Display   | Auto Filled                             | No       | Yes       | –                      | –                  | Credit Period | Vendor Credit Days          |
| Credit Limit  | credit_limit  | Currency Display | Auto Filled                             | No       | Yes       | –                      | –                  | Credit Limit  | Vendor Credit Limit         |
| Vendor Ledger | -             | Button           | Secondary Button                        | No       | No        | View Permission        | Open Vendor Ledger | Vendor Ledger | View Complete Vendor Ledger |

---

### 2.1.2. Section (Adjustment KPI Cards)

These cards are updated automatically whenever bills are selected or adjustment amounts are modified.

| KPI                         | Formula                            | Description                      |
|:--------------------------- |:---------------------------------- |:-------------------------------- |
| Total We Have To Pay Vendor | Sum(Selected Outstanding Payables) | Total payable bills              |
| Vendor Has To Pay Us        | Sum(Selected Vendor Credits)       | Total vendor credits             |
| Net Payable To Vendor       | Payables − Credits                 | Final settlement amount          |
| Total Bills Count           | Count(All Bills Loaded)            | Total available bills            |
| Bills Used For Adjustment   | Count(Selected Bills)              | Selected bills                   |
| Remaining After Adjustment  | Net Amount Remaining               | Must become ₹0 before submission |

---

### 2.1.3. Section (Outstanding Payables)

This section contains all bills where Company has to pay Vendor.

Supported Documents:

- Purchase Invoice
- Debit Note
- Freight Bill
- Additional Charges
- Service Invoice
- Rate Difference Invoice

Business Rules:

- Only Open/Pending bills are shown.
- Closed bills are hidden.
- Bills of another Vendor are never shown.
- Partial adjustment is allowed.
- Multiple bills can be adjusted together.
- One bill can be adjusted across multiple Payment Adjustments until fully settled.
- Adjustment Amount cannot exceed Available Balance.

| Name/Label | Data Source          | Type/Component | Component Specific Information     | Required | Read Only | Validations               | On Change   | Description       | Tooltip                    |
|:---------- |:-------------------- |:-------------- |:---------------------------------- |:-------- |:--------- |:------------------------- |:----------- |:----------------- |:-------------------------- |
| Bills      | outstanding_payables | DataTable      | Auto Loaded After Vendor Selection | Yes      | No        | Minimum One Bill Required | Update KPIs | Outstanding Bills | Bills Company Needs To Pay |

---

### 2.1.3a. DataTable (Outstanding Payables) - Columns

| Name/Label        | Data Source       | Type/Component | Component Specific Information                                                  | Required | Read Only | Validations                     | On Change      | Description       | Tooltip              |
|:----------------- |:----------------- |:-------------- |:------------------------------------------------------------------------------- |:-------- |:--------- |:------------------------------- |:-------------- |:----------------- |:-------------------- |
| Select            | is_selected       | Checkbox       | Row Selection                                                                   | No       | No        | –                               | Update Summary | Select Bill       | Include Bill         |
| Document Type     | document_type     | Badge          | Purchase Invoice, Debit Note, Freight Bill, Additional Charges, Service Invoice | Yes      | Yes       | –                               | –              | Document Type     | Source Document      |
| Document Number   | document_number   | Lookup         | Open Source Document                                                            | Yes      | Yes       | Valid Reference                 | View Document  | Document Number   | Open Document        |
| Document Date     | document_date     | Date           | Auto Filled                                                                     | Yes      | Yes       | –                               | –              | Document Date     | Date                 |
| Due Date          | due_date          | Date           | Auto Filled                                                                     | No       | Yes       | –                               | –              | Due Date          | Payment Due Date     |
| Original Amount   | original_amount   | Currency       | Auto Filled                                                                     | Yes      | Yes       | –                               | –              | Original Amount   | Original Bill Amount |
| Already Adjusted  | already_adjusted  | Currency       | Auto Filled                                                                     | Yes      | Yes       | –                               | –              | Already Adjusted  | Previous Adjustment  |
| Available Balance | available_balance | Currency       | Auto Filled                                                                     | Yes      | Yes       | ≥ 0                             | –              | Available Balance | Pending Balance      |
| Adjustment Amount | adjustment_amount | Currency Input | Decimal Allowed                                                                 | Yes      | No        | Cannot Exceed Available Balance | Update KPIs    | Adjustment Amount | Current Adjustment   |
| Remarks           | remarks           | Text           | Single Line                                                                     | No       | No        | –                               | –              | Remarks           | Remarks              |
| Action            | -                 | Action Menu    | View / Remove                                                                   | No       | No        | –                               | –              | Actions           | Row Actions          |

---

### 2.1.3b. DataTable (Outstanding Payables) - Toolbar Config

| Feature          | Settings                                 | On Click                |
|:---------------- |:---------------------------------------- |:----------------------- |
| Search           | Yes                                      | Search Bills            |
| View Toggle      | No                                       | –                       |
| Column Selection | Yes                                      | Show/Hide Columns       |
| Group By         | Yes                                      | Group By Document Type  |
| Filter           | Yes                                      | Open Filter Panel       |
| Export           | Yes<br/>Filename: `outstanding-payables` | Export Bills            |
| Share            | No                                       | –                       |
| Full Screen      | Yes                                      | Toggle Full Screen      |
| Add              | Yes                                      | Open Popup: `add_bills` |

---

### 2.1.3c. DataTable (Outstanding Payables) - Config

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

### 2.1.3d. DataTable (Outstanding Payables) - RowAction Menu

| Name   | Action                      | Visibility Criteria | Icon    | Tooltip       |
|:------ |:--------------------------- |:------------------- |:------- |:------------- |
| View   | Open Source Document        | View Permission     | eye     | View Document |
| Remove | Remove Bill From Adjustment | Modify Permission   | trash-2 | Remove Bill   |

---

### 2.1.3e. DataTable (Outstanding Payables) - Filters Fields

| Name            | Component    | Depends On | Possible Values                                                                 | Default Values | Output          | Tooltip            |
|:--------------- |:------------ |:---------- |:------------------------------------------------------------------------------- |:-------------- |:--------------- |:------------------ |
| Document Type   | Multi Select | –          | Purchase Invoice, Debit Note, Freight Bill, Additional Charges, Service Invoice | All            | document_types  | Filter By Type     |
| Due Date        | Date Range   | –          | Any Date                                                                        | All            | due_date        | Filter By Due Date |
| Pending Balance | Range        | –          | Numeric                                                                         | All            | balance         | Filter By Balance  |
| Document Number | Text         | –          | –                                                                               | –              | document_number | Search Document    |

### 2.1.4. Section (Vendor Credits)

This section contains all documents where Vendor has to pay Company or reduce Company's payable amount.

Supported Documents:

- Purchase Return
- Credit Note
- Vendor Advance
- Excess Payment
- Vendor Refund
- Rebate / Cashback
- Manual Vendor Credit

Business Rules:

- Only Open/Pending credit documents are shown.
- Closed/Fully Adjusted documents are hidden.
- Only selected Vendor's credit documents are displayed.
- Partial adjustment is allowed.
- One credit document can be adjusted multiple times until Available Balance becomes zero.
- Adjustment Amount cannot exceed Available Balance.
- Credit documents reduce Final Net Payable.

| Name/Label | Data Source    | Type/Component | Component Specific Information     | Required | Read Only | Validations                          | On Change   | Description    | Tooltip                         |
|:---------- |:-------------- |:-------------- |:---------------------------------- |:-------- |:--------- |:------------------------------------ |:----------- |:-------------- |:------------------------------- |
| Bills      | vendor_credits | DataTable      | Auto Loaded After Vendor Selection | Yes      | No        | Minimum One Credit Document Required | Update KPIs | Vendor Credits | Bills Vendor Has To Pay Company |

---

### 2.1.4a. DataTable (Vendor Credits) - Columns

| Name/Label        | Data Source       | Type/Component | Component Specific Information                                                      | Required | Read Only | Validations                     | On Change      | Description       | Tooltip                           |
|:----------------- |:----------------- |:-------------- |:----------------------------------------------------------------------------------- |:-------- |:--------- |:------------------------------- |:-------------- |:----------------- |:--------------------------------- |
| Select            | is_selected       | Checkbox       | Row Selection                                                                       | No       | No        | –                               | Update Summary | Select Credit     | Include Credit                    |
| Document Type     | document_type     | Badge          | Purchase Return, Credit Note, Vendor Advance, Excess Payment, Vendor Refund, Rebate | Yes      | Yes       | –                               | –              | Document Type     | Source Document                   |
| Document Number   | document_number   | Lookup         | Open Source Document                                                                | Yes      | Yes       | Valid Reference                 | View Document  | Document Number   | Open Document                     |
| Document Date     | document_date     | Date           | Auto Filled                                                                         | Yes      | Yes       | –                               | –              | Document Date     | Date                              |
| Due Date          | due_date          | Date           | Auto Filled                                                                         | No       | Yes       | –                               | –              | Effective Date    | Credit Effective Date             |
| Original Amount   | original_amount   | Currency       | Auto Filled                                                                         | Yes      | Yes       | –                               | –              | Original Amount   | Original Credit Amount            |
| Already Adjusted  | already_adjusted  | Currency       | Auto Filled                                                                         | Yes      | Yes       | –                               | –              | Already Adjusted  | Previously Adjusted Amount        |
| Available Balance | available_balance | Currency       | Auto Filled                                                                         | Yes      | Yes       | Greater Than Or Equal To Zero   | –              | Available Balance | Remaining Credit Balance          |
| Adjustment Amount | adjustment_amount | Currency Input | Decimal Allowed                                                                     | Yes      | No        | Cannot Exceed Available Balance | Update KPIs    | Adjustment Amount | Credit Used In Current Adjustment |
| Remarks           | remarks           | Text           | Single Line                                                                         | No       | No        | –                               | –              | Remarks           | Additional Remarks                |
| Action            | –                 | Action Menu    | View / Remove                                                                       | No       | No        | –                               | –              | Actions           | Row Actions                       |

---

### 2.1.4b. DataTable (Vendor Credits) - Toolbar Config

| Feature          | Settings                           | On Click                |
|:---------------- |:---------------------------------- |:----------------------- |
| Search           | Yes                                | Search Credit Documents |
| View Toggle      | No                                 | –                       |
| Column Selection | Yes                                | Show/Hide Columns       |
| Group By         | Yes                                | Group By Document Type  |
| Filter           | Yes                                | Open Filter Panel       |
| Export           | Yes<br/>Filename: `vendor-credits` | Export Credit Documents |
| Share            | No                                 | –                       |
| Full Screen      | Yes                                | Toggle Full Screen      |
| Add              | Yes                                | Open Popup: `add_bills` |

---

### 2.1.4c. DataTable (Vendor Credits) - Config

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

### 2.1.4d. DataTable (Vendor Credits) - RowAction Menu

| Name   | Action                        | Visibility Criteria | Icon    | Tooltip       |
|:------ |:----------------------------- |:------------------- |:------- |:------------- |
| View   | Open Source Document          | View Permission     | eye     | View Document |
| Remove | Remove Credit From Adjustment | Modify Permission   | trash-2 | Remove Credit |

---

### 2.1.4e. DataTable (Vendor Credits) - Filters Fields

| Name              | Component    | Depends On | Possible Values                                                                     | Default Values | Output            | Tooltip           |
|:----------------- |:------------ |:---------- |:----------------------------------------------------------------------------------- |:-------------- |:----------------- |:----------------- |
| Document Type     | Multi Select | –          | Purchase Return, Credit Note, Vendor Advance, Excess Payment, Vendor Refund, Rebate | All            | document_types    | Filter By Type    |
| Effective Date    | Date Range   | –          | Any Date                                                                            | All            | effective_date    | Filter By Date    |
| Available Balance | Range        | –          | Numeric                                                                             | All            | available_balance | Filter By Balance |
| Document Number   | Text         | –          | –                                                                                   | –              | document_number   | Search Document   |

---

### 2.1.5. Section (General Information)

This section captures payment adjustment transaction details.

| Name/Label        | Data Source       | Type/Component | Component Specific Information                                                  | Required | Read Only | Validations                | On Change            | Description       | Tooltip                 |
|:----------------- |:----------------- |:-------------- |:------------------------------------------------------------------------------- |:-------- |:--------- |:-------------------------- |:-------------------- |:----------------- |:----------------------- |
| Adjustment Number | adjustment_number | Text           | Prefix: PMTADJ, Auto Generated                                                  | Yes      | Yes       | Unique                     | Auto Generate        | Adjustment Number | System Generated Number |
| Adjustment Date   | adjustment_date   | Date Picker    | Single Date                                                                     | Yes      | No        | Cannot Be Future Date      | –                    | Adjustment Date   | Transaction Date        |
| Adjustment Type   | adjustment_type   | Select         | Payment Against Bills, Advance Adjustment, Credit Settlement, Manual Adjustment | Yes      | No        | Valid Type Required        | Update Rules         | Adjustment Type   | Settlement Type         |
| Payment Mode      | payment_mode      | Select         | Cash, Bank Transfer, UPI, Cheque, NEFT, RTGS                                    | Yes      | No        | Valid Payment Mode         | Show Payment Fields  | Payment Mode      | Mode Of Payment         |
| Bank Account      | bank_account_id   | Lookup         | Active Bank Accounts                                                            | No       | No        | Required For Bank Payments | Load Account Details | Bank Account      | Payment Bank            |
| Remarks           | remarks           | Text Area      | Multi Line                                                                      | No       | No        | Max 500 Characters         | –                    | Remarks           | Additional Notes        |

---

### 2.1.6. Section (Attachments)

| Name/Label  | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations      | On Change | Description          | Tooltip                 |
|:----------- |:----------- |:-------------- |:------------------------------ |:-------- |:--------- |:---------------- |:--------- |:-------------------- |:----------------------- |
| Attachments | attachments | DataTable      | Multiple Files Supported       | No       | No        | Valid File Types | –         | Supporting Documents | Upload Supporting Files |

---

## 2.2. Right Section

### 2.2.1. Section (Adjustment Summary)

Component: **Summary Card**

This section displays live financial summary of current Payment Adjustment.

Business Rules:

- Values update instantly whenever user selects/unselects bills.
- Final Net Payable determines whether Company needs to pay Vendor or Vendor needs to refund Company.
- If Net Payable becomes zero, settlement is considered fully adjusted.

| Name                       | Formula / Source                            | Description                                      |
|:-------------------------- |:------------------------------------------- |:------------------------------------------------ |
| Total Outstanding Payables | Sum(Outstanding Payables Adjustment Amount) | Bills Company Has To Pay                         |
| Total Vendor Credits       | Sum(Vendor Credits Adjustment Amount)       | Credits Available Against Vendor                 |
| Net Payable                | Payables − Vendor Credits                   | Final Amount After Adjustment                    |
| Remaining Balance          | Pending Amount After Current Adjustment     | Should Become ₹0 Before Completion               |
| Settlement Status          | Auto Calculated                             | Pay Vendor / Receive From Vendor / Fully Settled |

---

### 2.2.2. Section (Vendor Information)

Component: **Summary Card**

Displays important financial information of selected Vendor.

| Name                 | Source             |
|:-------------------- |:------------------ |
| Vendor Name          | Selected Vendor    |
| Vendor Code          | Vendor Master      |
| Credit Days          | Vendor Master      |
| Credit Limit         | Vendor Master      |
| Outstanding Amount   | Vendor Ledger      |
| Advance Balance      | Vendor Ledger      |
| Last Payment Date    | Vendor Payment     |
| Last Payment Amount  | Vendor Payment     |
| Last Adjustment Date | Payment Adjustment |
| Vendor Rating        | Vendor Master      |

---

### 2.2.3. Section (Workflow Timeline)

Component: **Workflow Timeline**

Displays complete workflow history of current Payment Adjustment.

| Name        | Content                  |
|:----------- |:------------------------ |
| Stage       | Workflow Stage           |
| Date & Time | Stage Updated On         |
| Updated By  | User Name                |
| Remarks     | Stage Remarks            |
| Attachment  | Optional Supporting File |

---

# 3. Add Bills Popup

This popup allows user to search and add payable bills or vendor credit documents into current Payment Adjustment.

Business Rules:

- Popup automatically filters records based on selected Vendor.
- Closed/Fully Adjusted documents are hidden.
- Duplicate documents cannot be added.
- Multiple documents can be selected together.

---

## 3.1. Section (Search Bills)

| Name/Label        | Type              | Description                                                                                                                   |
|:----------------- |:----------------- |:----------------------------------------------------------------------------------------------------------------------------- |
| Document Type     | Multi Select      | Purchase Invoice, Debit Note, Purchase Return, Credit Note, Vendor Advance, Vendor Refund, Excess Payment, Additional Charges |
| Document Number   | Search Box        | Search By Document Number                                                                                                     |
| Date Range        | Date Range Picker | Filter By Date                                                                                                                |
| Available Balance | Range             | Filter By Pending Amount                                                                                                      |
| Search            | Button            | Search Documents                                                                                                              |

---

## 3.2. Section (Available Bills)

### DataTable - Columns

| Name              | Description          |
|:----------------- |:-------------------- |
| Select            | Select Document      |
| Document Type     | Source Document Type |
| Document Number   | Reference Number     |
| Date              | Document Date        |
| Original Amount   | Original Amount      |
| Already Adjusted  | Previously Adjusted  |
| Available Balance | Remaining Balance    |
| Remarks           | Document Remarks     |

---

### Toolbar Config

| Feature          | Settings | On Click               |
|:---------------- |:-------- |:---------------------- |
| Search           | Yes      | Search Bills           |
| View Toggle      | No       | –                      |
| Column Selection | Yes      | Show/Hide Columns      |
| Group By         | Yes      | Group By Document Type |
| Filter           | Yes      | Open Filter Panel      |
| Export           | No       | –                      |
| Share            | No       | –                      |
| Full Screen      | Yes      | Toggle Full Screen     |
| Add              | Yes      | Add Selected Bills     |

---
