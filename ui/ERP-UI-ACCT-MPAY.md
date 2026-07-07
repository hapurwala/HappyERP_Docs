# HappyERP Make Payment (MPAY) UI/UX Documentation

This document gives UI details of all pages used in Make Payment module.

# Pages

| S. No. | Name                         | Type  | Purpose                                                                                           |
| ------ | ---------------------------- | ----- | ------------------------------------------------------------------------------------------------- |
| 1.     | Make Payment List            | Page  | - View all Vendor Payments<br/>- Search/filter payments<br/>- Perform quick actions on payment(s) |
| 2.     | Add/Modify Vendor Payment    | Page  | - View payment details<br/>- Add/modify current payment information                               |
| 3.     | Add/Modify Payment Reference | Popup | - Add/modify invoice reference details in current payment                                         |

---

# 1. Make PaymentList Page UI

This page shows list of Vendor Payments in tabular format. The Vendor Payments appearing in the list depend upon current user's permission and applied filters.

---

## 1.1. Make Payment List Data Table

### 1.1a. DataTable (Make Payment List) - Columns

| Name           | Content                  | Type     | On Click                    | Card Placement | Tooltip / On Hover     |
|:-------------- |:------------------------ |:-------- |:--------------------------- |:-------------- |:---------------------- |
| Payment Number | Unique Payment Number    | Text     | Open Payment View/Edit Page | Title          | -                      |
| Payment Date   | Payment Transaction Date | Date     | –                           | Subtitle       | -                      |
| Vendor         | Vendor Name              | Text     | Open Vendor Profile         | Body           | -                      |
| Payment Mode   | Cash/Bank/UPI/Cheque     | Badge    | Open Payment Details        | Body           | -                      |
| Invoice Count  | Linked Purchase Invoices | Number   | Open Invoice References     | Body           | -                      |
| Paid Amount    | Total Paid Amount        | Currency | –                           | Body           | -                      |
| TDS Amount     | TDS Deducted             | Currency | –                           | Body           | -                      |
| Net Amount     | Final Payment Amount     | Currency | –                           | Body           | -                      |
| Stage          | Current Workflow Stage   | Status   | Open Workflow Timeline      | Trailing       | Current Payment Status |
| Created By     | User Who Created Payment | User     | Open User Profile           | Footer         | Created By User        |
| Updated At     | Last Updated Date & Time | DateTime | –                           | Footer         | -                      |

---

### 1.1b. DataTable (Make Payment List) - Toolbar Config

| Feature          | Settings                                | On Click                         |
|:---------------- |:--------------------------------------- |:-------------------------------- |
| Search           | Yes                                     | Search Vendor Payments           |
| View Toggle      | Yes                                     | Switch Between DataTable Layouts |
| Column Selection | Yes                                     | Show/Hide Columns                |
| Group By         | Yes                                     | Group Records                    |
| Filter           | Yes                                     | Open Filter Panel                |
| Export           | Yes<br/>Filename: `vendor-payment-list` | Export Vendor Payments           |
| Share            | Yes                                     | Share Current View               |
| Full Screen      | Yes                                     | Toggle Full Screen               |
| Add              | Yes<br/>Page to open: `vendor_payment`  | Open Vendor Payment Create Page  |

---

### 1.1c. DataTable (Vendor Payment List) - Config

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

### 1.1d. DataTable (Vendor Payment List) - RowAction Menu

