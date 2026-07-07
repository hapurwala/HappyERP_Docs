# Module Design Details

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: **Product Management**
- **Module**: **Product Attribute**

---

## Roles

| Role              | Purpose                                              |
| :---------------- | :--------------------------------------------------- |
| Product Executive | Creates and manages product attribute records        |
| Product Manager   | Reviews and approves product attribute records       |
| System Admin      | Full system configuration and administrative control |

## Additional Roles

| Role         | Purpose                                              |
| :----------- | :--------------------------------------------------- |
| Auditor      | Read-only access for audit and compliance            |


---

## Workflow Stages

| Stage     | Description                                                   | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View To Roles                                               | System Action                            |
| :-------- | :------------------------------------------------------------ | :-------------- | :----------- | :----------- | :----------- | :---------------------------------------------------------------- | :--------------------------------------- |
| Draft     | Initial stage where the attribute is being created            | User            | Yes          | Yes          | No           | Product Executive, System Admin                                   | —                                        |
| Submitted | Attribute submitted for review and approval                   | User            | No           | No           | Yes          | Product Executive, Product Manager, System Admin                  | Notify Product Manager                   |
| Approved  | Attribute approved and available for use in product varieties | User            | Yes          | No           | No           | Product Executive, Product Manager, Auditor, Viewer, System Admin | Available for Product Variety Assignment |

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

| Field        | Description                                                                                       | Type      | Required |
| :----------- | :------------------------------------------------------------------------------------------------ | :-------- | :------- |
| Name         | Name of the Product Attribute                                                                     | Text      | Yes      |
| Short Name   | Code / Short Name of the Product Attribute                                                        | Text      | Yes      |
| Is Mandatory | Indicates whether this Attribute is mandatory while creating Product Varieties                    | Boolean   | No       |
| Sequence     | Display order of this Attribute among all Product Attributes                                      | Number    | Yes      |
| End Date     | Date when the Product Attribute is closed. Attribute is active from creation date until End Date. | Date      | No       |
| Remarks      | Internal remarks                                                                                  | Text Area | No       |

---

## Applicability

| Field      | Description                                                                                             | Type         |
| :--------- | :------------------------------------------------------------------------------------------------------ | :----------- |
| Product(s) | Products to which this Attribute is applicable. An attribute may be applicable to one or more products. | Multi Select |

---

## Values

| Field      | Description                                                        | Type   | Required |
| :--------- | :----------------------------------------------------------------- | :----- | :------- |
| Value      | Value of the Product Attribute                                     | Text   | Yes      |
| Sequence   | Display order of this Value within the Attribute                   | Number | Yes      |
| Start Date | Date from which this value is effective                            | Date   | No       |
| End Date   | Date until which this value is effective                           | Date   | No       |


---

## Reports

| Report                                | Purpose                                                  |
| :------------------------------------ | :------------------------------------------------------- |
| Product Attribute Report              | List of all product attributes                           |
| Attribute Wise Product Mapping Report | Products mapped to each Attribute                        |
| Attribute Value Usage Report          | Shows usage of Attribute Values across Product Varieties |

---

## System Information

| Field       | Description                          | Type     |
| :---------- | :----------------------------------- | :------- |
| Created By  | User who created the attribute       | User     |
| Created On  | Record creation date and time        | DateTime |
| Modified By | Last user who modified the attribute | User     |
| Modified On | Last modification date and time      | DateTime |

---

## Validation Rules

| Field         | Rule                                                                                                                     |
| :------------ | :----------------------------------------------------------------------------------------------------------------------- |
| Name          | Must be unique across all Product Attributes                                                                             |
| Short Name    | Must be unique across all Product Attributes                                                                             |
| Sequence      | Must be unique across all Product Attributes                                                                             |
| Value         | Must be unique within an Attribute                                                                                       |
| Value Sequence | Cannot duplicate among Values within the same Attribute                                                                 |
| Is Mandatory  | A mandatory Attribute must have at least one Active Value                                                                |
| Is Mandatory  | Changing to Yes is enforced only on new Product Varieties — existing approved varieties are grandfathered                |
| Applicability | Removing a Product from applicability is blocked if active approved varieties reference this Attribute for that Product  |
| End Date      | Cannot be set to a date earlier than today                                                                               |

---

# Firestore Database Structure

## Collection: `product_attributes`

### Document Structure

| Field Name         | Data Type  | Description                                                   |
| :----------------- | :--------- | :------------------------------------------------------------ |
| attributeId        | string     | Unique Product Attribute ID                                   |
| name               | string     | Attribute Name                                                |
| shortName          | string     | Attribute Code                                                |
| isMandatory        | boolean    | Mandatory while creating Product Varieties                    |
| sequence           | number     | Display Order among all attributes                            |
| endDate            | timestamp  | Closure Date (null if not closed)                             |
| stage              | string     | Current Workflow Stage — `Draft` \| `Submitted` \| `Approved` |
| applicableProducts | array<map> | Products this Attribute applies to                            |
| values             | array<map> | Attribute Values                                              |
| remarks            | string     | Additional Notes                                              |
| createdBy          | string     | User ID who created the record                                |
| createdByName      | string     | Creator Name Snapshot                                         |
| createdAt          | timestamp  | Record Creation Timestamp                                     |
| updatedBy          | string     | Last Updated User ID                                          |
| updatedByName      | string     | Last Updated User Name Snapshot                               |
| updatedAt          | timestamp  | Last Modified Timestamp                                       |
| stageHistory       | array<map> | Complete workflow audit history                               |
| companyId          | string     | Company ID                                                    |
| isDeleted          | boolean    | Soft Delete Flag                                              |

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

| Name/Label | Data Source             | Component | Tooltip   |
| :--------- | :---------------------- | :-------- | :-------- |
| Stage      | stage                   | Text      | -         |
| Set At     | actionAt                | Text      | -         |
| Set By     | actionBy / actionByName | Avatar    | User name |
| Remarks    | remarks                 | Text      | -         |
