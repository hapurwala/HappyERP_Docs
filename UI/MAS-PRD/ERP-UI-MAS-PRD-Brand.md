# Brand UI/UX Documentation

This document provides UI details of all pages used in Roles Management under User Management module.

# Pages

| S. No. | Name             | Type | Purpose                                                                        |
| ------ | ---------------- | ---- | ------------------------------------------------------------------------------ |
| 1.     | Brand List       | Page | - View all brands<br/>- Search/filter brands<br/>- Perform actions on brand(s) |
| 2.     | Add/Modify Brand | Page | - View details of the brand<br/>- Add/modify data of current brand             |

# 1. Brand List Page UI

This page displays a list of Brands in tabular format.

## 1.1. Brand List

**Component Type:** DataTable

### 1.1a. DataTable (Brand List) - Columns

| Name          | Data Source                                                                                           | Format      | On Click            | Card Placement | Tooltip/On Hover                                                                                               |
| ------------- | ----------------------------------------------------------------------------------------------------- | ----------- | ------------------- | -------------- | -------------------------------------------------------------------------------------------------------------- |
| -             | m_brand.attachments[].`url` where `is_primary` = True                                                 | Image       | Zoom Image          | Image          | –                                                                                                              |
| Name          | m_brand.`name`                                                                                        | String      | Open  Page: `brand` | Title          | –                                                                                                              |
| Short Name    | m_brand.`short_name`                                                                                  | String      | –                   | Subtitle       | –                                                                                                              |
| Active        | If m_brand.`end_date` = null<br/>**Active**<br/><br/>If s_prd_brand.`end_date` <> null<br/>**Closed** | String      | –                   |                | If m_brand.`end_date` = null<br/>since start_date <br/><br/>If m_brand.`end_date` <> null<br/>on end_date<br/> |
| Stage         | m_brand.stage.`name`                                                                                  | Stage Badge | -                   |                | -                                                                                                              |
| Product Count | m_brand.`product_count`                                                                               | Integer     | -                   |                | Number of Products linked to Brand                                                                             |

### 1.1b. DataTable (Brand List) - Toolbar Config

| Feature          | Settings       | On Click            |
| ---------------- | -------------- | ------------------- |
| Search           | Yes            | -                   |
| View Toggle      | Yes            | -                   |
| Column Selection | Yes            | -                   |
| Group By         | Yes            | -                   |
| Filter           | Yes            | -                   |
| Export           | Yes            | -                   |
| Share            | No             | -                   |
| Full Screen      | Yes            | -                   |
| Add              | Add permission | Open  Page: `brand` |

### 1.1c. DataTable (Brand List) - Table Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | Yes      |
| Pagination     | No       |

### 1.1d. DataTable (Brand List) - RowAction Menu

| Name             | Action                                            | Visibility Criteria                                                 | Icon   | Tooltip |
| ---------------- | ------------------------------------------------- | ------------------------------------------------------------------- | ------ | ------- |
| View             | Open Page:  `brand`                               | - View Permission<br/>- No Modify Permission (Due to Role or Stage) | eye    | -       |
| Modify           | Open Page:  `brand`                               | - Modify permission<br>- Modify allowed on status                   | pencil | -       |
| Set as <Stage_1> | Set stage to Stage_1                              | - Allowed stage based on current stage                              | -      | -       |
| Set as <Stage_2> | Set stage to Stage_2                              | - Allowed stage based on current stage                              | -      | -       |
| Undo Stage       | Move back to last Stage                           | - Permission to rollback current stage                              |        | -       |
| Mark as Closed   | - Ask end/close date<br>- Sets m_brand.`end_date` | - Modify permission<br>- If m_brand.`end_date` = null               | lock   | -       |
| Mark as Active   | Sets m_brand.`end_date` = null                    | - Modify permission<br>- If m_brand.`end_date` <> null              | unlock |         |
| Delete           | - Show confirmation message<br>- On Yes, delete   | - Delete permission<br>- Delete allowed on stage                    | delete | -       |

