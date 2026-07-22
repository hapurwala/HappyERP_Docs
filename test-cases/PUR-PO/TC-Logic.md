# TC — Purchase Order — Logic (Automated)

- **Module**: Purchase Order
- **Section**: Calculations, Workflow Transitions, RBAC
- **Test Type**: Automated (Jest)
- **TC Range**: TC-PO-071 – TC-PO-085
- **Reference**: [Specs](/specs/ERP-DES-PUR-PO.md)

---

## Calculations

| TC ID     | Test Case                                             | Category                         | Expected Result  | Priority |
| :-------- | :---------------------------------------------------- | :------------------------------- | :--------------- | :------- |
| TC-PO-071 | Discount calculated correctly for Percentage type     | Calculation Validation, Positive | `calculateDiscount(qty=10, rate=100, type='Percentage', value=10)` → `100` | High |
| TC-PO-072 | Discount calculated correctly for Fixed type          | Calculation Validation, Positive | `calculateDiscount(qty=10, rate=100, type='Fixed', value=50)` → `50`       | High |
| TC-PO-073 | Discount calculated correctly for Unit type           | Calculation Validation, Positive | `calculateDiscount(qty=10, rate=100, type='Unit', value=5)` → `50`         | High |
| TC-PO-074 | Line Total = Product Amount After Discount + Tax      | Calculation Validation, Positive | `calculateLineTotal(900, 162)` → `1062`                                    | High |
| TC-PO-075 | Net Amount = Subtotal − Total Discount + Total Tax    | Calculation Validation, Positive | `calculateNetAmount(1000, 100, 162)` → `1062`                              | High |

## Workflow

| TC ID      | Test Case                                               | Category                             | Expected Result | Priority |
| :--------- | :------------------------------------------------------ | :----------------------------------- | :-------------- | :------- |
| TC-PO-076 | Purchase Executive can submit a Draft PO                | Workflow Validation, Positive        | `canTransition('Draft', 'Submitted', 'Purchase Executive')` → `true`  | High   |
| TC-PO-077 | Purchase Executive cannot skip to Approved              | Workflow Validation, Negative        | `canTransition('Draft', 'Approved', 'Purchase Executive')` → `false`  | High   |
| TC-PO-078 | Purchase Manager can approve a Submitted PO             | Workflow Validation, Positive        | `canTransition('Submitted', 'Approved', 'Purchase Manager')` → `true` | High   |
| TC-PO-079 | Purchase Manager can move Approved PO to Ordered        | Workflow Validation, Positive        | `canTransition('Approved', 'Ordered', 'Purchase Manager')` → `true`   | High   |
| TC-PO-080 | Purchase Executive can modify a Draft PO                | Business Rules, Positive             | `canModify('Draft', 'Purchase Executive')` → `true`                   | High   |
| TC-PO-081 | Purchase Executive cannot modify a Submitted PO         | Business Rules, Negative             | `canModify('Submitted', 'Purchase Executive')` → `false`              | High   |

## RBAC

| TC ID      | Test Case                                               | Category                        | Expected Result                                              | Priority |
| :--------- | :------------------------------------------------------ | :------------------------------ | :----------------------------------------------------------- | :------- |
| TC-PO-082 | Purchase Executive scope is Self                        | Security & Permission, Positive | `getScope('Purchase Executive')` → `'Self'`                  | High     |
| TC-PO-083 | Purchase Manager scope is Team                          | Security & Permission, Positive | `getScope('Purchase Manager')` → `'Team'`                    | High     |
| TC-PO-084 | Procurement Head scope is All                           | Security & Permission, Positive | `getScope('Procurement Head')` → `'All'`                     | High     |
| TC-PO-085 | filterPOsByScope — Self returns only current user's POs | Security & Permission, Positive | Only POs where `createdById === currentUser.userId` returned | High     |

---

