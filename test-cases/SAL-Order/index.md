# Test Cases Index — Sales Order

- **Module**: Sales Order
- **Reference**: [UI Doc](/UI/ERP-UI-SAL-Order.md) | [Specs](/specs/ERP-DES-SAL-Order.md)

---

## Summary

| Section     | File                                       | Test Type          | TC Count |
| :---------- | :----------------------------------------- | :----------------- | :------- |
| List Page   | [TC-ListPage.md](TC-ListPage.md)           | Manual             | 38       |
| Entry Page  | [TC-EntryPage.md](TC-EntryPage.md)         | Manual             | 55       |
| Logic       | [TC-Logic.md](TC-Logic.md)                 | Automated (Jest)   | 15       |
| Backend     | [TC-Backend.md](TC-Backend.md)             | Manual + Automated | 10       |
| Performance | [TC-Performance.md](TC-Performance.md)     | Automated (k6)     | 5        |
| **Total**   |                                            |                    | **123**  |

---

## TC ID Range

| Range           | Section                                                              | File              |
| :-------------- | :------------------------------------------------------------------- | :---------------- |
| TC-SO-001 – 030 | List Page — Columns, Toolbar, Filters, Row Actions, Scope            | TC-ListPage.md    |
| TC-SO-031 – 075 | Entry Page — Header, Order Details, Summary, Attachments, Workflow   | TC-EntryPage.md   |
| TC-SO-076 – 090 | Logic — Calculations, Workflow, RBAC (Automated)                     | TC-Logic.md       |
| TC-SO-091 – 100 | Backend — DB, Audit, Integration (Manual + Automated)                | TC-Backend.md     |
| TC-SO-101 – 105 | Performance — Load, Filter, Concurrency, Export (Automated)          | TC-Performance.md |
| TC-SO-106 – 115 | Entry Page additions — Header, Order Details, Summary, Workflow      | TC-EntryPage.md   |
| TC-SO-116 – 123 | List Page additions — Scope (roles) + Reports                        | TC-ListPage.md    |

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
| Calculation Validation | Checks computed values — discount, tax, line total             |
| Security & Permission  | Checks role-based access and data visibility                   |
| Integration Validation | Checks data fetched from another master (customer, dispatch)   |
| Database Validation    | Checks data saved correctly to Firestore                       |
| Audit Validation       | Checks audit logs, stage history, created/updated by           |
| Performance Validation | Checks load time and response under concurrent usage           |
| Positive               | Input or action that should succeed                            |
| Negative               | Input or action that should be rejected by the system          |
| Edge Case              | Boundary or unusual but valid scenario                         |
