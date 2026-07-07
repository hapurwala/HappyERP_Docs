# Module Design Details

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: **Product Management**
- **Module**: **Product Master**

---

## Roles

| Role              | Purpose                                              |
| :---------------- | :--------------------------------------------------- |
| Product Executive | Creates and manages product master records           |
| Product Manager   | Reviews and approves product master records          |
| System Admin      | Full system configuration and administrative control |

## Additional Roles

| Role               | Purpose                                                            |
| :----------------- | :----------------------------------------------------------------- |
| Sales Manager      | Uses products in quotations, sales orders and invoicing            |
| Purchase Manager   | Uses products for procurement and vendor mapping                   |
| Inventory Manager  | Manages stock, warehouses and inventory configuration              |
| Production Manager | Uses products for manufacturing, BOM and production planning       |
| Accounts Executive | Uses products for accounting and tax-related transactions          |
| Store Manager      | Uses products during inward, outward and stock movement operations |
| Auditor            | Read-only access for audit and compliance                          |


---

## Field Ownership Participation

Product Master participates in Field Ownership Mode. Each of the four owned sections (Sale, Purchase, Production, Inventory) can be independently configured as **Direct Edit** (default — standard App Role modify permission applies) or **Data Change Request Required** (edits by the section-owning role are intercepted and routed through the DCR workflow).

**Section Owners:**

| Section                  | Owner              |
| :----------------------- | :----------------- |
| Sale Configuration       | Sales Manager      |
| Purchase Configuration   | Purchase Manager   |
| Production Configuration | Production Manager |
| Inventory Configuration  | Inventory Manager  |

**Primary Role for DCR review:** Product Executive (System Admin can also action).


---

## Workflow Stages

Governs the lifecycle of a **Product record**.

| Stage     | Description                                                          | Who Will Set It          | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles                                                                                                                                             | System Action                                                          |
| :-------- | :------------------------------------------------------------------- | :----------------------- | :----------- | :----------- | :----------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------- |
| Draft     | Initial stage where the product is being created and fully editable. | User (Product Executive) | Yes          | Yes          | No           | Product Executive, System Admin                                                                                                                                       | ---                                                                    |
| Submitted | Product submitted for review by Product Manager.                     | User (Product Executive) | No           | No           | Yes          | Product Executive, Product Manager, System Admin                                                                                                                      | Notify Product Manager, Lock Editing                                   |
| Approved  | Product approved and available for business transactions.            | User (Product Manager)   | Yes          | No           | No           | Product Executive, Product Manager, Sales Manager, Purchase Manager, Inventory Manager, Production Manager, Accounts Executive, Store Manager, Auditor, Viewer, Admin | Publish to Sales, Purchase, Inventory & Production; Create Audit Entry |

---

## DCR Workflow Stages

Governs the lifecycle of a **Data Change Request record** — a separate state machine that runs in parallel with the Product workflow, and only applies when a section is configured as **Data Change Request Required** under Field Ownership Mode. 

| Stage         | Description                                                                                                                     | Who Will Set It                         | Allow Modify | Allow Delete | Allow Cancel | Allow View/Share To Roles                  | System Action                                                      |
| :------------ | :------------------------------------------------------------------------------------------------------------------------------ | :-------------------------------------- | :----------- | :----------- | :----------- | :----------------------------------------- | :----------------------------------------------------------------- |
| DCR Draft     | Section-owning role has staged a field change on an Approved product but not yet submitted; or a rejected DCR pending revision. | User (Section-Owning Role)              | Yes          | Yes          | No           | Requester, System Admin                    | ---                                                                |
| DCR Submitted | DCR submitted for Product Executive review.                                                                                     | User (Section-Owning Role)              | Yes          | No           | Yes          | Product Executive, Requester, System Admin | Notify Product Executive; lock field on record                     |
| DCR Approved  | Product Executive approved; field updated on the Approved product.                                                              | User (Product Executive / System Admin) | No           | No           | No           | Product Executive, Requester, System Admin | Apply field change; append to `fieldChangeLog`; notify requester   |
| DCR Rejected  | Product Executive rejected.  DCR auto-rolls back to `DCR Draft` for requester revision.                                         | User (Product Executive / System Admin) | No           | No           | No           | Product Executive, Requester, System Admin | Capture rejection reason; roll back to DCR Draft; notify requester |
|               |                                                                                                                                 |                                         |              |              |              |                                            |                                                                    |

