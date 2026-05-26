# Happy ERP Receive Payment Module Detailed

# Workflow Engine

## Action Types

These are global for all modules.

| Action | Description |
| :---- | :---- |
| View | Read record |
| Create | Create new record |
| Edit/Modify | Modify existing record |
| Delete | Soft delete record |
| Cancel | Cancel workflow |
| Submit / Send Forward / Next Stage | Send workflow to next stage |
| Rollback Stage / Send Back | Return to previous stage |
| Export / Share / Print | Download reports/files |

---

# Receive Payment Module Roles

| Role | Purpose |
| :---- | :---- |
| Cashier | Handles actual money receipt process |
| Accounts Executive | Creates and manages receive payment entries |
| Accounts Manager | Reviews and approves payment receipt workflows |
| Finance Head | Handles final accounting approval and settlement |

---

# Receive Payment Module - Additional Roles

| Role | Purpose |
| :---- | :---- |
| Purchase Manager | Verifies debit note and vendor linkage |
| Auditor | Read-only audit and compliance access |
| System Admin | Full system configuration and control |

---

# Receive Payment

## Receive Payment - Workflow Stages

| Stage | Description | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles | System Action |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Draft | Initial stage where receive payment entry is created and editable. | User | Yes | Yes | Yes | Accounts Executive, Accounts Manager, Finance Head, System Admin | --- |
| Submitted | Receive payment submitted for approval and verification. | User | Yes | No | Yes | Accounts Executive, Accounts Manager, Finance Head, Purchase Manager, System Admin | Validate Debit Note and Vendor Ledger |
| Approved | Receive payment approved for collection or adjustment process. | User | No | No | Yes | Accounts Executive, Accounts Manager, Finance Head, Cashier, System Admin | Create Receivable Entry in Ledger |
| Adjusted | Amount adjusted against debit note/vendor ledger successfully. | System | No | No | No | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin | Update Vendor Ledger Balance |
| Completed | Receive payment lifecycle completed successfully. | System | No | No | No | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin | Close Vendor Receivable Entry |
| Cancelled | Receive payment cancelled due to accounting or operational issues. | User | No | No | No | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin | Reverse Receivable Ledger Entries |
| Rejected | Receive payment rejected during approval or verification process. | User | No | No | No | Accounts Executive, Accounts Manager, Finance Head, Purchase Manager, System Admin | Reverse Receivable Ledger Entries |

---

## Receive Payment Workflow Matrix

| Role | Current Stage | Scope | Create | Modify | Delete | Cancel | Next Stage | Rollback Stage |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Cashier / Accounts Executive | Draft | Self | Yes | Yes | Yes | No | Submit | No |
| Accounts Manager/ Finance Head | Submitted | Team | No | Yes | No | Yes | Approve | Yes |
| Account Executive | Approved | Team | No | No | No | No | Adjust | No |
| Cashier | Approved | Team | No | No | No | No | Completed/Fail | No |
| Cashier | Failed | Team | No | No | No | No | Complete | No |
| Auditor | Any | All | No | No | No | No | No | No |
| Admin | Any | All | Yes | Yes | Yes | Yes | Any | Any |

---

## Receive Payment Header Fields

| Field | Description | Type | Required |
| :---- | :---- | :---- | :---- |
| Receive Payment Number | Unique receive payment number | Auto Number | Yes |
| Receive Payment Date | Receive payment date | Date | Yes |
| Vendor | Vendor reference | Lookup | Yes |
| Linked Debit Note Number | Linked debit note | Lookup | Yes |
| Linked Purchase Return | Linked purchase return | Lookup | No |
| Currency | Currency code | Lookup | No |

---

## Receive Payment Details Section

| Field | Description | Type | Required |
| :---- | :---- | :---- | :---- |
| Debit Note Amount | Linked debit note amount | Currency | Yes |
| Previously Received Amount | Already received amount | Currency | No |
| Current Received Amount | Current received amount | Currency | Yes |
| Remaining Amount | Remaining receivable balance | Currency | Yes |
| Bank Charges | Bank/payment gateway charges | Currency | No |
| Discount Amount | Settlement discount amount | Currency | No |
| Penalty Amount | Penalty recovery amount | Currency | No |
| Net Received Amount | Final received amount | Currency | Yes |
| Remarks | Payment remarks | Text | No |

---

## Receive Payment Method Information

| Field | Description | Type | Required |
| :---- | :---- | :---- | :---- |
| Receive Method | Cash / UPI / Bank Transfer / Cheque | Dropdown | Yes |
| Company Bank Account | Company receiving bank account | Lookup | No |
| Vendor Bank Account | Vendor bank account | Lookup | No |
| UTR Number | Transaction reference number | Text | No |
| Cheque Number | Cheque reference number | Text | No |
| Transaction Status | Pending / Success / Failed | Dropdown | Yes |
| Failed Reason | Transaction Failled Reason | Text | No |
| Transaction Date | Actual transaction date | Date | No |

