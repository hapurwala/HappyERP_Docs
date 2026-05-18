# Happy ERP Purchase Module Detailed

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

# ERP Roles

## System Roles

These roles are global roles for all modules of the ERP.

| Role | Purpose |
| :---- | :---- |
| Auditor | Read-only audit and compliance access |
| Admin | Full system configuration and control |
| System Admin | Full system configuration and control and development access |

---

# Common Guidelines

## System Fields

These fields will be used in all modules (in all tables / collections)

| Field | Description |
| :---- | :---- |
| Created By | User who created record |
| Created Date | Record creation date |
| Updated By | Last modified user |
| Updated Date | Last modification date |
| Stage | Current workflow stage |
| Approval Stage | Approval workflow stage |

---

# Happy ERP Purchase Module Detailed

## Purchase Modules

| Module | Purpose | Main Features | Workflow | Important Notes |
| :---- | :---- | :---- | :---- | :---- |
| Purchase Requisition | Used by employees or departments to request products or raw materials. | Create requisition, approval flow, item selection, department wise request. | Draft → Submitted → Approved → Converted → Completed → Cancelled | Starting point of purchase lifecycle. |
| Request for Quotation (RFQ) | Send inquiry to multiple vendors for best price comparison. | Vendor selection, quotation comparison, email sharing, expiry date. | Draft → Sent → Vendor Responded → Compared → Completed → Cancelled | Used for vendor comparison and negotiation. |
| Vendor Quotation | Store quotations received from vendors. | Vendor rates, taxes, delivery time, comparison table. | Draft → Received → Compared → Approved → Rejected → Converted | Approved quotation converts to PO. |
| Purchase Order (PO) | Official order sent to vendor for purchasing items. | PO number, approval, item table, tax, payment terms. | Draft → Submitted → Approved → Ordered → Partial Received → Completed  → Cancelled → Rejected | Core module of purchase operations. |
| Goods Receipt Note (GRN) | Used when purchased items are received in warehouse/store. | Receive quantity, damaged quantity, batch tracking, stock update. | Draft → Received → Partial Verified → Verified → Completed → Cancelled | Updates inventory stock automatically. |
| Purchase Invoice | Vendor bill entry for accounting and payment process. | Invoice number, GST, amount, due date, attachments. | Draft → Submitted → Verified → Approved → Paid → Completed → Cancelled | Integrated with PO and GRN. |
| Vendor Payment | Track and manage vendor payments. | Payment entry, due tracking, payment mode, balance. | Draft → Submitted → Approved → Initiated → Completed → Cancelled | Integrated with accounting module. |
| Purchase Return | Return damaged or extra products back to vendor. | Return quantity, reason, debit note integration. | Draft → Submitted → Approved → Returned → Completed → Cancelled | Reduces stock automatically. |
| Dispatch | Used when purchased items are returned from warehouse/store. | Quantity, batch tracking, stock update. | Draft → Submitted → Partial Verified → Verified → Loaded → Completed → Cancelled | Updates inventory stock automatically. |
| Debit Note | Raised against vendor for damaged/incorrect goods. | Vendor adjustment, amount deduction, return linkage. | Draft → Submitted → Approved → Sent → Completed → Cancelled | Used for accounting adjustments. |
| Purchase Reports | View analytics and reports related to purchases. | Vendor ledger, PO report, GRN report, pending invoices. | Draft → Generated → Exported → Archived | Important for management decisions. |
---

## Masters

| Master | Purpose | Required Fields | Notes |
| :---- | :---- | :---- | :---- |
| Vendor Master | Store all vendor information. | Vendor Name, GSTIN, Address, Phone, Email, Payment Terms | Should support multiple contacts and bank details. |
| Product Master | Manage purchasable products and raw materials. | Name, SKU, UOM, Tax, Price, Category | Can connect with inventory and sales. |
| Warehouse Master | Manage warehouse locations. | Warehouse Name, Branch, Type | Used in GRN and stock transfer. |
| Tax Master | Manage GST and tax structure. | GST %, HSN Code, Tax Type | Required for invoice and compliance. |
| UOM Master | Manage units of measurement. | UOM Name, Symbol | Examples: Kg, PCS, Box. |
| Payment Terms | Define vendor payment rules. | Credit Days, Advance %, Due Rules | Used in PO and invoice. |

---

# Purchase Module Roles

| Role | Purpose |
| :---- | :---- |
| Purchase Executive | Creates and manages operational purchase documents |
| Purchase Manager | Reviews and approves purchase workflows |
| Procurement Head | Handles final approval and procurement strategy |

---

# Purchase Module \- Additional Roles

| Role | Purpose |
| :---- | :---- |
| Accounts Executive | Handles invoices and vendor payments |
| Store Manager | Handles GRN and warehouse receiving |
| Inventory Manager | Verifies stock and warehouse operations |
| Auditor | Read-only audit and compliance access |
| System Admin | Full system configuration and control |

