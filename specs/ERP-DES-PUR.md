# Application Design Details

This document gives basic design details of following application.

- **Software**: **HappyERP**
- **Application**: Purchase

## Purchase Modules

Purchase application consists of following modules.

| Module                      | Purpose                                                                                                                 | Main Features                                                                                                      | Maintain Workflow | Important Notes                                                                                    |
|:--------------------------- |:----------------------------------------------------------------------------------------------------------------------- |:------------------------------------------------------------------------------------------------------------------ |:----------------- |:-------------------------------------------------------------------------------------------------- |
| Purchase Requisition        | Used by employees or departments to request products or raw materials.                                                  | Create requisition, approval flow, item selection, department wise request.                                        | Yes               | Starting point of purchase lifecycle.                                                              |
| Request for Quotation (RFQ) | Send inquiry to multiple vendors for best price comparison.                                                             | Vendor selection, quotation comparison, email sharing, expiry date.                                                | Yes               | Used for vendor comparison and negotiation.                                                        |
| Vendor Quotation            | Store quotations received from vendors.                                                                                 | Vendor rates, taxes, delivery time, comparison table.                                                              | Yes               | Approved quotation converts to PO.                                                                 |
| Purchase Order (PO)         | Official order sent to vendor for purchasing items.                                                                     | PO number, approval, item table, tax, payment terms.                                                               | Yes               | Core module of purchase operations.                                                                |
| Goods Receipt Note (GRN)    | Used when purchased items are received in warehouse/store.                                                              | Receive quantity, damaged quantity, batch tracking, stock update.                                                  | Yes               | Updates inventory stock automatically.                                                             |
| Purchase Invoice            | Vendor bill entry for accounting and payment process.                                                                   | Invoice number, GST, amount, due date, attachments.                                                                | Yes               | Integrated with PO and GRN.                                                                        |
| Vendor Payment              | Track and manage vendor payments.                                                                                       | Payment entry, due tracking, payment mode, balance.                                                                | Yes               | Integrated with accounting module.                                                                 |
| Purchase Return             | Return damaged or extra products back to vendor.                                                                        | Return quantity, reason, debit note integration.                                                                   | Yes               | Reduces stock automatically.                                                                       |
| Dispatch                    | Used when purchased items are returned from warehouse/store.                                                            | Quantity, batch tracking, stock update.                                                                            | Yes               | Updates inventory stock automatically.                                                             |
| Debit Note                  | Raised against vendor for damaged/incorrect goods.                                                                      | Vendor adjustment, amount deduction, return linkage.                                                               | Yes               | Used for accounting adjustments.                                                                   |
| Receive Payment             | Track money received back from vendor against debit notes, refunds, adjustments, or overpayments.                       | Refund tracking, vendor settlement, adjustment entries, payment mode, balance reconciliation.                      | Yes               | Integrated with Debit Note, Vendor Ledger, and Accounting module.                                  |
| Payment Adjustment          | Manage vendor payment settlements, excess payments, short payments, debit note adjustments, and ledger reconciliations. | Vendor ledger adjustment, debit/credit settlement, balance reconciliation, payment allocation, manual adjustments. | Yes               | Integrated with Vendor Payment, Receive Payment, Debit Note, Vendor Ledger, and Accounting module. |
| Purchase Reports            | View analytics and reports related to purchases.                                                                        | Vendor ledger, PO report, GRN report, pending invoices.                                                            | -                 | Important for management decisions.                                                                |

## Masters

Following masters will be used in modules of this application.

| Master           | Purpose                                        | Required Fields                                          | Notes                                              |
|:---------------- |:---------------------------------------------- |:-------------------------------------------------------- |:-------------------------------------------------- |
| Vendor Master    | Store all vendor information.                  | Vendor Name, GSTIN, Address, Phone, Email, Payment Terms | Should support multiple contacts and bank details. |
| Product Master   | Manage purchasable products and raw materials. | Name, SKU, UOM, Tax, Price, Category                     | Can connect with inventory and sales.              |
| Warehouse Master | Manage warehouse locations.                    | Warehouse Name, Branch, Type                             | Used in GRN and stock transfer.                    |
| Tax Master       | Manage GST and tax structure.                  | GST %, HSN Code, Tax Type                                | Required for invoice and compliance.               |
| UOM Master       | Manage units of measurement.                   | UOM Name, Symbol                                         | Examples: Kg, PCS, Box.                            |
| Payment Terms    | Define vendor payment rules.                   | Credit Days, Advance %, Due Rules                        | Used in PO and invoice.                            |

## Roles

Following are the direct roles of this application.

| Role               | Purpose                                            |
|:------------------ |:-------------------------------------------------- |
| Purchase Executive | Creates and manages operational purchase documents |
| Purchase Manager   | Reviews and approves purchase workflows            |
| Purchase Head      | Handles final approval and procurement strategy    |

## Additional Roles

Following roles of other applications also use this application.

| Role               | Purpose                                 |
|:------------------ |:--------------------------------------- |
| Accounts Executive | Handles invoices and vendor payments    |
| Store Manager      | Handles GRN and warehouse receiving     |
| Inventory Manager  | Verifies stock and warehouse operations |
| Auditor            | Read-only audit and compliance access   |
| System Admin       | Full system configuration and control   |
