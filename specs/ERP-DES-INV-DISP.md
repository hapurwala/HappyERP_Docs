# Module Design Details

This document gives basic design details of following module.
- **Software**: **HappyERP**
- **Application**: Inventory
- **Module**: Dispatch

## Roles

| Role | Purpose |
| :---- | :---- |
| Dispatch Executive | Creates and manages dispatch operations |
| Dispatch Manager | Reviews and approves dispatch workflows |
| Logistics Head | Handles transportation and delivery coordination |

## Additional Roles

| Role | Purpose |
| :---- | :---- |
| Store Manager | Verifies warehouse stock and dispatch quantity |
| Purchase Manager | Reviews vendor return dispatches |
| Accounts Executive | Validates accounting and debit note relevance |
| Auditor | Read-only audit and compliance access |
| System Admin | Full system configuration and control |

## Workflow Stages

| Stage | Description | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles | System Action |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Draft | Initial dispatch stage where dispatch entry is created and editable. | User | Yes | Yes | Yes | Dispatch Executive, Dispatch Manager, System Admin | Validate Reference Document, Product Quantity and Warehouse Stock |
| Submitted | Dispatch submitted for verification and approval. | User | Yes | No | Yes | Dispatch Executive, Dispatch Manager, Transport Manager, Store Manager, System Admin | Lock Dispatch Number Sequence, Notify Verification Roles |
| Verified | Stock and dispatch quantity verified successfully. | User | No | No | Yes | Dispatch Executive, Dispatch Manager, Store Manager, Transport Manager, Purchase Manager, System Admin | Reserve Inventory Stock, Validate Batch and Serial Numbers |
| Loaded/Packed | Materials loaded into transport vehicle. | User | No | No | Yes | Dispatch Executive, Dispatch Manager, Transport Manager, System Admin | Generate Packing Slip, Update Loading Status |
| In Transit | Dispatch moved from warehouse and currently in transit. | User/System | No | No | No | Dispatch Executive, Dispatch Manager, Transport Manager, Accounts Executive, Auditor, System Admin | Deduct Inventory Stock, Generate E-Way Bill and Transport Tracking |
| Delivered | Full delivery completed successfully. | System/User | No | No | No | Dispatch Executive, Dispatch Manager, Transport Manager, Accounts Executive, Auditor, System Admin | Update Delivery Status, Trigger Invoice or Debit Note Workflow |
| Completed | Dispatch lifecycle completed successfully. | System | No | No | No | Dispatch Executive, Dispatch Manager, Transport Manager, Accounts Executive, Auditor, System Admin | Close Dispatch Lifecycle and Inventory Movement |
| Cancelled | Dispatch cancelled due to operational or logistics issues. | User | No | No | No | Dispatch Executive, Dispatch Manager, Transport Manager, Auditor, System Admin | Reverse Reserved Inventory and Transport Entries |
| Rejected | Dispatch rejected during approval or verification process. | User | No | No | No | Dispatch Executive, Dispatch Manager, Store Manager, System Admin | Create Rejection Audit Log |

## Workflow - Role Matrix

| Role | Current Stage | Scope | Create | Modify | Delete | Cancel | Next Stage | Rollback Stage |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Dispatch Executive | Draft | Self | Yes | Yes | Yes | No | Submit | No |
| Dispatch Manager | Submitted | Team | No | Yes | No | Yes | Verify | Yes |
| Store Manager | Verified | Team | No | No | No | Yes | Loading | Yes |
| Account Executive | Loaded | Team | No | No | No | Yes | Create Debit Note/ Invoice | Yes |
| Transport Manager | Invoiced | Team | No | No | No | Yes | Dispatch | Yes |
| Dispatch Executive/ Purchase Executive | Dispatched | Team | No | No | No | No | Delivered/ Complete | No |
| Auditor | Any | All | No | No | No | No | No | No |
| Admin | Any | All | Yes | Yes | Yes | Yes | Any | Any |

