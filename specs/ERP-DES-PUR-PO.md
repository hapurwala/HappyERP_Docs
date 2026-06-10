# Module Design Details

This document gives basic design details of following module.
- **Software**: **HappyERP**
- **Application**: Purchase
- **Module**: Purchase Order

## Roles

| Role               | Purpose                                            |
|:------------------ |:-------------------------------------------------- |
| Purchase Executive | Creates and manages operational purchase documents |
| Purchase Manager   | Reviews and approves purchase workflows            |
| Procurement Head   | Handles final approval and procurement strategy    |

## Additional Roles

| Role               | Purpose                                 |
|:------------------ |:--------------------------------------- |
| Accounts Executive | Handles invoices and vendor payments    |
| Store Manager      | Handles GRN and warehouse receiving     |
| Inventory Manager  | Verifies stock and warehouse operations |
| Auditor            | Read-only audit and compliance access   |
| System Admin       | Full system configuration and control   |

## Workflow Stages

| Stage            | Description                                                                               | Who Will Set It                                                                      | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles                                                                                                           | System Action                                           |
|:---------------- |:----------------------------------------------------------------------------------------- |:------------------------------------------------------------------------------------ |:------------ |:------------ |:------------ |:----------------------------------------------------------------------------------------------------------------------------------- |:------------------------------------------------------- |
| Draft            | Initial stage where Purchase Order is created and fully editable by Purchase Executive.   | User                                                                                 | Yes          | Yes          | Yes          | Purchase Executive, Purchase Manager, System Admin                                                                                  | ---                                                     |
| Submitted        | Purchase Order submitted for approval and waiting for manager review.                     | User                                                                                 | Yes          | No           | Yes          | Purchase Executive, Purchase Manager, Procurement Head, System Admin                                                                | Lock PO Number Sequence, Notify Approval Roles          |
| Approved         | Purchase Order approved by authorized manager and ready for vendor ordering process.      | User                                                                                 | No           | No           | Yes          | Purchase Executive, Purchase Manager, Procurement Head, Accounts Executive, Store Manager, System Admin                             | Reserve Budget, Create Approval Audit Log               |
| Ordered          | Official order placed with vendor and procurement process started.                        | User                                                                                 | No           | No           | Yes          | Purchase Executive, Purchase Manager, Procurement Head, Store Manager, Inventory Manager, Accounts Executive, System Admin          | Notify Vendor, Create Expected Delivery Schedule        |
| Partial Received | Some products or quantities received from vendor but order is not fully completed.        | System (Automatically updated when GRN quantity is less than ordered quantity)       | No           | No           | No           | Purchase Executive, Purchase Manager, Store Manager, Inventory Manager, Accounts Executive, System Admin                            | Update Inventory Stock, Update Pending Quantity         |
| Fully Received   | Full products and quantities received from vendor                                         | System (Automatically updated when GRN quantity is received as per ordered quantity) | No           | No           | No           | Purchase Executive, Purchase Manager, Store Manager, Inventory Manager, Accounts Executive, System Admin                            | Complete Inventory Stock Update, Close Pending Quantity |
| Completed        | All products successfully received and verified against Purchase Order.                   | User                                                                                 | No           | No           | No           | Purchase Executive, Purchase Manager, Procurement Head, Store Manager, Inventory Manager, Accounts Executive, Auditor, System Admin | Close Purchase Order Lifecycle                          |
| Cancelled        | Purchase Order cancelled before completion due to operational or business reasons.        | User                                                                                 | No           | No           | No           | Purchase Executive, Purchase Manager, Procurement Head, Auditor, System Admin                                                       | Release Reserved Budget, Cancel Pending Deliveries      |
| Rejected         | Purchase Order rejected during approval process because of validation or business issues. | User                                                                                 | No           | No           | No           | Purchase Executive, Purchase Manager, Procurement Head, System Admin                                                                | Create Rejection Audit Log                              |

## Workflow - Role Matrix

