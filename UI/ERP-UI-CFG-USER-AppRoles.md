# Roles UI/UX Documentation

This document gives UI details of all pages used in Roles module.

> **Revisions**
> 
> Checked by NKS on 27-Jun-2026

# Pages

| S. No. | Name        | Type | Purpose                                                                |
| ------ | ----------- | ---- | ---------------------------------------------------------------------- |
| 1.     | Roles List  | Page | - View all Roles<br/>- Search/filter Roles<br/>- Perform quick actions |
| 2.     | Modify Role | Page | - View role details<br/>- Manage permissions<br/>- View assigned users |

# 1. Roles List Page UI

This page shows list of Roles in tabular format. The roles appearing in the list depend upon current user's permissions and applied filters.

## 1.1. Roles List

**Component Type:** DataTable

### 1.1a. DataTable (Roles List) - Columns

| Header      | Data Source                                                                                                  | Format      | On Click                    | Card Placement | Tooltip/On Hover                                                                                                     |
| ----------- | ------------------------------------------------------------------------------------------------------------ | ----------- | --------------------------- | -------------- | -------------------------------------------------------------------------------------------------------------------- |
| Name        | s_app_role.`name`                                                                                            | String      | Open View/Modify page       | Title          | –                                                                                                                    |
| App Objects | **Count** of distinct a_app_role_permission.`app_object_id` for current `app_role_id`                        | Integer     | –                           | Body           | Number of AppObjects allowed in this Role                                                                            |
| Users       | **Count** of documents in m_party where m_party.roles().`app_role_id` = current `app_role_id`                | Integer     | –                           | Body           | Number of Users who have been assigned this role                                                                     |
| Active      | If s_app_role.`end_date` = null<br/>**Active**<br/><br/>If s_app_role.`end_date` <> null<br/>**Closed**<br/> | String      | –                           | Body           | If s_app_role.`end_date` = null<br/>since start_date <br/><br/>If s_app_role.`end_date` <> null<br/>on end_date<br/> |
| Stage       | s_app_role.stage.`name`                                                                                      | Stage Badge | Show workflow stage history | Body           | –                                                                                                                    |

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

### 1.1c. DataTable (Roles List) - Table Config

| Feature        | Settings              |
| -------------- | --------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

### 1.1d. DataTable (Roles List) - RowAction Menu

| Name             | Action                                                | Visibility Criteria                                                 | Icon   | Tooltip |
| ---------------- | ----------------------------------------------------- | ------------------------------------------------------------------- | ------ | ------- |
| View             | Open page: `role`                                     | - View Permission<br/>- No Modify Permission (Due to Role or Stage) | eye    | -       |
| Modify           | Open page: `role`                                     | - Modify permission<br>- Modify allowed on stage                    | pencil | -       |
| Set as <Stage_1> | Set stage to Stage_1                                  | - Allowed stage based on current stage                              | -      | -       |
| Set as <Stage_2> | Set stage to Stage_2                                  | - Allowed stage based on current stage                              | -      | -       |
| Undo Stage       | Move back to last stage                               | Permission to rollback current status                               | undo   | -       |
| Mark as Closed   | - Ask end/close date<br>- Sets s_app_role.`end_ date` | - Modify permission<br/>- s_app_role.`end_date` = Null              | lock   | -       |
| Mark as Opened   | - Sets s_app_role.`end_ date` = Null                  | - Modify permission<br/>- s_app_role.`end_date` <> Null             | unlock | -       |
| Delete           | - Show confirmation message<br>- On Yes, delete       | - Delete permission<br>- Delete allowed on stage                    | delete | -       |

### 1.1e. DataTable (Roles List) - Filters Fields

| Name       | Component    | Depends On | Possible Values              | Default Values  | Output                                                                  | Required | Tooltip |
| ---------- | ------------ | ---------- | ---------------------------- | --------------- | ----------------------------------------------------------------------- | -------- | ------- |
| Role       | Multi Select | -          | All Roles                    | <All>           | Comma separated `app_role_id`<br/>0 for <All><br/>"" for <None>         | -        | -       |
| App Object | Multi Select | -          | All App Objects              | <All>           | Comma separated `app_object_id`<br/>0 for <All><br/>"" for <None>       | -        | -       |
| Stage      | Multi Select | -          | All workflow stages of roles | Draft, Approved | Comma separated `app_object_stage_id`<br/>0 for <All><br/>"" for <None> | -        | -       |
| Active     | Switch       | -          | Yes/No                       | Yes             | 0: No<br/>1: Yes                                                        | Yes      | -       |

# 2. Role Entry Page UI

Fields in the main form are grouped in different sections.

## 2.1. Section (Basic Details)

| Name       | Data Source  | Component      | Component Specific Information          | Required | Read Only | Validations   | On Change | Description               | Tooltip         |
| ---------- | ------------ | -------------- | --------------------------------------- | -------- | --------- | ------------- | --------- | ------------------------- | --------------- |
| Name       | `name`       | Text           | -                                       | Yes      | No        | Length: 3-100 | -         | Name of the Role          | Role Name       |
| Start Date | `start_date` | DatePicker     | - Only Date<br/>- Format: As per Config | Yes      | No        | -             | -         | Date, when it was started | Role Start Date |
| Remarks    | `remarks`    | RichTextEditor | -                                       | No       | No        | -             | -         | -                         | -               |

