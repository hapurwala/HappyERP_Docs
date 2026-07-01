
#  Product Group UI/UX Documentation

This document provides UI details of all pages used in the ***MAS-PRD's*** Product Group module.

# Related Reference files

specs file : -

---

# Pages

| S. No. | Name                       | Type | Purpose                                                                                              |
| ------ | --------------------------- | ---- | ------------------------------------------------------------------------------------------------------ |
| 1.     | Product Group List          | Page | - View all product groups<br/>- Search/filter product groups<br/>- Perform quick actions on product group(s) |
| 2.     | Add/Modify Product Group    | Page | - View details of the product group<br/>- Add/modify product group data                               |

---

# 1. Product Group List Page UI 

This page displays a list of Product Group in tabular format.

---

### 1.1a. DataTable (Product Group List) - Columns

| Header      | Data Source                          | Format      | On Click              | Card Placement | Tooltip / On Hover                                              | Inline Edit Component |
| ----------- | ------------------------------------- | ----------- | --------------------- | -------------- | ------------------------------------------------------------------ | ---------------------- |
| Image       | s_prd_product_group.`primary_image`   | Image       | Zoom Image            | Image          | –                                                                    | -                      |
| Name        | s_prd_product_group.`name`            | String      | Open View/Modify Page | Title          | –                                                                    | -                      |
| Short Name  | s_prd_product_group.`short_name`      | String      | –                     | Subtitle       | –                                                                    | -                      |
| UoM         | s_prd_product_group.`uom`             | String      | –                     | Body           | –                                                                    | -                      |
| Comes Under | s_prd_parent_product_group_id         | String      | -                     | Body           | Another Product Group under which this Product Group is nested     | -                      |
| Stage       | s_prd_product_group_stage.`name`      | Stage Badge | -                     | Body           | –                                                                    | -                      |
| Start Date  | s_prd_product_group.`start_date`      | Date        | –                     | Body           | –                                                                    | -                      |

---


### 1.1b. DataTable (Product Group List) - Toolbar Config

| Feature          | Settings | On Click                                                                                                 |
| ---------------- | -------- | -------------------------------------------------------------------------------------------------------- |
| Search           | Yes      | Search Product Group records based on the entered keyword(s).                                            |
| View Toggle      | Yes      | Switch between available DataTable view layouts.                                                         |
| Column Selection | Yes      | Open column selector to show or hide DataTable columns.                                                  |
| Group By         | Yes      | Group DataTable records based on the selected column.                                                    |
| Filter           | Yes      | Open filter panel to apply additional filtering criteria.                                                |
| Export           | Yes      | Export the displayed records.                                                                            |
| Share            | No       | N/A                                                                                                      |
| Full Screen      | Yes      | Toggle the DataTable between normal and full-screen view.                                                |
| Add              | when  s_app_role_permission.`can_add = true` for Product Group. | Page to Open: `product_group` |

---

### 1.1c. DataTable (Product Group List) - Table Config

| Feature        | Settings            |
| -------------- | ------------------- |
| Row Selection  | No                  |
| Bulk Actions   | No                  |
| Sticky Header  | Yes                 |
| Column Resize  | Yes                 |
| Column Pinning | Yes                 |
| Sorting        | Yes                 |
| Pagination     | Yes<br>Page Size:20 |

---

### 1.1d. DataTable (Product Group List) - RowAction Menu

| Name           | Action                                                          | Visibility Criteria                                                 | Icon   | Tooltip |
| -------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------- | ------ | ------- |
| Modify         | Open page `product_group`                                          | - Modify permission<br>- Modify allowed on status                    | pencil | -       |
| Stage_1        | Set status to Stage_1                                              | - Allowed status based on current status                             | -      | -       |
| Stage_2        | Set status to Stage_2                                              | - Allowed status based on current status                             | -      | -       |
| Undo Stage     | Move back to last Stage                                            | - Permission to set current stage                                    | undo   | -       |
| Mark as Closed | - Ask end/close date<br>- Sets s_prd_product_group.`end_date`      | - Modify permission<br>- If s_prd_product_group.`end_date` = Null    | lock   | -       |
| Mark as Opened | - Sets s_prd_product_group.`end_date` = Null                       | - Modify permission<br/>- s_prd_product_group.`end_date` <> Null     | unlock | -       |
| Delete         | - Show confirmation message<br>- On Yes, delete                    | - Delete permission<br>- Delete allowed on status                    | trash  | -       |

