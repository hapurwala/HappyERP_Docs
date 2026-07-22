# Test Cases Index — Purchase Order

- **Module**: Purchase Order
- **Reference**: [UI Doc](/UI/PUR-PO/ERP_UI_PUR_PO.md) | [Specs](/specs/ERP-DES-PUR-PO.md) | [DB](/database/ERP-DB-PUR-PO.md)

---

## Summary

| Section     | File                                   | Test Type          | TC Count |
| :---------- | :------------------------------------- | :----------------- | :------- |
| List Page   | [TC-ListPage.md](TC-ListPage.md)       | Manual             | 33       |
| Entry Page  | [TC-EntryPage.md](TC-EntryPage.md)     | Manual             | 66       |
| Logic       | [TC-Logic.md](TC-Logic.md)             | Automated          | 15       |
| Backend     | [TC-Backend.md](TC-Backend.md)         | Manual + Automated | 10       |
| Performance | [TC-Performance.md](TC-Performance.md) | Automated          | 5        |
| **Total**   |                                        |                    | **129**  |

---

## TC ID Range

| Range             | Section                                                                    | File              |
| :---------------- | :------------------------------------------------------------------------- | :---------------- |
| TC-PO-001 – 025   | List Page — Columns, KPI Cards, Toolbar, Filters, Row Actions              | TC-ListPage.md    |
| TC-PO-026 – 070   | Entry Page — Header, Product Details, Delivery Schedule, Payment Terms, Summary, Attachments | TC-EntryPage.md |
| TC-PO-071 – 085   | Logic — Calculations, Workflow, RBAC (Automated)                           | TC-Logic.md       |
| TC-PO-086 – 095   | Backend — DB, Audit, Integration (Manual + Automated)                      | TC-Backend.md     |
| TC-PO-096 – 100   | Performance — Load, Filter, Concurrency, Export (Automated)                | TC-Performance.md |
| TC-PO-101 – 121   | Entry Page additions — Product Details, Delivery, Payment Terms, Summary, Additional Info, Attachments, Workflow | TC-EntryPage.md |
| TC-PO-122 – 129   | List Page additions — Scope (roles) + Reports                              | TC-ListPage.md    |

---

## Category Distribution by File

Each file owns specific categories based on its execution environment and purpose. Positive / Negative / Edge Case are cross-cutting tags that appear in every file.

| Category                        | TC-ListPage | TC-EntryPage | TC-Logic | TC-Backend | TC-Performance |
| :------------------------------ | :---------: | :----------: | :------: | :--------: | :------------: |
| UI Validation                   |      ✓      |      ✓       |          |            |                |
| Field Validation                |             |      ✓       |          |            |                |
| Business Rules                  |      ✓      |      ✓       |    ✓     |            |                |
| Workflow Validation             |      ✓      |      ✓       |    ✓     |            |                |
| Calculation Validation          |             |      ✓       |    ✓     |            |                |
| Security & Permission           |      ✓      |              |    ✓     |            |                |
| Integration Validation          |             |      ✓       |          |     ✓      |                |
| Database Validation             |             |              |          |     ✓      |                |
| Audit Validation                |             |      ✓       |          |     ✓      |                |
| Performance Validation          |             |              |          |            |       ✓        |
| Positive / Negative / Edge Case |      ✓      |      ✓       |    ✓     |     ✓      |       ✓        |

---

## Category Legend

| Category               | Description                                                    |
| :--------------------- | :------------------------------------------------------------- |
| UI Validation          | Checks screen behaviour — buttons, layout, display format      |
| Field Validation       | Checks required fields, input rules, format constraints        |
| Business Rules         | Checks system-enforced logic — auto-fill, defaults, locking    |
| Workflow Validation    | Checks stage transitions and approval flows                    |
| Calculation Validation | Checks computed values — discount, tax, line total, TDS        |
| Security & Permission  | Checks role-based access and data visibility                   |
| Integration Validation | Checks data fetched from another master (vendor, product, GRN) |
| Database Validation    | Checks data saved correctly to Firestore                       |
| Audit Validation       | Checks audit logs, stage history, created/updated by           |
| Performance Validation | Checks load time and response under concurrent usage           |
| Positive               | Input or action that should succeed                            |
| Negative               | Input or action that should be rejected by the system          |
| Edge Case              | Boundary or unusual but valid scenario                         |