| Name                      | Action                                                                                                                                                                               | Visibility Criteria                   | Icon               | Tooltip          |
|:------------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |:------------------------------------- |:------------------ |:---------------- |
| View                      | Open page: `vendor_payment`                                                                                                                                                          | View Permission                       | eye                | -                |
| Modify                    | Open page: `vendor_payment`                                                                                                                                                          | Modify Permission + Stage Allows Edit | pencil             | -                |
| Print Voucher             | Download Payment Voucher PDF                                                                                                                                                         | Print Permission                      | printer            | -                |
| Create Payment Adjustment | Open page: `payment_adjustment`                                                                                                                                                      | Adjustment Permission                 | rotate-ccw         | -                |
| Set as <Stage_1>          | Set stage to Stage_1                                                                                                                                                                 | Allowed Stage Based On Current Stage  | arrow-right-circle | -                |
| Set as <Stage_2>          | Set stage to Stage_2                                                                                                                                                                 | Allowed Stage Based On Current Stage  | arrow-right-circle | -                |
| Undo Stage                | Move back to last stage                                                                                                                                                              | Permission To Rollback                | undo-2             | Rollback Payment |
| Duplicate                 | Create copy of current payment.<br/>Copies: Vendor, Payment References, Payment Terms, Remarks.<br/>Resets: Payment Number, Transaction Details, Audit Logs, Workflow Stage (Draft). | Add Permission                        | copy               | -                |
| Cancel                    | Show confirmation and cancel payment                                                                                                                                                 | Cancel Permission + Allowed Stage     | cancel             | -                |
| Delete                    | Show confirmation and delete payment                                                                                                                                                 | Delete Permission + Allowed Stage     | trash-2            | -                |

---

### 1.1e. DataTable (Vendor Payment List) - Filters Fields

| Name           | Component         | Depends On   | Possible Values                                  | Default Values | Output            | Tooltip |
|:-------------- |:----------------- |:------------ |:------------------------------------------------ |:-------------- |:----------------- |:------- |
| Date Range     | Date Range Picker | –            | Any Date Range                                   | Today          | from_date,to_date | -       |
| Organisation   | Multi Select      | –            | Allowed Organisations                            | All            | organisation_ids  | -       |
| Branch         | Multi Select      | Organisation | Active Branches                                  | All            | branch_ids        | -       |
| Vendor         | Multi Select      | –            | Active Vendors                                   | All            | vendor_ids        | -       |
| Payment Mode   | Multi Select      | –            | Cash, UPI, Bank, Cheque, RTGS, NEFT              | All            | payment_modes     | -       |
| Payment Status | Multi Select      | –            | Pending, Partial, Paid, Failed, Cancelled        | Open Stages    | payment_statuses  | -       |
| Stage          | Multi Select      | –            | Draft, Submitted, Approved, Completed, Cancelled | Open Stages    | stage_ids         | -       |
| Created By     | User Lookup       | –            | Active Users                                     | Current User   | user_ids          | -       |

---

# 2. Vendor Payment Entry Page UI

Fields in the main form are grouped into different sections.

```text
2.1. Left: Main Form

- General Information
- Invoice References
- Payment Details
- Adjustments
- Attachments

2.2. Right: Summary and Timeline

- Payment Summary
- Vendor Summary
- Workflow Timeline
```

## 2.1. Left Part

### 2.1.1. Section (General Information)

| Name           | Data Source    | Component    | Component Specific Information                       | Required | Read Only | Validations               | On Change                | Description            | Tooltip                         |
|:-------------- |:-------------- |:------------ |:---------------------------------------------------- |:-------- |:--------- |:------------------------- |:------------------------ |:---------------------- |:------------------------------- |
| Payment Number | payment_number | Text         | Prefix=MPAY, Auto Increment, Format=MPAY-YYYY-000001 | Yes      | Yes       | Must Be Unique            | Auto Generated           | Unique Payment Number  | System Generated Payment Number |
| Payment Date   | payment_date   | DatePicker   | Single Date Picker                                   | Yes      | No        | Cannot Be Future Date     | –                        | Payment Date           | -                               |
| Vendor         | vendor_id      | Select       | Searchable Async Lookup                              | Yes      | No        | Vendor Must Be Active     | Load Vendor Details      | Vendor Selection       | -                               |
| Payment Type   | payment_type   | Select       | Invoice Payment, Advance, Security Deposit, Mixed    | Yes      | No        | Valid Payment Type        | Load References          | Payment Category       | -                               |
| Currency       | currency_id    | Select       | Auto Filled From Invoice Or Organisation Default     | Yes      | No        | Valid Currency            | Update Amount Fields     | Transaction Currency   | -                               |
| Exchange Rate  | exchange_rate  | Number Input | Decimal Allowed                                      | No       | No        | Must Be Greater Than Zero | Update Converted Amounts | Currency Exchange Rate | Exchange Rate To Base Currency  |
| Remarks        | remarks        | RichTextBox  | –                                                    | No       | No        | –                         | –                        | General Remarks        | -                               |

