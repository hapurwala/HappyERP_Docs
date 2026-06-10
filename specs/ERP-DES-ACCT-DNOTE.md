# Module Design Details

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: Accounts
- **Module**: Debit Note

## Roles

Direct roles that work on this module.

| Role               | Purpose                                                  |
|:------------------ |:-------------------------------------------------------- |
| Accounts Executive | Creates and manages debit note entries                   |
| Accounts Manager   | Reviews and approves debit note workflows                |
| Finance Head       | Handles final accounting approval and vendor adjustments |

## Additional Roles

Roles from other applications who also use this module. 

| Role             | Purpose                                        |
|:---------------- |:---------------------------------------------- |
| Purchase Manager | Verifies purchase return and vendor linkage    |
| Dispatch Manager | Verifies dispatch completion before adjustment |
| Auditor          | Read-only audit and compliance access          |
| Admin            | Full system configuration and control          |

## Workflow Stages

| Stage          | Description                                                                    | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles                                                                            | System Action                                        |
|:-------------- |:------------------------------------------------------------------------------ |:--------------- |:------------ |:------------ |:------------ |:---------------------------------------------------------------------------------------------------- |:---------------------------------------------------- |
| Draft          | Initial stage where debit note is created.                                     | System          | Yes          | Yes          | Yes          | Accounts Executive, Accounts Manager, Finance Head, System Admin                                     | Calculate Vendor Adjustment Amount                   |
| Submitted      | Debit note submitted for verification or  approval.                            | User            | Yes          | No           | Yes          | Accounts Executive, Accounts Manager, Finance Head, Purchase Manager, System Admin                   | Validate Purchase Return, Dispatch and Vendor Ledger |
| Verified       | Debit note verified successfully against purchase return and dispatch records. | User            | No           | No           | Yes          | Accounts Executive, Accounts Manager, Finance Head, Purchase Manager, Dispatch Manager, System Admin | Validate Quantity, Tax and Ledger Impact             |
| Approved       | Debit note approved for vendor accounting adjustment.                          | User            | No           | No           | Yes          | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin                            | Post Debit Adjustment Entries in Vendor Ledger       |
| Sent To Vendor | Debit note shared with vendor for adjustment acknowledgment.                   | User/System     | No           | No           | No           | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin                            | Send Vendor Notification and Generate Debit Note PDF |
| Partially Paid | Partial amount Paid against vendor balance/payment.                            | System          | No           | No           | No           | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin                            | Update Vendor Outstanding Adjustment Balance         |
| Paid           | Full debit note amount Paid successfully.                                      | System          | No           | No           | No           | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin                            | Close Vendor Adjustment Entry                        |
| Completed      | Debit note lifecycle completed successfully.                                   | System          | No           | No           | No           | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin                            | Archive Debit Note and Lock Financial Entries        |
| Cancelled      | Debit note cancelled due to operational or accounting issues.                  | User            | No           | No           | No           | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin                            | Reverse Debit Note Ledger Entries                    |
| Rejected       | Debit note rejected during approval or verification process.                   | User            | No           | No           | No           | Accounts Executive, Accounts Manager, Finance Head, Purchase Manager, System Admin                   | Create Rejection Audit Log                           |

## Workflow - Role Matrix

| Role                                            | Current Stage  | Scope | Create | Modify | Delete | Cancel | Next Stage | Rollback Stage |
|:----------------------------------------------- |:-------------- |:----- |:------ |:------ |:------ |:------ |:---------- |:-------------- |
| Accounts Executive                              | Draft          | Self  | Yes    | Yes    | Yes    | No     | Submit     | No             |
| Accounts Executive                              | Submitted      | Self  | No     | Yes    | No     | Yes    | Verify     | Yes            |
| Accounts Manager                                | Verified       | Team  | No     | No     | No     | Yes    | Approve    | Yes            |
| Finance Head/ Account Manager/ Purchase Manager | Approved       | All   | No     | No     | No     | Yes    | Sent       | Yes            |
| Accounts Executive                              | Sent           | Team  | No     | No     | No     | No     | Adjust     | No             |
| Accounts Executive                              | Partially Paid | Team  | No     | No     | No     | No     | Adjust     | No             |
| Accounts Executive/ Manager                     | Paid           | Team  | No     | No     | No     | No     | Complete   | No             |
| Auditor                                         | Any            | All   | No     | No     | No     | No     | No         | No             |
| Admin                                           | Any            | All   | Yes    | Yes    | Yes    | Yes    | Any        | Any            |