```js
import {
  calculateDiscount,
  calculateLineTotal,
  calculateNetAmount,
} from '@/modules/purchase-order/utils/calculations'

import {
  canTransition,
  canModify,
} from '@/modules/purchase-order/utils/workflow'

import {
  getScope,
  filterPOsByScope,
} from '@/modules/purchase-order/utils/rbac'

// ─────────────────────────────────────────────
// Calculation Tests
// ─────────────────────────────────────────────

describe('calculateDiscount', () => {

  it('TC-PO-071 | should return correct amount for Percentage type', () => {
    // Arrange
    const qty = 10, rate = 100, type = 'Percentage', value = 10

    // Act
    const result = calculateDiscount(qty, rate, type, value)

    // Assert — 10 × 100 × (10/100) = 100
    expect(result).toBe(100)
  })

  it('TC-PO-072 | should return correct amount for Fixed type', () => {
    // Arrange
    const qty = 10, rate = 100, type = 'Fixed', value = 50

    // Act
    const result = calculateDiscount(qty, rate, type, value)

    // Assert — Fixed discount is applied as-is
    expect(result).toBe(50)
  })

  it('TC-PO-073 | should return correct amount for Unit type', () => {
    // Arrange
    const qty = 10, rate = 100, type = 'Unit', value = 5

    // Act
    const result = calculateDiscount(qty, rate, type, value)

    // Assert — 5 per unit × 10 units = 50
    expect(result).toBe(50)
  })

})

describe('calculateLineTotal', () => {

  it('TC-PO-074 | should return Product Amount After Discount + Tax', () => {
    // Arrange
    const productAmountAfterDiscount = 900
    const taxValue = 162

    // Act
    const result = calculateLineTotal(productAmountAfterDiscount, taxValue)

    // Assert — 900 + 162 = 1062
    expect(result).toBe(1062)
  })

})

describe('calculateNetAmount', () => {

  it('TC-PO-075 | should return Subtotal − Total Discount + Total Tax', () => {
    // Arrange
    const subtotal = 1000
    const totalDiscount = 100
    const totalTax = 162

    // Act
    const result = calculateNetAmount(subtotal, totalDiscount, totalTax)

    // Assert — 1000 − 100 + 162 = 1062
    expect(result).toBe(1062)
  })

})

// ─────────────────────────────────────────────
// Workflow Transition Tests
// ─────────────────────────────────────────────

describe('canTransition — Purchase Order Workflow', () => {

  describe('Purchase Executive', () => {

    it('TC-PO-076 | should allow Draft → Submitted', () => {
      // Arrange
      const currentStage = 'Draft'
      const nextStage = 'Submitted'
      const role = 'Purchase Executive'

      // Act
      const result = canTransition(currentStage, nextStage, role)

      // Assert
      expect(result).toBe(true)
    })

    it('TC-PO-077 | should NOT allow Draft → Approved (must go through Submitted first)', () => {
      // Arrange
      const currentStage = 'Draft'
      const nextStage = 'Approved'
      const role = 'Purchase Executive'

      // Act
      const result = canTransition(currentStage, nextStage, role)

      // Assert — Purchase Executive cannot skip Manager approval
      expect(result).toBe(false)
    })

  })

  describe('Purchase Manager', () => {

    it('TC-PO-078 | should allow Submitted → Approved', () => {
      // Arrange
      const currentStage = 'Submitted'
      const nextStage = 'Approved'
      const role = 'Purchase Manager'

      // Act
      const result = canTransition(currentStage, nextStage, role)

      // Assert
      expect(result).toBe(true)
    })

    it('TC-PO-079 | should allow Approved → Ordered', () => {
      // Arrange
      const currentStage = 'Approved'
      const nextStage = 'Ordered'
      const role = 'Purchase Manager'

      // Act
      const result = canTransition(currentStage, nextStage, role)

      // Assert
      expect(result).toBe(true)
    })

  })

})

// ─────────────────────────────────────────────
// Modify Permission Tests
// ─────────────────────────────────────────────

describe('canModify — Purchase Order Workflow', () => {

  describe('Purchase Executive', () => {

    it('TC-PO-080 | should allow modify on Draft stage', () => {
      // Arrange
      const stage = 'Draft'
      const role = 'Purchase Executive'

      // Act
      const result = canModify(stage, role)

      // Assert
      expect(result).toBe(true)
    })

    it('TC-PO-081 | should NOT allow modify on Submitted stage', () => {
      // Arrange
      const stage = 'Submitted'
      const role = 'Purchase Executive'

      // Act
      const result = canModify(stage, role)

      // Assert — PO is locked after submission
      expect(result).toBe(false)
    })

  })

  describe('Auditor', () => {

    it('should NOT allow modify on any stage', () => {
      // Arrange
      const stages = ['Draft', 'Submitted', 'Approved', 'Ordered', 'Partial Received', 'Fully Received', 'Completed', 'Cancelled', 'Rejected']
      const role = 'Auditor'

      // Act & Assert
      stages.forEach((stage) => {
        expect(canModify(stage, role)).toBe(false)
      })
    })

  })

})

// ─────────────────────────────────────────────
// RBAC / Scope Tests
// ─────────────────────────────────────────────

const mockPOs = [
  { poId: 'PO-001', createdById: 'user-A', teamId: 'team-1' },
  { poId: 'PO-002', createdById: 'user-B', teamId: 'team-1' },
  { poId: 'PO-003', createdById: 'user-C', teamId: 'team-2' },
]

const currentUser = { userId: 'user-A', teamId: 'team-1', role: 'Purchase Executive' }

describe('getScope', () => {

  it('TC-PO-082 | Purchase Executive should have Self scope', () => {
    expect(getScope('Purchase Executive')).toBe('Self')
  })

  it('TC-PO-083 | Purchase Manager should have Team scope', () => {
    expect(getScope('Purchase Manager')).toBe('Team')
  })

  it('TC-PO-084 | Procurement Head should have All scope', () => {
    expect(getScope('Procurement Head')).toBe('All')
  })

})

describe('filterPOsByScope', () => {

  it('TC-PO-085 | Self scope should return only POs created by current user', () => {
    // Arrange
    const scope = 'Self'

    // Act
    const result = filterPOsByScope(mockPOs, currentUser, scope)

    // Assert — only PO-001 was created by user-A
    expect(result).toHaveLength(1)
    expect(result[0].poId).toBe('PO-001')
  })

  it('Team scope should return only POs from same team', () => {
    // Arrange
    const scope = 'Team'

    // Act
    const result = filterPOsByScope(mockPOs, currentUser, scope)

    // Assert — PO-001 and PO-002 are from team-1
    expect(result).toHaveLength(2)
  })

  it('All scope should return all POs', () => {
    // Arrange
    const scope = 'All'

    // Act
    const result = filterPOsByScope(mockPOs, currentUser, scope)

    // Assert
    expect(result).toHaveLength(3)
  })

})
```