---

### 2.1.2. Section (Invoice References)

This section allows linking Vendor Payment with one or multiple Purchase Invoices.

Supported Cases:

- Vendor Payment against Single Purchase Invoice
- Vendor Payment against Multiple Purchase Invoices
- Partial Payment against Purchase Invoice
- Full Payment against Purchase Invoice
- Advance Payment without Invoice
- Security Deposit Payment
- Mixed Payment (Invoice Payment + Advance Payment)
- Purchase Return Adjustment
- Credit Note Adjustment

Business Rules:

- Multiple Purchase Invoices are allowed.
- All selected invoices must belong to the same Vendor.
- Advance Payments do not require Invoice References.
- Mixed Payments require Finance Manager approval.
- Payment Amount cannot exceed Pending Invoice Amount.
- Purchase Return Adjustments reduce Payable Amount.

| Name                   | Data Source            | Component    | Component Specific Information              | Required | Read Only | Validations                             | On Change              | Description              | Tooltip                         |
|:---------------------- |:---------------------- |:------------ |:------------------------------------------- |:-------- |:--------- |:--------------------------------------- |:---------------------- |:------------------------ |:------------------------------- |
| Purchase Invoices      | invoice_ids            | Multi Select | Searchable, Async Lookup                    | No       | No        | All Invoices Must Belong To Same Vendor | Load Invoice Details   | Linked Purchase Invoices | Select One Or Multiple Invoices |
| Allow Advance Payment  | allow_advance_payment  | Toggle       | Yes / No                                    | Yes      | No        | Finance Manager Permission Required     | Show Advance Fields    | Allow Advance Payment    | Pay Vendor Without Invoice      |
| Allow Mixed Payment    | allow_mixed_payment    | Toggle       | Yes / No                                    | No       | No        | Finance Manager Approval Required       | Show Mixed Mode Fields | Mixed Payment Mode       | Invoice + Advance Payment       |
| Vendor                 | vendor_id              | Auto Filled  | Loaded From Selected Invoice                | Yes      | Yes       | Vendor Must Match All References        | Auto Populate          | Vendor Information       | -                               |
| Currency               | currency_id            | Auto Filled  | Loaded From Invoice Or Organisation Default | Yes      | Yes       | All Invoices Must Have Same Currency    | Auto Populate          | Transaction Currency     | -                               |
| Total Invoice Amount   | total_invoice_amount   | Currency     | Calculated Field                            | No       | Yes       | –                                       | Auto Calculate         | Total Invoice Amount     | -                               |
| Total Paid Amount      | total_paid_amount      | Currency     | Calculated Field                            | No       | Yes       | –                                       | Auto Calculate         | Already Paid Amount      | -                               |
| Total Pending Amount   | total_pending_amount   | Currency     | Calculated Field                            | No       | Yes       | –                                       | Auto Calculate         | Remaining Amount         | -                               |
| Advance Payment Amount | advance_payment_amount | Currency     | Manual Input                                | No       | No        | Must Be Greater Than Zero               | Update Net Amount      | Advance Payment          | Payment Without Invoice         |
| Adjustment Amount      | adjustment_amount      | Currency     | Purchase Return/Credit Note Adjustment      | No       | No        | Cannot Exceed Pending Amount            | Update Net Amount      | Adjustment Value         | -                               |
| Final Payable Amount   | final_payable_amount   | Currency     | Auto Calculated                             | Yes      | Yes       | Formula Validation                      | Auto Calculate         | Net Payable Amount       | -                               |

---

### 2.1.2a. DataTable (Invoice References) - Columns

