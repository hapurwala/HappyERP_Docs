# Module Design Details

Unit of Measurement (UoM) defines the standard unit used to measure and track product quantities across all business transactions. Each UoM belongs to a measurement class (e.g., Weight, Volume, Length) and specifies the number of decimal places for quantity precision.

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: **Product Management**
- **Module**: **Unit of Measurement (UoM)**

---

## Roles

| Role              | Purpose                             |
| :---------------- | :---------------------------------- |
| Product Executive | Creates and manages UoM records     |
| Product Manager   | Reviews and approves UoM records    |
| System Admin      | Full system configuration and control |

## Additional Roles

| Role               | Purpose                                                       |
| :----------------- | :------------------------------------------------------------ |
| Auditor            | Read-only access for audit and compliance                     |


---

## Workflow Stages

| Stage     | Description                                       | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View To Roles                                                                                                                                                          | System Action            |
| :-------- | :------------------------------------------------ | :-------------- | :----------- | :----------- | :----------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------- |
| Draft     | Initial stage where the UoM is being created      | User            | Yes          | Yes          | No           | Product Executive, System Admin                                                                                                                                              | —                        |
| Submitted | UoM submitted for review and approval             | User            | No           | No           | Yes          | Product Executive, Product Manager, System Admin                                                                                                                             | Notify Product Manager   |
| Approved  | UoM approved and available for use across modules | User            | Yes          | No           | No           | Product Executive, Product Manager, Sales Manager, Purchase Manager, Inventory Manager, Production Manager, Accounts Executive, Store Manager, Auditor, Viewer, System Admin | Available Across Modules |

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
| Viewer            | Any           | All   | No     | No     | No     | No     | No         | No             |

---

## Header Fields

| Field              | Description                                                                    | Type      | Required |
| :----------------- | :----------------------------------------------------------------------------- | :-------- | :------- |
| Name               | Name of the Unit of Measurement                                                | Text      | Yes      |
| Short Name         | Code / Short Name of the UoM (e.g., KG, LTR, PCS)                             | Text      | Yes      |
| Class              | Measurement category (e.g., Weight, Volume, Length, Time, Quantity)            | Text      | Yes      |
| Number of Decimals | Number of decimal places for quantity precision                                | Number    | Yes      |
| Start Date         | Date from which the UoM becomes active                                         | Date      | Yes      |
| End Date           | Date on which the UoM is closed                                                | Date      | No       |
| Remarks            | Internal remarks                                                               | Text Area | No       |

---

## Validation Rules

| Field                 | Rule                                                                    |
| :-------------------- | :---------------------------------------------------------------------- |
| Name                  | Must be unique across all UoMs                                          |
| Short Name            | Must be unique across all UoMs                                          |
| Number of Decimals    | Must be 0 or a positive integer                                         |
| Start Date / End Date | End Date cannot be earlier than Start Date                              |
| Approved UoM in use   | System Admin cannot delete an Approved UoM that is referenced by active products |

---

## Reports

| Report          | Purpose                                           |
| :-------------- | :------------------------------------------------ |
| UoM List        | All UoMs with class, stage and active status      |
| UoM Usage Report | Products and transactions using each UoM         |

---

## System Information

| Field       | Description                     | Type     |
| :---------- | :------------------------------ | :------- |
| Created By  | User who created the UoM        | User     |
| Created On  | Record creation date and time   | DateTime |
| Modified By | Last user who modified the UoM  | User     |
| Modified On | Last modification date and time | DateTime |

---

# Firestore Database Structure

## Collection: `uoms`

### Document Structure

| Field Name    | Data Type | Description                                                   |
| :------------ | :-------- | :------------------------------------------------------------ |
| uomId         | string    | Unique UoM ID                                                 |
| name          | string    | UoM Name                                                      |
| shortName     | string    | UoM Code / Short Name                                         |
| class         | string    | Measurement class (e.g., Weight, Volume, Length)              |
| decimalPlaces | number    | Number of decimal places for quantity precision               |
| stage         | string    | Current Workflow Stage — `Draft` \| `Submitted` \| `Approved` |
| startDate     | timestamp | UoM activation date                                           |
| endDate       | timestamp | UoM closure date                                              |
| remarks       | string    | Internal remarks                                              |
| createdBy     | string    | User ID of creator                                            |
| createdByName | string    | Creator name snapshot                                         |
| createdAt     | timestamp | Record creation timestamp                                     |
| updatedBy     | string    | User ID of last modifier                                      |
| updatedByName | string    | Last modifier name snapshot                                   |
| updatedAt     | timestamp | Last modification timestamp                                   |
| stageHistory  | array<map> | Complete workflow stage audit history                        |
| companyId     | string    | Company ID                                                    |
| isDeleted     | boolean   | Soft delete flag                                              |

---

### `stageHistory` Structure

```json
[
  {
    "stage": "Draft",
    "actionBy": "userId",
    "actionByName": "Rahul",
    "actionAt": "Timestamp",
    "remarks": "UoM created"
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