### 1.1e. DataTable (Brand List) - Filters Fields

| Name  | Component    | Depends On | Possible Values              | Default         | Output                                                                  | Required | Tooltip |
| ----- | ------------ | ---------- | ---------------------------- | --------------- | ----------------------------------------------------------------------- | -------- | ------- |
| Stage | Multi Select | -          | All workflow stages of brand | Draft, Approved | Comma separated `app_object_stage_id`<br/>0 for <All><br/>"" for <None> | Yes      | -       |

# 2. Brand Entry Page UI

Fields in the main form are grouped in different sections.

## Main Form Sections

- Basic Details
- Brand logo

## 2.1. Left Side

### 2.1.1. Section (Basic Details)

| Name        | Data Source           | Component   | Component Specific Information         | Required | Read Only | Validations                  | On Change | Description                       | Tooltip |
| ----------- | --------------------- | ----------- | -------------------------------------- | -------- | --------- | ---------------------------- | --------- | --------------------------------- | ------- |
| Name        | m_brand.`name`        | Text        | -                                      | Yes      | No        | - Length: 3-100<br>- unique  | –         | Name of the Brand                 | -       |
| Short Name  | m_brand.`short_name`  | Text        | -                                      | Yes      | No        | - Length: 2-25, <br>- Unique | –         | Code/Short Name of the Brand      | -       |
| Brand Type  | m_brand.`brand_type`  | select      | Values:<br/>Own Brand<br/>Vendor Brand | Yes      | No        | -                            | –         | Classification of Brand           | -       |
| Description | m_brand.`description` | RichTextBox |                                        | No       | No        | Max 1000 Characters          | –         | Detailed Description of the Brand | -       |
| Start Date  | m_brand.`start_date`  | Date Picker |                                        | Yes      | No        | No                           | Only Date | Brand Activation Date             | -       |
| Remarks     | m_brand.`remarks`     | Text Area   | Multi line                             | No       | No        | Max 255 Characters           | –         | Additional Notes                  | -       |

## 2.2. Right Side

### 2.2.1. Section : Brand Logo

| Name       | Data Source    | Component    | Component Specific Information | Required | Read Only | Validations                                                            | On Change              | Description       | Tooltip    |
| ---------- | -------------- | ------------ | ------------------------------ | -------- | --------- | ---------------------------------------------------------------------- | ---------------------- | ----------------- | ---------- |
| Brand Logo | m_brand.`logo` | Image Upload | Image File Upload with Preview | No       | No        | - Supported Image Formats (JPG/PNG/JPEG) <br>- Maximum File Size: 1 MB | Preview Uploaded Image | Logo of the Brand | Brand Logo |

### 2.2.3.  Section : System Information

| Name        | Data Source           | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                 | Tooltip |
| ----------- | --------------------- | --------- | ------------------------------ | -------- | --------- | ----------- | --------- | --------------------------- | ------- |
| Created By  | m_brand.`created_by`  | String    | System Generated               | No       | Yes       | -           | -         | User who created the record | -       |
| Created On  | m_brand.`created_at`  | DateTime  | System Generated               | No       | Yes       | -           | -         | Record Creation Date/Time   | -       |
| Modified By | m_brand.`modified_by` | String    | System Generated               | No       | Yes       | -           | -         | Last Modified User          | -       |
| Modified On | m_brand.`modified_at` | DateTime  | System Generated               | No       | Yes       | -           | -         | Last Modified Date/Time     | -       |

---

### 2.2.2. Section: Brand Summary

| Name          | Data Source             | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description            | Tooltip |
| ------------- | ----------------------- | --------- | ------------------------------ | -------- | --------- | ----------- | --------- | ---------------------- | ------- |
| Product Count | m_brand.`product_count` | Integer   | -                              | No       | Yes       | -           | -         | Total linked products  | -       |
| Current Stage | m_brand.stage.`name`    | Badge     | -                              | No       | Yes       | -           | -         | Current workflow stage | -       |