---

## Summary Section

| Field | Description |
| :---- | :---- |
| Total Debit Note Amount | Total debit note amount |
| Total Received Amount | Total received amount |
| Total Adjusted Amount | Total adjusted amount |
| Total Charges | Total bank/payment charges |
| Total Discounts | Total settlement discounts |
| Remaining Balance | Remaining receivable balance |
| Net Receive Amount | Final receive amount |
| Round Off | Rounded amount |

---

## Additional Information

| Field | Description |
| :---- | :---- |
| Remarks | Additional notes |
| Approval Notes | Internal approval comments |
| Verification Notes | Verification remarks |
| Rejection Reason | Reason for rejection or cancellation |

---

## Media & Attachments

| Field | Description | Type |
| :---- | :---- | :---- |
| Attachments | Upload supporting documents | File Upload |
| Receive Payment Receipt | Receive payment receipt copy | File Upload |
| Bank Transfer Slip | Bank transfer proof | File Upload |
| Cheque Image | Cheque image upload | Image Upload |
| Vendor Communication | Vendor emails/documents | File Upload |
| Debit Note Copy | Linked debit note copy | File Upload |
| File Visibility | Internal / Vendor Visible | Dropdown |
| Notes | Media related remarks | Text |

---

### Supported File Types

- Images: JPG, PNG, WEBP
- Documents: PDF, DOCX, XLSX
- Others: ZIP, CSV

### Features

- Multiple file upload
- File preview
- Download attachments
- File size validation
- Role based upload/delete permissions
- Attachment history tracking

---

## System Generated Rules

| Trigger | System Action |
| :---- | :---- |
| Receive payment approved | Create receivable transaction |
| Payment initiated | Lock payment modification |
| Partial payment received | Update stage to Partially Received |
| Full payment received | Update stage to Received |
| Adjustment completed | Update stage to Adjusted |
| Full settlement completed | Update stage to Completed |
| Receive payment cancelled | Reverse accounting adjustment |

---

## Validation Rules

| Rule | Description |
| :---- | :---- |
| Debit Note Required | Receive payment requires linked debit note |
| Amount Validation | Receive amount cannot exceed pending receivable balance |
| Duplicate Transaction Validation | Duplicate transaction reference not allowed |
| Bank Validation | Bank account mandatory for bank transfers |
| UTR Validation | UTR required for online transactions |
| Cheque Validation | Cheque number mandatory for cheque payments |
| Completed Receive Payment Locked | Completed receive payment cannot be modified |
| Rollback Required | Approved receive payment requires rollback before modification |

---

## Reports

| Report | Purpose |
| :---- | :---- |
| Receive Payment Register | Shows all receive payments |
| Pending Receivable Report | Pending receivable summary |
| Vendor Settlement Report | Vendor settlement tracking |
| Debit Note Recovery Report | Debit note recovery analysis |
| Payment Method Report | Receive method analysis |
| Bank Collection Report | Bank collection tracking |
| Adjustment Report | Vendor adjustment analysis |

---

# Firestore Database Structure

## Collection: receive_payments

### Document Structure

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| receivePaymentNumber | string | Unique receive payment number |
| receivePaymentDate | timestamp | Receive payment date |
| vendorId | string | Vendor reference |
| vendorName | string | Vendor snapshot |
| debitNoteId | string | Linked debit note |
| purchaseReturnId | string | Linked purchase return |
| receivePaymentType | string | Receive payment category |
| receiveMethod | string | Payment receive method |
| transactionStatus | string | Transaction status |
| transactionReference | string | UTR/Cheque reference |
| debitNoteAmount | number | Debit note amount |
| receivedAmount | number | Total received amount |
| adjustmentAmount | number | Adjustment amount |
| remainingAmount | number | Remaining receivable amount |
| chargesAmount | number | Bank/payment charges |
| discountAmount | number | Settlement discount |
| netReceiveAmount | number | Final receive amount |
| settlementStatus | string | Settlement workflow status |
| stage | string | Current workflow stage |
| remarks | string | Additional remarks |
| createdBy | string | User ID |
| createdAt | timestamp | Record creation time |
| updatedAt | timestamp | Record update time |
| branchId | string | Branch ID |
| companyId | string | Company ID |
| isDeleted | boolean | Soft delete flag |

---

## Subcollection: attachments

Path:

receive_payments/{receivePaymentId}/attachments/{attachmentId}

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| fileName | string | Uploaded file name |
| fileUrl | string | Cloud storage URL |
| fileType | string | MIME type |
| uploadedBy | string | User ID |
| uploadedAt | timestamp | Upload timestamp |
| notes | string | Attachment remarks |