# Product Attribute UI/UX Documentation

This document provides UI details of all pages used in the ***MAS-PRD's*** Product Attribute module.

# Related Reference files

specs file : [ERP-DES-PRD-Product-Attribute](/specs/ERP-DES-PRD-Product-Attribute.md)

---

# Pages

| S. No. | Name                        | Type | Purpose                                                                                              |
| ------ | --------------------------- | ---- | ------------------------------------------------------------------------------------------------------ |
| 1.     | Product Attribute List      | Page | - View all product attributes<br/>- Search/filter product attributes<br/>- Perform quick actions on product attribute(s) |
| 2.     | Add/Modify Product Attribute | Page | - View details of the product attribute<br/>- Add/modify product attribute data                       |

---

# 1. Product Attribute List Page UI 

This page displays a list of Product Attribute in tabular format.

---

### 1.1a. DataTable (Product Attribute List) - Columns

A dropdown will be placed above the list. Users will select an attribute here and corresponding data will be displayed in the list below:

| Header     | Data Source                  | Format      | On Click              | Card Placement | Tooltip / On Hover                       | Inline Edit Component |
| ---------- | ----------------------------- | ----------- | --------------------- | -------------- | ----------------------------------------- | --------------------- |
| Name       | s_prd_attribute.`name`        | String      | Open View/Modify Page | Title          | –                                         | -                      |
| Short Name | s_prd_attribute.`short_name`  | String      | –                     | Body           | –                                         | -                      |
| Sequence   | s_prd_attribute.`sequence`    | Integer     | –                     | Body           | Controls display order of this attribute  | -                      |
| Stage      | s_prd_attribute_stage.`name`  | Stage Badge | Show Stage History    | Body           | –                                         | -                      |

---

### 1.1b. DataTable (Product Attribute List) - Toolbar Config

| Feature          | Settings                                                            | On Click                                                   |
| ---------------- | -------------------------------------------------------------------- | ----------------------------------------------------------- |
| Search           | Yes                                                                   | Search records based on the entered keyword(s).            |
| View Toggle      | Yes                                                                   | Switch between list and card.                               |
| Column Selection | Yes                                                                   | Open column selector to show or hide DataTable columns.     |
| Group By         | Yes                                                                   | Group DataTable records based on the selected column.       |
| Filter           | Yes                                                                   | Open filter panel to apply additional filtering criteria.   |
| Export           | Yes                                                                   | Export the displayed records.                                |
| Share            | No                                                                    | N/A                                                          |
| Full Screen      | Yes                                                                   | Toggle the DataTable between normal and full-screen view.    |
| Add              | when  s_app_role_permission.`can_add = true` for Product Attribute. | Page to Open: `product_attribute`                            |

---

### 1.1c. DataTable (Product Attribute List) - Table Config

| Feature        | Settings             |
| -------------- | -------------------- |
| Row Selection  | No                   |
| Bulk Actions   | No                   |
| Sticky Header  | Yes                  |
| Column Resize  | Yes                  |
| Column Pinning | Yes                  |
| Sorting        | Yes                  |
| Pagination     | Yes<br>Page Size: 20 |

---

### 1.1d. DataTable (Product Attribute List) - RowAction Menu

| Name           | Action                                                     | Visibility Criteria                                            | Icon   | Tooltip |
| -------------- | ----------------------------------------------------------- | ----------------------------------------------------------------- | ------ | ------- |
| Modify         | Open page: `product_attribute`                              | - Modify permission<br>- Modify allowed on status                 | pencil | -       |
| Stage_1        | Set status to Stage_1                                       | - Allowed status based on current status                          | -      | -       |
| Stage_2        | Set status to Stage_2                                       | - Allowed status based on current status                          | -      | -       |
| Undo Stage     | Move back to last Stage                                     | - Permission to set current Stage                                 | undo   | -       |
| Mark as Closed | - Ask end/close date<br>- Sets s_prd_attribute.`end_date`   | - Modify permission<br>- If s_prd_attribute.`end_date` = Null     | lock   | -       |
| Mark as Opened | - Sets s_prd_attribute.`end_date` = Null                    | - Modify permission<br/>- s_prd_attribute.`end_date` <> Null      | unlock | -       |
| Delete         | - Show confirmation message<br>- On Yes, delete             | - Delete permission<br>- Delete allowed on status                  | trash  | -       |

---

### 1.1e. DataTable (Product Attribute List) - Filters Fields

| Name  | Component    | Depends On | Possible Values | Default Values | Output    | Required | Tooltip |
| ----- | ------------ | ---------- | ---------------- | --------------- | --------- | -------- | ------- |
| Stage | Multi Select | –          | Possible Stages  | All Open Stage  | stage_ids | -        | -       |

---

# 2. Product Attribute Entry Page UI

Fields in the main form are grouped into different sections.

## Main Form Sections

```
2.1. Left: Main Form
- Basic Details
- Applicability
- Values

2.2. Right: System Information
- System Information
```

---

## 2.1 Left Section

### 2.1.1 Basic Details