---

### 1.1e. DataTable (Product Group List) - Filters Fields

| Name  | Component    | Depends On | Possible Values | Default Values | Output    | Required | Tooltip |
| ----- | ------------ | ---------- | ---------------- | --------------- | --------- | -------- | ------- |
| Stage | Multi Select | –          | Possible Stages  | All Open Stage  | stage_ids | -        | -       |

---

# 2. Product Group Entry Page UI

Fields in the main form are grouped into different sections.

## Main Form Sections

```
2.1. Left: Main Form
- Basic Details

2.2. Right: Images and Summary
- Images
- System Information
```

---

## 2.1 Left Section

### 2.1.1 Basic Details

| Name        | Data Source                      | Component   | Component Specific Information | Required | Read Only | Validations        | On Change | Description                                                    | Tooltip |
| ----------- | --------------------------------- | ----------- | ---------------------------------- | -------- | --------- | -------------------- | --------- | ------------------------------------------------------------------ | ------- |
| Name        | s_prd_product_group.`name`        | Text        | -                                   | Yes      | No        | Length: 3-100        | –         | Name of the Product Group                                           | -       |
| Short Name  | s_prd_product_group.`short_name`  | Text        | -                                   | Yes      | No        | Length: 2-25         | –         | Code/Short Name of the Product Group                                | -       |
| UoM         | s_prd_product_group.`uom`         | Dropdown    | Select Unit of Measurement          | Yes      | No        | -                    | –         | UoM associated with this Product Group                              | -       |
| Comes Under | s_prd_parent_product_group_id     | Dropdown    | Select Parent Product Group         | No       | No        | -                    | –         | Another Product Group under which this Product Group is nested     | -       |
| Start Date  | s_prd_product_group.`start_date`  | Date Picker | DD/MM/YYYY                          | Yes      | No        | Only Date            | –         | Date when the Product Group was started/launched                    | -       |
| Remarks     | s_prd_product_group.`remarks`     | Text Area   | Multi-line                          | No       | No        | Max 255 Characters   | –         | Additional Notes                                                     | -       |

---

## 2.2 Right Section

### 2.2.1 Images

| Name          | Data Source                          | Component             | Component Specific Information                                                                                  | Required | Read Only | Validations              | On Change                | Description                                          | Tooltip |
| ------------- | -------------------------------------- | ----------------------- | --------------------------------------------------------------------------------------------------------------- | -------- | --------- | -------------------------- | -------------------------- | ------------------------------------------------------- | ------- |
| Primary Image | s_prd_product_group.`primary_image`    | Image Upload            | - Single Image Upload with Preview<br>- Supported Image Formats (JPG, JPEG, PNG)<br>- Maximum File Size: 2 MB   | No       | No        | Supported Image Formats    | Preview Uploaded Image     | Primary image of the Product Group                       | -       |
| Other Images  | s_prd_product_group.`other_images[]`   | Multiple Image Upload   | - Multiple Image Upload with Preview<br>- Supported Image Formats (JPG, JPEG, PNG)<br>- Maximum File Size: 2 MB | No       | No        | Supported Image Formats    | Preview Uploaded Images    | Additional images associated with the Product Group      | -       |

---

### 2.2.2 System Information

| Name        | Data Source      | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                                              | Tooltip |
| ----------- | ----------------- | --------- | --------------------------------- | -------- | --------- | ------------ | --------- | ------------------------------------------------------------ | ------- |
| Created By  | System Generated  | Text      | User Name                          | No       | Yes       | –            | –         | User who created the Product Group                            | -       |
| Created On  | System Generated  | Date Time | DD/MM/YYYY HH:mm                   | No       | Yes       | –            | –         | Date and Time when the Product Group was created               | -       |
| Modified By | System Generated  | Text      | User Name                          | No       | Yes       | –            | –         | User who last modified the Product Group                       | -       |
| Modified On | System Generated  | Date Time | DD/MM/YYYY HH:mm                   | No       | Yes       | –            | –         | Date and Time when the Product Group was last modified         | -       |
