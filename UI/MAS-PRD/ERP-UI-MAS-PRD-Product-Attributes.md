# Product Attribute UI/UX Documentation

Specific attributes are applicable to products based on the industry.

This document provides UI details of all pages used in the ***MAS-PRD's*** Product Attribute module.

---

# 1. Product Attribute List Page UI 

This page displays a list of Product Attribute in tabular format.

---

### 1.1a. DataTable (Product Attribute List) - Columns

A dropdown will be placed above the list. Users will select an attribute here and corresponding data will be displayed in the list below:

| Name       | Content                      | Format  | On Click              | Card Placement | Tooltip/On Hover                         |
| ---------- | ---------------------------- | ------- | --------------------- | -------------- | ---------------------------------------- |
| Name       | s_prd_attribute.`name`       | String  | Open View/Modify Page | Title          | –                                        |
| Short Name | s_prd_attribute.`short_name` | String  | –                     | Body           | –                                        |
| Sequence   | s_prd_attribute.`sequence`   | Integer | –                     | Body           | Controls display order of this attribute |
| Stage      | s_prd_attribute_stage.`name`      | Stage   | Show Stage History    | Body           | –                                        |

---

### 1.1b. DataTable (Product Attribute List) - Toolbar Config

| Feature          | Settings | On Click                                                                                                        |     |
| ---------------- | -------- | --------------------------------------------------------------------------------------------------------------- | --- |
| Search           | Yes      | Search records based on the entered keyword(s).                                                                 |     |
| View Toggle      | Yes      | Switch between list and card.                                                                                   |     |
| Column Selection | Yes      | Open column selector to show or hide DataTable columns.                                                         |     |
| Group By         | Yes      | Group DataTable records based on the selected column.                                                           |     |
| Filter           | Yes      | Open filter panel to apply additional filtering criteria.                                                       |     |
| Export           | Yes      | Export the displayed records.                                                                                   |     |
| Share            | No       | N/A                                                                                                             |     |
| Full Screen      | Yes      | Toggle the DataTable between normal and full-screen view.                                                       |     |
| Add              | Yes      | Open Add Product Attribute Page. Visible only when `s_app_role_permission.can_add = true` for Product Attribute |     |

---

### 1.1c. DataTable (Product Attribute List) - Config

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

### 1.1d. DataTable (Product Attribute List) - RowAction Menu

| Name           | Action                                          | Visibility Criteria                                        | Tooltip |
| -------------- | ----------------------------------------------- | ---------------------------------------------------------- | ------- |
| Modify         | Open page: `product_attribute`                  | - Modify permission<br>- Modify allowed on status          | -       |
| Stage_1        | Set status to Stage_1                           | - Allowed status based on current status                   | -       |
| Stage_2        | Set status to Stage_2                           | - Allowed status based on current status                   | -       |
| Undo Stage     | Move back to last Stage                         | - Permission to set current Stage                          | -       |
| Mark as Closed | - Ask end/close date<br>- Sets end date         | - Modify permission<br>- s_prd_attribute.`end_date` = Null | -       |
| Delete         | - Show confirmation message<br>- On Yes, delete | - Delete permission<br>- Delete allowed on status          | -       |

---

### 1.1e. DataTable (Product Attribute List) - Filters Fields

| Name       | Type         | Possible Values   | Default         |
| ---------- | ------------ | ----------------- | --------------- |
| Status     | Multi Select | Possible statuses | All Open Status |

---

# 2. Product Attribute Entry Page UI

Fields in the main form are grouped into different sections.

## Main Form Sections

- Basic Details
- Applicability
- Values

---
### 2.1 Section (Basic Details) 

| Name/Label | Data Source                  | Component | Component Specific Information | Required | Read Only | Validations            | On Change | Description                              | Tooltip          |
| ---------- | ---------------------------- | --------- | ------------------------------ | -------- | --------- | ---------------------- | --------- | ---------------------------------------- | ---------------- |
| Name       | s_prd_attribute.`name`       | Text      | -                              | Yes      | No        | Length: 3-100          | –         | Name of the Product Attribute            | -                |
| Short Name | s_prd_attribute.`short_name` | Text      | -                              | Yes      | No        | Length: 2-25           | –         | Code/Short Name of the Product Attribute | -                |
|Is Mandatory|s_prd_attribute.`is_mandatory`|Checkbox|Yes / No|No|No|–|–|Indicates whether this Attribute is mandatory while creating Product Varieties|Mandatory Attribute|
| Sequence   | s_prd_attribute.`sequence`   | Number    | Positive Integer               | Yes      | No        | Numeric Value Required | –         | Sequence used for display ordering       | Display Sequence |
|End Date|s_prd_attribute.`end_date`|Date Picker|DD/MM/YYYY|No|Yes|Only Date|–|Date when the Product Attribute was closed|Effective Until|

