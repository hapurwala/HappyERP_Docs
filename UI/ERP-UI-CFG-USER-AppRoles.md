# Roles UI/UX Documentation

This document gives UI details of all pages used in Roles module.

# Pages

| S. No. | Name        | Type | Purpose                                                                |
| ------ | ----------- | ---- | ---------------------------------------------------------------------- |
| 1.     | Roles List  | Page | - View all Roles<br/>- Search/filter Roles<br/>- Perform quick actions |
| 2.     | Modify Role | Page | - View role details<br/>- Manage permissions<br/>- View assigned users |

# 1. Roles List Page UI

This page shows list of Roles in tabular format. The roles appearing in the list depend upon current user's permissions and applied filters.

### 1.1a. DataTable (Roles List) - Columns

| Name        | Content                                                                                                                                            | Format      | On Click                    | Card Placement | Tooltip/On Hover                                 |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | --------------------------- | -------------- | ------------------------------------------------ |
| Name        | s_app_role.`name`                                                                                                                                  | String      | Open View/Modify page       | Title          | –                                                |
| App Objects | **Count** of distinct a_app_role_permission.`app_object_id` for current `app_role_id`                                                              | Integer     | –                           | Body           | Number of AppObjects allowed in this Role        |
| Users       | **Count** of documents in m_party where m_party.roles().`app_role_id` = current `app_role_id`                                                      | Integer     | –                           | Body           | Number of Users who have been assigned this role |
| Active      | If s_app_role.`end_date` = null<br/>**Active**<br/>since start_date <br/><br/>If s_app_role.`end_date` <> null<br/>**Closed**<br/>on end_date<br/> | String      | –                           | Body           | –                                                |
| Stage       | s_app_role.stage.`name`                                                                                                                            | Stage Badge | Show workflow stage history | Body           | –                                                |
|             |                                                                                                                                                    |             |                             |                |                                                  |

---

### 1.1b. DataTable (Roles List) - Toolbar Config

| Feature          | Settings       | On Click          |
| ---------------- | -------------- | ----------------- |
| Search           | Yes            | -                 |
| View Toggle      | Yes            | -                 |
| Column Selection | Yes            | -                 |
| Group By         | Yes            | -                 |
| Filter           | Yes            | -                 |
| Export           | Yes            | -                 |
| Share            | No             | -                 |
| Full Screen      | Yes            | -                 |
| Add              | Add permission | Open page: `role` |

---

### 1.1c. DataTable (Roles List) - Config

| Feature        | Settings              |
| -------------- | --------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

---

### 1.1d. DataTable (Roles List) - RowAction Menu

| Name             | Action                                          | Visibility Criteria                                                 | Tooltip |
| ---------------- | ----------------------------------------------- | ------------------------------------------------------------------- | ------- |
| View             | Open page: `role`                               | - View Permission<br/>- No Modify Permission (Due to Role or Stage) | -       |
| Modify           | Open page: `role`                               | - Modify permission<br>- Modify allowed on stage                    | -       |
| Set as <Stage_1> | Set status to Stage_1                           | - Allowed stage based on current stage                              | -       |
| Set as <Stage_2> | Set status to Stage_2                           | - Allowed stage based on current stage                              | -       |
| Undo Stage       | Move back to last stage                         |                                                                     | -       |
| Mark as Closed   | - Ask end/close date<br>- Sets end date         | - Modify permission<br/>- s_app_role.`end_date` = Null              | -       |
| Delete           | - Show confirmation message<br>- On Yes, delete | - Delete permission<br>- Delete allowed on stage                    | -       |

### 1.1e. DataTable (Roles List) - Filters Fields

| Name       | Type         | Possible Values | Default         |
| ---------- | ------------ | --------------- | --------------- |
| Role       | Multi Select | All Roles       | <All>           |
| App Object | Multi Select | All App Objects | <All>           |
| Stage      | Multi Select | All stages      | Draft, Approved |
| Active     | Switch       | Yes/No          | Yes             |

---

# 2. Role Entry Page UI

Fields in the main form are grouped in different sections.

## 2.1 Main Form

### 2.1.1 Section (Basic Details)

| Name/Label | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations   | On Change | Description               | Tooltip         |
| ---------- | ----------- | -------------- | ------------------------------ | -------- | --------- | ------------- | --------- | ------------------------- | --------------- |
| Name       | name        | Text           | -                              | Yes      | No        | Length: 3-100 | –         | Name of the Role          | Role Name       |
| Start Date | start_date  | Date Picker    | DD/MM/YYYY                     | Yes      | No        | Only Date     | –         | Date, when it was started | Role Start Date |
| Remarks    | remarks     | Text Area      | Multi-line                     | No       | No        |               | –         | -                         | -               |

