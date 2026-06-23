# Brand UI/UX Documentation

A business may deal with products from different brands. These brands may be owned by the company itself or supplied by external vendors. The Brand Master allows users to create and manage brand information used across the system.

This document provides UI details of all pages used in the ***Mas- PRD's*** Brand module.


---

## Role Access Matrix

This section defines which user roles can access and perform actions within the Brand module.

|Role|View|Add|Modify|Delete|Change Stage|Mark as Closed|
|---|---|---|---|---|---|---|
|Super Admin|Yes|Yes|Yes|Yes|Yes|Yes|
|Admin|Yes|Yes|Yes|Yes|Yes|Yes|
|Product Manager|Yes|Yes|Yes|No|Yes|Yes|
|Purchase Manager|Yes|Yes|Yes|No|No|No|
|Sales Manager|Yes|No|No|No|No|No|
|Viewer|Yes|No|No|No|No|No|

### Permission Definitions

|Permission|Description|
|---|---|
|View|View Brand records and details|
|Add|Create new Brand records|
|Modify|Edit existing Brand records|
|Delete|Permanently delete Brand records|
|Change Stage|Move Brand between workflow stages|
|Mark as Closed|Close Brand by setting End Date|

### Action Visibility Rules

|Action|Permission Required|
|---|---|
|Add Brand|`can_add`|
|Modify|`can_modify`|
|Delete|`can_delete`|
|Change Stage|`can_change_stage`|
|Mark as Closed|`can_modify`|

---

# 1. Brand List Page UI 

This page displays a list of Brands in tabular format.

---

### 1.1a. DataTable (Brand List) - Columns

| Name              | Content                                                                                                                                              | Format      | On Click              | Card Placement | Tooltip/On Hover                                                           |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | --------------------- | -------------- | -------------------------------------------------------------------------- |
| Logo              | Image of the Logo                                                                                                                                    | Image       | Zoom Image            | –              | –                                                                          |
| Name              | s_prd_brand.`name`                                                                                                                                   | String      | Open View/Modify Page | Title          | –                                                                          |
| Short Name        | s_prd_brand.`short_name`                                                                                                                             | String      | –                     | Subtitle       | –                                                                          |
| Active            | If s_prd_brand.`end_date` = null<br/>**Active**<br/>since start_date <br/><br/>If s_prd_brand.`end_date` <> null<br/>**Closed**<br/>on end_date<br/> | String      | –                     | Body           | Display Start Date.<br>Display End Date if s_prd_brand.`end_date`<>  null. |
| Stage             | s_prd_brand_stage.`name`                                                                                                                             | Stage Badge | -                     | Body           | -                                                                          |
|                   |                                                                                                                                                      |             |                       |                |                                                                            |
| Country of Origin | s_prd_brand.`country_id`                                                                                                                             | String      | -                     | Badge          | -                                                                          |

---

### 1.1b. DataTable (Brand List) - Toolbar Config

| Feature          | Settings | On Click                                                                                 |     |
| ---------------- | -------- | ---------------------------------------------------------------------------------------- | --- |
| Search           | Yes      | Search  records based on the entered keyword(s).                                         |     |
| View Toggle      | Yes      | Switch between list and card.                                                            |     |
| Column Selection | Yes      | Open column selector to show or hide DataTable columns.                                  |     |
| Group By         | Yes      | Group DataTable records based on the selected column.                                    |     |
| Filter           | Yes      | Open filter panel to apply additional filtering criteria.                                |     |
| Export           | Yes      | Export the displayed records.                                                            |     |
| Share            | No       | N/A                                                                                      |     |
| Full Screen      | Yes      | Toggle the DataTable between normal and full-screen view.                                |     |
| Add              | Yes      | Open Add Brand Page. Visible only when `s_app_role_permission.can_add = true` for Brand. |     |

---

### 1.1c. DataTable (Brand List) - Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | Yes      |
| Pagination     | Yes      |

---

### 1.1d. DataTable (Brand List) - RowAction Menu

