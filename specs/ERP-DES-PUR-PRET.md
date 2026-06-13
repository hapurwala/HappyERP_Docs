# Design Details

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: Purchase
- **Module**: Purchase Invoice

# Roles

| Role                | Purpose                                          |
|:------------------- |:------------------------------------------------ |
| Purchase  Executive | Creates and manages purchase return operations   |
| Purchase  Manager   | Reviews and approves warehouse return workflows  |
| Procurement Head    | Handles vendor return approvals and coordination |

# Additional Roles

| Role               | Purpose                                       |
|:------------------ |:--------------------------------------------- |
| Accounts Executive | Handles debit note and accounting adjustments |
| Dispatch Executive | Handles return dispatch process               |
| Auditor            | Read-only audit and compliance access         |
| System Admin       | Full system configuration and control         |

## Workflow Stages

| Stage                | Description                                                       | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles                                                                                       | System Action                                               |
|:-------------------- |:----------------------------------------------------------------- |:--------------- |:------------ |:------------ |:------------ |:--------------------------------------------------------------------------------------------------------------- |:----------------------------------------------------------- |
| Draft                | Initial stage where purchase return is created and editable.      | User            | Yes          | Yes          | Yes          | Store Executive, Store Manager, Purchase Manager, System Admin                                                  | Validate GRN, Invoice and Return Quantity                   |
| Submitted            | Purchase return submitted for approval and verification.          | User            | Yes          | No           | Yes          | Store Executive, Store Manager, Purchase Manager, Accounts Executive, System Admin                              | Lock Purchase Return Number Sequence, Notify Approval Roles |
| Approved             | Purchase return approved for dispatch and vendor processing.      | User            | No           | No           | Yes          | Store Executive, Store Manager, Purchase Manager, Dispatch Executive, Accounts Executive, System Admin          | Create Dispatch Request, Reserve Return Inventory           |
| Partially Dispatched | Some return items dispatched to vendor.                           | System/User     | No           | No           | No           | Store Executive, Store Manager, Purchase Manager, Dispatch Executive, Accounts Executive, Auditor, System Admin | Update Partial Return Dispatch Quantity                     |
| Dispatched           | Full return goods dispatched to vendor successfully.              | System/User     | No           | No           | No           | Store Executive, Store Manager, Purchase Manager, Dispatch Executive, Accounts Executive, Auditor, System Admin | Create Debit Note Request, Update Inventory Movement        |
| Debit Note Created   | Debit note generated against vendor for return adjustment.        | System          | No           | No           | No           | Accounts Executive, Purchase Manager, Auditor, System Admin                                                     | Post Vendor Adjustment Entry in Ledger                      |
| Completed            | Purchase return lifecycle completed successfully.                 | System          | No           | No           | No           | Store Executive, Store Manager, Purchase Manager, Accounts Executive, Auditor, System Admin                     | Close Purchase Return Lifecycle                             |
| Cancelled            | Purchase return cancelled due to operational reasons.             | User            | No           | No           | No           | Store Executive, Store Manager, Purchase Manager, Auditor, System Admin                                         | Reverse Reserved Return Inventory                           |
| Rejected             | Purchase return rejected during approval or verification process. | User            | No           | No           | No           | Store Executive, Store Manager, Purchase Manager, Quality Inspector, System Admin                               | Create Rejection Audit Log                                  |

## Workflow - Role Matrix

