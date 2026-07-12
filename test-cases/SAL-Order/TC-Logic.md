# TC — Sales Order — Logic (Automated)

- **Module**: Sales Order
- **Section**: Calculation, Workflow, RBAC — Unit Tests
- **Test Type**: Automated (Jest)
- **TC Range**: TC-SO-076 – TC-SO-090
- **Reference**: [Specs](/specs/ERP-DES-SAL-Order.md)

---

## Calculations

| TC ID     | Test Case                                                    | Category                          | Precondition   | Expected Result                                   | Priority |
| :-------- | :----------------------------------------------------------- | :-------------------------------- | :------------- | :------------------------------------------------ | :------- |
| TC-SO-076 | calculateLineTotal returns quantity × unit price             | Calculation Validation, Positive  | Jest unit test | `calculateLineTotal(10, 150)` → `1500`            | High     |
| TC-SO-077 | calculateLineTotal returns 0 when quantity is 0              | Calculation Validation, Edge Case | Jest unit test | `calculateLineTotal(0, 150)` → `0`                | Medium   |
| TC-SO-078 | calculateDiscount applies percentage correctly               | Calculation Validation, Positive  | Jest unit test | `calculateDiscount(1000, 10)` → `100`             | High     |
| TC-SO-079 | calculateNetAmount returns Total − Discount + Tax            | Calculation Validation, Positive  | Jest unit test | `calculateNetAmount(1000, 100, 90)` → `990`       | High     |
| TC-SO-080 | calculateNetAmount returns Total when discount and tax are 0 | Calculation Validation, Edge Case | Jest unit test | `calculateNetAmount(1000, 0, 0)` → `1000`         | Medium   |

## Workflow

| TC ID     | Test Case                                                 | Category                      | Precondition   | Expected Result                                                   | Priority |
| :-------- | :-------------------------------------------------------- | :---------------------------- | :------------- | :---------------------------------------------------------------- | :------- |
| TC-SO-081 | Sales Exec can transition Draft → Submitted               | Workflow Validation, Positive | Jest unit test | `canTransition('Draft','Submitted','Sales Executive')` → `true`   | High     |
| TC-SO-082 | Sales Exec cannot transition Draft → Approved             | Workflow Validation, Negative | Jest unit test | `canTransition('Draft','Approved','Sales Executive')` → `false`   | High     |
| TC-SO-083 | Sales Manager can transition Submitted → Approved         | Workflow Validation, Positive | Jest unit test | `canTransition('Submitted','Approved','Sales Manager')` → `true`  | High     |
| TC-SO-084 | Sales Manager cannot transition Draft → Submitted         | Workflow Validation, Negative | Jest unit test | `canTransition('Draft','Submitted','Sales Manager')` → `false`    | Medium   |
| TC-SO-085 | canModify returns true for Draft order by Sales Exec      | Business Rules, Positive      | Jest unit test | `canModify('Draft','Sales Executive')` → `true`                   | High     |
| TC-SO-086 | canModify returns false for Submitted order by Sales Exec | Business Rules, Negative      | Jest unit test | `canModify('Submitted','Sales Executive')` → `false`              | High     |

## RBAC

| TC ID     | Test Case                                               | Category                        | Precondition   | Expected Result                                   | Priority |
| :-------- | :------------------------------------------------------ | :------------------------------ | :------------- | :------------------------------------------------ | :------- |
| TC-SO-087 | getScope returns 'Self' for Sales Executive             | Security & Permission, Positive | Jest unit test | `getScope('Sales Executive')` → `'Self'`          | High     |
| TC-SO-088 | getScope returns 'Team' for Sales Manager               | Security & Permission, Positive | Jest unit test | `getScope('Sales Manager')` → `'Team'`            | High     |
| TC-SO-089 | getScope returns 'All' for Sales Head                   | Security & Permission, Positive | Jest unit test | `getScope('Sales Head')` → `'All'`                | High     |
| TC-SO-090 | filterOrdersByScope returns only own orders (Self scope) | Security & Permission, Positive | Jest unit test | Only orders where `createdBy === userId` returned | High     |

