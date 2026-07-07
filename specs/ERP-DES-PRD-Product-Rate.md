# Module Design Details

Product Rate allows businesses to define and manage price lists for products and product packs. A Rate List is created for a specific process type (Sale, Purchase, or Stock Transfer), currency, and optionally scoped to specific customers, suppliers, or agents. Multiple rates can be defined per product with effective dates, enabling full rate history tracking.

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: **Product Management**
- **Module**: **Product Rate**

---

## Roles

| Role              | Purpose                                              |
| :---------------- | :--------------------------------------------------- |
| Product Executive | Creates and manages product rate lists               |
| Product Manager   | Reviews and approves product rate lists              |
| System Admin      | Full system configuration and control                |

## Additional Roles

| Role               | Purpose                                                  |
| :----------------- | :------------------------------------------------------- |
| Auditor            | Read-only access for audit and compliance                |


---

## Currency Behaviour

- Each Rate List is associated with exactly one Currency.
- All product rates within the Rate List are maintained in that currency.
- Currency cannot be changed once Rate List Details (product rates) have been defined.
- Exchange Rate management is handled separately outside this module.

---

## Workflow Stages

| Stage     | Description                                                | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View To Roles                                                                                                    | System Action              |
| :-------- | :--------------------------------------------------------- | :-------------- | :----------- | :----------- | :----------- | :--------------------------------------------------------------------------------------------------------------------- | :------------------------- |
| Draft     | Initial stage where the rate list is being created         | User            | Yes          | Yes          | No           | Product Executive, System Admin                                                                                        | —                          |
| Submitted | Rate list submitted for review and approval                | User            | No           | No           | Yes          | Product Executive, Product Manager, System Admin                                                                       | Notify Product Manager     |
| Approved  | Rate list approved and available for business transactions | User            | Yes          | No           | No           | Product Executive, Product Manager, Sales Manager, Purchase Manager, Accounts Executive, Auditor, Viewer, System Admin | Available for Transactions |

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

| Field                  | Description                                                                    | Type                  | Required |
| :--------------------- | :----------------------------------------------------------------------------- | :-------------------- | :------- |
| Name                   | Name of the Rate List                                                          | Text                  | Yes      |
| Short Name             | Code / Short Name of the Rate List                                             | Text                  | Yes      |
| Rate Type              | Type of rate maintained in this list                                           | Dropdown              | Yes      |
| Process Type           | Business process for which this rate list applies                              | Dropdown              | Yes      |
| Currency               | Currency applicable for all rates in this list                                 | Dropdown              | Yes      |
| Customer / Supplier Type | Scope this rate list to a customer type or supplier type (based on Process Type) | Dropdown            | No       |
| Supplier(s)            | Specific suppliers for which this rate list applies (Purchase only)            | Multi-Select Dropdown | No       |
| Agent(s)               | Agents for which this rate list applies                                        | Multi-Select Dropdown | No       |
| Start Date             | Date from which this rate list becomes applicable                              | Date                  | Yes      |
| End Date               | Date on which this rate list is closed                                         | Date                  | No       |
| Remarks                | Internal remarks                                                               | Text Area             | No       |

**Rate Type Values:** `MRP` | `Rate`

**Process Type Values:** `Sale` | `Purchase` | `Stock Transfer`

> Customer / Supplier Type is shown based on Process Type:
> - Sale → Customer Type or specific Customer
> - Purchase → Supplier Type or specific Supplier
> - Stock Transfer → Not applicable

---

## Rate Details

Each Rate List contains line items defining the rate for each product or product pack.

| Field        | Description                                              | Type     | Required |
| :----------- | :------------------------------------------------------- | :------- | :------- |
| Product      | Product for which the rate is defined                    | Dropdown | Yes      |
| Product Pack | Specific product pack (if applicable)                    | Dropdown | No       |
| Rate         | Price per unit in the Rate List currency                 | Number   | Yes      |
| Start Date   | Effective date from which this rate applies              | Date     | Yes      |

> Each Product / Product Pack can have multiple rates with different effective dates (rate history). Only one rate is active at any given point in time.

---

## Rate History Timeline

The Rate History Timeline popup shows the complete history of rates for a specific Product / Product Pack within a Rate List.

| Field           | Description                                    | Type   |
| :-------------- | :--------------------------------------------- | :----- |
| Rate            | Rate value in the Rate List currency           | Number |
| Applicable From | Effective start date of this rate              | Date   |
| Applicable Upto | Effective end date of this rate (auto-derived) | Date   |
| Status          | Past / Current / Future                        | Badge  |