| Role                               | Current Stage                          | Scope | Create | Modify | Delete | Cancel | Next Stage        | Rollback Stage |
|:---------------------------------- |:-------------------------------------- |:----- |:------ |:------ |:------ |:------ |:----------------- |:-------------- |
| Purchase Executive                 | Draft                                  | Self  | Yes    | Yes    | Yes    | No     | Submit            | No             |
| Purchase Manager                   | Submitted                              | Team  | No     | No     | No     | Yes    | Approve           | Yes            |
| Dispatch Executive                 | Approved                               | All   | No     | No     | No     | No     | Create Dispatch   | No             |
| Dispatch Executive                 | Partially Dispatched                   | All   | No     | No     | No     | No     | Create Dispatch   | No             |
| Accounts Executive                 | Partially Dispatched/ Fully Dispatched | All   | No     | No     | No     | No     | Create Debit Note | No             |
| Purchase Manager/ Procurement Head | Debit Note Created                     | Team  | No     | No     | No     | No     | Complete          | No             |
| Auditor                            | Any                                    | All   | No     | No     | No     | No     | No                | No             |
| Admin                              | Any                                    | All   | Yes    | Yes    | Yes    | Yes    | Any               | Any            |

## Header Fields

| Field          | Description                   | Type        | Required |
|:-------------- |:----------------------------- |:----------- |:-------- |
| Return Number  | Unique purchase return number | Auto Number | Yes      |
| Return Date    | Return entry date             | Date        | Yes      |
| Vendor         | Vendor reference              | Lookup      | Yes      |
| PO Number      | Linked purchase order         | Lookup      | Yes      |
| GRN Number     | Linked GRN                    | Lookup      | Yes      |
| Invoice Number | Linked purchase invoice       | Lookup      | No       |
| Currency       | Currency code                 | Lookup      | No       |
| Freight Type   | Return freight type           | Dropdown    | No       |

## Details Section

| Field                | Description                             | Type            | Required |
|:-------------------- |:--------------------------------------- |:--------------- |:-------- |
| Product              | Product name                            | Lookup          | Yes      |
| Variety              | Product variety/specification           | Lookup/Text     | No       |
| Pack                 | Product packaging type                  | Lookup/Text     | No       |
| Batch Number         | Product batch number                    | Text            | No       |
| Return Quantity      | Quantity being returned                 | Decimal         | Yes      |
| UOM                  | Unit of measurement                     | Lookup          | Yes      |
| Rate                 | Product purchase rate                   | Currency        | Yes      |
| Tax                  | GST/tax amount                          | Decimal         | No       |
| Return Amount        | Total return amount                     | Currency        | Yes      |
| Return Type / Reason | Damaged / Expired / Excess / Wrong Item | Dropdown / Text | Yes      |
| Warehouse            | Return warehouse                        | Lookup          | Yes      |

## Summary Section

| Field                   | Description                   |
|:----------------------- |:----------------------------- |
| Total Return Quantity   | Total quantity being returned |
| Total Accepted Quantity | Approved return quantity      |
| Total Rejected Quantity | Rejected return quantity      |
| Total Return Amount     | Total return value            |
| Total Tax Amount        | Total GST/tax amount          |
| Net Return Amount       | Final return amount           |
| Round Off               | Rounded amount                |

## Additional Information

| Field            | Description                          |
|:---------------- |:------------------------------------ |
| Remarks          | Additional notes                     |
| Approval Notes   | Internal approval comments           |
| Rejection Reason | Reason for rejection or cancellation |

## Media & Attachments

| Field                | Description                 | Type         |
|:-------------------- |:--------------------------- |:------------ |
| Attachments          | Upload supporting documents | File Upload  |
| Product Images       | Returned product images     | Image Upload |
| Damage Proof Images  | Damaged product evidence    | Image Upload |
| Vendor Communication | Vendor emails/documents     | File Upload  |
| Dispatch Documents   | Dispatch challan/LR copy    | File Upload  |
| Debit Note Copy      | Debit note attachment       | File Upload  |
| File Visibility      | Internal / Vendor Visible   | Dropdown     |
| Notes                | Media related remarks       | Text         |

## System Generated Rules

| Trigger                    | System Action                        |
|:-------------------------- |:------------------------------------ |
| Purchase Return Approved   | Create Dispatch entry                |
| Dispatch Completed         | Update return stage to Dispatched    |
| Partial Dispatch           | Update stage to Partially Dispatched |
| Purchase Return Dispatched | Create Debit Note                    |
| Debit Note Created         | Update accounting adjustment         |
| Return quantity approved   | Reduce inventory stock               |
| Return cancelled           | Reverse reserved stock               |