> [!Note]
> **Flow:**
> - Happy path: `DCR Draft` → `DCR Submitted` → `DCR Approved` 
> - Rejection path: `DCR Draft` → `DCR Submitted` → `DCR Rejected` → `DCR Draft` *(requester revises and resubmits)*
>
> `DCR Approved` is the only terminal state. `DCR Rejected` is transient — the DCR immediately rolls back to `DCR Draft` with the rejection reason captured for the requester's revision.
---

## Workflow - Role Matrix

| Role                | Current Stage | Scope | Create | Modify                       | Delete | Cancel | Next Stage                  | Rollback Stage |
| :------------------ | :------------ | :---- | :----- | :--------------------------- | :----- | :----- | :-------------------------- | :------------- |
| Product Executive   | Draft         | Self  | Yes    | Yes                          | Yes    | No     | Submit                      | No             |
| Product Manager     | Submitted     | Team  | No     | No                           | No     | Yes    | Approve / Reject → Draft    | Yes            |
| Product Manager     | Approved      | Team  | No     | No                           | No     | No     | No                          | No             |
| Sales Manager       | Approved      | Team  | No     | Yes (Sale Config only)       | No     | No     | -                           | No             |
| Purchase Manager    | Approved      | Team  | No     | Yes (Purchase Config only)   | No     | No     | -                           | No             |
| Production Manager  | Approved      | Team  | No     | Yes (Production Config only) | No     | No     | -                           | No             |
| Inventory Manager   | Approved      | Team  | No     | Yes (Inventory Config only)  | No     | No     | -                           | No             |
| Accounts Executive  | Approved      | All   | No     | No                           | No     | No     | -                           | No             |
| Store Manager       | Approved      | All   | No     | No                           | No     | No     | -                           | No             |
| Auditor             | Any           | All   | No     | No                           | No     | No     | -                           | No             |
| System Admin        | Any           | All   | Yes    | Yes                          | Yes    | Yes    | Any Transition              | Yes            |
| Section-Owning Role | DCR Draft     | Self  | Yes    | Yes                          | Yes    | No     | Submit                      | No             |
| Section-Owning Role | DCR Submitted | Self  | No     | No                           | No     | Yes    | -                           | Yes (to Draft) |
| Product Executive   | DCR Submitted | Team  | No     | No                           | No     | No     | DCR Approved / DCR Rejected | No             |
| Product Executive   | DCR Approved  | Team  | No     | No                           | No     | No     | -                           | No             |
| Product Executive   | DCR Rejected  | Team  | No     | No                           | No     | No     | -                           | No             |

> [!Note]
> Additional roles' **Modify** capability on their owned section is subject to **Field Ownership Mode**:
> - Section = **Direct Edit** → modify applies immediately (Modified By/On updated).
> - Section = **Data Change Request Required** → modify is intercepted; user must raise a DCR.

---

## Header Fields

| Field         | Description               | Type      | Required |
| :------------ | :------------------------ | :-------- | :------- |
| Product Name  | Name of Product           | Text      | Yes      |
| Short Name    | Product Code / Short Name | Text      | Yes      |
| Product Group | Product classification    | Dropdown  | Yes      |
| Brand         | Product Brand             | Dropdown  | No       |
| UoM           | Unit of Measurement       | Dropdown  | Yes      |
| HSN Code      | GST HSN Code              | Text      | No\*     |
| Description   | Product description       | Text Area | No       |
| Start Date    | Product activation date   | Date      | Yes      |
| End Date      | Product deactivation date | Date      | No       |
| Remarks       | Internal remarks          | Text Area | No       |

> [!Note]
> `HSN Code (No\*)` — becomes **Required** when `Sale Enabled = Yes` (GST compliance).

---

## Sale Configuration

| Field                   | Description                                                      | Type    | Required |
| :---------------------- | :--------------------------------------------------------------- | :------ | :------- |
| Sale Enabled            | Indicates whether this product is available for sale             | Boolean | Yes      |
| Minimum Sale Quantity   | Minimum quantity required in a single sale order                 | Number  | No       |
| Maximum Sale Quantity   | Maximum quantity allowed in a single sale order                  | Number  | No       |
| Sale Delivery Lead Time | Minimum number of days required to deliver this product for sale | Number  | No       |

## Purchase Configuration

