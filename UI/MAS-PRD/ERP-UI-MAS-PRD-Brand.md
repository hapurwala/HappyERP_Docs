# Brand UI/UX Documentation

This document provides UI details of all pages used in the ***MAS-PRD's*** Brand module.

# Related Reference files

specs file : [ERP-DES-MAS-Brand](/specs/ERP-DES-PRD-Brand.md)

---

# Pages

| S. No. | Name             | Type | Purpose                                                                               |
| ------ | ---------------- | ---- | ------------------------------------------------------------------------------------- |
| 1.     | Brand List       | Page | - View all brands<br/>- Search/filter brands<br/>- Perform quick actions on brand(s)  |
| 2.     | Add/Modify Brand | Page | - View details of the brand<br/>- Add/modify brand data                               |

---

# 1. Brand List Page UI

This page displays a list of Brands in tabular format.

## 1.1. Brand List

**Component Type:** DataTable

### 1.1a. DataTable (Brand List) - Columns

| Header        | Data Source                                                                                              | Format      | On Click           | Card Placement | Tooltip / On Hover                                                                                         | Inline Edit Component |
| ------------- | -------------------------------------------------------------------------------------------------------- | ----------- | ------------------ | -------------- | ----------------------------------------------------------------------------------------------------------- | --------------------- |
| -             | m_brand.attachments[].`url` where `is_primary` = True                                                   | Image       | Zoom Image         | Image          | -                                                                                                           | -                     |
| Name          | m_brand.`name`                                                                                           | String      | Open Page: `brand` | Title          | -                                                                                                           | -                     |
| Short Name    | m_brand.`short_name`                                                                                     | String      | -                  | Subtitle       | -                                                                                                           | -                     |
| Active        | If m_brand.`end_date` = null<br/>**Active**<br/><br/>If m_brand.`end_date` <> null<br/>**Closed**        | String      | -                  | Body           | If m_brand.`end_date` = null<br/>since start_date<br/><br/>If m_brand.`end_date` <> null<br/>on end_date   | -                     |
| Stage         | m_brand.stage.`name`                                                                                     | Stage Badge | -                  | Body           | -                                                                                                           | -                     |
| Product Count | m_brand.`product_count`                                                                                  | Integer     | -                  | Body           | Number of Products linked to Brand                                                                          | -                     |

---

### 1.1b. DataTable (Brand List) - Toolbar Config

| Feature          | Settings                                                | On Click              |
| ---------------- | -------------------------------------------------------- | ---------------------- |
| Search           | Yes                                                      | -                      |
| View Toggle      | Yes                                                      | -                      |
| Column Selection | Yes                                                      | -                      |
| Group By         | Yes                                                      | -                      |
| Filter           | Yes                                                      | -                      |
| Export           | Yes                                                      | -                      |
| Share            | No                                                       | -                      |
| Full Screen      | Yes                                                      | -                      |
| Add              | when  s_app_role_permission.`can_add = true` for Brand.  | Page to Open: `brand`  |

---

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

---

### 1.1d. DataTable (Brand List) - RowAction Menu

| Name             | Action                                              | Visibility Criteria                                                  | Icon               | Tooltip |
| ---------------- | ----------------------------------------------------- | --------------------------------------------------------------------- | ------------------- | ------- |
| View             | Open Page: `brand`                                  | - View Permission<br/>- No Modify Permission (Due to Role or Stage)  | eye                | -       |
| Modify           | Open Page: `brand`                                  | - Modify permission<br/>- Modify allowed on stage                    | pencil             | -       |
| Set as <Stage_1> | Set stage to Stage_1                                | - Allowed stage based on current stage                               | arrow-right-circle | -       |
| Set as <Stage_2> | Set stage to Stage_2                                | - Allowed stage based on current stage                               | arrow-right-circle | -       |
| Undo Stage       | Move back to last stage                             | - Permission to rollback current stage                               | undo-2             | -       |
| Mark as Closed   | - Ask end/close date<br/>- Sets m_brand.`end_date`  | - Modify permission<br/>- m_brand.`end_date` = null                  | lock               | -       |
| Mark as Active   | - Sets m_brand.`end_date` = null                    | - Modify permission<br/>- m_brand.`end_date` <> null                 | unlock             | -       |
| Delete           | - Show confirmation message<br/>- On Yes, delete    | - Delete permission<br/>- Delete allowed on stage                    | trash-2            | -       |