## Header Fields

| Field | Description | Type | Required |
| :---- | :---- | :---- | :---- |
| Dispatch Number | Unique dispatch number | Auto Number | Yes |
| Dispatch Date | Dispatch entry date | Date | Yes |
| Dispatch Type | Purchase Return / Stock Transfer /  Replacement | Dropdown | Yes |
| Reference Number | Linked document number(e.g Purchase return No., Stock Transfer Number, Sales Order No. etc.) | Lookup | Yes |
| Vendor | Vendor reference | Lookup | No |
| Currency | Currency code | Lookup | No |
| Freight Type | Freight type selection | Dropdown | No |

## Details Section

| Field | Description | Type | Required |
| :---- | :---- | :---- | :---- |
| Product | Product name | Lookup | Yes |
| Variety | Product variety/specification | Lookup/Text | No |
| Pack | Product packaging type | Lookup/Text | No |
| Batch Number | Product batch number | Text | No |
| Serial Number | Product serial number | Text | No |
| Dispatch Quantity | Quantity being dispatched | Decimal | Yes |
| Delivered Quantity | Quantity delivered | Decimal | No |
| Pending Quantity | Remaining quantity | Decimal | No |
| UOM | Unit of measurement | Lookup | Yes |
| Dispatch Remarks | Item-level dispatch remarks | Text | No |
| Source Warehouse | Source warehouse | Lookup | Yes |
| Destination Location | Destination warehouse/vendor/customer | Lookup | Yes |

## Transport Information

| Field | Description | Type |
| :---- | :---- | :---- |
| Transporter Name | Transport company/vendor | Text |
| Vehicle Number | Transport vehicle number | Text |
| Driver Name | Driver name | Text |
| Driver Contact | Driver contact number | Text |
| Freight | Freight Charges | Text |
| LR Number | Logistics receipt number | Text |
| E-Way Bill Number | Government E-way bill reference | Text |
| Route Details | Delivery route details | Text |
| Dispatch Priority | High / Medium / Low | Dropdown |
| Expected Delivery Date | Expected delivery date | Date |
| Actual Delivery Date | Actual delivery date | Date |
| Delivery Status | Pending / Partial / Delivered | Dropdown |
| Delivery Remarks | Delivery related remarks | Text |

## Summary Section

| Field | Description |
| :---- | :---- |
| Total Dispatch Quantity | Total quantity dispatched |
| Total Delivered Quantity | Total quantity delivered |
| Total Pending Quantity | Remaining quantity |
| Total Dispatch Amount | Total dispatch value |
| Freight Charges | Total freight charges |

## Additional Information

| Field | Description |
| :---- | :---- |
| Remarks | Additional notes |
| Approval Notes | Internal approval comments |
| Verification Notes | Dispatch verification remarks |
| Rejection Reason | Reason for rejection or cancellation |

## Media & Attachments

| Field | Description | Type |
| :---- | :---- | :---- |
| Attachments | Upload supporting documents | File Upload |
| Dispatch Challan | Dispatch challan copy | File Upload |
| LR Copy | Logistics receipt attachment | File Upload |
| E-Way Bill Copy | E-way bill document | File Upload |
| Vehicle Images | Vehicle/loading images | Image Upload |
| Delivery Proof | Proof of delivery documents/images | File Upload |
| Product Images | Product images during dispatch | Image Upload |
| File Visibility | Internal / Vendor Visible | Dropdown |
| Notes | Media related remarks | Text |

## System Generated Rules

| Trigger | System Action |
| :---- | :---- |
| Dispatch verified | Reserve inventory stock |
| Dispatch loaded | Mark stock as reserved |
| Dispatch in transit | Deduct inventory stock |
| Partial delivery | Update stage to Partially Delivered |
| Full delivery | Update stage to Delivered |
| Dispatch completed | Close inventory movement |
| Dispatch cancelled | Release reserved inventory |
| Dispatch linked with Purchase Return | Enable Debit Note creation |