## 2.2. Section (Permissions)

**Component Type:** DataTable

### 2.2a DataTable (Permissions) - Columns

| Header       | Data Source                                 | Format   | On Click | Card Placement | Tooltip / On Hover                          | Inline Edit Component                                      |
| ------------ | ------------------------------------------- | -------- | -------- | -------------- | ------------------------------------------- | ---------------------------------------------------------- |
| App Object   | s_app_role_permission.`app_object_id`       | String   | –        | Title          | Application Object                          | Select<br/>Options: All AppObjects                         |
| Can Add      | s_app_role_permission..`can_add`            | Checkbox | –        |                | Can create records for this AppObject       | Checkbox                                                   |
| Can View     | s_app_role_permission..`can_view`           | Checkbox | -        |                | Can view records for this AppObject         | Checkbox                                                   |
| Scope        | s_app_role_permission.`view_scope_category` | String   | –        | Subtitle       | Indicates which records this role can view. | Select<br/>Multi-select<br/>Options: Self, Team, Peer, All |
| Stage        | stage_permissions[].`stage_id`              | String   | –        |                | Name of Stage                               | Select<br/>Options: All Stages of selected AppObject       |
| Can View     | stage_permissions[].`can_view`              | Checkbox | –        |                | Can view                                    | Checkbox                                                   |
| Can Modify   | stage_permissions[].`can_modify`            | Checkbox | –        |                | Can modify                                  | Checkbox                                                   |
| Can Delete   | stage_permissions[].`can_delete`            | Checkbox | –        |                | Can delete                                  | Checkbox                                                   |
| Can Cancel   | stage_permissions[].`can_cancel`            | Checkbox | –        |                | Can cancel                                  | Checkbox                                                   |
| Can Set      | stage_permissions[].`can_set`               | Checkbox | –        |                | Can this role set this stage?               | Checkbox                                                   |
| Can Rollback | stage_permissions[].`can_rollback`          | Checkbox | –        |                | Can this role rollback from this stage?     | Checkbox                                                   |

### 2.2b. DataTable (Permissions) - Toolbar Config

| Feature          | Settings | On Click         |
| ---------------- | -------- | ---------------- |
| Search           | No       | -                |
| View Toggle      | No       | -                |
| Column Selection | No       | -                |
| Group By         | No       | -                |
| Filter           | No       | -                |
| Export           | No       | -                |
| Share            | No       | -                |
| Full Screen      | No       | -                |
| Add              | Yes      | Adds a blank row |

### 2.2c. DataTable (Permissions) - Table Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | No       |
| Pagination     | No       |

### 2.2d DataTable (Permissions) - RowAction Menu

| Name   | Action                          | Visibility Criteria       | Icon   | Tooltip |
| ------ | ------------------------------- | ------------------------- | ------ | ------- |
| Modify | Inline Edit                     | Modify Permission on Role | pencil | -       |
| Remove | Remove permission configuration | Modify Permission on Role | delete | -       |

### 2.2e. DataTable (Permissions) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Required | Tooltip |
| ---- | --------- | ---------- | --------------- | -------------- | ------ | -------- | ------- |
| -    | -         | –          | -               | -              | -      | -        | -       |

## 2.3 Section (Users)

**Component Type:** DataTable

### 2.3a DataTable (Users) - Columns

| Header     | Data Source                                                                      | Format | On Click | Card Placement | Tooltip / On Hover    | Inline Edit Component                                                  |
| ---------- | -------------------------------------------------------------------------------- | ------ | -------- | -------------- | --------------------- | ---------------------------------------------------------------------- |
| User Name  | m_party.`short_name` where m_party.roles().`app_role_id` = current `app_role_id` | String | –        | Title          | User Name             | Select<br/>Mandatory<br/>Options: All Users except already in the list |
| Start Date | m_party.roles().`start_date`                                                     | Date   | –        |                | Assignment Start Date | DatePicker<br/>Mandatory<br/>Date should be >= Today                   |
| End Date   | m_party.roles().`end_date`                                                       | Date   | –        |                | Assignment End Date   | DatePicker<br/>Optional<br/>Date should be >= Today                    |

### 2.3b. DataTable (Users) - Toolbar Config

| Feature          | Settings | On Click       |
| ---------------- | -------- | -------------- |
| Search           | No       | -              |
| View Toggle      | No       | -              |
| Column Selection | No       | -              |
| Group By         | No       | -              |
| Filter           | No       | -              |
| Export           | No       | -              |
| Share            | No       | -              |
| Full Screen      | No       | -              |
| Add              | Yes      | Adds blank row |

### 2.3c. DataTable (Roles List) - Table Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | No       |
| Column Resize  | Yes      |
| Column Pinning | No       |
| Sorting        | Yes      |
| Pagination     | No       |

### 2.3d DataTable (Users) - RowAction Menu

| Name   | Action                         | Visibility Criteria | Icon   | Tooltip               |
| ------ | ------------------------------ | ------------------- | ------ | --------------------- |
| Modify | Inline Edit                    | Modify Permission   | pencil | Modify Start/End Date |
| Remove | Remove assigned user from role | Modify Permission   | delete | Remove User           |

### 2.3e. DataTable (Users) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Required | Tooltip |
| ---- | --------- | ---------- | --------------- | -------------- | ------ | -------- | ------- |
| -    | -         | –          | -               | -              | -      | -        | -       |