---

# Purchase Order (PO)

## Purchase Order \- Workflow Stages

| Stage | Description | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Draft | Initial stage where Purchase Order is created and fully editable by Purchase Executive. | User  | Yes  | Yes |  Yes | Purchase Executive, Purchase Manager, System Admin |
| Submitted | Purchase Order submitted for approval and waiting for manager review. | User | Yes | No  | Yes | Purchase Executive, Purchase Manager, Procurement Head, System Admin |
| Approved | Purchase Order approved by authorized manager and ready for vendor ordering process. | User | No | No | Yes | Purchase Executive, Purchase Manager, Procurement Head, Accounts Executive, Store Manager, System Admin |
| Ordered | Official order placed with vendor and procurement process started. | User | No | No | Yes | Purchase Executive, Purchase Manager, Procurement Head, Store Manager, Inventory Manager, Accounts Executive, System Admin |
| Partial Received | Some products or quantities received from vendor but order is not fully completed. | System (Automatically updated when GRN quantity is less than ordered quantity) | No | No | No | Purchase Executive, Purchase Manager, Store Manager, Inventory Manager, Accounts Executive, System Admin |
| Fully Received | Full products and quantities received from vendor | System (Automatically updated when GRN quantity is received as per ordered quantity) | No | No | No | Purchase Executive, Purchase Manager, Store Manager, Inventory Manager, Accounts Executive, System Admin |
| Completed | All products successfully received and verified against Purchase Order. | User | No | No | No | Purchase Executive, Purchase Manager, Procurement Head, Store Manager, Inventory Manager, Accounts Executive, Auditor, System Admin |
| Cancelled | Purchase Order cancelled before completion due to operational or business reasons. | User | No | No | No | Purchase Executive, Purchase Manager, Procurement Head, Auditor, System Admin |
| Rejected | Purchase Order rejected during approval process because of validation or business issues. | User | No | No | No | Purchase Executive, Purchase Manager, Procurement Head, System Admin |

---

## Purchase Order Workflow Matrix

| Role | Current Stage | Scope | Create | Modify | Delete | Cancel | Next Stage | Rollback Stage |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Purchase Executive | Draft | Self | Yes | Yes | Yes | No | Submit | No |
| Purchase Manager | Submitted | Team | No | Yes | No | Yes | Approve | Yes |
| Purchase Manager | Approved | Team | No | No | No | Yes | Order | Yes |
| Procurement Head | Approved | All | No | No | No | Yes | Order | Yes |
| Store Manager | Ordered | Team | No | No | No | No | Create GRN | No |
| Store Manager | Partial Received | Team | No | No | No | No | Create GRN/ Create Invoice | No |
| Accounts Executive | Completed | Team | No | No | No | No | Create Invoice | No |
| Auditor | Any | All | No | No | No | No | No | No |
| Admin | Draft | All | Yes | Yes | Yes | Yes | Submit | No |
| Admin | Submitted | All | No | Yes | No | Yes | Approve | Yes |
| Admin | Approved | All | No | No | No | Yes | Order | Yes |
| Admin | Ordered | All | No | No | No | No | Create GRN | Yes |
| Admin | Partial Received | All | No | No | No | No | Create GRN | No |
| Admin | Completed | All | No | No | No | No | Close | Yes |
| Admin | Closed | All | No | No | No | No | Reopen | No |
| Admin | Cancelled | All | No | No | No | No | Reopen | No |
| Admin | Rejected | All | No | No | No | No | Rollback | No |

---

## Header Fields

| Field | Description | Type | Required |
| :---- | :---- | :---- | :---- |
| PO Number | Unique purchase order number | Auto Number | Yes |
| PO Date | Date of order | Date | Yes |
| Vendor | Vendor selection | Lookup | Yes |
| Currency | Currency selection | Lookup | No |
| Freight Type | Type Selection(FOR/Paid/Included in rate) | Lookup | No |

---

## Line Item / Details Section

| Field | Description | Type | Required |
| :---- | :---- | :---- | :---- |
| Product | Product name | Lookup | Yes |
| Variety | Product variety/grade/specification | Lookup/Text | No |
| Pack | Product packaging type or packing quantity | Lookup/Text | No |
| Quantity | Ordered quantity | Decimal | Yes |
| Tolerance | Allowed Tolerance in quantity payment will be full | Decimal | Yes |
| UOM | Unit of measurement | Lookup | Yes |
| Rate | Purchase rate | Currency | Yes |
| Discount | Discount amount, rate or percent | Decimal | No |
| Tax | GST or tax | Decimal | Yes |
| Line Total | Total item amount | Currency | Yes |

---

## Delivery Location / Delivery Schedule

