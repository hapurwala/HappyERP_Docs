# Happy ERP Purchase Order (PO) UI/UX Documentation

# Page Types

## 1. PO List Page

Purpose:

* View all purchase orders
* Search/filter orders
* Perform quick actions

---

## 2. Create/Edit PO Page

Purpose:

* Create purchase order
* Edit draft/submitted PO
* Manage line items
* Configure delivery/payment terms

---

# Navigation Flow

```text
PO List
↓
Create PO
↓
Submit
↓
Approval
↓
Ordered
↓
GRN
↓
Invoice
↓
Vendor Payment
```

---

# PO List Page UI

## Header Section

Components:

* Page title
* Breadcrumb
* Create PO button
* Import button
* Export dropdown

---

Summary Cards

Cards:

- Total POs
- Pending Approval
- Ordered
- Partial Received
- Completed
- Cancelled

---

## PO List Data Table

| Name            | Content                       | Type     | On Click                 | Tooltip / On Hover                                      |
|:--------------- |:----------------------------- |:-------- |:------------------------ |:------------------------------------------------------- |
| PO Number       | Unique Purchase Order Number  | Text     | Open PO View / Edit Page | View Purchase Order Details                             |
| PO Date         | Purchase Order Creation Date  | Date     | –                        | Purchase Order Date                                     |
| Vendor          | Vendor Name                   | Text     | Open Vendor Profile      | View Vendor Details                                     |
| Total Amount    | Total PO Amount Including Tax | Currency | –                        | Total Purchase Order Value Breackup                     |
| Stage           | Current Workflow Stage        | Status   | Open Workflow Timeline   | Current PO Status with updatedBy, updatedAt and remarks |
| Delivery Status | Delivery Progress Status      | Status   | Open Delivery Details    | View Delivery Progress                                  |
| Payment Status  | Payment Progress Status       | Status   | Open Payment Summary     | View Payment Progress                                   |
| Created By      | User Who Created PO           | User     | Open User Profile        | Created By User                                         |
| Updated At      | Last Updated Date & Time      | DateTime | –                        | Last Modification Date                                  |

---

## PO List Table Features

| Feature        | Settings           |
|:-------------- |:------------------ |
| Column Resize  | Yes                |
| Column Pinning | Tes                |
| Sorting        | Yes                |
| Pagination     | Yes; Page Size: 20 |
| Row Selection  | No                 |
| Bulk Actions   | No                 |
| Export         | Yes                |
| Sticky Header  | Yes                |

---

#### Action Menu

| Name               | Action                                                 | Visibility Criteria                                                 | Tooltip |
| ------------------ | ------------------------------------------------------ | ------------------------------------------------------------------- | ------- |
| View               | Open View/Modify page                                  | - Vew Permission-<br/>- No Modify Permission (Due to Role or Stage) | -       |
| Modify             | Open View/Modify page                                  | - Modify permission- Modify allowed on status                       | –       |
| Modify Date/Number | Open page where user can change the date/number        | - Modify permission- Modify allowed on status                       | –       |
| Status_1           | Set status to Status_1                                 | Allowed status based on current status                              | –       |
| Status_2           | Set status to Status_2                                 | –                                                                   |         |
| Undo Status        | Move back to last status                               | - Permission to set current status                                  | –       |
| Print              | Downloads PDF of the order in given format             | - Undo is allowed at this status                                    | –       |
| Cancel             | - Show confirmation message- On Yes, mark as cancelled | - Cancel permission- Cancel allowed on status                       | <br>    |
| Delete             | - Show confirmation message- On Yes, delete brand      | - Delete permission- Delete allowed on status                       | –       |

---

## PO List Filters

