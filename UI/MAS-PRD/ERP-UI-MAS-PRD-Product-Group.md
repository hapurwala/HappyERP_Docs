
#  Product Group UI/UX Documentation

The products can be grouped together for easy handling. E.g. a Kirana store has product groups like Pulses, Spices etc. Users may also maintain nested Product Groups.

This document provides UI details of all pages used in the ***MAS-PRD's*** Product Group module.

---

# 1. Product Group List Page UI 

This page displays a list of Product Group in tabular format.

---

### 1.1a. DataTable (Product Group List) - Columns

| Name        | Content                             | Format      | On Click              | Card Placement | Tooltip/On Hover                                               |
| ----------- | ----------------------------------- | ----------- | --------------------- | -------------- | -------------------------------------------------------------- |
| –           | s_prd_product_group.`primary_image` | Image       | Zoom Image            | Thumbnail      | –                                                              |
| Name        | s_prd_product_group.`name`          | String      | Open View/Modify Page | Title          | –                                                              |
| Short Name  | s_prd_product_group.`short_name`    | String      | –                     | Subtitle       | –                                                              |
| UoM         | s_prd_product_group.`uom`           | String      | –                     | Body           | –                                                              |
| Comes Under | s_prd_parent_product_group_id       | String      | -                     | Body           | Another Product Group under which this Product Group is nested |
| Stage       | s_prd_product_group_stage`name`     | Stage Badge | -                     | Body           | –                                                              |
| Start Date  | s_prd_product_group.`start_date`    | Date        | –                     | Body           | –                                                              |

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
| Add              | Yes      | Open Add Product Group Page. Visible only when `s_app_role_permission.can_add = true` for Product Group. |

---

### 1.1c. DataTable (Product Group List) - Config

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

### 1.1d. DataTable (Product Group List) - RowAction Menu

| Name           | Action                                          | Visibility Criteria                               | Tooltip |
| -------------- | ----------------------------------------------- | ------------------------------------------------- | ------- |
| Modify         | Open View/Modify `Product Group Page`                    | - Modify permission<br>- Modify allowed on status | -       |
| Stage_1        | Set status to Stage_1                           | - Allowed status based on current status          | -       |
| Stage_2        | Set status to Stage_2                           | - Allowed status based on current status          | -       |
| Undo Status    | Move back to last status                        | - Permission to set current status                | -       |
| Mark as Closed | - Ask end/close date<br>- Sets end date         | - Modify permission                               | -       |
| Delete         | - Show confirmation message<br>- On Yes, delete | - Delete permission<br>- Delete allowed on status | -       |

---

### 1.1e. DataTable (Product Group List) - Filters Fields

| Name       | Type         | Possible Values   | Default         |
| ---------- | ------------ | ----------------- | --------------- |
| Status     | Multi Select | Possible statuses | All Open Status |

---

# 2. Product Group Entry Page UI

Fields in the main form are grouped into different sections.

## Main Form Sections

- Basic Details
- Group logo

### 2.1.1 Section (Basic Details)

| Name/Label  | Data Source                      | Component   | Component Specific Information | Required | Read Only | Validations        | On Change | Description                                                    | Tooltip              |
| ----------- | -------------------------------- | ----------- | ------------------------------ | -------- | --------- | ------------------ | --------- | -------------------------------------------------------------- | -------------------- |
| Name        | s_prd_product_group.`name`       | Text        | -                              | Yes      | No        | Length: 3-100      | –         | Name of the Product Group                                      | -                    |
| Short Name  | s_prd_product_group.`short_name` | Text        | -                              | Yes      | No        | Length: 2-25       | –         | Code/Short Name of the Product Group                           | -                    |
| UoM         | s_prd_product_group.`uom`        | Dropdown    | Select Unit of Measurement     | Yes      | No        | -                  | –         | UoM associated with this Product Group                         | -                    |
| Comes Under | s_prd_parent_product_group_id    | Dropdown    | Select Parent Product Group    | No       | No        | -                  | –         | Another Product Group under which this Product Group is nested | Parent Product Group |
| Start Date  | s_prd_product_group.`start_date` | Date Picker | DD/MM/YYYY                     | Yes      | No        | Only Date          | –         | Date when the Product Group was started/launched               | -                    |
| Remarks     | s_prd_product_group.`remarks`    | Text Area   | Multi-line                     | No       | No        | Max 255 Characters | –         | Additional Notes                                               | -                    |

---

### 2.1.2a Section (Images)

| Name/Label    | Data Source                          | Component             | Component Specific Information                                                                                  | Required | Read Only | Validations             | On Change               | Description                                         | Tooltip       |
| ------------- | ------------------------------------ | --------------------- | --------------------------------------------------------------------------------------------------------------- | -------- | --------- | ----------------------- | ----------------------- | --------------------------------------------------- | ------------- |
| Primary Image | s_prd_product_group.`primary_image`  | Image Upload          | - Single Image Upload with Preview<br>- Supported Image Formats (JPG, JPEG, PNG)<br>- Maximum File Size: 2 MB   | No       | No        | Supported Image Formats | Preview Uploaded Image  | Primary image of the Product Group                  | Primary Image |
| Other Images  | s_prd_product_group.`other_images[]` | Multiple Image Upload | - Multiple Image Upload with Preview<br>- Supported Image Formats (JPG, JPEG, PNG)<br>- Maximum File Size: 2 MB | No       | No        | Supported Image Formats | Preview Uploaded Images | Additional images associated with the Product Group | Other Images  |
|               |                                      |                       |                                                                                                                 |          |           |                         |                         |                                                     |               |
### 2.1.2b Right Section (System Information)

| Name/Label  | Data Source      | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                                             | Tooltip     |
| ----------- | ---------------- | --------- | ------------------------------ | -------- | --------- | ----------- | --------- | ------------------------------------------------------- | ----------- |
| Created By  | System Generated | Text      | User Name                      | No       | Yes       | –           | –         | User who created the Product Group                      | Created By  |
| Created On  | System Generated | Date Time | DD/MM/YYYY HH:mm               | No       | Yes       | –           | –         | Date and Time when the Product Group  was created       | Created On  |
| Modified By | System Generated | Text      | User Name                      | No       | Yes       | –           | –         | User who last modified the Product Group                | Modified By |
| Modified On | System Generated | Date Time | DD/MM/YYYY HH:mm               | No       | Yes       | –           | –         | Date and Time when the Product  Group was last modified | Modified On |
