# TC — Sales Order — Backend

- **Module**: Sales Order
- **Section**: Database, Audit, Integration
- **Test Type**: Manual (Firestore / Postman) + Automated (Jest)
- **TC Range**: TC-SO-091 – TC-SO-100
- **Reference**: [DB Schema](/database/ERP-DB-SAL-Order.md) | [Specs](/specs/ERP-DES-SAL-Order.md)

---

## Database Validation

| TC ID     | Test Case                                                                    | Category                      | Precondition                                         | Steps                                                                                                                                                                                                                    | Expected Result                                                                                                                                                                                                                   | Priority | Test Type |
| :-------- | :--------------------------------------------------------------------------- | :---------------------------- | :--------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------- | :-------- |
| TC-SO-091 | Saved Sales Order contains all required Firestore fields                     | Database Validation, Positive | Firestore console access; a saved Sales Order exists | 1. Create and save a Sales Order with all fields filled<br/>2. Open Firestore → `sales_orders` collection<br/>3. Open the document<br/>4. Verify all expected fields                                                    | Document contains: `orderNumber`, `orderDate`, `customerId`, `customerName`, `agentId`, `placedById`, `priority`, `currencyId`, `subtotal`, `totalDiscount`, `totalTax`, `netAmount`, `roundOff`, `minDeliveryDate`, `stage`, `stageHistory`, `createdBy`, `updatedBy`, `createdAt`, `updatedAt`, `branchId`, `companyId`, `isDeleted` | High     | Manual    |
| TC-SO-092 | Order lines saved in `order_lines` sub-collection with correct fields        | Database Validation, Positive | A saved Sales Order with at least 1 line item        | 1. Navigate to Firestore → `sales_orders` → `{orderId}` → `order_lines`<br/>2. Open a line document<br/>3. Verify all expected fields                                                                                   | Each line contains: `productId`, `varietyId`, `packId`, `noOfPacks`, `quantity`, `uomId`, `rate`, `discountPercent`, `discountAmount`, `taxAmount`, `lineTotal`, `deliveryDate`, `paymentTermsId`, `notes`, `lineStage`           | High     | Manual    |
| TC-SO-093 | Deleting an order sets `is_deleted = true` — document not removed            | Database Validation, Positive | A Draft Sales Order; Firestore access                | 1. Delete a Draft order from the UI<br/>2. Open Firestore and locate the document<br/>3. Check the `is_deleted` field                                                                                                    | Document still exists; `is_deleted` is `true`; order no longer appears in the list                                                                                                                                                | High     | Manual    |

## Audit Validation

| TC ID     | Test Case                                                                  | Category                   | Precondition                              | Steps                                                                                                                                                                                   | Expected Result                                                                                         | Priority | Test Type |
| :-------- | :------------------------------------------------------------------------- | :------------------------- | :---------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------ | :------- | :-------- |
| TC-SO-094 | Stage transition appends an entry to the `stageHistory` array                              | Audit Validation, Positive | Draft Sales Order; Firestore access       | 1. Submit a Draft order (Set as Submitted)<br/>2. Open Firestore → `sales_orders` → `{orderId}`<br/>3. Inspect the `stageHistory` array                                                  | New entry with: `from_stage: "Draft"`, `to_stage: "Submitted"`, `changed_by` (user ID), `changed_at` (ISO timestamp) | High     | Manual    |
| TC-SO-095 | `created_by` and `created_at` not changed on update                        | Audit Validation, Positive | A newly created Sales Order; Firestore access | 1. Note `created_by` and `created_at` after creation<br/>2. Edit and save the order<br/>3. Re-check in Firestore                                                                    | `created_by` and `created_at` remain unchanged after the update                                        | High     | Manual    |
| TC-SO-096 | `updated_by` and `updated_at` change on every save                        | Audit Validation, Positive | An existing Sales Order                   | 1. Edit and save as User A — note `updated_by` and `updated_at`<br/>2. Edit and save again as User B<br/>3. Check Firestore values                                                     | `updated_by` changes to User B's ID; `updated_at` reflects the latest save timestamp                   | High     | Manual    |

## Integration Validation

| TC ID     | Test Case                                                                 | Category                         | Precondition                                                   | Steps                                                                                                                                                              | Expected Result                                                        | Priority | Test Type |
| :-------- | :------------------------------------------------------------------------ | :------------------------------- | :------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------- | :------- | :-------- |
| TC-SO-097 | Dispatch note creation increments `dispatched_quantity` on the order line | Integration Validation, Positive | Approved Sales Order with at least 1 line; dispatch accessible | 1. Note current `dispatched_quantity` (should be 0)<br/>2. Create a dispatch note with quantity = 10<br/>3. Save<br/>4. Re-check the line in Firestore             | `dispatched_quantity` updates to 10                                    | High     | Manual    |
| TC-SO-098 | Order stage auto-updates to Partial Dispatched on partial dispatch        | Integration Validation, Positive | Approved order with ordered_quantity = 100                     | 1. Create a dispatch note covering 60 of 100 units<br/>2. Save<br/>3. Check the Sales Order stage                                                                 | Stage changes to "Partial Dispatched" automatically                    | High     | Manual    |
| TC-SO-099 | calculateDispatchStatus returns 'Partial Dispatched' when qty partial     | Integration Validation, Positive | Jest unit test                                                 | Call `calculateDispatchStatus(100, 60)`                                                                                                                            | `'Partial Dispatched'`                                                 | High     | Automated |
| TC-SO-100 | calculateDispatchStatus returns 'Fully Dispatched' when all dispatched    | Integration Validation, Positive | Jest unit test                                                 | Call `calculateDispatchStatus(100, 100)`                                                                                                                           | `'Fully Dispatched'`                                                   | High     | Automated |

---

```js
import { calculateDispatchStatus } from '@/modules/sales-order/utils/dispatch'

// ─────────────────────────────────────────────
// TC-SO-099: Partial Dispatched
// ─────────────────────────────────────────────
describe('calculateDispatchStatus', () => {
  it('TC-SO-099 | returns Partial Dispatched when dispatched < ordered', () => {
    // Arrange
    const orderedQty = 100
    const dispatchedQty = 60

    // Act
    const result = calculateDispatchStatus(orderedQty, dispatchedQty)

    // Assert
    expect(result).toBe('Partial Dispatched')
  })

  // ─────────────────────────────────────────────
  // TC-SO-100: Fully Dispatched
  // ─────────────────────────────────────────────
  it('TC-SO-100 | returns Fully Dispatched when dispatched equals ordered', () => {
    // Arrange
    const orderedQty = 100
    const dispatchedQty = 100

    // Act
    const result = calculateDispatchStatus(orderedQty, dispatchedQty)

    // Assert
    expect(result).toBe('Fully Dispatched')
  })
})
```