| Name           | Action                                          | Visibility Criteria                                       | Tooltip |
| -------------- | ----------------------------------------------- | --------------------------------------------------------- | ------- |
| Modify         | Open Page `Brand Modify`                        | - Modify permission<br>- Modify allowed on status         | -       |
| Stage_1        | Set status to Stage_1                           | - Allowed status based on current status                  | -       |
| Stage_2        | Set status to Stage_2                           | - Allowed status based on current status                  | -       |
| Undo Stage     | Move back to last Stage                         | - Permission to set current stage                         | -       |
| Mark as Closed | - Ask end/close date<br>- Sets end date         | - Modify permission<br>- If s_prd_brand.`end_date` = null | -       |
| Delete         | - Show confirmation message<br>- On Yes, delete | - Delete permission<br>- Delete allowed on status         | -       |

---

### 1.1e. DataTable (Brand List) - Filters Fields

| Name              | Type         | Possible Values | Default        |
| ----------------- | ------------ | --------------- | -------------- |
| Stage             | Multi Select | Possible Stages | All Open Stage |


---

# 2. Brand Entry Page UI

Fields in the main form are grouped into different sections.

## Main Form Sections

- Basic Details
- Brand logo

---

### 2.1.1 Left Section (Basic Details)

| Name/Label        | Data Source               | Component   | Component Specific Information | Required | Read Only | Validations                                   | On Change | Description                       | Tooltip |
| ----------------- | ------------------------- | ----------- | ------------------------------ | -------- | --------- | --------------------------------------------- | --------- | --------------------------------- | ------- |
| Name              | s_prd_brand.`name`        | Text        | -                              | Yes      | No        | - Length: 3-100<br>- unique                   | –         | Name of the Brand                 | -       |
| Short Name        | s_prd_brand.`short_name`  | Text        | -                              | Yes      | No        | - Length: 2-25, <br>- Unique                  | –         | Code/Short Name of the Brand      | -       |
| Brand Type        | s_prd_brand.`brand_type`  | Dropdown    | Own Brand / Vendor Brand       | Yes      | No        | Mandatory                                     | –         | Classification of Brand           | -       |
| Description       | s_prd_brand.`description` | Text Area   | Multi-line                     | No       | No        | Max 500 Characters                            | –         | Detailed Description of the Brand | -       |
| Start Date        | s_prd_brand.`start_date`  | Date Picker | DD/MM/YYYY                     | Yes      | No        | No                                            | Only Date | Brand Activation Date             | -       |
| Remarks           | s_prd_brand.`remarks`     | Text Area   | Multi-line                     | No       | No        | Max 255 Characters                            | –         | Additional Notes                  | -       |
| End Date          | s_prd_brand.`end_date`    | Date Picker | DD/MM/YYYY                     | No       | Yes       | - Visible only when brand `marked as closed ` | Only Date | Brand Closure Date                | -       |

---

### 2.1.3  System Information --(maybe)

| Name/Label    | Data Source                 | Component | Component Specific Information | Required | Read Only | Description                        |
| ------------- | --------------------------- | --------- | ------------------------------ | -------- | --------- | ---------------------------------- |
| Product Count | s_prd_brand.`product_count` | Number    | Calculated                     | No       | Yes       | Number of Products linked to Brand |
| Created By    | s_prd_brand.`created_by`    | Text      | System Generated               | No       | Yes       | User who created the record        |
| Created On    | s_prd_brand.`created_on`    | DateTime  | System Generated               | No       | Yes       | Record Creation Date               |
| Modified By   | s_prd_brand.`modified_by`   | Text      | System Generated               | No       | Yes       | Last Modified User                 |
| Modified On   | s_prd_brand.`modified_on`   | DateTime  | System Generated               | No       | Yes       | Last Modified Date                 |


---

### 2.1.2 Right Section (Brand Logo)

| Name/Label | Data Source        | Component    | Component Specific Information | Required | Read Only | Validations                                                            | On Change              | Description       | Tooltip    |
| ---------- | ------------------ | ------------ | ------------------------------ | -------- | --------- | ---------------------------------------------------------------------- | ---------------------- | ----------------- | ---------- |
| Brand Logo | s_prd_brand.`logo` | Image Upload | Image File Upload with Preview | No       | No        | - Supported Image Formats (JPG/PNG/JPEG) <br>- Maximum File Size: 2 MB | Preview Uploaded Image | Logo of the Brand | Brand Logo |


