# Happy ERP Purchase Invoice Module Detailed

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

# Purchase Invoice Module Roles

| Role | Purpose |
| :---- | :---- |
| Accounts Executive | Creates and manages purchase invoices |
| Accounts Manager | Reviews and approves accounting workflows |
| Finance Head | Handles final financial approval and payable strategy |

---

# Purchase Invoice Module - Additional Roles

| Role | Purpose |
| :---- | :---- |
| Purchase Manager | Verifies PO and GRN linkage |
| Store Manager | Validates received quantity and inventory relevance |
| Auditor | Read-only audit and compliance access |
| System Admin | Full system configuration and control |

---

# Purchase Invoice

## Purchase Invoice - Workflow Stages

| Stage | Description | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles | System Action |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Draft | Initial invoice stage where invoice is created and editable. | User | Yes | Yes | Yes | Accounts Executive, Accounts Manager, Finance Head, System Admin | --- |
| Submitted | Invoice submitted for verification and approval process. | User | Yes | No | Yes | Accounts Executive, Accounts Manager, Finance Head, Purchase Manager, System Admin | Lock Invoice Number Sequence |
| Verified | Invoice verified successfully against PO and GRN. | User | No | No | Yes | Accounts Executive, Accounts Manager, Finance Head, Purchase Manager, Store Manager, System Admin | Validate PO, GRN and Tax Matching |
| Approved | Invoice approved for payment processing. | User | No | No | Yes | Accounts Executive, Accounts Manager, Finance Head, Purchase Manager, System Admin | Post Financial Entries in Ledger, Create Vendor Payable |
| Partially Paid | Partial payment completed against invoice. | System | No | No | No | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin | Update Vendor Outstanding Balance |
| Paid/Completed | Full invoice amount paid successfully. | System | No | No | No | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin | Close Vendor Payable Entry |
| Cancelled | Invoice cancelled due to business or accounting issues. | User | No | No | No | Accounts Executive, Accounts Manager, Finance Head, Auditor, System Admin | Reverse Financial Entries from Ledger |
| Rejected | Invoice rejected during approval or verification process. | User | No | No | No | Accounts Executive, Accounts Manager, Finance Head, Purchase Manager, System Admin | Reverse Financial Entries from Ledger |
---

## Purchase Invoice Workflow Matrix

| Role | Current Stage | Scope | Create | Modify | Delete | Cancel | Next Stage | Rollback Stage |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Accounts Executive | Draft | Self | Yes | Yes | Yes | No | Submit | No |
| Accounts Executive | Submitted | Self | No | Yes | No | Yes | Verify | Yes |
| Accounts Manager | Verified | Team | No | No | No | Yes | Approve | Yes |
| Finance Head | Approved | All | No | No | No | Yes | Ready For Payment | Yes |
| Cashier | Ready For Payment | Team | No | No | No | No | Make Payment | No |
| Cashier | Partially Paid | Team | No | No | No | No | Make Payment | No |
| Auditor | Any | All | No | No | No | No | No | No |

---

## Purchase Invoice Header Fields

| Field | Description | Type | Required |
| :---- | :---- | :---- | :---- |
| Invoice Number | Unique vendor invoice number | Text | Yes |
| Invoice Date | Vendor invoice date | Date | Yes |
| Vendor | Vendor reference | Lookup | Yes |
| GSTIN | Vendor GST number | Text | No
| PO Number | Linked purchase order | Lookup | Yes |
| GRN Number | Linked goods receipt note | Lookup | Yes |
| Invoice Type | Tax Invoice / Debit Invoice / Service Invoice | Dropdown | Yes |
| Currency | Currency selection | Lookup | No |
| Due Date | Invoice payment due date | Date | Yes |
| Payment Terms | Vendor payment terms | Lookup | No |

---

## Tax & GST Details

| Field | Description | Type |
| :---- | :---- | :---- |
| GST Type | CGST / SGST / IGST | Dropdown |
| HSN/SAC Code | GST product/service code | Text |
| Taxable Amount | Amount before GST | Currency |
| CGST Amount | Central GST amount | Currency |
| SGST Amount | State GST amount | Currency |
| IGST Amount | Integrated GST amount | Currency |
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

## Payment Information

| Field | Description | Type |
| :---- | :---- | :---- |
| Payment Status | Pending / Partial / Paid | Dropdown |
| Payment Mode | Cash / UPI / Bank Transfer / Cheque | Dropdown |
| TDS Amount | Deducted TDS amount | Currency |
| Early Payment Discount | As Per Payment Terms | Currency |
| Late Payment Panelty | As Per Payment Terms | Currency |
| Paid Amount | Total paid amount | Currency |
| Balance Amount | Remaining payable amount | Currency |
| Payment Date | Actual payment date | Date |

---

## System Generated Rules

| Trigger | System Action |
| :---- | :---- |
| Invoice approved | Create vendor payable entry |
| Payment completed | Update invoice stage to Paid |
| Partial payment completed | Update invoice stage to Partially Paid |
| Invoice amount > PO amount | Trigger approval alert |
| Invoice linked with GRN | Validate received quantity |

---

## Validation Rules

| Rule | Description |
| :---- | :---- |
| PO Required | Invoice cannot be created without linked PO |
| GRN Required | Invoice cannot be created without verified GRN |
| Quantity Validation | Invoice quantity cannot exceed accepted GRN quantity |
| GST Validation | GSTIN required for taxable invoices |
| Duplicate Invoice Validation | Duplicate vendor invoice number not allowed |

---

# Firestore Database Structure

## Collection: purchase_invoices

### Document Structure

| Field Name | Data Type | Description |
| :---- | :---- | :---- |
| invoiceNumber | string | Vendor invoice number |
| invoiceDate | timestamp | Invoice date |
| vendorId | string | Vendor reference |
| poId | string | Linked PO ID |
| grnId | string | Linked GRN ID |
| subtotal | number | Total before tax |
| totalTax | number | Total GST/tax |
| paidAmount | number | Paid amount |
| balanceAmount | number | Remaining amount |
| netAmount | number | Final invoice amount |
| stage | string | Current workflow stage |
| createdAt | timestamp | Record creation timestamp |
| updatedAt | timestamp | Record update timestamp |

---