| Field                       | Description                                                            | Type    | Required |
| :-------------------------- | :--------------------------------------------------------------------- | :------ | :------- |
| Purchase Enabled            | Indicates whether this product is available for purchase               | Boolean | Yes      |
| Minimum Purchase Quantity   | Minimum quantity required in a single purchase order                   | Number  | No       |
| Maximum Purchase Quantity   | Maximum quantity allowed in a single purchase order                    | Number  | No       |
| Purchase Delivery Lead Time | Minimum number of days required to receive this product after purchase | Number  | No       |

## Production Configuration

| Field              | Description                                                              | Type    | Required |
| :----------------- | :----------------------------------------------------------------------- | :------ | :------- |
| Production Enabled | Indicates whether this product is produced / manufactured by the company | Boolean | Yes      |

## Inventory Configuration

| Field                  | Description                               | Type    | Required |
| :--------------------- | :---------------------------------------- | :------ | :------- |
| Is Goods               | True if physical good, False if service   | Boolean | Yes      |
| Maintain Stock         | Enable stock tracking for this product    | Boolean | Yes      |
| Maintain Batch         | Enable batch-level tracking               | Boolean | No       |
| Maintain Expiry        | Enable expiry date tracking per batch     | Boolean | No       |
| Minimum Stock Quantity | Minimum stock level before alert          | Number  | No       |
| Maximum Stock Quantity | Maximum stock level allowed               | Number  | No       |
| Reorder Stock Quantity | Stock level at which reorder is triggered | Number  | No       |

---

## Packs

| Field       | Description         | Type      | Required |
| :---------- | :------------------ | :-------- | :------- |
| Pack Name   | Product Pack Name   | Text      | Yes      |
| Pack Code   | Product Pack Code   | Text      | Yes      |
| UoM         | Unit of Measurement | Dropdown  | Yes      |
| Qty in Pack | Quantity in Pack    | Number    | Yes      |
| Remarks     | Additional Notes    | Text Area | No       |

---

## Media & Attachments

| Field         | Description               | Type         |
| :------------ | :------------------------ | :----------- |
| Primary Image | Primary Product Image     | Image Upload |
| Product Media | Additional Product Images | Image Upload |
| Attachments   | Supporting Documents      | File Upload  |

| Property                   | Images         | Attachments          |
| :------------------------- | :------------- | :------------------- |
| Supported Formats          | PNG, JPG, JPEG | PDF, DOCX, XLSX, CSV |
| Maximum File Size per File | 2 MB           | 5 MB                 |

---

## Validation Rules

| Field / Constraint     | Rule                                                                                          |
| :--------------------- | :-------------------------------------------------------------------------------------------- |
| Product Name           | Must be unique across all  products                                                |
| Short Name             | Must be unique across all  products                                                |
| Start Date / End Date  | End Date cannot be earlier than Start Date                                                    |
| Start Date             | Cannot be modified after product moves to `Approved` (locked for audit)                       |
| Sale Quantity          | Minimum Sale Quantity cannot exceed Maximum Sale Quantity                                     |
| Purchase Quantity      | Minimum Purchase Quantity cannot exceed Maximum Purchase Quantity                             |
| Stock Quantity         | Minimum Stock Quantity cannot exceed Maximum Stock Quantity                                   |
| Reorder Stock Quantity | Reorder Stock Quantity cannot exceed Maximum Stock Quantity; must be ≥ Minimum Stock Quantity |
| Pack Code              | Must be unique within a product                                                               |
| Is Goods = False       | Maintain Stock, Maintain Batch, Maintain Expiry must be `No` — locked and disabled on form    |
| Maintain Batch         | Can only be `Yes` if Maintain Stock = `Yes`                                                   |
| Maintain Expiry        | Can only be `Yes` if Maintain Batch = `Yes`                                                   |
| HSN Code               | Required if Sale Enabled = `Yes` (GST compliance)                                             |
| UoM change             | Blocked if any Pack rows exist that depend on the UoM; must clear Packs first                 |
| Product Group change   | Allowed with confirmation; adds an entry to `fieldChangeLog`                                  |
| End Date backdating    | Cannot be set to a date earlier than the latest transaction referencing this product          |

---

## Reports

| Report                   | Purpose                                                                             |
| :----------------------- | :---------------------------------------------------------------------------------- |
| Product List             | All products with stage, group, brand, active status, usage                         |
| Pending Approval Report  | Products currently in `Submitted` stage                                             |
| Inactive Products Report | Products past their End Date (Closed)                                               |
| Product Field Change Log | Full audit of every field change on every product, with mode-at-time-of-change      |


