# Module Design Details

This document gives basic design details of following module.

- **Software**: **HappyERP**
- **Application**: Product Management
- **Module**: Brand Master

---

# Roles

|Role|Purpose|
|:--|:--|
|Product Manager|Creates and manages brand records|
|Purchase Manager|Reviews vendor brands and procurement mappings|
|Sales Manager|Uses brands for sales and reporting|
|Admin|Full access to brand configuration|
|Viewer|Read-only access|

---

# Workflow Stages

|Stage|Description|Who Will Set It|Allow Modify|Allow Delete|Allow Close|Allow View To Roles|System Action|
|:--|:--|:--|:--|:--|:--|:--|:--|
|Draft|Initial stage where brand is being created|User|Yes|Yes|No|Product Manager, Admin|---|
|Submitted|Brand submitted for review and approval|User|No|No|No|Product Manager, Purchase Manager, Admin|Notify Approval Roles|
|Approved|Brand approved and available for product mapping|User|Yes|No|Yes|Product Manager, Purchase Manager, Sales Manager, Admin, Viewer|Available for Product Assignment|
|Closed|Brand no longer active for new products|User|No|No|No|All Authorized Roles|Prevent New Product Mapping|

---

# Workflow - Role Matrix

|Role|Current Stage|Create|Modify|Delete|Close|Next Stage|Rollback Stage|
|:--|:--|:--|:--|:--|:--|:--|:--|
|Product Manager|Draft|Yes|Yes|Yes|No|Submit|No|
|Purchase Manager|Submitted|No|No|No|No|Approve|Draft|
|Admin|Draft|Yes|Yes|Yes|No|Submit|No|
|Admin|Submitted|No|Yes|No|No|Approve|Draft|
|Admin|Approved|No|Yes|No|Yes|Close|Submitted|
|Viewer|Any|No|No|No|No|No|No|

---

# Header Fields

|Field|Description|Type|Required|
|:--|:--|:--|:--|
|Brand Name|Name of Brand|Text|Yes|
|Short Name|Brand Code / Short Name|Text|Yes|
|Brand Type|Own Brand / Vendor Brand|Dropdown|Yes|
|Start Date|Brand activation date|Date|Yes|
|End Date|Brand closure date|Date|No|

---

# Brand Details

|Field|Description|Type|Required|
|:--|:--|:--|:--|
|Description|Brand description|Text Area|No|
|Remarks|Internal remarks|Text Area|No|

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

---

# Firestore Database Structure

## Collection: brands

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
|stage|string|Draft / Submitted / Approved|
|startDate|timestamp|Activation Date|
|endDate|timestamp|Closure Date|
|remarks|string|Additional Notes|
|createdBy|string|User ID|
|createdByName|string|User Name Snapshot|
|updatedBy|string|Last Updated User|
|createdAt|timestamp|Creation Timestamp|
|updatedAt|timestamp|Last Modified Timestamp|
|companyId|string|Company ID|
|branchId|string|Branch ID|
|isDeleted|boolean|Soft Delete Flag|