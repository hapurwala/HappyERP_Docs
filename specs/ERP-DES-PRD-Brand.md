# Module Design Details

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: Product Management
- **Module**: Brand Master

---

# Roles

|Role|Purpose|
|:--|:--|
|Product Executive|Creates and manages brand records|
|Product Manager|Reviews vendor brands and procurement mappings|
|Sales Manager|Uses brands for sales and reporting|
|Admin|Full access to brand configuration|
|Viewer|Read-only access|

---

# Workflow Stages

| Stage     | Description                                      | Who Will Set It | Allow Modify | Allow Delete | Allow Archive | Allow View To Roles                                             | System Action                    |
| :-------- | :----------------------------------------------- | :-------------- | :----------- | :----------- | :---------- | :-------------------------------------------------------------- | :------------------------------- |
| Draft     | Initial stage where brand is being created       | User            | Yes          | Yes          | No          | Product Executive, Admin                                          | ---                              |
| Submitted | Brand submitted for review and approval          | User            | No           | No           | No          | Product Manager, Admin                        | Notify Approval Roles            |
| Approved  | Brand approved and available for product mapping | User            | Yes          | No           | Yes         | Product Executive, Product Manager, Sales Manager, Admin, Viewer | Available for Product Assignment |
| Archived    | Brand no longer active for new products          | User            | No           | No           | No          | All Authorized Roles                                            | Prevent New Product Mapping      |

---

# Workflow - Role Matrix

| Role              | Current Stage | Create | Modify | Delete | Close | Next Stage | Rollback Stage |
| :---------------- | :------------ | :----- | :----- | :----- | :---- | :--------- | :------------- |
| Product Executive | Draft         | Yes    | Yes    | Yes    | No    | Submit     | No             |
| Product Manager   | Submitted     | No     | No     | No     | No    | Approve    | Draft          |
| Admin             | Draft         | Yes    | Yes    | Yes    | No    | Submit     | No             |
| Admin             | Submitted     | No     | Yes    | No     | No    | Approve    | Draft          |
| Admin             | Approved      | No     | No     | No     | Yes   | Archive      | Submitted      |
| Viewer            | Any           | No     | No     | No     | No    | No         | No             |

---

# Header Fields

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

# Brand Summary

|Field|Description|Type|
|:--|:--|:--|
|Product Count|Number of products linked to this brand|Number|
|Current Stage|Current workflow stage|Badge|
|Status|Active / Closed|Badge|

---

# Media & Attachments

|Field|Description|Type|
|:--|:--|:--|
|Brand Logo|Brand logo image|Image Upload|
|Attachments|Supporting brand documents|File Upload|

|Property|Value|
|---|---|
|Supported Formats|PNG, JPG, JPEG|
|Maximum File Size|2 MB|

---

# Reports

|Report|Purpose|
|:--|:--|
|Brand Report|List of all brands|
|Brand Product Mapping Report|Products grouped by brand|
|Active Brand Report|Active brands only|
|Closed Brand Report|Closed brands|
|Brand Wise Sales Report|Sales analysis by brand|
|Brand Wise Purchase Report|Purchase analysis by brand|
|Brand Usage Report|Shows products, purchase, sales and inventory associated with each brand.|

---

# System Information

| Field               | Description                      | Type     |     |
| :------------------ | :------------------------------- | :------- | --- |
| Created By          | User who created the brand       | User     |     |
| Created On          | Record creation date and time    | DateTime |     |
| Modified By         | Last user who modified the brand | User     |     |
| Modified On         | Last modification date and time  | DateTime |     |


---

# Stage Audit

|Field|Description|Type|
|:--|:--|:--|
|Current Stage|Current workflow stage|Badge|
|Submitted By|User who submitted the brand|User|
|Submitted On|Date and time of submission|DateTime|
|Approved By|User who approved the brand|User|
|Approved On|Date and time of approval|DateTime|
|Archived By|User who archived the brand|User|
|Archived On|Date and time of archival|DateTime|
|Stage Remarks|Comments entered during stage transition|Text Area|

---
## Firestore Database Structure

### Collection: `brands`

#### Document Structure

|Field Name|Data Type|Description|
|:--|:--|:--|
|brandId|string|Unique Brand ID|
|name|string|Brand Name|
|shortName|string|Brand Code|
|brandType|string|Own Brand / Vendor Brand|
|description|string|Brand Description|
|logo|string|Logo URL|
|productCount|number|Linked Product Count|
|stage|string|Current Workflow Stage (Draft / Submitted / Approved / Archived)|
|status|string|Active / Closed|
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
  },
  {
    "stage": "Archived",
    "actionBy": "adminId",
    "actionByName": "Admin",
    "actionAt": "Timestamp",
    "remarks": "Brand archived"
  }
]
```