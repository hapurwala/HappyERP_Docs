# Module Design Details

Product Groups allow businesses to classify and organise products into a hierarchical structure. A product group can be nested under a parent group, enabling multi-level classification. The UoM defined at the group level serves as the default unit for all products within that group.

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: **Product Management**
- **Module**: **Product Group**

---

## Roles

| Role              | Purpose                                    |
| :---------------- | :----------------------------------------- |
| Product Executive | Creates and manages product group records  |
| Product Manager   | Reviews and approves product group records |
| System Admin      | Full system configuration and control      |

## Additional Roles

| Role    | Purpose                                   |
| :------ | :---------------------------------------- |
| Auditor | Read-only access for audit and compliance |


---

## Workflow Stages

| Stage     | Description                                             | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View To Roles                                                                              | System Action                    | Rollback Stage |
| :-------- | :------------------------------------------------------ | :-------------- | :----------- | :----------- | :----------- | :----------------------------------------------------------------------------------------------- | :------------------------------- | :------------- |
| Draft     | Initial stage where the product group is being created  | User            | Yes          | Yes          | No           | Product Executive, System Admin                                                                  | —                                | —              |
| Submitted | Product group submitted for review and approval         | User            | No           | No           | Yes          | Product Executive, Product Manager, System Admin                                                 | Notify Product Manager           | Draft          |
| Approved  | Product group approved and available for product mapping | User           | Yes          | No           | No           | Product Executive, Product Manager, Sales Manager, Purchase Manager, Inventory Manager, Production Manager, Auditor, Viewer, System Admin | Available for Product Assignment | —  |

---

## Workflow - Role Matrix

| Role              | Current Stage | Scope | Create | Modify | Delete | Cancel | Next Stage | Rollback Stage |
| :---------------- | :------------ | :---- | :----- | :----- | :----- | :----- | :--------- | :------------- |
| Product Executive | Draft         | Self  | Yes    | Yes    | Yes    | No     | Submit     | No             |
| Product Manager   | Submitted     | Team  | No     | No     | No     | Yes    | Approve    | Yes            |
| Product Manager   | Approved      | Team  | No     | No     | No     | Yes    | No         | No             |
| System Admin      | Draft         | All   | Yes    | Yes    | Yes    | No     | Submit     | No             |
| System Admin      | Submitted     | All   | No     | Yes    | No     | Yes    | Approve    | Yes            |
| System Admin      | Approved      | All   | No     | Yes    | No     | No     | No         | No             |
| Viewer            | Any           | All   | No     | No     | No     | No     | No         | No             |

---

## Header Fields

| Field       | Description                                           | Type      | Required |
| :---------- | :---------------------------------------------------- | :-------- | :------- |
| Name        | Name of the Product Group                             | Text      | Yes      |
| Short Name  | Code / Short Name of the Product Group                | Text      | Yes      |
| UoM         | Default Unit of Measurement for products in this group | Dropdown | Yes      |
| Comes Under | Parent Product Group (for hierarchical classification) | Dropdown | No       |
| Start Date  | Date when the Product Group becomes active            | Date      | Yes      |
| End Date    | Date when the Product Group is closed                 | Date      | No       |
| Remarks     | Internal remarks                                      | Text Area | No       |

---

## Media

| Field         | Description                                | Type               |
| :------------ | :----------------------------------------- | :----------------- |
| Primary Image | Primary image of the Product Group         | Image Upload       |
| Other Images  | Additional images of the Product Group     | Multiple Image Upload |

| Property                  | Value          |
| :------------------------ | :------------- |
| Supported Formats         | PNG, JPG, JPEG |
| Maximum File Size per File | 2 MB          |

---

## Validation Rules

| Field                 | Rule                                                                                          |
| :-------------------- | :-------------------------------------------------------------------------------------------- |
| Name                  | Must be unique across all Product Groups                                                      |
| Short Name            | Must be unique across all Product Groups                                                      |
| Start Date / End Date | End Date cannot be earlier than Start Date                                                    |
| Comes Under           | A Product Group cannot be set as its own parent (direct or indirect circular reference)       |

---

## Reports

| Report                      | Purpose                                              |
| :-------------------------- | :--------------------------------------------------- |
| Product Group List          | All product groups with stage and status             |
| Product Group Hierarchy     | Tree view of parent-child product group structure    |
| Product Group Wise Products | Products grouped by their product group              |

---

## System Information

| Field       | Description                              | Type     |
| :---------- | :--------------------------------------- | :------- |
| Created By  | User who created the product group       | User     |
| Created On  | Record creation date and time            | DateTime |
| Modified By | Last user who modified the product group | User     |
| Modified On | Last modification date and time          | DateTime |

---

# Firestore Database Structure

## Collection: `product_groups`

### Document Structure

| Field Name     | Data Type    | Description                                                  |
| :------------- | :----------- | :----------------------------------------------------------- |
| productGroupId | string       | Unique Product Group ID                                      |
| name           | string       | Product Group Name                                           |
| shortName      | string       | Product Group Code / Short Name                              |
| uomId          | string       | Default UoM reference                                        |
| parentGroupId  | string       | Parent Product Group ID (null if top-level)                  |
| primaryImage   | string       | Primary Image URL                                            |
| otherImages    | array<string> | Additional Image URLs                                       |
| stage          | string       | Current Workflow Stage — `Draft` \| `Submitted` \| `Approved` |
| startDate      | timestamp    | Group activation date                                        |
| endDate        | timestamp    | Group closure date                                           |
| remarks        | string       | Internal remarks                                             |
| createdBy      | string       | User ID of creator                                           |
| createdByName  | string       | Creator name snapshot                                        |
| createdAt      | timestamp    | Record creation timestamp                                    |
| updatedBy      | string       | User ID of last modifier                                     |
| updatedByName  | string       | Last modifier name snapshot                                  |
| updatedAt      | timestamp    | Last modification timestamp                                  |
| stageHistory   | array<map>   | Complete workflow stage audit history                        |
| companyId      | string       | Company ID                                                   |
| isDeleted      | boolean      | Soft delete flag                                             |

---

### `stageHistory` Structure

```json
[
  {
    "stage": "Draft",
    "actionBy": "userId",
    "actionByName": "Rahul",
    "actionAt": "Timestamp",
    "remarks": "Product group created"
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
