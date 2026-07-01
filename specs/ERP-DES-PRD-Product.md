# Module Design Details

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: **Product Management**
- **Module**: **Product Master**

---

## Roles

|Role|Purpose|
|:--|:--|
|Product Executive|Creates and manages product master records|
|Product Manager|Reviews and approves product master records|

## Additional Roles

|Role|Purpose|
|:--|:--|
|Purchase Manager|Uses products for procurement and vendor mapping|
|Sales Manager|Uses products in quotations, sales orders and invoicing|
|Inventory Manager|Manages stock, warehouses and inventory configuration|
|Production Manager|Uses products for manufacturing, BOM and production planning|
|Accounts Executive|Uses products for accounting and tax-related transactions|
|Store Manager|Uses products during inward, outward and stock movement operations|
|Auditor|Read-only access for audit and compliance|
|System Admin|Full system configuration and administrative control|

---

## Workflow Stages

| Stage     | Description                                              | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View To Roles                                                                                                       | System Action                                         |
| :-------- | :------------------------------------------------------- | :-------------- | :----------- | :----------- | :----------- | :------------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------- |
| Draft     | Initial stage where the product is being created         | User            | Yes          | Yes          | No           | Product Executive, System Admin                                                                                                  | —                                                     |
| Submitted | Product submitted for review and approval                | User            | Yes          | No           | Yes          | Product Manager, System Admin                                                                                                    | Notify Approval Roles                                 |
| Approved  | Product approved and available for business transactions | User            | Yes          | No           | No           | Product Executive, Product Manager, Inventory Manager, Sales Manager, Purchase Manager, Production Manager, System Admin, Viewer | Available for Sales, Purchase, Inventory & Production |

---

## Workflow - Role Matrix

|Role|Current Stage|Scope|Create|Modify|Delete|Cancel|Next Stage|Rollback Stage|
|:--|:--|:--|:--|:--|:--|:--|:--|:--|
|Product Executive|Draft|Self|Yes|Yes|Yes|No|Submit|No|
|Product Manager|Submitted|Team|No|No|No|Yes|Approve|Draft|
|System Admin|Draft|All|Yes|Yes|Yes|No|Submit|No|
|System Admin|Submitted|All|No|Yes|No|No|Approve|Draft|
|System Admin|Approved|All|No|Yes|No|No|No|No|
|Viewer|Any|All|No|No|No|No|No|No|

---

## Header Fields

| Field         | Description                       | Type      | Required |
| :------------ | :-------------------------------- | :-------- | :------- |
| Product Name  | Name of Product                   | Text      | Yes      |
| Short Name    | Product Code / Short Name         | Text      | Yes      |
| Product Group | Product classification            | Dropdown  | Yes      |
| Brand         | Product Brand                     | Dropdown  | No       |
| UoM           | Unit of Measurement               | Dropdown  | Yes      |
| HSN Code      | GST HSN Code                      | Text      | No       |
| Description   | Product description               | Text Area | No       |
| Operational Status | Operational status of the product | Dropdown  | Yes      |
| Start Date    | Product activation date           | Date      | Yes      |
| End Date      | Product closure date              | Date      | No       |
| Remarks       | Internal remarks                  | Text Area | No       |

---

## Product Summary

| Field         | Description                       | Type  |
| :------------ | :-------------------------------- | :---- |
| Brand         | Associated Brand                  | Text  |
| Product Group | Product Classification            | Text  |
| UoM           | Unit of Measurement               | Text  |
| Current Stage | Current Workflow Stage            | Badge |
| Operational Status | Active / Inactive / Phasing Out / Discontinued  | Badge |

---

## Functions

| Field                       | Description                                                            | Type    |
| :-------------------------- | :--------------------------------------------------------------------- | :------ |
| Sale Enabled                | Indicates whether this Product is available for sale                   | Boolean |
| Minimum Sale Quantity       | Minimum quantity required in a single sale order                       | Number  |
| Maximum Sale Quantity       | Maximum quantity allowed in a single sale order                        | Number  |
| Sale Delivery Lead Time     | Minimum number of days required to deliver this Product for sale       | Number  |
| Purchase Enabled            | Indicates whether this Product is available for purchase               | Boolean |
| Minimum Purchase Quantity   | Minimum quantity required in a single purchase order                   | Number  |
| Maximum Purchase Quantity   | Maximum quantity allowed in a single purchase order                    | Number  |
| Purchase Delivery Lead Time | Minimum number of days required to receive this Product after purchase | Number  |
| Production Enabled          | Indicates whether this Product is produced/manufactured by the company | Boolean |

---

## Inventory Configuration

|Field|Description|Type|
|:--|:--|:--|
|Goods Type|Goods / Service|Dropdown|
|Maintain Stock|Stock Tracking|Boolean|
|Maintain Batch|Batch Tracking|Boolean|
|Maintain Expiry|Expiry Tracking|Boolean|
|Minimum Stock Quantity|Minimum Stock Level|Number|
|Maximum Stock Quantity|Maximum Stock Level|Number|
|Reorder Stock Quantity|Reorder Level|Number|

---

## Packs

|Field|Description|Type|
|:--|:--|:--|
|Pack Name|Product Pack Name|Text|
|Pack Code|Product Pack Code|Text|
|UoM|Unit of Measurement|Dropdown|
|Qty in Pack|Quantity in Pack|Number|
|Remarks|Additional Notes|Text Area|

