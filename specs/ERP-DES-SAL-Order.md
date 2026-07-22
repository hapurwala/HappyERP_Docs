# Module Design Details

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: Sales
- **Module**: Sales Order

## Roles

| Role            | Purpose                                             |
|:--------------- |:--------------------------------------------------- |
| Sales Executive | Creates and manages operational sales order records |
| Sales Manager   | Reviews and approves sales order workflows          |
| Sales Head      | Handles final approval and sales strategy           |

## Additional Roles

| Role              | Purpose                                        |
|:----------------- |:---------------------------------------------- |
| Dispatch Manager    | Handles order dispatch and delivery operations |
| Account Executive | Handles customer invoicing and payment receipt |
| Auditor           | Read-only audit and compliance access          |
| System Admin      | Full system configuration and control          |

## Workflow Stages

| Stage              | Description                                                                            | Who Will Set It                                                                     | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles                                                                  | System Action                                  |
| :----------------- | :------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------- | :----------- | :----------- | :----------- | :----------------------------------------------------------------------------------------- | :--------------------------------------------- |
| Draft              | Initial stage where Sales Order is created and fully editable by Sales Executive.      | User                                                                                | Yes          | Yes          | Yes          | Sales Executive, Sales Manager, System Admin                                               | —                                              |
| Submitted          | Sales Order submitted for approval and waiting for manager review.                     | User                                                                                | No           | No           | Yes          | Sales Executive, Sales Manager,  Sales Head, System Admin                                  | Notify Sales Manager                           |
| Approved           | Sales Order approved and ready for dispatch.                                           | User                                                                                | No           | No           | Yes          | Sales Executive, Sales Manager, Dispatch Manager, Account Executive, System Admin          | —                                              |
| Partial Dispatched | Some products or quantities dispatched but order is not fully fulfilled.               | System (Automatically updated when dispatch quantity is less than ordered quantity) | No           | No           | No           | Sales Executive, Sales Manager, Dispatch Manager, Account Executive, System Admin          | Update Dispatch Quantities, Update Pending Qty |
| Fully Dispatched   | All products and quantities dispatched from warehouse.                                 | System (Automatically updated when dispatch quantity matches ordered quantity)      | No           | No           | No           | Sales Executive, Sales Manager, Dispatch Manager, Account Executive, System Admin          | Complete Dispatch Update, Close Pending Qty    |
| Completed          | All dispatched goods confirmed and order lifecycle closed.                             | User                                                                                | No           | No           | No           | Sales Executive, Sales Manager, Dispatch Manager, Account Executive, Auditor, System Admin | Close Sales Order Lifecycle                    |
| Cancelled          | Sales Order cancelled before completion due to operational or business reasons.        | User                                                                                | No           | No           | No           | Sales Executive, Sales Manager, Auditor, System Admin                                      | Cancel Pending Dispatches                      |
| Rejected           | Sales Order rejected by Sales Manager at Submitted stage. Sales Executive can correct and resubmit. | User                                                                                | No           | No           | No           | Sales Executive, Sales Manager, System Admin                                               | Notify Sales Executive, Create Rejection Audit Log |

## Workflow - Role Matrix