---

### 2.1.2 Section (Permissions)

**Component Type:** DataTable

#### 2.1.2a DataTable (Permissions) - Columns

| Name/Label   | Data Source                                 | Type/Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                                 | Tooltip                     |
| ------------ | ------------------------------------------- | -------------- | ------------------------------ | -------- | --------- | ----------- | --------- | ------------------------------------------- | --------------------------- |
| App Object   | s_app_role_permission.`app_object_id`       | Dropdown       | List of AppObjects             | Yes      | No        | -           | –         | Name of the AppObject                       | Application Object          |
| Can Add      | s_app_role_permission..`can_add`            | Checkbox       | -                              | Yes      | No        | -           | –         | Can create records for this AppObject       | Create Permission           |
| Can View     | s_app_role_permission..`can_view`           | Checkbox       | -                              | Yes      | No        | -           | -         | Can view records for this AppObject         |                             |
| Scope        | s_app_role_permission.`view_scope_category` | Dropdown       | Self, Team, Peer, All          | Yes      | No        | -           | –         | Indicates which records this role can view. | Permission Scope            |
| Stage        | stage_permissions[].`stage_id`              | Dropdown       | Workflow Stages                | Yes      | No        | -           | –         | Name of Stage                               | Workflow Stage              |
| Can View     | stage_permissions[].`can_view`              | Checkbox       | -                              | Yes      | No        | -           | –         | Can view                                    | View Permission             |
| Can Modify   | stage_permissions[].`can_modify`            | Checkbox       | -                              | Yes      | No        | -           | –         | Can modify                                  | Modify Permission           |
| Can Delete   | stage_permissions[].`can_delete`            | Checkbox       | -                              | Yes      | No        | -           | –         | Can delete                                  | Delete Permission           |
| Can Cancel   | stage_permissions[].`can_cancel`            | Checkbox       | -                              | Yes      | No        | -           | –         | Can cancel                                  | Cancel Permission           |
| Can Set      | stage_permissions[].`can_set`               | Checkbox       | -                              | Yes      | No        | -           | –         | Can this role set this stage?               | Stage Assignment Permission |
| Can Rollback | stage_permissions[].`can_rollback`          | Checkbox       | -                              | Yes      | No        | -           | –         | Can this role rollback from this stage?     | Stage Rollback Permission   |

--- 

### 2.1.2b. DataTable (Permissions) - Toolbar Config

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

---

### 2.1.2c. DataTable (Permissions) - Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | No       |
| Pagination     | No       |

#### 2.1.2d DataTable (Permissions) - RowAction Menu

| Name   | Action                          | Visibility Criteria | Tooltip |
| ------ | ------------------------------- | ------------------- | ------- |
| Modify | Inline Edit                     | Modify Permission   | -       |
| Remove | Remove permission configuration | Modify Permission   | -       |

### 2.1.2e. DataTable (Permissions) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
| ---- | --------- | ---------- | --------------- | -------------- | ------ | ------- |
| -    | -         | –          | -               | -              | -      | -       |

---

### 2.1.3 Section (Users)

**Component Type:** DataTable

#### 2.1.3a DataTable (Users) - Columns

| Name/Label | Data Source                                                                      | Format | Component Specific Information | Required | Read Only | Validations | On Change | Description                 | Tooltip               |
| ---------- | -------------------------------------------------------------------------------- | ------ | ------------------------------ | -------- | --------- | ----------- | --------- | --------------------------- | --------------------- |
| User Name  | m_party.`short_name` where m_party.roles().`app_role_id` = current `app_role_id` | String | -                              | No       | Yes       | –           | –         | Assigned User               | User Name             |
| Start Date | m_party.roles().`start_date`                                                     | Date   | -                              | No       | Yes       | –           | –         | Date, role was assigned     | Assignment Start Date |
| End Date   | m_party.roles().`end_date`                                                       | Date   | -                              | No       | Yes       | –           | –         | Date, role assignment ended | Assignment End Date   |

--- 

### 2.1.3b. DataTable (Users) - Toolbar Config

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

---

### 2.1.3c. DataTable (Roles List) - Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | No       |
| Column Resize  | Yes      |
| Column Pinning | No       |
| Sorting        | Yes      |
| Pagination     | No       |

---

#### 2.1.3d DataTable (Users) - RowAction Menu

| Name   | Action                         | Visibility Criteria | Tooltip     |
| ------ | ------------------------------ | ------------------- | ----------- |
| Remove | Remove assigned user from role | Modify Permission   | Remove User |

---

### 2.1.3e. DataTable (Users) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
| ---- | --------- | ---------- | --------------- | -------------- | ------ | ------- |
| -    | -         | –          | -               | -              | -      | -       |
