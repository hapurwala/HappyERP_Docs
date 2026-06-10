# Module Design Details

This document gives basic design details of following module.
- **Software**: **HappyERP**
- **Application**: Accounts
- **Module**: Payment Adjustment

## Roles

| Role | Purpose |
| :---- | :---- |
| Cashier | Validates payment receipt/payment references |
| Accounts Executive | Creates and manages payment adjustments |
| Accounts Manager | Reviews and verifies accounting adjustments |
| Finance Head | Approves final financial settlement adjustments |

## Additional Roles

| Role | Purpose |
| :---- | :---- |
| Purchase Manager | Verifies vendor and purchase linkage |
| Auditor | Read-only audit and compliance access |
| System Admin | Full system configuration and control |

## Workflow Stages

| Stage | Description | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles | System Action |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Draft | Initial stage where adjustment entry is created and editable. | User | Yes | Yes | Yes | Accounts Executive, Accounts Manager, Finance Head, System Admin | Calculate Early Payment Discount or Late Payment Penalty |
| Approved | Adjustment approved for accounting settlement process. | User | No | No | Yes | Accounts Executive, Accounts Manager, Finance Head, Cashier, Auditor, System Admin | Post Adjustment Entries in Vendor Ledger |

## Workflow - Role Matrix

| Role | Current Stage | Scope | Create | Modify | Delete | Cancel | Next Stage | Rollback Stage |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Accounts Executive / Purchase Manager | Draft | Self | Yes | Yes | Yes | No | --- | No |
| Accounts Manager / Purchase Manager / Finance Head | Draft | Team | No | No | No | Yes | Approve | Yes |

## Header Fields

| Field | Description | Type | Required |
| :---- | :---- | :---- | :---- |
| Adjustment Date | Adjustment entry date | Date | Yes |
| Vendor | Vendor reference | Lookup | Yes |
| Currency | Currency code | Lookup | No |
| Linked Vendor Payment | Linked vendor payment | Lookup | No |
| Linked Receive Payment | Linked receive payment | Lookup | No |
| Linked Debit Note | Linked debit note | Lookup | No |
| Linked Purchase Invoice | Linked purchase invoice | Lookup | No |
| Adjustment Amount | Current adjustment amount | Currency | Yes |
| Settlement Method | Manual / Auto Adjustment | Dropdown | Yes |
| Remarks | Adjustment remarks | Text | No |

## Summary Section

| Field | Description |
| :---- | :---- |
| Unadjusted Credit Amount | |
| Unadjusted Debit Amount | |
| Unadjusted Balance | |
| Ledger Balance | |
| Unapproved Adjustment  Amount | |

## Additional Information

| Field | Description |
| :---- | :---- |
| Remarks | Additional notes |
| Approval Notes | Internal approval comments |

## Media & Attachments

| Field | Description | Type |
| :---- | :---- | :---- |
| Attachments | Upload supporting documents | File Upload |
| Adjustment Proof | Adjustment supporting document | File Upload |
| Ledger Statement | Vendor ledger statement | File Upload |
| Vendor Communication | Vendor emails/documents | File Upload |
| Payment Reference Copy | Payment proof/reference | File Upload |
| Debit Note Copy | Linked debit note copy | File Upload |
| File Visibility | Internal / Vendor Visible | Dropdown |
| Notes | Media related remarks | Text |

## System Generated Rules

| Trigger | System Action |
| :---- | :---- |
| Adjustment approved | Create ledger adjustment entry |
| Linked payment updated | Sync vendor ledger balances |

## Validation Rules

| Rule | Description |
| :---- | :---- |
| Vendor Required | Adjustment requires linked vendor |
| Amount Validation | Adjustment amount cannot exceed remaining balance |
| Debit/Credit Validation | Adjustment can be done between debit and credit entries only. debit to debit or credit to credit adjustment not allow |

## Reports

| Report | Purpose |
| :---- | :---- |
| Payment Adjustment Register | Shows all payment adjustments |
| Vendor Ledger Adjustment Report | Vendor-wise adjustment analysis |
| Reconciliation Report | Adjustment reconciliation tracking |
| Debit/Credit Adjustment Report | Accounting adjustment summary |
| Outstanding Balance Report | Pending unsettled balances |
| Vendor Settlement Report | Vendor settlement analysis |
| Ledger Posting Report | Accounting posting tracking |

---

# Firestore Database Structure

## Collection: payment_adjustments

### Document Structure

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| adjustmentNumber | string | Unique adjustment number |
| adjustmentDate | timestamp | Adjustment date |
| vendorId | string | Vendor reference |
| vendorName | string | Vendor snapshot |
| adjustmentType | string | Adjustment category |
| linkedVendorPaymentId | string | Linked vendor payment |
| linkedReceivePaymentId | string | Linked receive payment |
| linkedDebitNoteId | string | Linked debit note |
| linkedInvoiceId | string | Linked invoice |
| originalAmount | number | Original transaction amount |
| adjustmentAmount | number | Adjustment amount |
| remainingBalance | number | Remaining balance |
| ledgerImpactAmount | number | Ledger impact amount |
| taxAdjustmentAmount | number | GST/TDS adjustment |
| discountAdjustment | number | Discount adjustment |
| penaltyAdjustment | number | Penalty adjustment |
| netAdjustmentAmount | number | Final adjustment amount |
| adjustmentDirection | string | Debit/Credit |
| reconciliationStatus | string | Reconciliation workflow |
| stage | string | Current workflow stage |
| remarks | string | Additional remarks |
| createdBy | string | User ID |
| createdAt | timestamp | Record creation time |
| updatedAt | timestamp | Record update time |
| branchId | string | Branch ID |
| companyId | string | Company ID |
| isDeleted | boolean | Soft delete flag |


## Subcollection: attachments

Path:

payment_adjustments/{adjustmentId}/attachments/{attachmentId}

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| fileName | string | Uploaded file name |
| fileUrl | string | Cloud storage URL |
| fileType | string | MIME type |
| uploadedBy | string | User ID |
| uploadedAt | timestamp | Upload timestamp |
| notes | string | Attachment remarks |