---

## Media & Attachments

|Field|Description|Type|
|:--|:--|:--|
|Primary Image|Primary Product Image|Image Upload|
|Product Media|Additional Product Images|Image Upload|
|Attachments|Supporting Documents|File Upload|

|Property|Value|
|---|---|
|Supported Formats|PNG, JPG, JPEG|
|Maximum File Size|2 MB|

---

## Reports

| Report                    | Purpose                                                              |
| :------------------------ | :------------------------------------------------------------------- |
| Product Report            | List of all products                                                 |
| Product Group Report      | Products grouped by Product Group                                    |
| Brand Wise Product Report | Products grouped by Brand                                            |
| Product Sales Report      | Sales analysis by product                                            |
| Product Purchase Report   | Purchase analysis by product                                         |
| Product Usage Report      | Shows product usage across sales, purchase, inventory and production |

---

## System Information

|Field|Description|Type|
|:--|:--|:--|
|Created By|User who created the product|User|
|Created On|Record creation date and time|DateTime|
|Modified By|Last user who modified the product|User|
|Modified On|Last modification date and time|DateTime|

---

## Validation Rules

|Field|Rule|
|:--|:--|
|Product Name|Must be unique across all Products|
|Short Name|Must be unique across all Products|
|Sale Quantity|Minimum Sale Quantity cannot exceed Maximum Sale Quantity|
|Purchase Quantity|Minimum Purchase Quantity cannot exceed Maximum Purchase Quantity|
|Stock Quantity|Minimum Stock Quantity cannot exceed Maximum Stock Quantity; Reorder Stock Quantity cannot exceed Maximum Stock Quantity|
|Start Date / End Date|End Date cannot be earlier than Start Date|
|Pack Code|Must be unique within a Product|

---

# Firestore Database Structure

## Collection: `products`

### Document Structure

| Field Name                | Data Type  | Description                                                      |
| :------------------------ | :--------- | :--------------------------------------------------------------- |
| productId                 | string     | Unique Product ID                                                |
| name                      | string     | Product Name                                                     |
| shortName                 | string     | Product Code                                                     |
| productGroupId            | string     | Product Group                                                    |
| brandId                   | string     | Brand                                                            |
| uomId                     | string     | Unit of Measurement                                              |
| hsnCode                   | string     | GST HSN Code                                                     |
| description               | string     | Product Description                                              |
| primaryImage              | string     | Primary Image URL                                                |
| media                     | array<map> | Product Media                                                    |
| packs                     | array<map> | Product Packs                                                    |
| isSale                    | boolean    | Sale Enabled                                                     |
| saleMinOrderQty           | number     | Minimum Sale Quantity                                            |
| saleMaxOrderQty           | number     | Maximum Sale Quantity                                            |
| saleMinDeliveryPeriod     | number     | Sale Delivery Lead Time                                          |
| isPurchase                | boolean    | Purchase Enabled                                                 |
| purchaseMinOrderQty       | number     | Minimum Purchase Quantity                                        |
| purchaseMaxOrderQty       | number     | Maximum Purchase Quantity                                        |
| purchaseMinDeliveryPeriod | number     | Purchase Delivery Lead Time                                      |
| isProduce                 | boolean    | Production Enabled                                               |
| isGoodsType               | boolean    | Goods / Service                                                  |
| maintainStock             | boolean    | Maintain Stock                                                   |
| maintainBatch             | boolean    | Batch Tracking                                                   |
| maintainExpiry            | boolean    | Expiry Tracking                                                  |
| minStockQty               | number     | Minimum Stock Quantity                                           |
| maxStockQty               | number     | Maximum Stock Quantity                                           |
| reorderStockQty           | number     | Reorder Quantity                                                 |
| stage                     | string     | Current Workflow Stage (Draft / Submitted / Approved)            |
| status                    | string     | Operational Status (Active / Inactive / Phasing Out / Discontinued) |
| startDate                 | timestamp  | Activation Date                                                  |
| endDate                   | timestamp  | Closure Date                                                     |
| remarks                   | string     | Additional Notes                                                 |
| createdBy                 | string     | User ID who created the record                                   |
| createdByName             | string     | Creator Name Snapshot                                            |
| createdAt                 | timestamp  | Record Creation Timestamp                                        |
| updatedBy                 | string     | Last Updated User ID                                             |
| updatedByName             | string     | Last Updated User Name Snapshot                                  |
| updatedAt                 | timestamp  | Last Modified Timestamp                                          |
| stageHistory              | array<map> | Complete workflow audit history                                  |
| companyId                 | string     | Company ID                                                       |
| branchId                  | string     | Branch ID                                                        |
| isDeleted                 | boolean    | Soft Delete Flag                                                 |

---

### `packs` Structure

```json
[
  {
    "packName": "Box of 10",
    "packCode": "BOX10",
    "uomId": "uomId1",
    "qtyInPack": 10,
    "remarks": "Standard retail pack"
  }
]
```

---

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

| Name/Label | Data Source              | Component | Tooltip   |
| :--------- | :------------------------ | :-------- | :-------- |
| Stage      | stage                     | Text      | -         |
| Set At     | actionAt                  | Text      | -         |
| Set By     | actionBy / actionByName   | Avatar    | User name |
| Remarks    | remarks                   | Text      | -         |