New rates can be added directly from this popup by specifying a Rate and an Applicable From date.

> Adding a new rate with a future date does not remove existing rates — it extends the timeline forward.
> Two rates for the same Product / Pack in the same Rate List cannot have overlapping effective date ranges.

---

## Validation Rules

| Field                     | Rule                                                                                           |
| :------------------------ | :--------------------------------------------------------------------------------------------- |
| Name                      | Must be unique across all Rate Lists                                                           |
| Short Name                | Must be unique across all Rate Lists                                                           |
| Start Date / End Date     | End Date cannot be earlier than Start Date                                                     |
| Currency                  | Cannot be changed once Rate List Details (product rates) exist                                 |
| Rate                      | Must be a positive number                                                                      |
| Rate Effective Date Overlap | Two rates for the same Product / Pack in the same Rate List cannot have overlapping date ranges |
| Applicable From (new rate) | Must not conflict with an existing rate's effective date range for the same Product / Pack    |

---

## Reports

| Report                    | Purpose                                                       |
| :------------------------ | :------------------------------------------------------------ |
| Rate List Register        | All rate lists with type, process, currency, stage and status |
| Product Rate Report       | Current effective rates for all products across rate lists    |
| Rate History Report       | Full rate history for a product / pack within a rate list     |
| Expired Rate Lists Report | Rate lists past their End Date                                |

---

## System Information

| Field       | Description                           | Type     |
| :---------- | :------------------------------------ | :------- |
| Created By  | User who created the rate list        | User     |
| Created On  | Record creation date and time         | DateTime |
| Modified By | Last user who modified the rate list  | User     |
| Modified On | Last modification date and time       | DateTime |

---

# Firestore Database Structure

## Collection: `rate_lists`

### Document Structure

| Field Name  | Data Type     | Description                                                   |
| :---------- | :------------ | :------------------------------------------------------------ |
| rateListId  | string        | Unique Rate List ID                                           |
| name        | string        | Rate List Name                                                |
| shortName   | string        | Rate List Code / Short Name                                   |
| rateType    | string        | Type of rate — `MRP` \| `Rate`                                |
| processType | string        | Applicable process — `Sale` \| `Purchase` \| `Stock Transfer` |
| currencyId  | string        | Currency reference                                            |
| partyType   | string        | Customer Type / Supplier Type scope                           |
| supplierIds | array<string> | Specific Supplier references                                  |
| agentIds    | array<string> | Agent references                                              |
| stage       | string        | Current Workflow Stage — `Draft` \| `Submitted` \| `Approved` |
| startDate   | timestamp     | Rate List activation date                                     |
| endDate     | timestamp     | Rate List closure date                                        |
| remarks     | string        | Internal remarks                                              |
| createdBy   | string        | User ID of creator                                            |
| createdByName | string      | Creator name snapshot                                         |
| createdAt   | timestamp     | Record creation timestamp                                     |
| updatedBy   | string        | User ID of last modifier                                      |
| updatedByName | string      | Last modifier name snapshot                                   |
| updatedAt   | timestamp     | Last modification timestamp                                   |
| stageHistory | array<map>   | Complete workflow stage audit history                         |
| companyId   | string        | Company ID                                                    |
| isDeleted   | boolean       | Soft delete flag                                              |

---

## Sub-collection: `rate_list_details`

Each Rate List has a sub-collection of rate detail records — one per Product / Product Pack entry.

| Field Name    | Data Type  | Description                                            |
| :------------ | :--------- | :----------------------------------------------------- |
| detailId      | string     | Unique Rate Detail ID                                  |
| rateListId    | string     | Reference to the parent Rate List                      |
| productId     | string     | Product reference                                      |
| productPackId | string     | Product Pack reference (null if not pack-specific)     |
| rateHistory   | array<map> | All historical rates with effective dates for this entry |
| companyId     | string     | Company ID                                             |
| isDeleted     | boolean    | Soft delete flag                                       |

---

### `rateHistory` Structure

```json
[
  {
    "rate": 150.00,
    "startDate": "Timestamp",
    "endDate": "Timestamp",
    "status": "Past",
    "addedBy": "userId",
    "addedByName": "Rahul",
    "addedAt": "Timestamp"
  },
  {
    "rate": 175.00,
    "startDate": "Timestamp",
    "endDate": null,
    "status": "Current",
    "addedBy": "userId",
    "addedByName": "Rahul",
    "addedAt": "Timestamp"
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
    "remarks": "Rate list created"
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
