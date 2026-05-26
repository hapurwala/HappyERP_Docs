# Happy ERP Goods Receipt Note (GRN) Module Detailed

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

# GRN Module Roles

| Role | Purpose |
| :---- | :---- |
| Store Executive | Creates and manages receiving operations |
| Store Manager | Reviews and approves store workflows |
| Store Head | Handles final approval and inventory strategy |
---

# GRN Module \- Additional Roles
| Role | Purpose |
| :---- | :---- |
| Purchase Manager | Reviews received materials |
| Accounts Executive | Validates invoice and payment relevance |
| Auditor | Read-only audit and compliance access |
| System Admin | Full system configuration and control |

---

# Goods Receipt Note (GRN)

## GRN - Workflow Stages

| Stage | Description | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles | System Action |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Draft | Initial stage where GRN is created and editable before verification. | User | Yes | Yes | Yes | Store Manager, Store Head, System Admin | Validate PO and Ordered Quantity |
| Submitted | GRN submitted for inventory verification. | User | No | No | Yes | Store Manager, Store Head, Purchase Manager, System Admin | Lock GRN Number Sequence, Notify Verification Roles |
| Verified | Products verified successfully and stock ready for update. | User | No | No | No | Store Manager, Store Head, Purchase Manager, Accounts Executive, System Admin | Update Inventory Stock, Create Inventory Ledger Entry |
| Partial Verified | Partial products or quantities verified successfully. | User | No | No | No | Store Manager, Store Head, Purchase Manager, Accounts Executive, System Admin | Update Partial Inventory Stock, Update Pending Quantity |
| Completed | Full receiving and inventory update completed successfully. | User | No | No | No | Store Manager, Store Head, Purchase Manager, Accounts Executive, Auditor, System Admin | Close GRN Lifecycle, Update PO Receiving Status |
| Cancelled | GRN cancelled due to operational or verification issues. | User | No | No | No | Store Manager, Store Head, Purchase Manager, Auditor, System Admin | Reverse Inventory Stock Entries |
| Rejected | GRN rejected during verification process. | User | No | No | No | Store Manager, Store Head, Purchase Manager, System Admin | Create Rejection Audit Log |

---

## GRN Workflow Matrix

| Role | Current Stage | Scope | Create | Modify | Delete | Cancel | Next Stage | Rollback Stage |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Store Executive | Draft | Self | Yes | Yes | Yes | No | Submit | No |
| Store Executive | Submitted | Self | No | Yes | No | Yes | Verify | Yes |
| Store Manager | Verified | Team | No | No | No | Yes | Complete | Yes |
| Store Head | Verified | All | No | No | No | Yes | Complete | Yes |
| Accounts Executive | Completed | Team | No | No | No | No | Match Invoice | No |
| Auditor | Any | All | No | No | No | No | No | No |
| Admin | Draft | All | Yes | Yes | Yes | Yes | Submit | No |
| Admin | Submitted | All | No | Yes | No | Yes | Verify | Yes |
| Admin | Verified | All | No | No | No | No | Complete | Yes |
| Admin | Completed | All | No | No | No | No | Close | Yes |
| Admin | Closed | All | No | No | No | No | Reopen | No |
| Admin | Cancelled | All | No | No | No | No | Reopen | No |
| Admin | Rejected | All | No | No | No | No | Rollback | No |
---

## GRN Header Fields

| Field | Description | Type | Required |
| :---- | :---- | :---- | :---- |
| GRN Number | Unique GRN number | Auto Number | Yes |
| GRN Date | Date of goods receiving | Date | Yes |
| PO Number | Linked purchase order | Lookup | Yes |
| Vendor | Vendor reference | Lookup | Yes |
| Invoice Number | Vendor invoice reference | Text | No |
| Vehicle Number | Transport vehicle number | Text | No |
| Transporter Name | Delivery transporter details | Text | No |
| Warehouse | Receiving warehouse | Lookup | Yes |
| Receiving Employee | Employee handling receiving | Lookup | Yes |
| Freight Type | Freight type selection | Lookup | No |

---

## GRN Line Item / Details Section

| Field | Description | Type | Required |
| :---- | :---- | :---- | :---- |
| Product | Product name | Lookup | Yes |
| Variety | Product variety/grade/specification | Lookup/Text | No |
| Pack | Product packaging type or packing quantity | Lookup/Text | No |
| Ordered Quantity | Quantity ordered in PO | Decimal | Yes |
| Received Quantity | Quantity received in current GRN | Decimal | Yes |
| Accepted Quantity | Accepted quantity after verification | Decimal | Yes |
| Rejected Quantity | Rejected quantity after verification | Decimal | No |
| Damaged Quantity | Damaged quantity | Decimal | No |
| Tolerance | Allowed receiving tolerance | Decimal | Yes |
| UOM | Unit of measurement | Lookup | Yes |
| Batch Number | Product batch number | Text | No |
| Manufacturing Date | Manufacturing date | Date | No |
| Expiry Date | Product expiry date | Date | No |
| Remarks | Product receiving remarks | Text | No |