---

### 1.1e. DataTable (Brand List) - Filters Fields

| Name  | Component    | Depends On | Possible Values              | Default Values  | Output                                                                    | Required | Tooltip |
| ----- | ------------ | ---------- | ----------------------------- | ---------------- | --------------------------------------------------------------------------- | -------- | ------- |
| Stage | Multi Select | -          | All workflow stages of brand  | Draft, Approved | Comma separated `app_object_stage_id`<br/>0 for <All><br/>"" for <None>    | Yes      | -       |

---

# 2. Brand Entry Page UI

Fields in the main form are grouped in different sections.

## Main Form Sections

```
2.1. Left: Main Form
- Basic Details

2.2. Right: Brand Logo and Summary
- Brand Logo
- Brand Summary
- System Information
```

---

## 2.1 Left Section

### 2.1.1 Basic Details

| Name        | Data Source           | Component   | Component Specific Information   | Required | Read Only | Validations                  | On Change | Description                       | Tooltip |
| ----------- | --------------------- | ----------- | --------------------------------- | -------- | --------- | ---------------------------- | --------- | --------------------------------- | ------- |
| Name        | m_brand.`name`        | Text        | -                                 | Yes      | No        | - Length: 3-100<br/>- Unique | -         | Name of the Brand                 | -       |
| Short Name  | m_brand.`short_name`  | Text        | -                                 | Yes      | No        | - Length: 2-25<br/>- Unique  | -         | Code/Short Name of the Brand      | -       |
| Brand Type  | m_brand.`brand_type`  | Select      | Values: Own Brand / Vendor Brand  | Yes      | No        | -                            | -         | Classification of Brand           | -       |
| Description | m_brand.`description` | RichTextBox | -                                 | No       | No        | Max 1000 Characters          | -         | Detailed Description of the Brand | -       |
| Start Date  | m_brand.`start_date`  | DatePicker  | Single Date Picker                | Yes      | No        | -                            | Only Date | Brand Activation Date             | -       |
| Remarks     | m_brand.`remarks`     | RichTextBox | Multi line                        | No       | No        | Max 255 Characters           | -         | Additional Notes                  | -       |

---

## 2.2 Right Section

### 2.2.1 Brand Logo

| Name       | Data Source    | Component    | Component Specific Information  | Required | Read Only | Validations                                                             | On Change              | Description       | Tooltip |
| ---------- | -------------- | ------------ | -------------------------------- | -------- | --------- | ----------------------------------------------------------------------- | ---------------------- | ----------------- | ------- |
| Brand Logo | m_brand.`logo` | Image Upload | Image File Upload with Preview  | No       | No        | - Supported Image Formats (JPG/PNG/JPEG)<br/>- Maximum File Size: 1 MB | Preview Uploaded Image | Logo of the Brand | Brand Logo |

---

### 2.2.2 Brand Summary

| Name          | Data Source             | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description            | Tooltip |
| ------------- | ----------------------- | --------- | ------------------------------ | -------- | --------- | ----------- | --------- | ---------------------- | ------- |
| Product Count | m_brand.`product_count` | Integer   | -                              | No       | Yes       | -           | -         | Total linked products  | -       |
| Current Stage | m_brand.stage.`name`    | Badge     | -                              | No       | Yes       | -           | -         | Current workflow stage | -       |

---

### 2.2.3 System Information

| Name        | Data Source           | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                 | Tooltip |
| ----------- | --------------------- | --------- | ------------------------------ | -------- | --------- | ----------- | --------- | --------------------------- | ------- |
| Created By  | m_brand.`created_by`  | Text      | System Generated               | No       | Yes       | -           | -         | User who created the record | -       |
| Created On  | m_brand.`created_at`  | DateTime  | System Generated               | No       | Yes       | -           | -         | Record Creation Date/Time   | -       |
| Modified By | m_brand.`modified_by` | Text      | System Generated               | No       | Yes       | -           | -         | Last Modified User          | -       |
| Modified On | m_brand.`modified_at` | DateTime  | System Generated               | No       | Yes       | -           | -         | Last Modified Date/Time     | -       |