| Name         | Data Source                     | Component   | Component Specific Information | Required | Read Only | Validations             | On Change | Description                                                                    | Tooltip             |
| ------------ | -------------------------------- | ----------- | -------------------------------- | -------- | --------- | ------------------------ | --------- | -------------------------------------------------------------------------------- | -------------------- |
| Name         | s_prd_attribute.`name`           | Text        | -                                 | Yes      | No        | Length: 3-100             | –         | Name of the Product Attribute                                                     | -                    |
| Short Name   | s_prd_attribute.`short_name`     | Text        | -                                 | Yes      | No        | Length: 2-25               | –         | Code/Short Name of the Product Attribute                                          | -                    |
| Is Mandatory | s_prd_attribute.`is_mandatory`   | Checkbox    | Yes / No                          | No       | No        | –                         | –         | Indicates whether this Attribute is mandatory while creating Product Varieties    | -                    |
| Sequence     | s_prd_attribute.`sequence`       | Number      | Positive Integer                  | Yes      | No        | Numeric Value Required    | –         | Sequence used for display ordering                                                | -                    |
| End Date     | s_prd_attribute.`end_date`       | Date Picker | DD/MM/YYYY                        | No       | Yes       | Only Date                  | –         | Date when the Product Attribute was closed                                        | -                    |

---

### 2.1.2 Applicability

This section contains data in list format

| Name       | Data Source                                         | Component     | Component Specific Information                            | Required | Read Only | Validations | On Change | Description                                              | Tooltip |
| ---------- | ---------------------------------------------------- | -------------- | ----------------------------------------------------------- | -------- | --------- | ------------ | --------- | ---------------------------------------------------------- | ------- |
| Product(s) | s_prd_attribute_applicable_products[].`product_id` | Checkbox grid  | Displays all available Products as selectable checkboxes    | No       | No        | -            | -         | An attribute may be applicable to one or more products.    | -       |

---

### 2.1.3a. DataTable (Values) - Columns

| Header     | Data Source                           | Format | On Click | Card Placement | Tooltip / On Hover                       | Inline Edit Component      |
| ---------- | ------------------------------------- | ------ | -------- | -------------- | ---------------------------------------- | -------------------------- |
| Value      | s_prd_attribute_values[].`value`      | String | –        | -              | -                                         | Textbox                    |
| Start Date | s_prd_attribute_values[].`start_date` | Date   | –        | -              | Date from which this value is effective  | Date Picker<br/>DD/MM/YYYY |
| End Date   | s_prd_attribute_values[].`end_date`   | Date   | –        | -              | Date until which this value is effective | Date Picker<br/>DD/MM/YYYY |
| Status     | s_prd_attribute_values[].`status`     | String | –        | -              | -                                         | –                          |

---

### 2.1.3b. DataTable (Values) - Toolbar Config

| Feature          | Settings | On Click |
| ---------------- | -------- | -------- |
| Search           | No       | -        |
| View Toggle      | No       | -        |
| Column Selection | No       | -        |
| Group By         | No       | -        |
| Filter           | No       | -        |
| Export           | No       | -        |
| Share            | No       | -        |
| Full Screen      | No       | -        |
| Add              | No       | -        |

---

### 2.1.3c. DataTable (Values) - Table Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | No       |
| Column Resize  | No       |
| Column Pinning | No       |
| Sorting        | No       |
| Pagination     | No       |

---

### 2.1.3d. DataTable (Values) - RowAction Menu

| Name           | Action                                                            | Visibility Criteria | Icon   | Tooltip                |
| -------------- | ------------------------------------------------------------------- | -------------------- | ------ | ------------------------ |
| Modify         | Inline Edit (shows Save/Cancel on the active row)                    | Modify Permission    | edit   | Modify Attribute Value  |
| Mark as Closed | Set End Date and Close Value<br>- Open a Popup and ask for remark    | Modify Permission    | lock   | Close Attribute Value   |
| Remove         | Remove Value                                                          | Delete Permission    | delete | Delete Attribute Value  |

---

### 2.1.3e. DataTable (Values) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Required | Tooltip |
| ---- | --------- | ---------- | ----------------- | ----------------- | ------ | -------- | ------- |
| -    | -         | –          | -                  | -                  | -      | -        | -       |

---

## 2.2 Right Section

### 2.2.1 System Information

| Name        | Data Source      | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                                                  | Tooltip     |
| ----------- | ----------------- | --------- | --------------------------------- | -------- | --------- | ------------ | --------- | ---------------------------------------------------------------- | ------------ |
| Created By  | System Generated  | Text      | User Name                          | No       | Yes       | –            | –         | User who created the Product Attribute                            | -           |
| Created On  | System Generated  | Date Time | DD/MM/YYYY HH:mm                   | No       | Yes       | –            | –         | Date and Time when the Product Attribute was created              | -           |
| Modified By | System Generated  | Text      | User Name                          | No       | Yes       | –            | –         | User who last modified the Product Attribute                      | -           |
| Modified On | System Generated  | Date Time | DD/MM/YYYY HH:mm                   | No       | Yes       | –            | –         | Date and Time when the Product Attribute was last modified        | -           |