| Name           | Content                         | Type              | On Change      | Tooltip                                   |
|:-------------- |:------------------------------- |:----------------- |:-------------- |:----------------------------------------- |
| PO Number      | Search by Purchase Order Number | Text Search       | Filter Records | Find specific Purchase Order              |
| Vendor         | Filter by Vendor                | Lookup            | Filter Records | Show POs of selected Vendor               |
| Date Range     | Filter by PO Date Range         | Date Range Picker | Filter Records | Show POs within selected dates            |
| Stage          | Filter by Workflow Stage        | Dropdown          | Filter Records | Draft, Submitted, Approved, Ordered, etc. |
| Warehouse      | Filter by Warehouse             | Lookup            | Filter Records | Show POs linked to selected Warehouse     |
| Branch         | Filter by Branch                | Lookup            | Filter Records | Show POs of selected Branch               |
| Payment Status | Filter by Payment Status        | Dropdown          | Filter Records | Pending, Partial, Paid, Overdue           |
| Created By     | Filter by Creator               | User Lookup       | Filter Records | Show POs created by selected User         |

# Create/Edit PO Page

# Page Layout

```text
Left:
- Main form

Right:
- Summary panel
- Workflow timeline
- Activity log
```

---

# Sticky Header Actions

Buttons:

* Save Draft
* Submit
* Approve
* Order
* Cancel
* Rollback
* Print
* Share PDF

---

## General Information

| Name         | Content                      | Type        | On Change            | Tooltip                    |
|:------------ |:---------------------------- |:----------- |:-------------------- |:-------------------------- |
| PO Number    | Unique Purchase Order Number | Auto Number | Auto Generated       | System Generated PO Number |
| Vendor       | Vendor Selection             | Lookup      | Load Vendor Details  | Select Vendor              |
| PO Date      | Purchase Order Date          | Date        | Update Document Date | PO Creation Date           |
| Currency     | Transaction Currency         | Lookup      | Recalculate Amounts  | Currency Used In PO        |
| Freight Type | Freight Responsibility       | Dropdown    | Update Freight Rules | Paid By Vendor / Company   |

---

## Line Items Features

| Name                    | Content                | Type            | On Click              | Tooltip                   |
|:----------------------- |:---------------------- |:--------------- |:--------------------- |:------------------------- |
| Add Row                 | Add New Product Row    | Action Button   | Insert New Row        | Add Product Line          |
| Bulk Add                | Add Multiple Products  | Action Button   | Open Bulk Add Dialog  | Add Multiple Products     |
| Product Search          | Search Product Master  | Lookup          | Open Product Selector | Find Product              |
| SKU Scan                | Scan Product SKU       | Scanner/Input   | Search Product        | Scan Barcode / SKU        |
| Inline Editing          | Edit Row Without Popup | Grid Feature    | Edit Cell             | Quick Modification        |
| Excel Paste             | Paste Data From Excel  | Grid Feature    | Paste Rows            | Bulk Product Entry        |
| Dynamic Tax Calculation | Auto Tax Computation   | System Function | Recalculate Tax       | Auto Calculate Tax Amount |

---

## Delivery Schedule Features

| Name                       | Content                          | Type          | On Click           | Tooltip                     |
|:-------------------------- |:-------------------------------- |:------------- |:------------------ |:--------------------------- |
| Multi Warehouse Schedule   | Split Delivery Across Warehouses | Grid Feature  | Open Schedule Grid | Manage Warehouse Deliveries |
| Delivery Priority          | High / Medium / Low Priority     | Dropdown      | Set Priority       | Delivery Importance         |
| Expected Delivery Tracking | Track Planned Delivery Dates     | Date Tracking | Update Schedule    | Delivery Monitoring         |

---

## Payment Terms Features

| Name                      | Content                       | Type            | On Change          | Tooltip                 |
|:------------------------- |:----------------------------- |:--------------- |:------------------ |:----------------------- |
| EMI Rules                 | Installment Payment Rules     | Configuration   | Calculate EMI      | Payment In Installments |
| Discount Rules            | Early Payment Discount Rules  | Configuration   | Calculate Discount | Early Payment Benefits  |
| Penalty Rules             | Late Payment Penalty Rules    | Configuration   | Calculate Penalty  | Late Payment Charges    |
| Auto Due Date Calculation | Automatic Due Date Generation | System Function | Calculate Due Date | Due Date Based On Terms |

---

## Attachments Features