| Role               | Current Stage    | Scope | Create | Modify | Delete | Cancel | Next Stage                 | Rollback Stage |
|:------------------ |:---------------- |:----- |:------ |:------ |:------ |:------ |:-------------------------- |:-------------- |
| Purchase Executive | Draft            | Self  | Yes    | Yes    | Yes    | No     | Submit                     | No             |
| Purchase Manager   | Submitted        | Team  | No     | Yes    | No     | Yes    | Approve                    | Yes            |
| Purchase Manager   | Approved         | Team  | No     | No     | No     | Yes    | Order                      | Yes            |
| Procurement Head   | Approved         | All   | No     | No     | No     | Yes    | Order                      | Yes            |
| Store Manager      | Ordered          | Team  | No     | No     | No     | No     | Create GRN                 | No             |
| Store Manager      | Partial Received | Team  | No     | No     | No     | No     | Create GRN/ Create Invoice | No             |
| Accounts Executive | Completed        | Team  | No     | No     | No     | No     | Create Invoice             | No             |
| Auditor            | Any              | All   | No     | No     | No     | No     | No                         | No             |
| Admin              | Draft            | All   | Yes    | Yes    | Yes    | Yes    | Submit                     | No             |
| Admin              | Submitted        | All   | No     | Yes    | No     | Yes    | Approve                    | Yes            |
| Admin              | Approved         | All   | No     | No     | No     | Yes    | Order                      | Yes            |
| Admin              | Ordered          | All   | No     | No     | No     | No     | Create GRN                 | Yes            |
| Admin              | Partial Received | All   | No     | No     | No     | No     | Create GRN                 | No             |
| Admin              | Completed        | All   | No     | No     | No     | No     | Close                      | Yes            |
| Admin              | Closed           | All   | No     | No     | No     | No     | Reopen                     | No             |
| Admin              | Cancelled        | All   | No     | No     | No     | No     | Reopen                     | No             |
| Admin              | Rejected         | All   | No     | No     | No     | No     | Rollback                   | No             |

## Header Fields

| Field        | Description                               | Type        | Required |
|:------------ |:----------------------------------------- |:----------- |:-------- |
| PO Number    | Unique purchase order number              | Auto Number | Yes      |
| PO Date      | Date of order                             | Date        | Yes      |
| Vendor       | Vendor selection                          | Lookup      | Yes      |
| Currency     | Currency selection                        | Lookup      | No       |
| Freight Type | Type Selection(FOR/Paid/Included in rate) | Lookup      | No       |

## Line Item / Details Section

| Field      | Description                                        | Type        | Required |
|:---------- |:-------------------------------------------------- |:----------- |:-------- |
| Product    | Product name                                       | Lookup      | Yes      |
| Variety    | Product variety/grade/specification                | Lookup/Text | No       |
| Pack       | Product packaging type or packing quantity         | Lookup/Text | No       |
| Quantity   | Ordered quantity                                   | Decimal     | Yes      |
| Tolerance  | Allowed Tolerance in quantity payment will be full | Decimal     | Yes      |
| UOM        | Unit of measurement                                | Lookup      | Yes      |
| Rate       | Purchase rate                                      | Currency    | Yes      |
| Discount   | Discount amount, rate or percent                   | Decimal     | No       |
| Tax        | GST or tax                                         | Decimal     | Yes      |
| Line Total | Total item amount                                  | Currency    | Yes      |

## Delivery Schedule

| Field                  | Description                                | Type        |
|:---------------------- |:------------------------------------------ |:----------- |
| Product                | Product                                    | Lookup      |
| Variety                | Product variety/grade/specification        | Lookup/Text |
| Pack                   | Product packaging type or packing quantity | Lookup/Text |
| Quantity               | Quantity                                   | Decimal     |
| UoM                    | Unit of Product                            | string      |
| Delivery Location      | Warehouse/Vendor/Customer/Employee         | Lookup      |
| Expected Delivery Date | Warehouse-wise delivery date               | Date        |
| Delivery Priority      | High / Medium / Low                        | Dropdown    |
| Delivery Remarks       | Notes for warehouse delivery               | Text        |

## Payment Terms

