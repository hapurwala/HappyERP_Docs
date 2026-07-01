# Module Design Details

A business may deal with products from different brands. These brands may be owned by the company itself or supplied by external vendors. The Brand Master allows users to create and manage brand information used across the system.

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: **Product Management**
- **Module**: **Brand Master**

---

## Roles

| Role              | Purpose                                        |
| :---------------- | :--------------------------------------------- |
| Product Executive | Creates and manages brand records              |
| Product Manager   | Reviews vendor brands and procurement mappings |
| Sales Manager     | Uses brands for sales and reporting            |
| System Admin      | Full access to brand configuration             |
| Viewer            | Read-only access                               |

---

## Workflow Stages

| Stage     | Description                                      | Who Will Set It | Allow Modify | Allow Delete | Allow Cancel | Allow View To Roles                                              | System Action                    |
| :-------- | :----------------------------------------------- | :-------------- | :----------- | :----------- | :----------- | :--------------------------------------------------------------- | :------------------------------- |
| Draft     | Initial stage where brand is being created       | User            | Yes          | Yes          | No           | Product Executive, System Admin                                         | —                                 |
| Submitted | Brand submitted for review and approval          | User            | No           | No           | Yes          | Product Manager, System Admin                                           | Notify Approval Roles            |
| Approved  | Brand approved and available for product mapping | User            | Yes          | No           | No           | Product Executive, Product Manager, Sales Manager, System Admin, Viewer | Available for Product Assignment |

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

| Field      | Description              | Type      | Required |
| :--------- | :----------------------- | :-------- | :------- |
| Brand Name | Name of Brand            | Text      | Yes      |
| Short Name | Brand Code / Short Name  | Text      | Yes      |
| Description | Brand description | Text Area | No       |
| Brand Type | Own Brand / Vendor Brand | Dropdown  | Yes      |
| Start Date | Brand activation date    | Date      | Yes      |
| End Date   | Brand closure date       | Date      | No       |
| Remarks    | Internal remarks         | Text Area | No       |

---

## Brand Summary

|Field|Description|Type|
|:--|:--|:--|
|Product Count|Number of products linked to this brand|Number|
|Current Stage|Current workflow stage|Badge|
|Status|Active / Closed (system-derived from End Date)|Badge|

---

## Media & Attachments

|Field|Description|Type|
|:--|:--|:--|
|Brand Logo|Brand logo image|Image Upload|
|Attachments|Supporting brand documents|File Upload|

|Property|Value|
|---|---|
|Supported Formats|PNG, JPG, JPEG|
|Maximum File Size|2 MB|

---

## Reports

| Report                       | Purpose                                                                   |
| :--------------------------- | :------------------------------------------------------------------------ |
| Brand Report                 | List of all brands (filterable by Status)                                 |
| Brand Wise Sales Report      | Sales analysis by brand                                                   |
| Brand Wise Purchase Report   | Purchase analysis by brand                                                |
| Brand Usage Report           | Shows products, purchase, sales and inventory associated with each brand. |

---

## System Information

| Field       | Description                       | Type     |
| :---------- | :--------------------------------- | :------- |
| Created By  | User who created the brand         | User     |
| Created On  | Record creation date and time      | DateTime |
| Modified By | Last user who modified the brand   | User     |
| Modified On | Last modification date and time    | DateTime |

---

## Validation Rules

|Field|Rule|
|:--|:--|
|Brand Name|Must be unique across all Brands|
|Short Name|Must be unique across all Brands|
|Start Date / End Date|End Date cannot be earlier than Start Date|
|Status|Status is system-derived from End Date (Active when End Date is empty or in the future, Closed once End Date has passed); it cannot be set directly|

---

# Firestore Database Structure

## Collection: `brands`

### Document Structure

|Field Name|Data Type|Description|
|:--|:--|:--|
|brandId|string|Unique Brand ID|
|name|string|Brand Name|
|shortName|string|Brand Code|
|brandType|string|Own Brand / Vendor Brand|
|description|string|Brand Description|
|logo|string|Logo URL|
|productCount|number|Linked Product Count|
|stage|string|Current Workflow Stage (Draft / Submitted / Approved)|
|status|string|Active / Closed (system-derived from endDate; not directly settable)|
|startDate|timestamp|Activation Date|
|endDate|timestamp|Closure Date|
|remarks|string|Additional Notes|
|createdBy|string|User ID who created the record|
|createdByName|string|Creator Name Snapshot|
|createdAt|timestamp|Record Creation Timestamp|
|updatedBy|string|Last Updated User ID|
|updatedByName|string|Last Updated User Name Snapshot|
|updatedAt|timestamp|Last Modified Timestamp|
|stageHistory|array<map>|Complete workflow audit history|
|companyId|string|Company ID|
|branchId|string|Branch ID|
|isDeleted|boolean|Soft Delete Flag|

---

### `stageHistory` Structure

```json
[
  {
    "stage": "Draft",
    "actionBy": "userId",
    "actionByName": "Rahul",
    "actionAt": "Timestamp",
    "remarks": "Brand created"
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