| Name               | Content                        | Type             | On Click          | Tooltip                   |
|:------------------ |:------------------------------ |:---------------- |:----------------- |:------------------------- |
| Drag & Drop Upload | Upload Files Using Drag & Drop | Upload Component | Upload Files      | Quick File Upload         |
| Preview            | Preview Uploaded Documents     | Viewer           | Open Preview      | View File Before Download |
| Download           | Download Uploaded Files        | Action Button    | Download File     | Save File Locally         |
| Image Viewer       | View Uploaded Images           | Viewer           | Open Image Viewer | Zoom & Preview Images     |

---

## Workflow Timeline Features

| Name           | Content                         | Type          | On Click           | Tooltip                |
|:-------------- |:------------------------------- |:------------- |:------------------ |:---------------------- |
| Stage History  | Complete Workflow Stage History | Timeline      | View History       | All Workflow Movements |
| Approval Logs  | Approval/Rejection Records      | Log Viewer    | Open Approval Logs | Approval Tracking      |
| User Activity  | User Action History             | Activity Feed | View Activities    | User Audit Trail       |
| Audit Tracking | System Audit Information        | Audit Log     | Open Audit Details | Compliance Tracking    |

---

# Line Item Table UI

Columns:

* Product
* Variety
* Pack
* Quantity
* UOM
* Rate
* Discount
* Tax
* Line Total

---

# Line Item Features

Features:

* Keyboard navigation
* Auto calculation
* Duplicate row
* Product quick preview
* Batch add
* Barcode scan
* Dynamic row validation

---

# Workflow Actions

## Draft Stage

Actions:

* Save
* Modify
* Delete
* Submit

---

## Submitted Stage

Actions:

* Approve
* Reject
* Rollback
* Add Remarks

---

## Approved Stage

Actions:

* Order
* Cancel
* Print
* Share

---

## Ordered Stage

Actions:

* Create GRN
* View Delivery Status

---

## Partial Received Stage

Actions:

* Create GRN
* Create Invoice
* View Pending Quantity

---

## Completed Stage

Actions:

* View
* Export
* Print

---

# Workflow Timeline UI

Timeline should show:

* Stage name
* User
* Date/time
* Remarks
* System action
* Approval notes

---

# Right Sidebar Widgets

Widgets:

* Vendor Balance
* Pending Deliveries
* Linked GRNs
* Linked Invoices
* Payment Summary
* Vendor Rating

---

# Notification UI

Notifications:

* Approval pending
* Delivery delayed
* Payment overdue
* GRN created
* Invoice created

---

# Validation UX

## Real-Time Validation

Validate:

* Duplicate product
* Quantity mismatch
* Tax mismatch
* Required fields
* Budget exceeded

---

# Error Handling

Show:

* Inline errors
* Toast notifications
* Modal confirmations

---

# Responsive Design

## Desktop

* Multi-column layout
* Side summary panel

---

## Tablet

* Collapsible side panels

---

## Mobile

* Tab accordion layout
* Floating action buttons

---

# Modal/Dialog Screens

Required Modals:

* Approve PO
* Reject PO
* Rollback PO
* Cancel PO
* Share PO
* Print Preview

---

# Reports Integration

Quick Links:

* PO Report
* Vendor Ledger
* Pending PO
* GRN Report

---

# Performance Guidelines

* Lazy load tables
* Virtual scrolling
* Pagination
* Cached lookups
* Debounced search

---

# Recommended UI Components

## Components

* DataGrid
* Drawer
* Tabs
* Stepper
* Timeline
* Summary Cards
* Lookup Dialog
* Attachment Viewer
* Activity Feed

---

# Recommended Tech Stack

Frontend:

* React
* MUI
* TanStack Table
* React Hook Form
* Zustand
* Firebase SDK

---

# Recommended Folder Structure

```text
/modules/purchase-order
    /pages
    /components
    /tables
    /forms
    /dialogs
    /workflow
    /services
    /hooks
```

---

# Final Recommendation

PO UI should behave like:

* workflow dashboard
* procurement workspace
* approval engine
* inventory coordination screen
* financial preparation module

not just a normal CRUD form.