---

## System Information

| Field       | Description                        | Type     |
| :---------- | :--------------------------------- | :------- |
| Created By  | User who created the product       | User     |
| Created On  | Record creation date and time      | DateTime |
| Modified By | Last user who modified the product | User     |
| Modified On | Last modification date and time    | DateTime |

---

# Firestore Database Structure

## Collection: `products`

### Document Structure

| Field Name                | Data Type  | Description                                                              |
| :------------------------ | :--------- | :----------------------------------------------------------------------- |
| productId                 | string     | Unique Product ID                                                        |
| name                      | string     | Product Name                                                             |
| shortName                 | string     | Product Code / Short Name                                                |
| productGroupId            | string     | Product Group reference                                                  |
| brandId                   | string     | Brand reference                                                          |
| uomId                     | string     | Unit of Measurement reference                                            |
| hsnCode                   | string     | GST HSN Code                                                             |
| description               | string     | Product Description                                                      |
| primaryImage              | string     | Primary Image URL                                                        |
| media                     | array<map> | Additional product images                                                |
| attachments               | array<map> | Supporting documents                                                     |
| packs                     | array<map> | Product Packs                                                            |
| isSale                    | boolean    | Sale Enabled                                                             |
| saleMinOrderQty           | number     | Minimum Sale Quantity                                                    |
| saleMaxOrderQty           | number     | Maximum Sale Quantity                                                    |
| saleMinDeliveryPeriod     | number     | Sale Delivery Lead Time (days)                                           |
| isPurchase                | boolean    | Purchase Enabled                                                         |
| purchaseMinOrderQty       | number     | Minimum Purchase Quantity                                                |
| purchaseMaxOrderQty       | number     | Maximum Purchase Quantity                                                |
| purchaseMinDeliveryPeriod | number     | Purchase Delivery Lead Time (days)                                       |
| isProduce                 | boolean    | Production Enabled                                                       |
| isGoods                   | boolean    | True = physical good, False = service                                    |
| maintainStock             | boolean    | Stock tracking enabled                                                   |
| maintainBatch             | boolean    | Batch tracking enabled                                                   |
| maintainExpiry            | boolean    | Expiry tracking enabled                                                  |
| minStockQty               | number     | Minimum Stock Quantity                                                   |
| maxStockQty               | number     | Maximum Stock Quantity                                                   |
| reorderStockQty           | number     | Reorder Stock Quantity                                                   |
| stage                     | string     | Current Workflow Stage — `Draft` \| `Submitted` \| `Approved` |
| startDate                 | timestamp  | Product activation date                                                  |
| endDate                   | timestamp  | Product deactivation date                                                |
| remarks                   | string     | Internal remarks                                                         |
| createdBy                 | string     | User ID of creator                                                       |
| createdByName             | string     | Creator name snapshot                                                    |
| createdAt                 | timestamp  | Record creation timestamp                                                |
| updatedBy                 | string     | User ID of last modifier                                                 |
| updatedByName             | string     | Last modifier name snapshot                                              |
| updatedAt                 | timestamp  | Last modification timestamp                                              |
| stageHistory              | array<map> | Complete workflow stage audit history                                    |
| fieldChangeLog            | array<map> | Per-field change audit (see structure below)                             |
| companyId                 | string     | Company ID                                                               |
| isDeleted                 | boolean    | Soft delete flag                                                         |


### `stageHistory` Structure

```json
[
  {
    "stage": "Draft",
    "actionBy": "userId",
    "actionByName": "Rahul",
    "actionAt": "Timestamp",
    "remarks": "Product created"
  },
  {
    "stage": "Submitted",
    "actionBy": "userId",
    "actionByName": "Rahul",
    "actionAt": "Timestamp",
    "remarks": "Submitted for approval"
  },
  {
    "stage": "Approved",
    "actionBy": "managerId",
    "actionByName": "Product Manager",
    "actionAt": "Timestamp",
    "remarks": "Verified and approved"
  }
]
```

---

## Workflow Timeline

Component to use: `StageHistoryViewer`

Data Source: `stageHistory` array (see `stageHistory` Structure above)

| Name/Label | Data Source             | Component | Tooltip   |
| :--------- | :---------------------- | :-------- | :-------- |
| Stage      | stage                   | Text      | -         |
| Set At     | actionAt                | Text      | -         |
| Set By     | actionBy / actionByName | Avatar    | User name |
| Remarks    | remarks                 | Text      | -         |
