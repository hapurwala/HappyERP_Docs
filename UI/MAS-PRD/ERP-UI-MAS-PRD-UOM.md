#  Unit Of Measurement (UOM) UI/UX Documentation

This document provides UI details of all pages used in the ***MAS-PRD's*** UOM module.

# Related Reference files

specs file : -

---

# Pages

| S. No. | Name             | Type | Purpose                                                                              |
| ------ | ---------------- | ---- | ------------------------------------------------------------------------------------ |
| 1.     | UOM List         | Page | - View all UOMs<br/>- Search/filter UOMs<br/>- Perform quick actions on UOM(s)       |
| 2.     | Add/Modify UOM   | Page | - View details of the UOM<br/>- Add/modify UOM data                                  |

---

# 1. UOM List Page UI 

This page displays a list of UOM in tabular format.

---

### 1.1a. DataTable (UOM List) - Columns

| Header         | Data Source                              | Format      | On Click              | Card Placement | Tooltip / On Hover                           | Inline Edit Component |
| -------------- | ----------------------------------------- | ----------- | --------------------- | -------------- | --------------------------------------------- | ---------------------- |
| Name           | s_prd_uom.`name`                          | String      | Open View/Modify Page | Title          | –                                              | -                      |
| Short Name     | s_prd_uom.`short_name`                    | String      | –                     | Subtitle       | –                                              | -                      |
| Class          | s_prd_uom.`class`                         | Integer     | –                     | Body           | Class of measurement, e.g. weight, volume      | -                      |
| Decimal Places | s_prd_uom.`decimal_places`                | Integer     | -                     | Body           | -                                               | -                      |
| Stage          | s_prd_uom_stage.`name`                    | Stage Badge | -                     | Body           | –                                              | -                      |
| Active         | Derived from `start_date` and `end_date`  | Status      | -                     | Body           | Active since Start Date / Closed on End Date   | -                      |

---


### 1.1b. DataTable (UOM List) - Toolbar Config

| Feature          | Settings | On Click                                                                            |
| ---------------- | -------- | ----------------------------------------------------------------------------------- |
| Search           | Yes      | Search  records based on the entered keyword(s).                                    |
| View Toggle      | Yes      | Switch between list and card view                                                   |
| Column Selection | Yes      | Open column selector to show or hide DataTable columns.                             |
| Group By         | Yes      | Group DataTable records based on the selected column.                               |
| Filter           | Yes      | Open filter panel to apply additional filtering criteria.                           |
| Export           | Yes      | Export the displayed records.                                                       |
| Share            | No       | N/A                                                                                 |
| Full Screen      | Yes      | Toggle the DataTable between normal and full-screen view.                           |
| Add              | when  s_app_role_permission.`can_add = true` for UOM. | Page to Open: `uom` |

---

### 1.1c. DataTable (UOM List) - Table Config

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

### 1.1d. DataTable (UOM List) - RowAction Menu

| Name           | Action                                                  | Visibility Criteria                                       | Icon   | Tooltip |
| -------------- | -------------------------------------------------------- | ----------------------------------------------------------- | ------ | ------- |
| Modify         | Open page `uom`                                          | - Modify permission<br>- Modify allowed on status            | pencil | -       |
| Stage_1        | Set status to Stage_1                                     | - Allowed status based on current status                     | -      | -       |
| Stage_2        | Set status to Stage_2                                     | - Allowed status based on current status                     | -      | -       |
| Undo Stage     | Move back to last Stage                                   | - Permission to set current stage                            | undo   | -       |
| Mark as Closed | - Ask end/close date<br>- Sets s_prd_uom.`end_date`       | - Modify permission<br>- If s_prd_uom.`end_date` = Null      | lock   | -       |
| Mark as Opened | - Sets s_prd_uom.`end_date` = Null                        | - Modify permission<br/>- s_prd_uom.`end_date` <> Null       | unlock | -       |
| Delete         | - Show confirmation message<br>- On Yes, delete           | - Delete permission<br>- Delete allowed on status             | trash  | -       |

---

### 1.1e. DataTable (UOM List) - Filters Fields

| Name  | Component    | Depends On | Possible Values | Default Values | Output    | Required | Tooltip |
| ----- | ------------ | ---------- | ---------------- | --------------- | --------- | -------- | ------- |
| Stage | Multi Select | –          | Possible Stages  | All Open Stage  | stage_ids | -        | -       |

---

# 2. UOM Entry Page UI

Fields in the main form are grouped into different sections.

## Main Form Sections

```
2.1. Left: Main Form
- Basic Details

2.2. Right: System Information
- System Information
```

---

## 2.1 Left Section

### 2.1.1 Basic Details

| Name                | Data Source                | Component   | Component Specific Information         | Required | Read Only | Validations         | On Change | Description                                                                         | Tooltip |
| ------------------- | --------------------------- | ----------- | --------------------------------------- | -------- | --------- | -------------------- | --------- | ------------------------------------------------------------------------------------- | ------- |
| Name                | s_prd_uom.`name`            | Text        | –                                        | Yes      | No        | -                     | –         | Name of the Unit of Measurement                                                          | -       |
| Short Name          | s_prd_uom.`short_name`      | Text        | –                                        | Yes      | No        | -                     | –         | Code/Short Name of the Unit of Measurement                                               | -       |
| Class               | s_prd_uom.`class`           | Text        | Examples: Weight, Volume, Length, Time   | Yes      | No        | -                     | –         | Indicates the measurement category to which the UoM belongs                              | -       |
| Number of Decimals  | s_prd_uom.`decimal_places`  | Number      | Non-negative Integer                     | Yes      | No        | -                     | –         | Number of decimal places used for maintaining and displaying quantities in this UoM      | -       |
| Start Date          | s_prd_uom.`start_date`      | Date Picker | DD/MM/YYYY                               | Yes      | No        | Only Date             | –         | Date from which the UOM becomes active                                                   | -       |
| End Date            | s_prd_uom.`end_date`        | Date Picker | DD/MM/YYYY                               | No       | Yes       | Only Date             | –         | Date on which the UOM was closed                                                          | -       |
| Remarks             | s_prd_uom.`remarks`         | Text Area   | Multi-line                               | No       | No        | Max 255 Characters    | –         | Additional Notes                                                                          | -       |

---

## 2.2 Right Section

### 2.2.1 System Information

| Name        | Data Source      | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                                  | Tooltip |
| ----------- | ----------------- | --------- | --------------------------------- | -------- | --------- | ------------ | --------- | ------------------------------------------------ | ------- |
| Created By  | System Generated  | Text      | User Name                          | No       | Yes       | –            | –         | User who created the UOM                          | -       |
| Created On  | System Generated  | Date Time | DD/MM/YYYY HH:mm                   | No       | Yes       | –            | –         | Date and Time when the UOM was created            | -       |
| Modified By | System Generated  | Text      | User Name                          | No       | Yes       | –            | –         | User who last modified the UOM                    | -       |
| Modified On | System Generated  | Date Time | DD/MM/YYYY HH:mm                   | No       | Yes       | –            | –         | Date and Time when the UOM was last modified      | -       |