| Field | Description | Type |
| :---- | :---- | :---- |
| Product | Product | Lookup |
| Variety | Product variety/grade/specification | Lookup/Text |
| Pack | Product packaging type or packing quantity | Lookup/Text |
| Quantity | Quantity | Decimal |
| UoM | Unit of Product | string |
| Delivery Location | Warehouse/Vendor/Customer/Employee | Lookup |
| Expected Delivery Date | Warehouse-wise delivery date | Date |
| Delivery Priority | High / Medium / Low | Dropdown |
| Delivery Remarks | Notes for warehouse delivery | Text |

---

## Payment Terms

| Field | Description | Type |
| :---- | :---- | :---- |
| Payment Type | Prepaid / Postpaid / EMI / Partial Payment | Dropdown |
| Payment Methods | Cash / UPI / Bank Transfer / Cheque / Card | Multi Select |
| Credit Days | Allowed credit period | Number |
| Grace Period Days | Allowed extra days before penalty | Number |
| Due Date | Final payment due date | Date |
| Advance Amount | Initial advance payment | Currency |
| Remaining Amount | Pending payable amount | Currency |
| EMI Enabled | Enable installment payment | Boolean |
| EMI Months | Number of installments | Number |
| EMI Interest % | EMI interest percentage | Decimal |
| Early Payment Discount Enabled | Enable early payment discount | Boolean |
| Early Payment Discount Rules | Multiple early payment discount slabs | Array<Map> |
| Late Payment Penalty Enabled | Enable late payment penalty | Boolean |
| Late Payment Penalty Rules | Multiple late payment penalty slabs | Array<Map> |
| Payment Terms Notes | Additional payment conditions | Text |

### Early Payment Discount Rules Structure

```json
[
  {
    "validDays": 7, //within 7 days after invoice date
    "type": "Percentage",
    "value": 5,
  },
  {
    "validDays": 15, //within 15 days after invoice date
    "type": "Fixed Amount",
    "value": 1000,
  }
]
```

### Late Payment Penalty Rules Structure

```json
[
  {
    "lateDays": 7, // 7 days after due date
    "type": "Fixed Amount",
    "value": 5,
    "frequency": "Monthly",
  },
  {
    "lateDays": 15, // 15 days after due date
    "type": "Percentage",
    "value": 2,
    "frequency": "Daily",
  }
]
```

---

### Example Business Rules

- Pay within 15 days → 5% discount  
- Pay after due date → 5% monthly penalty  
- EMI interest applied automatically  
- Partial payment allowed for trusted vendors  
- Penalty starts after grace period

---

## Summary Section

| Field | Description |
| :---- | :---- |
| Subtotal | Total before tax and discount |
| Total Item Discount | Combined discount amount |
| Total Tax | Total GST/tax amount |
| Invoice level discount | discount applied at PO Level |
| Net Amount | Final payable amount |
| Round Off | Rounded final amount |

---

## Additional Information

| Field | Description |
| :---- | :---- |
| Remarks | Additional notes |
| Terms & Conditions | Purchase terms |
| Approval Notes | Internal approval comments |

---

## Media & Attachments

| Field | Description | Type |
| :---- | :---- | :---- |
| Attachments | Upload supporting documents | File Upload |
| Images | Product or document images | Image Upload |
| Vendor Quotation PDF | Upload quotation PDF | File Upload |
| Invoice Attachment | Attach vendor invoice copy | File Upload |
| Approval Documents | Internal approval files | File Upload |
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

## Reports

| Report | Purpose |
| :---- | :---- |
| Purchase Register | Shows all purchase transactions. |
| Vendor Ledger | Vendor-wise balance and transactions. |
| Pending Purchase Orders | Track pending orders. |
| GRN Report | Shows received goods details. |
| Purchase Return Report | Track returned products. |
| Vendor Payment Report | Track pending and completed payments. |
| GST Purchase Report | GST and tax reporting. |

---

# Firestore Database Structure

## Collection: purchase\_orders

### Document Structure

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| poNumber | string | Unique PO number |
| poDate | timestamp | Purchase order date |
| vendorId | string | Vendor document ID |
| vendorName | string | Vendor name snapshot |
| currency | string | Currency code |
| paymentMode | string | Prepaid / Postpaid / EMI |
| creditDays | number | Allowed credit days |
| advanceAmount | number | Advance payment |
| emiMonths | number | EMI duration |
| emiInterest | number | EMI interest percentage |
| subtotal | number | Total before tax |
| totalDiscount | number | Total discount |
| totalTax | number | Total GST/tax |
| netAmount | number | Final payable amount |
| roundOff | number | Rounded amount |
| remarks | string | Additional remarks |
| approvalNotes | string | Internal approval notes |
| stage | string | Current workflow stage |
| approvalStage | string | Approval workflow stage |
| createdBy | string | User ID |
| createdByName | string | User name snapshot |
| updatedBy | string | Last updated user |
| createdAt | timestamp | Record creation timestamp |
| updatedAt | timestamp | Record update timestamp |
| branchId | string | Branch ID |
| companyId | string | Company ID |
| isDeleted | boolean | Soft delete flag |

---

