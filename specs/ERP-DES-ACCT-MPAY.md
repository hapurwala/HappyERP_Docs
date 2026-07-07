# Module Design Details

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: Accounts
- **Module**: Make Payment

## Roles

| Role               | Purpose                                               |
|:------------------ |:----------------------------------------------------- |
| Cashier            | Executes actual payment transactions                  |
| Accounts Executive | Creates and manages vendor payments                   |
| Accounts Manager   | Reviews and approves payment workflows                |
| Finance Head       | Handles final financial approval and payment strategy |

## Additional Roles

| Role             | Purpose                               |
|:---------------- |:------------------------------------- |
| Purchase Manager | Verifies PO and invoice linkage       |
| Auditor          | Read-only audit and compliance access |
| System Admin     | Full system configuration and control |

## Workflow Stages

| Stage     | Description                    | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles                                                          | System Action                                             |
|:--------- |:------------------------------ |:--------------- |:------------ |:------------ |:------------ |:---------------------------------------------------------------------------------- |:--------------------------------------------------------- |
| Draft     | Initial payment entry stage    | User            | Yes          | Yes          | Yes          | Accounts Executive, Accounts Manager, Finance Head, System Admin                   | Calculate Early Payment Discount or Late Payment Penalty  |
| Submitted | Submitted for approval         | User            | Yes          | No           | Yes          | Accounts Executive, Accounts Manager, Finance Head, Purchase Manager, System Admin | Validate Invoice, Vendor Ledger and Payment Terms         |
| Approved  | Approved for payment execution | User            | No           | No           | Yes          | Accounts Executive, Accounts Manager, Finance Head, Cashier, System Admin          | Create Vendor Payable Settlement Entry                    |
| Initiated | Payment initiated              | User/System     | No           | No           | No           | Accounts Executive, Accounts Manager, Finance Head, Cashier, Auditor, System Admin | Lock Payment Modification, Generate Transaction Reference |
| Completed | Full payment completed         | User/System     | No           | No           | No           | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin          | Close Vendor Payable, Update Vendor Ledger Balance        |
| Failed    | Payment failed                 | User/System     | Yes          | No           | Yes          | Accounts Executive, Accounts Manager, Finance Head, System Admin                   | Reverse Failed Payment Entries                            |
| Cancelled | Payment cancelled              | User            | No           | No           | No           | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin          | Reverse Payment Ledger Entries                            |
| Rejected  | Payment rejected               | User            | No           | No           | No           | Accounts Executive, Accounts Manager, Finance Head, Purchase Manager, System Admin | Create Rejection Audit Log                                |

## Workflow - Role Matrix

| Role                           | Current Stage | Scope | Create | Modify | Delete | Cancel | Next Stage       | Rollback Stage |
|:------------------------------ |:------------- |:----- |:------ |:------ |:------ |:------ |:---------------- |:-------------- |
| Accounts Executive             | Draft         | Self  | Yes    | Yes    | Yes    | No     | Submit           | No             |
| Accounts Manager/ Finance Head | Submitted     | Team  | No     | Yes    | No     | Yes    | Approve          | Yes            |
| Cashier                        | Approved      | All   | No     | No     | No     | No     | Initiate Payment | Yes            |
| Cashier                        | Initiated     | Team  | No     | No     | No     | No     | Complete/Fail    | No             |
| Cashier                        | Failed        | Team  | No     | No     | No     | No     | Complete         | No             |
| Auditor                        | Any           | All   | No     | No     | No     | No     | No               | No             |
| Admin                          | Any           | All   | Yes    | Yes    | Yes    | Yes    | Any              | Any            |

## Header Fields

| Field                 | Description              | Type        | Required |
|:--------------------- |:------------------------ |:----------- |:-------- |
| Payment Number        | Unique payment number    | Auto Number | Yes      |
| Payment Date          | Date of payment          | Date        | Yes      |
| Vendor                | Vendor reference         | Lookup      | Yes      |
| Linked Invoice Number | Linked invoice           | Lookup      | Yes      |
| Linked PO Number      | Linked purchase order    | Lookup      | No       |
| Payment Type          | Advance / Full / Partial | Dropdown    | Yes      |
| Currency              | Currency code            | Lookup      | No       |

## Detail Fields

| Field               | Description            | Type     | Required |
|:------------------- |:---------------------- |:-------- |:-------- |
| Invoice Amount      | Invoice amount         | Currency | Yes      |
| Paid Amount         | Amount paid            | Currency | Yes      |
| Remaining Amount    | Remaining balance      | Currency | Yes      |
| TDS Amount          | TDS deduction          | Currency | No       |
| Discount Amount     | Early payment discount | Currency | No       |
| Penalty Amount      | Late payment penalty   | Currency | No       |
| Net Payment Amount  | Final payable amount   | Currency | Yes      |
| Amount (being paid) | Amount being paid      | Currency | Yes      |
| Balance Amount      | Balance after payment  | Currency | Yes      |

## Payment Method Information

| Field               | Description                                | Type     | Required |
|:------------------- |:------------------------------------------ |:-------- |:-------- |
| Payment Method      | Cash / UPI / Bank Transfer / Cheque / Card | Dropdown | Yes      |
| Bank Account        | Company bank account                       | Lookup   | No       |
| Vendor Bank Account | Vendor bank account                        | Lookup   | No       |
| UTR Number          | Transaction reference number               | Text     | No       |
| Cheque Number       | Cheque reference number                    | Text     | No       |
| Transaction Status  | Pending / Success / Failed                 | Dropdown | Yes      |

## Media & Attachments

| Field              | Description          | Type         |
|:------------------ |:-------------------- |:------------ |
| Attachments        | Supporting documents | File Upload  |
| Payment Receipt    | Payment receipt copy | File Upload  |
| Bank Transfer Slip | Bank transfer proof  | File Upload  |
| Cheque Image       | Cheque image         | Image Upload |

## System Generated Rules

| Trigger          | System Action              |
|:---------------- |:-------------------------- |
| Payment approved | Create payable transaction |
| Completed        | Update stage to Completed  |
| Payment failure  | Update stage to Failed     |

## Validation Rules

| Rule                    | Description                                                                                                    |
|:----------------------- |:-------------------------------------------------------------------------------------------------------------- |
| Invoice Required        | Payment requires approved invoice                                                                              |
| Amount Validation       | Payment cannot exceed pending balance                                                                          |
| Amount Based Permission | Aproval of payment may be allowed based on the amount range. e.g amount> 1LAKHS ? Finance Head:Account Manager |
| Bank Validation         | Bank account required for bank payments                                                                        |
| UTR Validation          | UTR required for online payments                                                                               |

## Reports

| Report                    | Purpose                   |
|:------------------------- |:------------------------- |
| Vendor Payment Register   | Shows all vendor payments |
| Pending Payment Report    | Pending payment summary   |
| Payment Aging Report      | Due and overdue payments  |
| Vendor Outstanding Report | Vendor pending balances   |

---

# Firestore Database Structure

## Collection: vendor_payments

| Field Name       | Data Type |
|:---------------- |:--------- |
| paymentNumber    | string    |
| paymentDate      | timestamp |
| vendorId         | string    |
| invoiceId        | string    |
| poId             | string    |
| paymentMethod    | string    |
| invoiceAmount    | number    |
| paidAmount       | number    |
| balanceAmount    | number    |
| netPaymentAmount | number    |
| stage            | string    |
| createdAt        | timestamp |
| updatedAt        | timestamp |