---

### 2.2 Section (Applicability)

This section contains data in list format

| Name/Label | Data Source                                        | Type/Component | Component Specific Information                           | Required | Read Only | Validations | On Change | Description                                             | Tooltip |
| ---------- | -------------------------------------------------- | -------------- | -------------------------------------------------------- | -------- | --------- | ----------- | --------- | ------------------------------------------------------- | ------- |
| Product(s) | s_prd_attribute_applicable_products[].`product_id` | Checkbox grid  | Displays all available Products as selectable checkboxes | No       | No        | -           | -         | An attribute may be applicable to one or more products. | -¬      |

---

### 2.3 Section (Values)

**Component Type:** DataTable
 
> This section contains data in tabular format.

---

#### 2.3a DataTable (Values) - Columns

| Name/Label | Data Source                            | Format/Component     | Component Specific Information | Required | Read Only | Validations      | On Change | Description                              | Tooltip         |
| ---------- | -------------------------------------- | -------------------- | ------------------------------ | -------- | --------- | ---------------- | --------- | ---------------------------------------- | --------------- |
| Value      | s_prd_attribute_values[].`value`       | String / Text        | Attribute Value                | Yes      | No        | -                | –         | Value of the Product Attribute           | Attribute Value |
| Start Date | s_prd_attribute_values[].`_start_date` | String / Date Picker | DD/MM/YYYY                     | Yes      | No        | Only Date        | –         | Date from which this value is effective  | Effective From  |
| End Date   | s_prd_attribute_values[].`end_date`    | String / Date Picker | DD/MM/YYYY                     | No       | No        | Only Date        | –         | Date until which this value is effective | Effective Until |
| Status     | s_prd_attribute_values[].`status`     | Badge / Dropdown     | Active/Inactive/Closed         | Yes      | Yes       | System Generated | –         | Current status of the Attribute Value    | Value Status    |

---

#### 2.3b DataTable (Values) - Toolbar Config

| Feature     | Settings | On Click |
| ----------- | -------- | -------- |
| Search      | No       | N/A      |
| Filter      | No       | N/A      |
| Export      | No       | N/A      |
| Full Screen | No       | N/A      |
| Add         | No       | N/A      |

---

#### 2.3c DataTable (Values) - Config

| Feature | Settings |
| -------- | -------- |
| Row Selection | No |
| Bulk Actions | No |
| Sticky Header | No |
| Column Resize | No |
| Column Pinning | No |
| Sorting | No |
| Pagination | No |

---

#### 2.3d DataTable (Values) - RowAction Menu

| Name           | Action                                                            | Visibility Criteria | Icon   | Tooltip                |
| -------------- | ----------------------------------------------------------------- | ------------------- | ------ | ---------------------- |
| Modify         | Inline Edit (shows Save/Cancel on the active row)                 | Modify Permission   | edit   | Modify Attribute Value |
| Mark as Closed | Set End Date and Close Value<br>- Open a Popup and ask for remark | Modify Permission   | lock   | Close Attribute Value  |
| Remove         | Remove Value                                                      | Delete Permission   | delete | Delete Attribute Value |

---

### 2.3e. DataTable (Values) - Filters Fields

| Name       | Type         | Possible Values   | Default         |
| ---------- | ------------ | ----------------- | --------------- |
| -     | - | - | - |


---
### 2.4 Right Section (System Information)

| Name/Label  | Data Source      | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                                                | Tooltip     |
| ----------- | ---------------- | --------- | ------------------------------ | -------- | --------- | ----------- | --------- | ---------------------------------------------------------- | ----------- |
| Created By  | System Generated | Text      | User Name                      | No       | Yes       | –           | –         | User who created the Product Attribute                     | Created By  |
| Created On  | System Generated | Date Time | DD/MM/YYYY HH:mm               | No       | Yes       | –           | –         | Date and Time when the Product Attribute was created       | Created On  |
| Modified By | System Generated | Text      | User Name                      | No       | Yes       | –           | –         | User who last modified the Product Attribute               | Modified By |
| Modified On | System Generated | Date Time | DD/MM/YYYY HH:mm               | No       | Yes       | –           | –         | Date and Time when the Product Attribute was last modified | Modified On |