| Name             | Data Source       | Component | Component Specific Information | Required | Read Only | Validations                    | On Change            | Description            | Tooltip                        |
|:---------------- |:----------------- |:--------- |:------------------------------ |:-------- |:--------- |:------------------------------ |:-------------------- |:---------------------- |:------------------------------ |
| Invoice Number   | invoice_number    | Lookup    | Active Purchase Invoices       | Yes      | No        | Must Belong To Selected Vendor | Load Invoice Details | Purchase Invoice       | Linked Purchase Invoice        |
| Invoice Date     | invoice_date      | Date      | Auto Filled                    | Yes      | Yes       | –                              | –                    | Invoice Date           | -                              |
| Total Amount     | total_amount      | Currency  | Auto Filled                    | Yes      | Yes       | –                              | –                    | Invoice Amount         | -                              |
| Paid Amount      | paid_amount       | Currency  | Auto Filled                    | Yes      | Yes       | –                              | –                    | Already Paid           | -                              |
| Pending Amount   | pending_amount    | Currency  | Auto Filled                    | Yes      | Yes       | Cannot Be Negative             | –                    | Pending Amount         | -                              |
| Payment Amount   | payment_amount    | Currency  | Decimal Allowed                | Yes      | No        | Cannot Exceed Pending Amount   | Recalculate Totals   | Current Payment Amount | Amount To Be Paid In This MPAY |
| Adjustment Value | adjustment_amount | Currency  | Return/Credit Adjustment       | No       | No        | Cannot Exceed Pending Amount   | Recalculate Totals   | Adjustment Amount      | -                              |
| Net Amount       | net_amount        | Currency  | Auto Calculated                | Yes      | Yes       | Formula Validation             | Auto Calculate       | Final Payable Amount   | Payment - Adjustments          |
| Remarks          | remarks           | TextArea  | Multi Line                     | No       | No        | –                              | –                    | Reference Remarks      | -                              |

---

### 2.1.2b. DataTable (Invoice References) - Toolbar Config

| Feature          | Settings | On Click                  |
|:---------------- |:-------- |:------------------------- |
| Search           | Yes      | Search Invoice References |
| View Toggle      | No       | –                         |
| Column Selection | No       | –                         |
| Group By         | No       | –                         |
| Filter           | No       | –                         |
| Export           | No       | –                         |
| Share            | No       | –                         |
| Full Screen      | Yes      | Toggle Full Screen        |
| Add              | Yes      | Open Popup: `payment_ref` |

---

### 2.1.2c. DataTable (Invoice References) - Config

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

### 2.1.2d. DataTable (Invoice References) - RowAction Menu

| Name      | Action                    | Visibility Criteria | Icon   | Tooltip |
|:--------- |:------------------------- |:------------------- |:------ |:------- |
| Modify    | Open Popup: `payment_ref` | Modify Permission   | pencil | -       |
| Duplicate | Copy Invoice Reference    | Add Permission      | copy   | -       |
| Delete    | Delete Invoice Reference  | Delete Permission   | trash  | -       |

---

### 2.1.2e. DataTable (Invoice References) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| –    | –         | –          | –               | –              | –      | –       |

### 2.1.3. Section (Payment Details)

This section captures actual payment transaction details.

Supported Cases:

- Single Payment Mode
- Multiple Payment Modes
- Partial Payment
- Advance Payment
- Mixed Payment
- TDS Deduction
- Early Payment Discount
- Late Payment Penalty
- Purchase Return Adjustment

---

