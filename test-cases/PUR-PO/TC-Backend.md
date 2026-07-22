# TC — Purchase Order — Backend

- **Module**: Purchase Order
- **Section**: Database Validation, Integration Validation, Audit Validation
- **Test Type**: Manual + Automated
- **Reference**: [DB Doc](/database/ERP-DB-PUR-PO.md) | [Specs](/specs/ERP-DES-PUR-PO.md)

---

## Database Validation

| TC ID     | Test Case                                             | Category                      | Precondition                       | Steps                                                                                                                                                                                           | Expected Result                                                                                                        | Priority | Test Type |
| :-------- | :---------------------------------------------------- | :---------------------------- | :--------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------- | :------- | :-------- |
| TC-PO-086 | PO saved with all required Firestore fields           | Database Validation, Positive | A PO has been created and saved    | 1. Create a new PO with all required fields filled<br/>2. Save the PO<br/>3. Open Firestore → `purchase_orders` collection<br/>4. Find the document<br/>5. Verify all required fields           | Document contains: `poNumber`, `poDate`, `vendorId`, `vendorName`, `currency`, `stage`, `createdBy`, `createdAt`, `branchId`, `companyId` | High     | Manual    |
| TC-PO-087 | isDeleted flag set to true on PO deletion             | Database Validation, Positive | A PO in Draft stage exists         | 1. Delete a Draft PO via row action<br/>2. Open Firestore and find the document by ID<br/>3. Check the `isDeleted` field                                                                        | `isDeleted` is `true`; document not physically deleted from Firestore                                                  | High     | Manual    |
| TC-PO-094 | pending_quantity = ordered_quantity − received_quantity | Database Validation, Calculation Validation, Positive | Jest unit test | Call `calculatePendingQuantity(orderedQty=100, receivedQty=60)`                                                                                                                     | Returns `40`                                                                                                           | High     | Automated |
| TC-PO-095 | pending_quantity cannot go below zero                 | Database Validation, Edge Case, Negative | Jest unit test              | Call `calculatePendingQuantity(orderedQty=100, receivedQty=110)`                                                                                                                                | Returns `0` — floored at 0                                                                                             | High     | Automated |

## Audit Validation

| TC ID     | Test Case                                         | Category                   | Precondition                                  | Steps                                                                                                                                                                           | Expected Result                                                                                                    | Priority | Test Type |
| :-------- | :------------------------------------------------ | :------------------------- | :-------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :----------------------------------------------------------------------------------------------------------------- | :------- | :-------- |
| TC-PO-088 | stageHistory appended on every stage change, including rejection | Audit Validation, Positive + Negative | A PO that has moved through at least 2 stages | **Positive path:** 1. Move PO from Draft to Submitted<br/>2. Check `stageHistory` in Firestore → verify entry added<br/>3. Move to Approved<br/>4. Check again<br/><br/>**Negative (rejection) path:** 5. Login as Purchase Manager<br/>6. Open a Submitted PO<br/>7. Reject with a remark<br/>8. Check `stageHistory` in Firestore | Positive: each transition appends an entry with stage name, timestamp, userId, and remarks<br/>Rejection: entry has `stage = "Rejected"`, the entered remarks, and manager's userId | High     | Manual    |
| TC-PO-090 | updatedBy and updatedAt updated on every save     | Audit Validation, Positive | An existing PO                                | 1. Open a Draft PO<br/>2. Modify a field (e.g. Freight Type)<br/>3. Click Save<br/>4. Check `updatedBy` and `updatedAt` in Firestore                                            | `updatedBy` shows the logged-in user's ID; `updatedAt` shows the current timestamp                                 | Medium   | Manual    |

## Integration Validation

| TC ID     | Test Case                                                        | Category                                         | Precondition                                    | Steps                                                                                                                                                                     | Expected Result                                   | Priority | Test Type |
| :-------- | :--------------------------------------------------------------- | :----------------------------------------------- | :---------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :------------------------------------------------ | :------- | :-------- |
| TC-PO-091 | received_quantity updated via GRN                                | Integration Validation, Positive                 | A PO in Ordered stage; a GRN has been created   | 1. Note current `received_quantity` (should be 0)<br/>2. Create a GRN for partial qty (e.g. 5 of 10)<br/>3. Check `received_quantity` and `pending_quantity` on the line  | `received_quantity` = 5; `pending_quantity` = 5   | High     | Manual    |
| TC-PO-092 | PO stage auto-updates to Partial Received when GRN < ordered qty | Integration Validation, Business Rules, Positive | PO in Ordered stage with total quantity = 100   | 1. Create a GRN for 60 units<br/>2. Save the GRN<br/>3. Observe the PO stage                                                                                             | PO stage changes to "Partial Received"            | High     | Manual    |
| TC-PO-093 | PO stage auto-updates to Fully Received when GRN = ordered qty   | Integration Validation, Business Rules, Positive | PO in Partial Received stage; pending_qty = 40  | 1. Create a GRN for the remaining 40 units<br/>2. Save the GRN<br/>3. Observe the PO stage                                                                               | PO stage changes to "Fully Received"              | High     | Manual    |

---

```js
import { calculatePendingQuantity } from '@/modules/purchase-order/utils/grn'

describe('calculatePendingQuantity', () => {

  it('TC-PO-094 | should return correct pending quantity', () => {
    // Arrange
    const orderedQty = 100
    const receivedQty = 60

    // Act
    const result = calculatePendingQuantity(orderedQty, receivedQty)

    // Assert — 100 − 60 = 40
    expect(result).toBe(40)
  })

  it('TC-PO-095 | should return 0 when received exceeds ordered (edge case)', () => {
    // Arrange
    const orderedQty = 100
    const receivedQty = 110

    // Act
    const result = calculatePendingQuantity(orderedQty, receivedQty)

    // Assert — pending qty cannot go negative; floor at 0
    expect(result).toBe(0)
  })

})
```