## Header Fields

| Field                  | Description                                          | Type        | Required |
|:---------------------- |:---------------------------------------------------- |:----------- |:-------- |
| Debit Note Number      | Unique debit note number                             | Auto Number | Yes      |
| Debit Note Date        | Debit note date                                      | Date        | Yes      |
| Vendor                 | Vendor reference                                     | Lookup      | Yes      |
| Currency               | Currency code                                        | Lookup      | No       |
| Linked Purchase Return | Linked purchase return                               | Lookup      | Yes      |
| Linked Dispatch Number | Linked dispatch document                             | Lookup      | Yes      |
| Linked Invoice Number  | Linked purchase invoice                              | Lookup      | No       |
| Adjustment Type        | Return / Damage / Excess Charge / Rate Difference    | Dropdown    | Yes      |
| Reason Category        | Vendor Damage / Expiry / Wrong Item / Quantity Issue | Dropdown    | Yes      |

## Detail Fields

| Field           | Description                   | Type          | Required |
|:--------------- |:----------------------------- |:------------- |:-------- |
| Product         | Product name                  | Lookup        | Yes      |
| Variety         | Product variety/specification | Lookup/Text   | No       |
| Pack            | Product packaging type        | Lookup/Text   | No       |
| Batch Number    | Product batch number          | Text          | No       |
| Return Quantity | Returned quantity             | Decimal       | Yes      |
| UOM             | Unit of measurement           | Lookup        | Yes      |
| Rate            | Product rate                  | Currency      | Yes      |
| Tax             | GST/tax amount                | Decimal       | No       |
| Line Amount     | Item adjustment amount        | Currency      | Yes      |
| Debit Reason    | Reason for debit adjustment   | Dropdown/Text | Yes      |
| Remarks         | Item remarks                  | Text          | No       |

## Tax & Financial Information

| Field            | Description               | Type     |
|:---------------- |:------------------------- |:-------- |
| Subtotal         | Total before tax          | Currency |
| Total Tax        | Total GST/tax amount      | Currency |
| Discount Amount  | Discount amount           | Currency |
| Net Debit Amount | Final debit amount        | Currency |
| Round Off        | Rounded amount            | Currency |
| TDS Adjustment   | TDS adjustment amount     | Currency |
| Financial Year   | Accounting financial year | Lookup   |

## Summary Section

| Field                   | Description               |
|:----------------------- |:------------------------- |
| Total Return Quantity   | Total returned quantity   |
| Total Accepted Quantity | Total approved quantity   |
| Total Item Amount       | Total item value          |
| Total Tax Amount        | Total GST/tax amount      |
| Total Adjusted Amount   | Total adjusted amount     |
| Remaining Adjustment    | Pending adjustment amount |
| Net Debit Amount        | Final debit amount        |
| Round Off               | Rounded amount            |

## Additional Information

| Field              | Description                          |
|:------------------ |:------------------------------------ |
| Remarks            | Additional notes                     |
| Approval Notes     | Internal approval comments           |
| Verification Notes | Verification remarks                 |
| Rejection Reason   | Reason for rejection or cancellation |

## Media & Attachments

| Field                 | Description                 | Type         |
|:--------------------- |:--------------------------- |:------------ |
| Attachments           | Upload supporting documents | File Upload  |
| Debit Note PDF        | Generated debit note copy   | File Upload  |
| Vendor Communication  | Vendor emails/documents     | File Upload  |
| Purchase Return Copy  | Purchase return attachment  | File Upload  |
| Dispatch Documents    | Dispatch proof/challan      | File Upload  |
| Product Damage Images | Product damage evidence     | Image Upload |
| File Visibility       | Internal / Vendor Visible   | Dropdown     |
| Notes                 | Media related remarks       | Text         |

## System Generated Rules