| Role              | Current Stage      | Scope | Create | Modify | Delete | Cancel | Next Stage          | Rollback Stage |
| :---------------- | :----------------- | :---- | :----- | :----- | :----- | :----- | :------------------ | :------------- |
| Sales Executive   | Draft              | Self  | Yes    | Yes    | Yes    | No     | Submit              | No             |
| Sales Executive   | Rejected           | Self  | No     | Yes    | No     | No     | Submit              | No             |
| Sales Manager     | Submitted          | Team  | No     | No     | No     | Yes    | Approved / Rejected | Yes            |
| Sales Head        | Approved           | All   | No     | No     | No     | Yes    | No                  | Yes            |
| Dispatch Manager  | Approved           | Team  | No     | No     | No     | No     | Create Dispatch     | No             |
| Dispatch Manager  | Partial Dispatched | Team  | No     | No     | No     | No     | Create Dispatch     | No             |
| Account Executive | Completed          | Team  | No     | No     | No     | No     | Create Invoice      | No             |
| Auditor           | Any                | All   | No     | No     | No     | No     | No                  | No             |
| Admin             | Draft              | All   | Yes    | Yes    | Yes    | Yes    | Submit              | No             |
| Admin             | Submitted          | All   | No     | No     | No     | Yes    | Approve             | Yes            |
| Admin             | Approved           | All   | No     | No     | No     | Yes    | Create Dispatch     | Yes            |
| Admin             | Partial Dispatched | All   | No     | No     | No     | No     | Create Dispatch     | No             |
| Admin             | Fully Dispatched   | All   | No     | No     | No     | No     | Complete            | No             |
| Admin             | Completed          | All   | No     | No     | No     | No     | Close               | Yes            |
| Admin             | Cancelled          | All   | No     | No     | No     | No     | Reopen              | No             |
| Admin             | Rejected           | All   | No     | No     | No     | No     | Rollback            | No             |

## Header Fields

| Field              | Description                                                   | Type        | Required         |
| :----------------- | :------------------------------------------------------------ | :---------- | :--------------- |
| Order Number       | Unique sales order number — system generated after first save | Auto Number | System Generated |
| Order Date         | Date of the sales order                                       | Date        | Yes              |
| Customer           | Customer placing the order                                    | Lookup      | Yes              |
| Agent              | Agent associated with this order                              | Lookup      | No               |
| Placed By          | Sales executive handling the order                            | Lookup      | Yes              |
| Currency           | Currency selection.                                           | Lookup      | No               |
| Priority           | Order priority (High / Medium / Low)                          | Dropdown    | Yes              |
| Remarks            | Internal notes                                                | Text Area   | No               |
| Terms & Conditions | Order terms and conditions                                    | Text Area   | No               |



## Details Section

| Field           | Description                                                     | Type     | Required          |
| :-------------- | :-------------------------------------------------------------- | :------- | :---------------- |
| Barcode         | Product barcode — auto-fills Product, Variety, Pack on scan     | Text     | No                |
| Product         | Product being ordered                                           | Lookup   | Yes               |
| Variety         | Product variety or grade                                        | Lookup   | No                |
| Pack            | Product pack type                                               | Lookup   | Yes               |
| No. of Packs    | Number of outer packs ordered                                   | Integer  | No                |
| Quantity        | Ordered quantity                                                | Integer  | Yes               |
| UoM             | Unit of measurement (auto-filled from pack)                     | Text     | —                 |
| Rate            | Rate per unit                                                   | Currency | Yes               |
| Discount %      | Discount percentage applied on this line. Amount auto-calculated and shown in brackets: `X% (₹Y)` | Decimal  | No                |
| Tax             | Tax amount (auto-calculated from product tax config)            | Decimal  | System Calculated |
| Total           | Total for this line (Quantity × Rate − Discount Amount + Tax)   | Currency | System Calculated |
| Delivery Date   | Expected delivery date for this line                            | Date     | Yes               |
| Payment Terms   | Payment terms for this line (defaults from customer master)     | Lookup   | No                |
| Notes           | Line-level notes                                                | Text     | No                |
| Stage           | Current dispatch status of this line (system-managed)           | Text     | System Managed    |

## Summary Section

| Field              | Description                        |
|:------------------ |:---------------------------------- |
| Total Items        | Count of unique products           |
| Total Packs        | Sum of no. of packs across all lines |
| Total Quantity     | Sum of quantity across all lines   |
| Subtotal           | Total before discount and tax      |
| Total Discount     | Combined discount amount           |
| Total Tax          | Total tax amount                   |
| Net Amount         | Final payable amount               |
| Round Off          | Rounded final amount               |
| Min. Delivery Date | Minimum delivery date across lines |

