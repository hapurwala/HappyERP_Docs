# Roles UI/UX Documentation

This document gives UI details of all pages used in Roles module.

# Pages

| S. No. | Name | Type | Purpose |
| ------ | ---- | ---- | ------- |
| 1. | Roles List | Page | - View all Roles<br/>- Search/filter Roles<br/>- Perform quick actions |
| 2. | Modify Role | Page | - View role details<br/>- Manage permissions<br/>- View assigned users |

---

# 1. Roles List Page UI

This page shows list of Roles in tabular format. The roles appearing in the list depend upon current user's permissions and applied filters.

---
### 1.1a. DataTable (Roles List) - Columns

| Name        | Content                                          | Type   | On Click              | Card Placement | Tooltip/On Hover |
| ----------- | ------------------------------------------------ | ------ | --------------------- | -------------- | ---------------- |
| Name        | Name of the Role                                 | Text   | Open View/Modify page | Title          | –                |
| App Objects | Number of AppObjects allowed in this Role        | Number | –                     | Body           | –                |
| Users       | Number of Users who have been assigned this role | Number | –                     | Body           | –                |
| Start       | Since when it was started                        | Date   | –                     | Body           | –                |
| Status      | Current status of the Role                       | Status | Show status history   | Body           | –                |
|             |                                                  |        |                       |                |                  |

---

### 1.1b. DataTable (Roles List) - Toolbar Config

| Feature | Settings | On Click |
| -------- | -------- | -------- |
| Search | Yes | Search Role records based on the entered keyword(s). |
| View Toggle | Yes | Switch between available DataTable view layouts. |
| Column Selection | Yes | Open column selector to show or hide DataTable columns. |
| Group By | Yes | Group DataTable records based on the selected column. |
| Filter | Yes | Open filter panel to apply additional filtering criteria. |
| Export | Yes | Export the displayed records. |
| Share | No | N/A |
| Full Screen | Yes | Toggle the DataTable between normal and full-screen view. |
| Add | Yes | Open Add Role page. |

---

### 1.1c. DataTable (Roles List) - Config

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

### 1.1d. DataTable (Roles List) - RowAction Menu

| Name           | Action                                          | Visibility Criteria                               | Tooltip                     |
| -------------- | ----------------------------------------------- | ------------------------------------------------- | --------------------------- |
| Modify         | Open View/Modify page                           | - Modify permission<br>- Modify allowed on status | View or modify role details |
| Stage_1        | Set status to Stage_1                           | - Allowed status based on current status          | Move role to Stage_1        |
| Stage_2        | Set status to Stage_2                           | - Allowed status based on current status          | Move role to Stage_2        |
| Undo Status    | Move back to last status                        | - Permission to set current status                | Revert to previous status   |
| Mark as Closed | - Ask end/close date<br>- Sets end date         | - Modify permission                               | Close role and set end date |
| Delete         | - Show confirmation message<br>- On Yes, delete | - Delete permission<br>- Delete allowed on status | Delete role                 |

---

### 1.1e. DataTable (Roles List) - Filters Fields

| Name       | Type         | Possible Values   | Default         |
| ---------- | ------------ | ----------------- | --------------- |
| Status     | Multi Select | Possible statuses | All Open Status |
| App Object | Multi Select | App Objects       | All             |
| Role       | Multi Select | Roles             | All             |

---

# 2. Role Entry Page UI

Fields in the main form are grouped in different sections.

## 2.1 Main Form

### 2.1.1 Section (Basic Details)

| Name/Label | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations   | On Change | Description                        | Tooltip         |
| ---------- | ----------- | -------------- | ------------------------------ | -------- | --------- | ------------- | --------- | ---------------------------------- | --------------- |
| Name       | name        | Text           | Length 3-100                   | Yes      | No        | Length: 3-100 | –         | Name of the Role                   | Role Name       |
| Start Date | start_date  | Date Picker    | DD/MM/YYYY                     | Yes      | No        | Only Date     | –         | Date, when it was started/launched | Role Start Date |
| Remarks    | remarks     | Text Area      | Multi-line                     | No       | No        | Optional      | –         | Additional Notes                   | Remarks         |

---

### 2.1.2 Section (Permissions)

**Component Type:** DataTable

#### 2.1.2a DataTable (Permissions) - Columns