| Name/Label             | Data Source          | Type/Component | Component Specific Information               | Required | Read Only | Validations                           | On Change                 | Description           | Tooltip                           |
|:---------------------- |:-------------------- |:-------------- |:-------------------------------------------- |:-------- |:--------- |:------------------------------------- |:------------------------- |:--------------------- |:--------------------------------- |
| Payment Modes          | payment_modes        | Multi Select   | Cash, UPI, Bank Transfer, Cheque, NEFT, RTGS | Yes      | No        | At Least One Payment Mode Required    | Show Payment Mode Details | Payment Methods       | -                                 |
| Payment Date           | payment_date         | Date Picker    | Single Date                                  | Yes      | No        | Cannot Be Future Date                 | Update Accounting Date    | Transaction Date      | -                                 |
| Base Date              | base_date            | Select         | Invoice Date, Due Date, Current Date         | Yes      | No        | Valid Base Date Required              | Recalculate Due Date      | Payment Base Date     | Base Date For Payment Calculation |
| Credit Days            | credit_days          | Number Input   | Integer Only                                 | No       | No        | Must Be Greater Than Or Equal To Zero | Recalculate Due Date      | Credit Period         | Vendor Credit Period              |
| Due Date               | due_date             | Date Display   | Auto Calculated                              | Yes      | Yes       | Formula Validation                    | Auto Calculate            | Final Due Date        | -                                 |
| TDS Applicable         | tds_applicable       | Toggle         | Yes / No                                     | No       | No        | –                                     | Show/Hide TDS Fields      | TDS Deduction         | -                                 |
| TDS Category           | tds_category_id      | Lookup         | Single Select, Source=TDS Master             | No       | No        | Required If TDS Applicable            | Load TDS Percentage       | TDS Category          | -                                 |
| TDS Percentage         | tds_percentage       | Percentage     | Auto Filled From TDS Category                | No       | Yes       | 0–100%                                | Calculate TDS Amount      | TDS Rate              | -                                 |
| TDS Value              | tds_value            | Currency       | Auto Calculated                              | No       | Yes       | Formula Validation                    | Auto Calculate            | TDS Deduction Amount  | -                                 |
| Early Payment Discount | early_discount_value | Currency       | Auto Calculated                              | No       | Yes       | Cannot Exceed Payable Amount          | Update Net Payment        | Early Payment Benefit | -                                 |
| Late Payment Penalty   | late_penalty_value   | Currency       | Auto Calculated                              | No       | Yes       | Cannot Be Negative                    | Update Net Payment        | Late Payment Penalty  | -                                 |
| Adjustment Value       | adjustment_value     | Currency       | Purchase Return / Credit Adjustment          | No       | No        | Cannot Exceed Pending Amount          | Recalculate Final Amount  | Adjustment Amount     | -                                 |
| Total Payment Value    | total_payment_value  | Currency       | Auto Calculated                              | Yes      | Yes       | Formula Validation                    | Auto Calculate            | Total Payment Amount  | -                                 |
| Net Payment Value      | total_net_value      | Currency       | Auto Calculated                              | Yes      | Yes       | Formula Validation                    | Auto Calculate            | Final Payment Amount  | -                                 |
| Remarks                | remarks              | Text Area      | Multi Line                                   | No       | No        | –                                     | –                         | Payment Remarks       | -                                 |

---

### 2.1.3a. DataTable (Payment Schedule) - Columns

| Name/Label       | Data Source      | Type/Component | Component Specific Information      | Required | Read Only | Validations                 | On Change          | Description           | Tooltip                   |
|:---------------- |:---------------- |:-------------- |:----------------------------------- |:-------- |:--------- |:--------------------------- |:------------------ |:--------------------- |:------------------------- |
| Payment Mode     | payment_mode     | Select         | Cash, UPI, Bank, Cheque, NEFT, RTGS | Yes      | No        | Valid Payment Mode Required | Show Mode Fields   | Payment Method        | -                         |
| Reference Number | reference_number | Text           | Transaction Id / Cheque Number      | No       | No        | Unique Within Payment       | –                  | Transaction Reference | Bank/Cheque/UPI Reference |
| Reference Date   | reference_date   | Date Picker    | Single Date                         | No       | No        | Cannot Be Future Date       | –                  | Reference Date        | Transaction Date          |
| Bank Account     | bank_account_id  | Lookup         | Active Organisation Bank Accounts   | No       | No        | Required For Bank Payments  | Load Bank Details  | Bank Account          | -                         |
| Amount           | amount           | Currency       | Decimal Allowed                     | Yes      | No        | Must Be Greater Than Zero   | Recalculate Totals | Payment Amount        | -                         |
| Remarks          | remarks          | Text Area      | Multi Line                          | No       | No        | –                           | –                  | Mode Remarks          | -                         |

---

### 2.1.3b. DataTable (Payment Schedule) - Toolbar Config

| Feature          | Settings | On Click             |
|:---------------- |:-------- |:-------------------- |
| Search           | No       | –                    |
| View Toggle      | No       | –                    |
| Column Selection | No       | –                    |
| Group By         | No       | –                    |
| Filter           | No       | –                    |
| Export           | No       | –                    |
| Share            | No       | –                    |
| Full Screen      | Yes      | Toggle Full Screen   |
| Add              | Yes      | Add Payment Mode Row |

---

### 2.1.3c. DataTable (Payment Schedule) - Config

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