## Media & Attachments

| Field       | Description                 | Type        |
|:----------- |:--------------------------- |:----------- |
| Attachments | Upload supporting documents | File Upload |

## Reports

| Report                      | Purpose                                                        |
| :-------------------------- | :------------------------------------------------------------- |
| Sales Order Summary Report  | Line-wise detail of all sales orders with amounts and stage    |
| Pending Sales Orders Report | Orders not yet fully dispatched — track unfulfilled quantities |
| Delivery Schedule Report    | Orders grouped by delivery date for dispatch planning          |
| Agent-wise Sales Report     | Sales performance and order value broken down by agent         |


---

# Firestore Database Structure

## Collection: `sales_orders`

### Document Structure

| Field Name      | Data Type  | Description                     |
|:--------------- |:---------- |:------------------------------- |
| orderNumber     | string     | Unique SO number                |
| orderDate       | timestamp  | Sales order date                |
| customerId      | string     | Customer document ID            |
| customerName    | string     | Customer name snapshot          |
| agentId         | string     | Agent document ID               |
| agentName       | string     | Agent name snapshot             |
| placedById      | string     | Sales executive user ID         |
| placedByName    | string     | Sales executive name snapshot   |
| priority        | string     | High / Medium / Low             |
| currencyId      | string     | Currency ID                     |
| totalItems      | number     | Count of unique products        |
| totalPacks      | number     | Total no. of packs ordered      |
| totalQuantity   | number     | Total quantity ordered          |
| subtotal        | number     | Total before discount and tax   |
| totalDiscount   | number     | Total discount amount           |
| totalTax        | number     | Total tax amount                |
| netAmount       | number     | Final order value               |
| roundOff        | number     | Round off amount                |
| minDeliveryDate | timestamp  | Minimum delivery date           |
| remarks         | string     | Internal remarks                |
| terms           | string     | Terms and conditions            |
| stage           | string     | Current workflow stage          |
| stageHistory    | array<map> | Complete workflow audit history |
| createdBy       | string     | User ID                         |
| createdByName   | string     | User name snapshot              |
| updatedBy       | string     | Last updated user ID            |
| updatedByName   | string     | Last updated user name          |
| createdAt       | timestamp  | Record creation timestamp       |
| updatedAt       | timestamp  | Record update timestamp         |
| branchId        | string     | Branch ID                       |
| companyId       | string     | Company ID                      |
| isDeleted       | boolean    | Soft delete flag                |

---

### Sub-collection: `order_lines`

| Field Name       | Data Type | Description                        |
| :--------------- | :-------- | :--------------------------------- |
| lineId           | string    | Unique line ID                     |
| barcode          | string    | Product barcode                    |
| productId        | string    | Product document ID                |
| productName      | string    | Product name snapshot              |
| varietyId        | string    | Variety document ID                |
| varietyName      | string    | Variety name snapshot              |
| packId           | string    | Pack document ID                   |
| packName         | string    | Pack name snapshot                 |
| noOfPacks        | number    | Number of outer packs              |
| quantity         | number    | Ordered quantity (integer)         |
| uomId            | string    | UoM document ID                    |
| uomName          | string    | UoM name snapshot                  |
| rate             | number    | Rate per unit                      |
| discountPercent  | number    | Discount percentage                |
| discountAmount   | number    | Discount amount (auto-calculated)  |
| taxAmount        | number    | Tax amount (auto-calculated)       |
| lineTotal        | number    | Line total value (auto-calculated) |
| deliveryDate     | timestamp | Expected delivery date             |
| paymentTermsId   | string    | Payment terms document ID          |
| paymentTermsName | string    | Payment terms name snapshot        |
| notes            | string    | Line-level notes                   |
| lineStage        | string    | Current line dispatch status       |
| createdAt        | timestamp | Line creation timestamp            |
| updatedAt        | timestamp | Line update timestamp              |