## Validation Rules

| Rule                    | Description                                           |
|:----------------------- |:----------------------------------------------------- |
| PO Required             | Purchase return requires linked PO                    |
| GRN Required            | Purchase return requires linked GRN                   |
| Quantity Validation     | Return quantity cannot exceed GRN accepted quantity   |
| Dispatch Validation     | Dispatch required before completion                   |
| Debit Note Validation   | Debit note required for financial adjustment          |
| Batch Validation        | Batch mandatory for batch-managed products            |
| Completed Return Locked | Completed return cannot be modified                   |
| Rollback Required       | Approved return requires rollback before modification |

## Reports

| Report                   | Purpose                        |
|:------------------------ |:------------------------------ |
| Purchase Return Register | Shows all purchase returns     |
| Vendor Return Report     | Vendor-wise return summary     |
| Damaged Goods Report     | Track damaged products         |
| Return Dispatch Report   | Return dispatch tracking       |
| Debit Note Report        | Vendor adjustment report       |
| Warehouse Return Report  | Warehouse-wise return analysis |
| Product Return Analysis  | Product-wise return trends     |

---

# Firestore Database Structure

## Collection: purchase_returns

### Document Structure

| Field Name          | Data Type | Description                |
|:------------------- |:--------- |:-------------------------- |
| returnNumber        | string    | Unique return number       |
| returnDate          | timestamp | Return date                |
| vendorId            | string    | Vendor reference           |
| vendorName          | string    | Vendor snapshot            |
| poId                | string    | Linked PO ID               |
| grnId               | string    | Linked GRN ID              |
| invoiceId           | string    | Linked invoice ID          |
| warehouseId         | string    | Warehouse reference        |
| returnType          | string    | Return type                |
| totalReturnQuantity | number    | Total return quantity      |
| totalReturnAmount   | number    | Total return amount        |
| totalTaxAmount      | number    | Total GST/tax              |
| netReturnAmount     | number    | Final amount               |
| dispatchStatus      | string    | Dispatch workflow status   |
| debitNoteStatus     | string    | Debit note workflow status |
| stage               | string    | Current workflow stage     |
| remarks             | string    | Additional remarks         |
| createdBy           | string    | User ID                    |
| createdAt           | timestamp | Record creation time       |
| updatedAt           | timestamp | Record update time         |
| branchId            | string    | Branch ID                  |
| companyId           | string    | Company ID                 |
| isDeleted           | boolean   | Soft delete flag           |

## Subcollection: items

Path:

purchase_returns/{returnId}/items/{itemId}

| Field Name       | Data Type | Description         |
|:---------------- |:--------- |:------------------- |
| productId        | string    | Product reference   |
| productName      | string    | Product name        |
| sku              | string    | Product SKU         |
| variety          | string    | Product variety     |
| pack             | string    | Product packaging   |
| batchNumber      | string    | Batch number        |
| returnQuantity   | number    | Return quantity     |
| acceptedQuantity | number    | Accepted quantity   |
| rejectedQuantity | number    | Rejected quantity   |
| uom              | string    | Unit of measurement |
| rate             | number    | Product rate        |
| tax              | number    | GST/tax amount      |
| returnAmount     | number    | Return value        |
| returnReason     | string    | Return reason       |
| qualityRemarks   | string    | Quality notes       |

## Subcollection: attachments

Path:

purchase_returns/{returnId}/attachments/{attachmentId}

| Field Name | Data Type | Description        |
|:---------- |:--------- |:------------------ |
| fileName   | string    | Uploaded file name |
| fileUrl    | string    | Cloud storage URL  |
| fileType   | string    | MIME type          |
| uploadedBy | string    | User ID            |
| uploadedAt | timestamp | Upload timestamp   |
| notes      | string    | Attachment remarks |