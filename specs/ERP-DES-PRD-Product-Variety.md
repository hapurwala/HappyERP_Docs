# Module Design Details

A Product Variety represents a specific variant of a product defined by a unique combination of product attribute values (e.g., colour, size, grade). Each variety belongs to exactly one product. Varieties allow businesses to manage distinct SKUs under a single product master.

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: **Product Management**
- **Module**: **Product Variety**

---

## Roles

| Role              | Purpose                                     |
| :---------------- | :------------------------------------------ |
| Product Executive | Creates and manages product variety records |
| Product Manager   | Reviews and approves product variety records |
| System Admin      | Full system configuration and control       |

## Additional Roles

| Role    | Purpose                                   |
| :------ | :---------------------------------------- |
| Auditor | Read-only access for audit and compliance |


---

## Workflow Stages

| Stage     | Description                                              | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View To Roles                                                                                                                       | System Action              |
| :-------- | :------------------------------------------------------- | :-------------- | :----------- | :----------- | :----------- | :---------------------------------------------------------------------------------------------------------------------------------------- | :------------------------- |
| Draft     | Initial stage where the product variety is being created | User            | Yes          | Yes          | No           | Product Executive, System Admin                                                                                                           | —                          |
| Submitted | Product variety submitted for review and approval        | User            | No           | No           | Yes          | Product Executive, Product Manager, System Admin                                                                                          | Notify Product Manager     |
| Approved  | Product variety approved and available for transactions  | User            | Yes          | No           | No           | Product Executive, Product Manager, Auditor, System Admin | Available for Transactions |

---

## Workflow - Role Matrix

| Role              | Current Stage | Scope | Create | Modify | Delete | Cancel | Next Stage | Rollback Stage |
| :---------------- | :------------ | :---- | :----- | :----- | :----- | :----- | :--------- | :------------- |
| Product Executive | Draft         | Self  | Yes    | Yes    | Yes    | No     | Submit     | No             |
| Product Manager   | Submitted     | Team  | No     | No     | No     | Yes    | Approve    | Yes            |
| Product Manager   | Approved      | Self  | No     | No     | No     | No     | No         | No             |
| System Admin      | Draft         | All   | Yes    | Yes    | Yes    | No     | Submit     | No             |
| System Admin      | Submitted     | All   | No     | Yes    | No     | Yes    | Approve    | Yes            |
| System Admin      | Approved      | All   | No     | Yes    | No     | No     | No         | No             |

---

## Header Fields

| Field              | Description                                                         | Type                | Required |
| :----------------- | :------------------------------------------------------------------ | :------------------ | :------- |
| Product            | Product for which this variety is created                           | Dropdown            | Yes      |
| Dynamic Attributes | Attribute fields generated based on the Product's attribute configuration — each attribute shows a dropdown of its allowed values | Dynamic Dropdowns | Yes |
| Start Date         | Effective start date of this variety                                | Date                | Yes      |
| End Date           | Date on which this variety is closed                                | Date                | No       |
| Remarks            | Additional notes                                                    | Text Area           | No       |

> Dynamic Attributes are reloaded when Product is changed. The number and type of attribute fields depend on the attributes configured for the selected product.

---

## Media

| Field         | Description                    | Type         |
| :------------ | :----------------------------- | :----------- |
| Primary Image | Primary image of this variety  | Image Upload |

| Property                   | Value          |
| :-------------------------- | :------------- |
| Supported Formats           | PNG, JPG, JPEG |
| Maximum File Size per File  | 2 MB           |

---

## Validation Rules

| Field                    | Rule                                                                                                          |
| :----------------------- | :------------------------------------------------------------------------------------------------------------ |
| Product + Attributes     | The combination of Product and all Attribute Values must be unique — no two varieties of the same product can have identical attribute values |
| Attribute Values         | All required attribute fields must be filled before submitting                                                |
| Start Date / End Date    | End Date cannot be earlier than Start Date                                                                    |

---

## Reports

| Report                    | Purpose                                                  |
| :------------------------ | :------------------------------------------------------- |
| Product Variety List      | All varieties with product, attributes, stage and status |
| Product Wise Varieties    | Varieties grouped by product                             |
| Pending Approval Report   | Varieties currently in Submitted stage                   |

---

## System Information

| Field       | Description                               | Type     |
| :---------- | :---------------------------------------- | :------- |
| Created By  | User who created the product variety      | User     |
| Created On  | Record creation date and time             | DateTime |
| Modified By | Last user who modified the product variety | User    |
| Modified On | Last modification date and time           | DateTime |

---

# Firestore Database Structure

## Collection: `product_varieties`

### Document Structure

| Field Name  | Data Type  | Description                                                   |
| :---------- | :--------- | :------------------------------------------------------------ |
| varietyId   | string     | Unique Variety ID                                             |
| productId   | string     | Reference to the parent product                               |
| attributes  | array<map> | Attribute values that define this variety                     |
| primaryImage | string    | Primary Image URL                                             |
| stage       | string     | Current Workflow Stage — `Draft` \| `Submitted` \| `Approved` |
| startDate   | timestamp  | Variety activation date                                       |
| endDate     | timestamp  | Variety closure date                                          |
| remarks     | string     | Internal remarks                                              |
| createdBy   | string     | User ID of creator                                            |
| createdByName | string   | Creator name snapshot                                         |
| createdAt   | timestamp  | Record creation timestamp                                     |
| updatedBy   | string     | User ID of last modifier                                      |
| updatedByName | string   | Last modifier name snapshot                                   |
| updatedAt   | timestamp  | Last modification timestamp                                   |
| stageHistory | array<map> | Complete workflow stage audit history                        |
| companyId   | string     | Company ID                                                    |
| isDeleted   | boolean    | Soft delete flag                                              |

---

### `attributes` Structure

```json
[
  {
    "attributeId": "attr_color",
    "attributeName": "Colour",
    "valueId": "val_red",
    "valueName": "Red"
  },
  {
    "attributeId": "attr_size",
    "attributeName": "Size",
    "valueId": "val_large",
    "valueName": "Large"
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
    "remarks": "Variety created"
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

Data Source: `stageHistory` array

| Name/Label | Data Source             | Component | Tooltip   |
| :--------- | :---------------------- | :-------- | :-------- |
| Stage      | stage                   | Text      | -         |
| Set At     | actionAt                | Text      | -         |
| Set By     | actionBy / actionByName | Avatar    | User name |
| Remarks    | remarks                 | Text      | -         |
