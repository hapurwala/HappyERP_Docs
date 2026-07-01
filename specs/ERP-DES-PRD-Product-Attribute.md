# Module Design Details

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: **Product Management**
- **Module**: **Product Attribute**

---

## Roles

|Role|Purpose|
|:--|:--|
|Product Executive|Creates and manages product attribute records|
|Product Manager|Reviews and approves product attribute records|

## Additional Roles

|Role|Purpose|
|:--|:--|
|Sales Manager|Uses attributes for product variety selection in sales|
|Production Manager|Uses attributes for product variety in production planning|
|Auditor|Read-only access for audit and compliance|
|System Admin|Full system configuration and administrative control|

---

## Workflow Stages

| Stage     | Description                                                | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View To Roles                                 | System Action                            |
| :-------- | :---------------------------------------------------------- | :--------------- | :----------- | :----------- | :----------- | :--------------------------------------------------- | :----------------------------------------- |
| Draft     | Initial stage where the attribute is being created          | User              | Yes          | Yes          | No           | Product Executive, System Admin                              | —                                          |
| Submitted | Attribute submitted for review and approval                 | User              | Yes          | No           | Yes          | Product Manager, System Admin                                | Notify Approval Roles                      |
| Approved  | Attribute approved and available for use in product varieties | User              | Yes          | No           | No           | Product Executive, Product Manager, Sales Manager, Production Manager, System Admin, Viewer | Available for Product Variety Assignment |

---

## Workflow - Role Matrix

| Role              | Current Stage | Scope | Create | Modify | Delete | Cancel | Next Stage | Rollback Stage |
| :---------------- | :------------ | :---- | :----- | :----- | :----- | :----- | :--------- | :------------- |
| Product Executive | Draft         | Self  | Yes    | Yes    | Yes    | No     | Submit     | No             |
| Product Manager   | Submitted     | Team  | No     | No     | No     | Yes    | Approve    | Draft          |
| System Admin      | Draft         | All   | Yes    | Yes    | Yes    | No     | Submit     | No             |
| System Admin      | Submitted     | All   | No     | Yes    | No     | No     | Approve    | Draft          |
| System Admin      | Approved      | All   | No     | Yes    | No     | No     | No         | No             |
| Viewer            | Any           | All   | No     | No     | No     | No     | No         | No             |

---

## Header Fields

| Field        | Description                                                                    | Type      | Required |
| :----------- | :----------------------------------------------------------------------------- | :-------- | :------- |
| Name         | Name of the Product Attribute                                                  | Text      | Yes      |
| Short Name   | Code / Short Name of the Product Attribute                                     | Text      | Yes      |
| Is Mandatory | Indicates whether this Attribute is mandatory while creating Product Varieties | Boolean   | No       |
| Sequence     | Display order of this Attribute                                                | Number    | Yes      |
| End Date     | Date when the Product Attribute was closed                                     | Date      | No       |
| Remarks      | Internal remarks                                                               | Text Area | No       |

---

## Applicability

| Field      | Description                                                                                             | Type         |
| :--------- | :------------------------------------------------------------------------------------------------------ | :----------- |
| Product(s) | Products to which this Attribute is applicable. An attribute may be applicable to one or more products. | Multi Select |

---

## Values

|Field|Description|Type|
|:--|:--|:--|
|Value|Value of the Product Attribute|Text|
|Sequence|Display order of this Value within the Attribute|Number|
|Start Date|Date from which this value is effective|Date|
|End Date|Date until which this value is effective|Date|
|Status|Current status of the Attribute Value (Active / Inactive / Closed)|Badge|

---

## Reports

|Report|Purpose|
|:--|:--|
|Product Attribute Report|List of all product attributes|
|Attribute Wise Product Mapping Report|Products mapped to each Attribute|
|Attribute Value Usage Report|Shows usage of Attribute Values across Product Varieties|

---

## System Information

|Field|Description|Type|
|:--|:--|:--|
|Created By|User who created the attribute|User|
|Created On|Record creation date and time|DateTime|
|Modified By|Last user who modified the attribute|User|
|Modified On|Last modification date and time|DateTime|

---

## Validation Rules

|Field|Rule|
|:--|:--|
|Name|Must be unique across all Product Attributes|
|Short Name|Must be unique across all Product Attributes|
|Value Sequence|Cannot duplicate among Values within the same Attribute|
|Value|Must be unique within an Attribute|
|Is Mandatory|A mandatory Attribute must have at least one Active Value|
|End Date|Cannot be earlier than the Attribute's creation date|

---

# Firestore Database Structure

## Collection: `product_attributes`

### Document Structure

| Field Name          | Data Type  | Description                                       |
| :------------------- | :--------- | :------------------------------------------------- |
| attributeId          | string     | Unique Product Attribute ID                        |
| name                 | string     | Attribute Name                                      |
| shortName            | string     | Attribute Code                                      |
| isMandatory          | boolean    | Mandatory while creating Product Varieties          |
| sequence             | number     | Display Order                                       |
| endDate              | timestamp  | Closure Date                                        |
| stage                | string     | Current Workflow Stage (Draft / Submitted / Approved) |
| applicableProducts   | array<map> | Products this Attribute applies to                  |
| values               | array<map> | Attribute Values                                    |
| remarks              | string     | Additional Notes                                    |
| createdBy            | string     | User ID who created the record                      |
| createdByName        | string     | Creator Name Snapshot                                |
| createdAt            | timestamp  | Record Creation Timestamp                            |
| updatedBy            | string     | Last Updated User ID                                 |
| updatedByName        | string     | Last Updated User Name Snapshot                      |
| updatedAt            | timestamp  | Last Modified Timestamp                              |
| stageHistory         | array<map> | Complete workflow audit history                      |
| companyId            | string     | Company ID                                           |
| branchId             | string     | Branch ID                                            |
| isDeleted            | boolean    | Soft Delete Flag                                     |

---

### `applicableProducts` Structure

```json
[
  { "productId": "productId1" },
  { "productId": "productId2" }
]
```

---

### `values` Structure

```json
[
  {
    "value": "Red",
    "sequence": 1,
    "startDate": "Timestamp",
    "endDate": null,
    "status": "Active"
  },
  {
    "value": "Blue",
    "sequence": 2,
    "startDate": "Timestamp",
    "endDate": null,
    "status": "Active"
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
    "remarks": "Attribute created"
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

| Name/Label | Data Source            | Component | Tooltip   |
| :--------- | :---------------------- | :-------- | :-------- |
| Stage      | stage                   | Text      | -         |
| Set At     | actionAt                | Text      | -         |
| Set By     | actionBy / actionByName | Avatar    | User name |
| Remarks    | remarks                 | Text      | -         |