### 2.1.3d. DataTable (Payment Schedule) - RowAction Menu

| Name      | Action                | Visibility Criteria | Icon   | Tooltip |
|:--------- |:--------------------- |:------------------- |:------ |:------- |
| Modify    | Edit Payment Row      | Modify Permission   | pencil | -       |
| Duplicate | Duplicate Payment Row | Add Permission      | copy   | -       |
| Delete    | Delete Payment Row    | Delete Permission   | trash  | -       |

---

### 2.1.3e. DataTable (Payment Schedule) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
|:---- |:--------- |:---------- |:--------------- |:-------------- |:------ |:------- |
| –    | –         | –          | –               | –              | –      | –       |

---

### 2.1.5. Section (Attachments)

| Name        | Data Source               | Component        | Component Specific Information | Required | Read Only | Validations | On Change | Description | Tooltip |
|:----------- |:------------------------- |:---------------- |:------------------------------ |:-------- |:--------- |:----------- |:--------- |:----------- |:------- |
| Attachments | payment_out.`attachments` | AttachmentViewer | PDF, Image, DOC Preview        | No       | No        | -           | -         | MPAY Media  | -       |

---

## 2.2. Right Side Widgets

### 2.2.1. Payment Summary

Component to use: `SummaryCard`

| Name                   | Content                      |
|:---------------------- |:---------------------------- |
| Total Invoice Amount   | Sum(Invoice Amounts)         |
| Total Paid Amount      | Sum(Current Payment Amounts) |
| Total Adjustment Value | Sum(All Adjustments)         |
| Total TDS Value        | Calculated TDS Amount        |
| Net Payment Value      | Final Payable Amount         |
| Payment Modes          | Selected Payment Modes       |
| Total Invoice Count    | Count(Linked Invoices)       |

---

### 2.2.2. Vendor Summary

Component to use: `SummaryCard`

| Name                    | Content                    |
|:----------------------- |:-------------------------- |
| Vendor Name             | Selected Vendor            |
| Outstanding Amount      | Vendor Outstanding Balance |
| Total Purchase Invoices | Count(Vendor Invoices)     |
| Total Payments          | Count(Vendor Payments)     |
| Last Payment Date       | Last Payment Date          |
| Credit Days             | Vendor Credit Days         |
| Vendor Rating           | Vendor Performance Rating  |

---

### 2.2.3. Workflow Timeline

Component to use: `StageHistoryViewer`

| Name       | Content               |
|:---------- |:--------------------- |
| Stage      | Stage Name            |
| Set At     | Date Time             |
| Set By     | User                  |
| Remarks    | Stage Remarks         |
| Attachment | Supporting Attachment |

---

# 3. Payment Reference Popup UI

---

## 3.1. Section (Invoice Information)

| Name              | Type             | Validation                   |
|:----------------- |:---------------- |:---------------------------- |
| Purchase Invoice  | Lookup           | Mandatory                    |
| Payment Amount    | Currency Input   | Cannot Exceed Pending Amount |
| Adjustment Amount | Currency Input   | Cannot Exceed Payment Amount |
| Net Amount        | Currency Display | Auto Calculated              |
| Remarks           | Text Area        | Optional                     |

---

## 3.2. Section (Payment Mode Information)

| Name             | Type           | Validation                 |
|:---------------- |:-------------- |:-------------------------- |
| Payment Mode     | Select         | Mandatory                  |
| Reference Number | Text           | Optional                   |
| Reference Date   | Date           | Cannot Be Future Date      |
| Bank Account     | Lookup         | Required For Bank Payments |
| Amount           | Currency Input | Must Be Greater Than Zero  |
| Remarks          | Text Area      | Optional                   |

---

## 3.3. Section (Adjustment Information)

| Name               | Type           | Validation                    |
|:------------------ |:-------------- |:----------------------------- |
| Purchase Return    | Multi Select   | Same Vendor Required          |
| Credit Note        | Multi Select   | Same Vendor Required          |
| Debit Note         | Multi Select   | Same Vendor Required          |
| Manual Adjustment  | Currency Input | Approval Required Above Limit |
| Adjustment Remarks | Text Area      | Optional                      |

---