---

```js
import {
  calculateLineTotal,
  calculateDiscount,
  calculateNetAmount,
} from '@/modules/sales-order/utils/calculations'

import { canTransition, canModify } from '@/modules/sales-order/utils/workflow'
import { getScope, filterOrdersByScope } from '@/modules/sales-order/utils/rbac'

// ─────────────────────────────────────────────
// CALCULATIONS
// ─────────────────────────────────────────────
describe('calculateLineTotal', () => {
  it('TC-SO-076 | returns quantity × unit price', () => {
    // Arrange
    const quantity = 10
    const unitPrice = 150
    // Act
    const result = calculateLineTotal(quantity, unitPrice)
    // Assert
    expect(result).toBe(1500)
  })

  it('TC-SO-077 | returns 0 when quantity is 0', () => {
    // Arrange / Act / Assert
    expect(calculateLineTotal(0, 150)).toBe(0)
  })
})

describe('calculateDiscount', () => {
  it('TC-SO-078 | applies percentage discount correctly', () => {
    expect(calculateDiscount(1000, 10)).toBe(100)
  })
})

describe('calculateNetAmount', () => {
  it('TC-SO-079 | returns Total − Discount + Tax', () => {
    expect(calculateNetAmount(1000, 100, 90)).toBe(990)
  })

  it('TC-SO-080 | returns Total when discount and tax are both 0', () => {
    expect(calculateNetAmount(1000, 0, 0)).toBe(1000)
  })
})

// ─────────────────────────────────────────────
// WORKFLOW
// ─────────────────────────────────────────────
describe('canTransition', () => {
  it('TC-SO-081 | Sales Executive can transition Draft to Submitted', () => {
    expect(canTransition('Draft', 'Submitted', 'Sales Executive')).toBe(true)
  })

  it('TC-SO-082 | Sales Executive cannot transition Draft to Approved', () => {
    expect(canTransition('Draft', 'Approved', 'Sales Executive')).toBe(false)
  })

  it('TC-SO-083 | Sales Manager can transition Submitted to Approved', () => {
    expect(canTransition('Submitted', 'Approved', 'Sales Manager')).toBe(true)
  })

  it('TC-SO-084 | Sales Manager cannot transition Draft to Submitted', () => {
    expect(canTransition('Draft', 'Submitted', 'Sales Manager')).toBe(false)
  })
})

describe('canModify', () => {
  it('TC-SO-085 | Sales Executive can modify a Draft order', () => {
    expect(canModify('Draft', 'Sales Executive')).toBe(true)
  })

  it('TC-SO-086 | Sales Executive cannot modify a Submitted order', () => {
    expect(canModify('Submitted', 'Sales Executive')).toBe(false)
  })
})

// ─────────────────────────────────────────────
// RBAC
// ─────────────────────────────────────────────
describe('getScope', () => {
  it('TC-SO-087 | returns Self for Sales Executive', () => {
    expect(getScope('Sales Executive')).toBe('Self')
  })

  it('TC-SO-088 | returns Team for Sales Manager', () => {
    expect(getScope('Sales Manager')).toBe('Team')
  })

  it('TC-SO-089 | returns All for Sales Head', () => {
    expect(getScope('Sales Head')).toBe('All')
  })
})

describe('filterOrdersByScope', () => {
  it('TC-SO-090 | Sales Executive sees only their own orders', () => {
    // Arrange
    const userId = 'user-001'
    const scope = 'Self'
    const mockOrders = [
      { id: 'SO-001', createdBy: 'user-001' },
      { id: 'SO-002', createdBy: 'user-002' },
      { id: 'SO-003', createdBy: 'user-001' },
    ]
    // Act
    const result = filterOrdersByScope(mockOrders, scope, userId)
    // Assert
    expect(result).toHaveLength(2)
    expect(result.every((o) => o.createdBy === userId)).toBe(true)
  })
})
```