| Name/Label | Data Source   | Type/Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                                                | Tooltip                     |
| ---------- | ------------- | -------------- | ------------------------------ | -------- | --------- | ----------- | --------- | ---------------------------------------------------------- | --------------------------- |
| App Object | app_object_id | Dropdown       | List of AppObjects             | Yes      | No        | -           | –         | Name of the AppObject                                      | Application Object          |
| Can Add    | can_add       | Boolean        | Yes/No                         | Yes      | No        | -           | –         | Can create records for this AppObject                      | Create Permission           |
| Scope      | scope         | Dropdown       | Self, Team, All                | Yes      | No        | -           | –         | Indicate on which records above permissions are applicable | Permission Scope            |
| Stage      | stage_id      | Dropdown       | Workflow Stages                | Yes      | No        | -           | –         | Name of Stage                                              | Workflow Stage              |
| Can Set    | can_set       | Boolean        | Yes/No                         | Yes      | No        | -           | –         | Can this role set this stage?                              | Stage Assignment Permission |
| View       | can_view      | Boolean        | Yes/No                         | Yes      | No        | -           | –         | Can view                                                   | View Permission             |
| Modify     | can_modify    | Boolean        | Yes/No                         | Yes      | No        | -           | –         | Can modify                                                 | Modify Permission           |
| Delete     | can_delete    | Boolean        | Yes/No                         | Yes      | No        | -           | –         | Can delete                                                 | Delete Permission           |
| Cancel     | can_cancel    | Boolean        | Yes/No                         | Yes      | No        | -           | –         | Can cancel                                                 | Cancel Permission           |

--- 

### 2.1.2b. DataTable (Permissions) - Toolbar Config

| Feature          | Settings | On Click            |
| ---------------- | -------- | ------------------- |
| Search           | No       | N/A                 |
| View Toggle      | No       | N/A                 |
| Column Selection | No       | N/A                 |
| Group By         | No       | N/A                 |
| Filter           | No       | N/A                 |
| Export           | No       | N/A                 |
| Share            | No       | N/A                 |
| Full Screen      | No       | N/A                 |

---

### 2.1.2c. DataTable (Permissions) - Config

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

#### 2.1.2d DataTable (Permissions) - RowAction Menu

|Name|Action|Visibility Criteria|Tooltip|
|---|---|---|---|
|Modify|Modify permission configuration|Modify Permission|Modify permission|
|Remove|Remove permission configuration|Modify Permission|Remove permission|

---

### 2.1.2e. DataTable (Permissions) - Filters Fields

| Name        | Component    | Depends On | Possible Values  | Default Values | Output          | Tooltip                                                                  |
| ----------- | ------------ | ---------- | ---------------- | -------------- | --------------- | ------------------------------------------------------------------------ |
| - | - | –          | - | -            | - | - |

---

### 2.1.3 Section (Users)

**Component Type:** DataTable

#### 2.1.3a DataTable (Users) - Columns

|Name/Label|Data Source|Type/Component|Component Specific Information|Required|Read Only|Validations|On Change|Description|Tooltip|
|---|---|---|---|---|---|---|---|---|---|
|User Name|user_name|Text|Length 3-100|No|Yes|–|–|Assigned User|User Name|
|Start Date|start_date|Date|DD/MM/YYYY|No|Yes|–|–|Date role was assigned|Assignment Start Date|
|End Date|end_date|Date|DD/MM/YYYY|No|Yes|–|–|Date role assignment ended|Assignment End Date|

--- 

### 2.1.3b. DataTable (Users) - Toolbar Config

| Feature          | Settings | On Click            |
| ---------------- | -------- | ------------------- |
| Search           | No       | N/A                 |
| View Toggle      | No       | N/A                 |
| Column Selection | No       | N/A                 |
| Group By         | No       | N/A                 |
| Filter           | No       | N/A                 |
| Export           | No       | N/A                 |
| Share            | No       | N/A                 |
| Full Screen      | No       | N/A                 |

---

### 2.1.3c. DataTable (Roles List) - Config

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

#### 2.1.3d DataTable (Users) - RowAction Menu

|Name|Action|Visibility Criteria|Tooltip|
|---|---|---|---|
|Remove|Remove assigned user from role|Modify Permission|Remove User|

---

### 2.1.3e. DataTable (Users) - Filters Fields

| Name        | Component    | Depends On | Possible Values  | Default Values | Output          | Tooltip                                                                  |
| ----------- | ------------ | ---------- | ---------------- | -------------- | --------------- | ------------------------------------------------------------------------ |
| - | - | –          | - | -            | - | - |