| Trigger                   | System Action                       |
|:------------------------- |:----------------------------------- |
| Purchase Return Completed | Enable debit note creation          |
| Dispatch Delivered        | Validate physical return completion |
| Debit Note Approved       | Create vendor ledger adjustment     |
| Partial vendor adjustment | Update stage to Partially Adjusted  |
| Full vendor adjustment    | Update stage to Adjusted            |
| Debit Note Completed      | Close accounting adjustment         |
| Debit Note Cancelled      | Reverse accounting adjustment       |

## Validation Rules

| Rule                        | Description                                               |
|:--------------------------- |:--------------------------------------------------------- |
| Purchase Return Required    | Debit note requires linked purchase return                |
| Dispatch Validation         | Dispatch must be completed before adjustment              |
| Quantity Validation         | Debit quantity cannot exceed return quantity              |
| Vendor Validation           | Vendor must match linked return documents                 |
| Financial Validation        | Adjustment amount must match approved quantity            |
| Tax Validation              | GST/tax validation mandatory for taxable items            |
| Completed Debit Note Locked | Completed debit note cannot be modified                   |
| Rollback Required           | Approved debit note requires rollback before modification |

# Reports

| Report                            | Purpose                            |
|:--------------------------------- |:---------------------------------- |
| Debit Note Register               | Shows all debit notes              |
| Vendor Adjustment Report          | Vendor-wise adjustment summary     |
| Pending Adjustment Report         | Pending vendor adjustment analysis |
| Tax Adjustment Report             | GST/tax adjustment report          |
| Purchase Return Adjustment Report | Linked return analysis             |
| Vendor Ledger Adjustment Report   | Vendor balance adjustment tracking |
| Debit Note Aging Report           | Pending adjustment aging           |

---

# Firestore Database Structure

## Collection: debit_notes

### Document Structure

| Field Name       | Data Type | Description                |
|:---------------- |:--------- |:-------------------------- |
| debitNoteNumber  | string    | Unique debit note number   |
| debitNoteDate    | timestamp | Debit note date            |
| vendorId         | string    | Vendor reference           |
| vendorName       | string    | Vendor snapshot            |
| purchaseReturnId | string    | Linked purchase return     |
| dispatchId       | string    | Linked dispatch ID         |
| invoiceId        | string    | Linked invoice ID          |
| adjustmentType   | string    | Adjustment category        |
| subtotal         | number    | Total before tax           |
| totalTax         | number    | Total GST/tax              |
| discountAmount   | number    | Discount amount            |
| netDebitAmount   | number    | Final debit amount         |
| adjustedAmount   | number    | Adjusted amount            |
| remainingAmount  | number    | Remaining adjustment       |
| adjustmentStatus | string    | Adjustment workflow status |
| stage            | string    | Current workflow stage     |
| remarks          | string    | Additional remarks         |
| createdBy        | string    | User ID                    |
| createdAt        | timestamp | Record creation time       |
| updatedAt        | timestamp | Record update time         |
| branchId         | string    | Branch ID                  |
| companyId        | string    | Company ID                 |
| isDeleted        | boolean   | Soft delete flag           |

## Subcollection: items

Path:

debit_notes/{debitNoteId}/items/{itemId}

| Field Name       | Data Type | Description            |
|:---------------- |:--------- |:---------------------- |
| productId        | string    | Product reference      |
| productName      | string    | Product name           |
| sku              | string    | Product SKU            |
| variety          | string    | Product variety        |
| pack             | string    | Product packaging      |
| batchNumber      | string    | Batch number           |
| returnQuantity   | number    | Return quantity        |
| acceptedQuantity | number    | Accepted quantity      |
| uom              | string    | Unit of measurement    |
| rate             | number    | Product rate           |
| tax              | number    | GST/tax amount         |
| lineAmount       | number    | Item adjustment amount |
| debitReason      | string    | Adjustment reason      |
| remarks          | string    | Item remarks           |

## Subcollection: attachments

Path:

debit_notes/{debitNoteId}/attachments/{attachmentId}

| Field Name | Data Type | Description        |
|:---------- |:--------- |:------------------ |
| fileName   | string    | Uploaded file name |
| fileUrl    | string    | Cloud storage URL  |
| fileType   | string    | MIME type          |
| uploadedBy | string    | User ID            |
| uploadedAt | timestamp | Upload timestamp   |
| notes      | string    | Attachment remarks |