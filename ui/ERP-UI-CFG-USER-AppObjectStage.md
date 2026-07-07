# Workflow Stage UI/UX Documentation

This document gives UI details of all pages used in Workflow Stage module.

# Pages

| S. No. | Name                      | Type  | Purpose                                                                                        |
| ------ | ------------------------- | ----- | ---------------------------------------------------------------------------------------------- |
| 1.     | Workflow Stage List       | Page  | - View all Workflow Stage configurations- Search/filter Workflow stages- Perform quick actions |
| 2.     | Add/Modify Workflow Stage | Page  | - View Workflow stage configuration- Add/modify Workflow stage details and workflow stages     |
| 3.     | Add/Modify Stage          | Popup | - Add/modify individual workflow stage configuration                                           |

---

# 1. Workflow Stage List Page UI

This page shows list of Workflow Stage configurations in tabular format. The records appearing in the list depend upon current user's permissions and applied filters.

---

### 1.1a. DataTable (Workflow Stage List) - Columns

| Name              | Content                 | Type    | On Click                                       | Card Placement | Tooltip / On Hover                 |
| ----------------- | ----------------------- | ------- | ---------------------------------------------- | -------------- | ---------------------------------- |
| Application       | Name of Application     | Text    | –                                              | Title          | Application Name                   |
| Object            | Name of AppObject       | Status  | Page to open: `Workflow_stage`Mode : View/Edit | Subtitle       | View Workflow Stage Details        |
| Financial Voucher | Link Financial Voucher? | Boolean | –                                              | Body           | Financial Voucher Creation Enabled |
| Stock Voucher     | Link Stock Voucher?     | Boolean | –                                              | Body           | Stock Voucher Creation Enabled     |

---

### 1.1b. DataTable (Workflow Stage List) - Toolbar Config

| Feature          | Settings                           | On Click                                                                               |
| ---------------- | ---------------------------------- | -------------------------------------------------------------------------------------- |
| Search           | Yes                                | Search Workflow stage records based on the entered keyword(s).                         |
| View Toggle      | Yes                                | Switch between available DataTable view layouts.                                       |
| Column Selection | Yes                                | Open column selector to show or hide DataTable columns.                                |
| Group By         | Yes                                | Group DataTable records based on the selected column.                                  |
| Filter           | Yes                                | Open filter panel to apply additional filtering criteria.                              |
| Export           | YesFilename: `Workflow-stage-list` | Export the displayed records using the filename `Workflow-stage-list`.                 |
| Share            | Yes                                | Generate and share the current DataTable view, including applied filters and settings. |
| Full Screen      | Yes                                | Toggle the DataTable between normal and full-screen view.                              |
| Add              | No                                 | N/A                                                                                    |

---

### 1.1c. DataTable (Workflow Stage List) - Config

| Feature        | Settings         |
| -------------- | ---------------- |
| Row Selection  | No               |
| Bulk Actions   | No               |
| Sticky Header  | Yes              |
| Column Resize  | Yes              |
| Column Pinning | Yes              |
| Sorting        | Yes              |
| Pagination     | YesPage Size: 20 |

---

### 1.1d. DataTable (Workflow Stage List) - RowAction Menu

| Name   | Action                                 | Visibility Criteria                       | Icon   | Tooltip                           |
| ------ | -------------------------------------- | ----------------------------------------- | ------ | --------------------------------- |
| Modify | Open page: `Workflow_stage`Mode : Edit | Modify PermissionModify allowed on status | pencil | Edit Workflow stage configuration |

---

### 1.1e. DataTable (Workflow Stage List) - Filters Fields

| Name        | Component    | Depends On | Possible Values  | Default Values | Output          | Tooltip                                                                  |
| ----------- | ------------ | ---------- | ---------------- | -------------- | --------------- | ------------------------------------------------------------------------ |
| Application | Multi Select | –          | All Applications | All            | application_ids | Select one or more applications to filter Workflow stage configurations. |

---

# 2. Workflow Stage Entry Page UI

Fields in the main form are grouped in different sections.

## 2.1 Left: Main Form

- Basic Details

- Stages

## 2.2 Right: Widgets

- Workflow Timeline

---

## 2.1.1 Section (Basic Details)

> This section is always read-only and cannot be modified.

| Name/Label             | Data Source              | Type/Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                                    | Tooltip                               |
| ---------------------- | ------------------------ | -------------- | ------------------------------ | -------- | --------- | ----------- | --------- | ---------------------------------------------- | ------------------------------------- |
| Name                   | name                     | Text           | Length 3-100                   | Yes      | Yes       | Mandatory   | –         | Name of AppObject                              | Workflow Object Name                  |
| Application            | application_name         | Text           | Length 3-100                   | Yes      | Yes       | Mandatory   | –         | Name of Application                            | Application Name                      |
| Link Financial Voucher | create_financial_voucher | Toggle         | Yes/No                         | Yes      | Yes       | Mandatory   | –         | Create Financial Voucher at this Workflow flow | Financial Voucher Integration Enabled |
| Link Stock Voucher     | create_stock_voucher     | Toggle         | Yes/No                         | Yes      | Yes       | Mandatory   | –         | Create Stock Voucher at this Workflow flow     | Stock Voucher Integration Enabled     |

---

## 2.1.2 Section (Stages)

**Component Type:** DataTable

### 2.1.2a DataTable (Stages) - Columns

