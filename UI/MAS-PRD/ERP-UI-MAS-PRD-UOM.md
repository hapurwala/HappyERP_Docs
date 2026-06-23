#  Unit Of Measurement (UOM) UI/UX Documentation

Unit of Measurement is used to specify the quantity of the products. E.g. Kg, Mtr, Ltr, Nos are some UoMs

This document provides UI details of all pages used in the ***MAS-PRD's*** UOM module.

---

# 1. UOM List Page UI 

This page displays a list of UOM in tabular format.

---

### 1.1a. DataTable (UOM List) - Columns

> Only those which have active status

| Name           | Content                                  | Format      | On Click              | Card Placement | Tooltip/On Hover                             |     |
| -------------- | ---------------------------------------- | ----------- | --------------------- | -------------- | -------------------------------------------- | --- |
| Name           | s_prd__uom.`name`                        | String      | Open View/Modify Page | Title          | –                                            |     |
| Short Name     | s_prd_uom.`short_name`                   | String      | –                     | Subtitle       | –                                            |     |
| Class          | s_prd_uom.`class`                        | Integer     | –                     | Body           | Class of measurement, e.g. weight, volume    |     |
| Decimal Places |                                          | Integer     | -                     | Body           | Decimal places quantities are tracked to     |     |
| Stage          | s_pd_uom_stage.`name`                    | Stage Badge | -                     | Body           | –                                            |     |
| Active         | Derived from `start_date` and `end_date` | Status      | -                     | Body           | Active since Start Date / Closed on End Date |     |

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
| Add              | Yes       | Open Add UOM Page. Visible only when `s_app_role_permission.can_add = true` for UOM |

---

### 1.1c. DataTable (UOM List) - Config

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

### 1.1d. DataTable (UOM List) - RowAction Menu

| Name           | Action                                          | Visibility Criteria                               | Tooltip |
| -------------- | ----------------------------------------------- | ------------------------------------------------- | ------- |
| Modify         | Open page: `uom`                                | - Modify permission<br>- Modify allowed on status | -       |
| Stage_1        | Set status to Stage_1                           | - Allowed status based on current status          | -       |
| Stage_2        | Set status to Stage_2                           | - Allowed status based on current status          | -       |
| Undo Stage     | Move back to last Stage                         | - Permission to set current Stage                 | -       |
| Mark as Closed | - Ask end/close date<br>- Sets end date         | - Modify permission                               | -       |
| Delete         | - Show confirmation message<br>- On Yes, delete | - Delete permission<br>- Delete allowed on status | -       |

---

### 1.1e. DataTable (UOM List) - Filters Fields

| Name       | Type         | Possible Values   | Default         |
| ---------- | ------------ | ----------------- | --------------- |
| Status     | Multi Select | Possible statuses | All Open Status |

---

# 2. UOM Entry Page UI

Fields in the main form are grouped into different sections.

## Main Form Sections

- Basic Details

### 2.1.1 Section (Basic Details)

| Name/Label         | Data Source                | Component | Component Specific Information         | Required | Read Only | Validations | On Change | Description                                                                         | Tooltip        |
| ------------------ | -------------------------- | --------- | -------------------------------------- | -------- | --------- | ----------- | --------- | ----------------------------------------------------------------------------------- | -------------- |
| Name               | s_prd_uom.`name`           | Text      | –                                      | Yes      | No        | -           | –         | Name of the Unit of Measurement                                                     | UoM Name       |
| Short Name         | s_prd_uom.`short_name`     | Text      | –                                      | Yes      | No        | -           | –         | Code/Short Name of the Unit of Measurement                                          | UoM Code       |
| Class              | s_prd_uom.`class`          | Text      | Examples: Weight, Volume, Length, Time | Yes      | No        | -           | –         | Indicates the measurement category to which the UoM belongs                         | UoM Class      |
| Number of Decimals | s_prd_uom.`decimal_places` | Number    | Non-negative Integer                   | Yes      | No        | -           | –         | Number of decimal places used for maintaining and displaying quantities in this UoM | Decimal Places |
| Start Date | s_prd_uom.`start_date` | Date Picker/ System generated | DD/MM/YYYY                     | Yes      | No        | Only Date          | –         | Date from which the UOM becomes active | Effective From  |
| End Date   | s_prd_uom.`end_date`   | System generated| DD/MM/YYYY                     | No       | Yes       | Only Date          | –         | Date on which the UOM was closed       | Effective Until |
| Remarks    | s_prd_uom.`remarks`    | Text Area   | Multi-line                     | No       | No        | Max 255 Characters | –         | Additional Notes                       | Remarks         |
|            |                        |             |                                |          |           |                    |           |                                        |                 |

---

### 2.1.2 Section (System Information)

|Name/Label|Data Source|Component|Component Specific Information|Required|Read Only|Validations|On Change|Description|Tooltip|
|---|---|---|---|---|---|---|---|---|---|
|Modified By|System Generated|Text|User Name|No|Yes|–|–|User who last modified the UOM|Modified By|
|Modified On|System Generated|Date Time|DD/MM/YYYY HH:mm|No|Yes|–|–|Date and Time when the UOM was last modified|Modified On|
