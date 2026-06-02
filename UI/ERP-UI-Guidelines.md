# UI Design Guidelines

## Design Type

- Modern ERP dashboard UI
- Minimal enterprise design
- Responsive layout
- Multi-tab workflow screen
- Drawer + Modal based actions
- Sticky action header
- Grid/table optimized layout

---

# Layout Structure

## Main Layout

```text
------------------------------------------------
Top Navbar
------------------------------------------------
Sidebar Menu
------------------------------------------------
Page Header
------------------------------------------------
Action Toolbar
------------------------------------------------
Summary Cards
------------------------------------------------
Tabs Section
------------------------------------------------
Footer Actions
------------------------------------------------
```

---

## Table Features

| Name            | Content                       | Type     | On Click                 | Tooltip / On Hover                                      |
| --------------- | ----------------------------- | -------- | ------------------------ | ------------------------------------------------------- |
| PO Number       | Unique Purchase Order Number  | Text     | Open PO View / Edit Page | View Purchase Order Details                             |
| PO Date         | Purchase Order Creation Date  | Date     | –                        | Purchase Order Date                                     |
| Vendor          | Vendor Name                   | Text     | Open Vendor Profile      | View Vendor Details                                     |
| Total Amount    | Total PO Amount Including Tax | Currency | –                        | Total Purchase Order Value Breackup                     |
| Stage           | Current Workflow Stage        | Status   | Open Workflow Timeline   | Current PO Status with updatedBy, updatedAt and remarks |
| Delivery Status | Delivery Progress Status      | Status   | Open Delivery Details    | View Delivery Progress                                  |
| 100             |                               |          |                          |                                                         |
| Payment Status  | Payment Progress Status       | Status   | Open Payment Summary     | View Payment Progress                                   |
| Created By      | User Who Created PO           | User     | Open User Profile        | Created By User                                         |
| Updated At      | Last Updated Date & Time      | DateTime | –                        |                                                         |