---

## Summary Section

| Field | Description |
| :---- | :---- |
| Total Ordered Quantity | Total ordered quantity |
| Total Received Quantity | Total received quantity |
| Total Accepted Quantity | Total accepted quantity |
| Total Rejected Quantity | Total rejected quantity |
| Total Damaged Quantity | Total damaged quantity |
| Pending Quantity | Remaining pending quantity |

---

## Additional Information

| Field | Description |
| :---- | :---- |
| Remarks | Additional remarks |
| Verification Notes | Inventory verification notes |
| Approval Notes | Internal approval comments |
| Rejection Reason | Reason for rejection or cancellation |

---

## Media & Attachments

| Field | Description | Type |
| :---- | :---- | :---- |
| Attachments | Upload supporting documents | File Upload |
| Invoice Copy | Vendor invoice attachment | File Upload |
| Delivery Challan | Delivery challan copy | File Upload |
| Product Images | Received product images | Image Upload |
| Damage Proof Images | Damaged stock images | Image Upload |
| Quality Inspection Documents | QC verification documents | File Upload |
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
| GRN quantity < PO quantity | Update PO stage to Partial Received |
| GRN quantity = PO quantity | Update PO stage to Completed |
| Accepted quantity updated | Update inventory stock |
| Rejected quantity updated | Create quality inspection alert |
| GRN rollback approved | Unlock GRN for modification |

---

## Validation Rules

| Rule | Description |
| :---- | :---- |
| PO Required | GRN cannot be created without linked PO |
| Quantity Validation | Received quantity cannot exceed pending PO quantity |
| Mandatory Warehouse | Warehouse is mandatory for stock update |
| Completed GRN Locked | Completed GRN cannot be modified directly |
| Rollback Required | Verified GRN requires rollback before modification |
| Batch Validation | Batch number required for batch-managed products |
| Expiry Validation | Expiry date required for expiry-managed products |

---

## Reports

| Report | Purpose |
| :---- | :---- |
| GRN Register | Shows all GRN transactions |
| Pending GRN Report | Pending verification records |
| Warehouse Receiving Report | Warehouse-wise receiving summary |
| Damaged Goods Report | Track damaged inventory |
| Vendor Delivery Report | Vendor delivery performance |
| Inventory Update Report | Inventory stock movement |

---

# Firestore Database Structure

## Collection: grns

### Document Structure

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| grnNumber | string | Unique GRN number |
| grnDate | timestamp | GRN date |
| poId | string | Linked PO ID |
| poNumber | string | PO number snapshot |
| vendorId | string | Vendor reference |
| vendorName | string | Vendor name snapshot |
| warehouseId | string | Warehouse reference |
| warehouseName | string | Warehouse name |
| invoiceNumber | string | Vendor invoice number |
| stage | string | Current workflow stage |
| totalOrderedQuantity | number | Ordered quantity |
| totalReceivedQuantity | number | Received quantity |
| totalAcceptedQuantity | number | Accepted quantity |
| totalRejectedQuantity | number | Rejected quantity |
| totalDamagedQuantity | number | Damaged quantity |
| pendingQuantity | number | Remaining quantity |
| totalItemValue | number | Inventory value |
| freightCharges | number | Freight charges |
| netInventoryValue | number | Final inventory value |
| createdBy | string | User ID |
| createdAt | timestamp | Record creation time |
| updatedAt | timestamp | Record update time |
| companyId | string | Company ID |
| branchId | string | Branch ID |
| isDeleted | boolean | Soft delete flag |

---

## Subcollection: items

Path:

grns/{grnId}/items/{itemId}

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| productId | string | Product reference |
| productName | string | Product name |
| variety | string | Product variety |
| pack | string | Product pack |
| orderedQuantity | number | Ordered quantity |
| receivedQuantity | number | Current received quantity |
| acceptedQuantity | number | Accepted quantity |
| rejectedQuantity | number | Rejected quantity |
| damagedQuantity | number | Damaged quantity |
| pendingQuantity | number | Remaining quantity |
| batchNumber | string | Batch number |
| serialNumber | string | Serial number |
| manufacturingDate | timestamp | Manufacturing date |
| expiryDate | timestamp | Expiry date |
| remarks | string | Item remarks |

---

## Subcollection: attachments

Path:

grns/{grnId}/attachments/{attachmentId}

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| fileName | string | Uploaded file name |
| fileUrl | string | Cloud storage URL |
| fileType | string | MIME type |
| uploadedBy | string | User ID |
| uploadedAt | timestamp | Upload timestamp |
| notes | string | Attachment remarks |