| Field                          | Description                                | Type         |
|:------------------------------ |:------------------------------------------ |:------------ |
| Payment Type                   | Prepaid / Postpaid / EMI / Partial Payment | Dropdown     |
| Payment Methods                | Cash / UPI / Bank Transfer / Cheque / Card | Multi Select |
| Credit Days                    | Allowed credit period                      | Number       |
| Grace Period Days              | Allowed extra days before penalty          | Number       |
| Due Date                       | Final payment due date                     | Date         |
| Advance Amount                 | Initial advance payment                    | Currency     |
| Remaining Amount               | Pending payable amount                     | Currency     |
| EMI Enabled                    | Enable installment payment                 | Boolean      |
| EMI Months                     | Number of installments                     | Number       |
| EMI Interest %                 | EMI interest percentage                    | Decimal      |
| Early Payment Discount Enabled | Enable early payment discount              | Boolean      |
| Early Payment Discount Rules   | Multiple early payment discount slabs      | Array<Map>   |
| Late Payment Penalty Enabled   | Enable late payment penalty                | Boolean      |
| Late Payment Penalty Rules     | Multiple late payment penalty slabs        | Array<Map>   |
| Payment Terms Notes            | Additional payment conditions              | Text         |

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

### Example Business Rules

- Pay within 15 days → 5% discount  
- Pay after due date → 5% monthly penalty  
- EMI interest applied automatically  
- Partial payment allowed for trusted vendors  
- Penalty starts after grace period

## Summary Section

| Field                  | Description                   |
|:---------------------- |:----------------------------- |
| Subtotal               | Total before tax and discount |
| Total Item Discount    | Combined discount amount      |
| Total Tax              | Total GST/tax amount          |
| Invoice level discount | discount applied at PO Level  |
| Net Amount             | Final payable amount          |
| Round Off              | Rounded final amount          |

## Additional Information

| Field              | Description                |
|:------------------ |:-------------------------- |
| Remarks            | Additional notes           |
| Terms & Conditions | Purchase terms             |
| Approval Notes     | Internal approval comments |


## Media & Attachments

| Field                | Description                 | Type         |
|:-------------------- |:--------------------------- |:------------ |
| Attachments          | Upload supporting documents | File Upload  |
| Images               | Product or document images  | Image Upload |
| Vendor Quotation PDF | Upload quotation PDF        | File Upload  |
| Invoice Attachment   | Attach vendor invoice copy  | File Upload  |
| Approval Documents   | Internal approval files     | File Upload  |
| File Visibility      | Internal / Vendor Visible   | Dropdown     |
| Notes                | Media related remarks       | Text         |

## Reports

| Report                  | Purpose                               |
|:----------------------- |:------------------------------------- |
| PO Report               | Shows all purchase transactions.      |
| Vendor Ledger           | Vendor-wise balance and transactions. |
| Pending Purchase Orders | Track pending orders.                 |
| GRN Report              | Shows received goods details.         |
| Purchase Return Report  | Track returned products.              |
| Vendor Payment Report   | Track pending and completed payments. |
| Recieve Payment         |                                       |
| GST Purchase Report     | GST and tax reporting.                |

---

# Firestore Database Structure

## Collection: purchase_orders

### Document Structure

| Field Name    | Data Type | Description               |
|:------------- |:--------- |:------------------------- |
| poNumber      | string    | Unique PO number          |
| poDate        | timestamp | Purchase order date       |
| vendorId      | string    | Vendor document ID        |
| vendorName    | string    | Vendor name snapshot      |
| currency      | string    | Currency code             |
| paymentMode   | string    | Prepaid / Postpaid / EMI  |
| creditDays    | number    | Allowed credit days       |
| advanceAmount | number    | Advance payment           |
| emiMonths     | number    | EMI duration              |
| emiInterest   | number    | EMI interest percentage   |
| subtotal      | number    | Total before tax          |
| totalDiscount | number    | Total discount            |
| totalTax      | number    | Total GST/tax             |
| netAmount     | number    | Final payable amount      |
| roundOff      | number    | Rounded amount            |
| remarks       | string    | Additional remarks        |
| approvalNotes | string    | Internal approval notes   |
| stage         | string    | Current workflow stage    |
| approvalStage | string    | Approval workflow stage   |
| createdBy     | string    | User ID                   |
| createdByName | string    | User name snapshot        |
| updatedBy     | string    | Last updated user         |
| createdAt     | timestamp | Record creation timestamp |
| updatedAt     | timestamp | Record update timestamp   |
| branchId      | string    | Branch ID                 |
| companyId     | string    | Company ID                |
| isDeleted     | boolean   | Soft delete flag          |