## Validation Rules

| Rule | Description |
| :---- | :---- |
| Reference Required | Dispatch requires linked module reference |
| Warehouse Validation | Source warehouse mandatory |
| Quantity Validation | Dispatch quantity cannot exceed available stock |
| Vehicle Validation | Vehicle number required before transit |
| E-Way Bill Validation | Mandatory for taxable transport movement |
| Batch Validation | Batch mandatory for batch-managed products |
| Inventory Validation | Stock must be available before dispatch |
| Completed Dispatch Locked | Completed dispatch cannot be modified |
| Rollback Required | Verified dispatch requires rollback before modification |


## Reports

| Report | Purpose |
| :---- | :---- |
| Dispatch Register | Shows all dispatch records |
| Dispatch Tracking Report | Track dispatch movement status |
| Delivery Report | Delivery completion tracking |
| Pending Dispatch Report | Pending dispatch analysis |
| Vehicle Dispatch Report | Vehicle-wise dispatch summary |
| Warehouse Dispatch Report | Warehouse-wise dispatch report |
| Transporter Performance Report | Transport vendor analysis |
| Inventory Movement Report | Inventory movement tracking |

---

# Firestore Database Structure

## Collection: dispatches

### Document Structure

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| dispatchNumber | string | Unique dispatch number |
| dispatchDate | timestamp | Dispatch date |
| dispatchType | string | Dispatch type |
| referenceModule | string | Linked module |
| referenceId | string | Linked document ID |
| vendorId | string | Vendor reference |
| sourceWarehouseId | string | Source warehouse |
| destinationLocationId | string | Destination location |
| vehicleNumber | string | Vehicle number |
| transporterName | string | Transporter details |
| lrNumber | string | LR reference |
| eWayBillNumber | string | E-way bill reference |
| totalDispatchQuantity | number | Total dispatch quantity |
| totalDeliveredQuantity | number | Delivered quantity |
| totalPendingQuantity | number | Pending quantity |
| totalDispatchAmount | number | Total dispatch amount |
| freightCharges | number | Freight charges |
| netDispatchAmount | number | Final amount |
| deliveryStatus | string | Delivery workflow status |
| inventoryStatus | string | Inventory movement status |
| stage | string | Current workflow stage |
| remarks | string | Additional remarks |
| createdBy | string | User ID |
| createdAt | timestamp | Record creation time |
| updatedAt | timestamp | Record update time |
| branchId | string | Branch ID |
| companyId | string | Company ID |
| isDeleted | boolean | Soft delete flag |

## Subcollection: items

Path:

dispatches/{dispatchId}/items/{itemId}

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| productId | string | Product reference |
| productName | string | Product name |
| sku | string | Product SKU |
| variety | string | Product variety |
| pack | string | Product packaging |
| batchNumber | string | Batch number |
| serialNumber | string | Serial number |
| dispatchQuantity | number | Dispatch quantity |
| deliveredQuantity | number | Delivered quantity |
| pendingQuantity | number | Remaining quantity |
| uom | string | Unit of measurement |
| rate | number | Product rate |
| tax | number | GST/tax amount |
| itemAmount | number | Total item amount |
| remarks | string | Item remarks |

## Subcollection: tracking_logs

Path:

dispatches/{dispatchId}/tracking_logs/{trackingId}

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| status | string | Tracking status |
| location | string | Current dispatch location |
| remarks | string | Tracking remarks |
| updatedBy | string | Updated user |
| updatedAt | timestamp | Tracking update time |


## Subcollection: attachments

Path:

dispatches/{dispatchId}/attachments/{attachmentId}

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| fileName | string | Uploaded file name |
| fileUrl | string | Cloud storage URL |
| fileType | string | MIME type |
| uploadedBy | string | User ID |
| uploadedAt | timestamp | Upload timestamp |
| notes | string | Attachment remarks |