| Name/Label   | Data Source  | Type/Component | Component Specific Information | Required | Read Only | Validations     | On Change        | Description                            | Tooltip                    |
| ------------ | ------------ | -------------- | ------------------------------ | -------- | --------- | --------------- | ---------------- | -------------------------------------- | -------------------------- |
| Sequence     | sequence     | Number         | Positive Integer               | Yes      | No        | Unique Sequence | –                | Sequence of the stage                  | Stage execution order      |
| Name         | stage_name   | Status         | Clickable                      | Yes      | No        | Mandatory       | Open Stage Popup | Name of the stage                      | Open stage details         |
| Internal     | is_internal  | Boolean        | Yes/No                         | Yes      | No        | Mandatory       | –                | Is this stage internal?                | Internal workflow stage    |
| Allow Modify | allow_edit   | Boolean        | Yes/No                         | Yes      | No        | Mandatory       | –                | Can record be modified at this stage?  | Modify permission at stage |
| Allow Delete | allow_delete | Boolean        | Yes/No                         | Yes      | No        | Mandatory       | –                | Can record be deleted at this stage?   | Delete permission at stage |
| Allow Cancel | allow_cancel | Boolean        | Yes/No                         | Yes      | No        | Mandatory       | –                | Can record be cancelled at this stage? | Cancel permission at stage |

---

### 2.1.2b DataTable (Stages) - Toolbar Config

| Feature     | Settings | On Click                                                  |
| ----------- | -------- | --------------------------------------------------------- |
| Search      | No       | N/A                                                       |
| Filter      | No       | N/A                                                       |
| Export      | No       | N/A                                                       |
| Full Screen | Yes      | Toggle the DataTable between normal and full-screen view. |
| Add         | Yes      | Add a new workflow stage.                                 |

---

### 2.1.2c DataTable (Stages) - RowAction Menu

| Name   | Action                        | Visibility Criteria                                   | Icon   | Tooltip                  |
| ------ | ----------------------------- | ----------------------------------------------------- | ------ | ------------------------ |
| Modify | Open Stage Popup In Edit Mode | Visible for all stages                                | edit   | Edit stage configuration |
| Delete | Remove Stage                  | Visible for all stages except system protected stages | delete | Delete selected stage    |

---

## 2.2 Right Side Widgets

### 2.2.1 Workflow Timeline

**Component to use:** StageHistoryViewer

| Name/Label | Data Source | Component                   | Tooltip                                                  |
| ---------- | ----------- | --------------------------- | -------------------------------------------------------- |
| Stage      | stage_name  | Text                        | Workflow stage assigned to the record                    |
| Set At     | set_at      | Date Time                   | Date and time when the stage was assigned                |
| Set By     | set_by      | User                        | User who assigned or changed the stage                   |
| Remarks    | remarks     | Text Area                   | Comments or remarks entered during the stage transition  |
| Attachment | attachment  | File Viewer / Download Link | Supporting document attached during the stage transition |

---

# 3. Stage Entry Popup UI

## 3.1 Section (Basic Details)

| Name/Label | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations                | On Change | Description    | Tooltip                                |
| ---------- | ----------- | -------------- | ------------------------------ | -------- | --------- | -------------------------- | --------- | -------------- | -------------------------------------- |
| Name       | name        | Text           | Length 3-100                   | Yes      | No        | Mandatory, Length 3-100    | –         | Stage Name     | Enter workflow stage name              |
| Sequence   | sequence    | Number         | Positive Integer               | Yes      | No        | Mandatory, Unique Sequence | –         | Stage Sequence | Execution order of stage               |
| Internal   | internal    | Toggle         | Yes/No                         | Yes      | No        | Mandatory                  | –         | Internal Stage | Indicates whether stage is internal    |
| Optional   | optional    | Toggle         | Yes/No                         | Yes      | No        | Mandatory                  | –         | Optional Stage | Indicates whether stage can be skipped |

---

## 3.2 Section (Permissions)

| Name/Label   | Data Source  | Type/Component | Component Specific Information | Required | Read Only | Validations | On Change | Description       | Tooltip                                  |
| ------------ | ------------ | -------------- | ------------------------------ | -------- | --------- | ----------- | --------- | ----------------- | ---------------------------------------- |
| Allow Edit   | allow_edit   | Toggle         | Yes/No                         | Yes      | No        | Mandatory   | –         | Edit Permission   | Can AppObject be modified at this stage  |
| Allow Delete | allow_delete | Toggle         | Yes/No                         | Yes      | No        | Mandatory   | –         | Delete Permission | Can AppObject be deleted at this stage   |
| Allow Cancel | allow_cancel | Toggle         | Yes/No                         | Yes      | No        | Mandatory   | –         | Cancel Permission | Can AppObject be cancelled at this stage |

---

## 3.3 Section (Others)

| Name/Label               | Data Source              | Type/Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                | Tooltip                                             |
| ------------------------ | ------------------------ | -------------- | ------------------------------ | -------- | --------- | ----------- | --------- | -------------------------- | --------------------------------------------------- |
| Remarks Mandatory        | remarks_mandatory        | Toggle         | Yes/No                         | Yes      | No        | Mandatory   | –         | Remarks Required           | Remarks mandatory while setting this stage          |
| Create Financial Voucher | create_financial_voucher | Toggle         | Yes/No                         | Yes      | No        | Mandatory   | –         | Financial Voucher Creation | Create financial voucher when this stage is reached |
| Create Stock Voucher     | create_stock_voucher     | Toggle         | Yes/No                         | Yes      | No        | Mandatory   | –         | Stock Voucher Creation     | Create stock voucher when this stage is reached     |
