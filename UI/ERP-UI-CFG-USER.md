# Users UI/UX Documentation

This document gives UI details of all pages used in Users module.

# Pages

| S. No. | Name | Type | Purpose |
| ------- | ---- | ---- | ------- |
| 1. | Users List | Page | - View all Users<br/>- Search/filter Users<br/>- Perform quick actions |
| 2. | Modify User | Page | - View user details<br/>- Modify user information and role assignments |

---

# 1. Users List Page UI

This page shows list of users in tabular format. The users appearing in the list depend upon current user's permissions and applied filters.

---

### 1.1a. DataTable (Users List) - Columns

| Name | Content | Type | On Click | Card Placement | Tooltip/On Hover |
|------|---------|------|----------|---------------|------------------|
| – | Primary Image | Image | Zoom image | – | |
| UserId | Username | Text | Open View/Modify page | Title | – |
| Name | Name | Text | – | Subtitle | – |
| Mobile | Mobile | Text | – | Body | – |
| Email | Email | Text | – | Body | – |
| Start | Since when it was started | Date | – | Body | – |
| Status | Current Status | Status | Show status history | Body | – |

---

### 1.1b. DataTable (Users List) - Toolbar Config

| Feature | Settings | On Click |
| -------- | -------- | -------- |
| Search | Yes | Search User records based on entered keyword(s). |
| View Toggle | Yes | Switch between available DataTable view layouts (List/Card). |
| Column Selection | Yes | Open column selector to show or hide DataTable columns. |
| Group By | Yes | Group DataTable records based on selected column. |
| Filter | Yes | Open filter panel to apply additional filtering criteria. |
| Export | Yes<br/>Filename: `users-list` | Export displayed records using filename `users-list`. |
| Share | No | N/A |
| Full Screen | Yes | Toggle DataTable between normal and full-screen view. |
| Add | No | N/A |

---

### 1.1c. DataTable (Users List) - Config

| Feature        | Settings              |
| -------------- | --------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 10 |

---

### 1.1d. DataTable (Users List) - RowAction Menu

| Name            | Action                                      | Visibility Criteria                               | Tooltip                     |
| --------------- | ------------------------------------------- | ------------------------------------------------- | --------------------------- |
| Modify          | Open View/Modify page                       | - Modify permission<br>- Modify allowed on status | View or modify user details |
| Change Password | Open Change Password page                   | Modify permission                                 | Change user's password      |
| Stage_1         | Set status to Stage_1                       | Allowed status based on current status            | Move user to Stage_1        |
| Stage_2        | Set status to Stage_2                      | Allowed status based on current status            | Move user to Stage_2       |
| Undo Status     | Move back to last status                    | Permission to set current status                  | Revert to previous status   |
| Mark as Closed  | Ask end/close date<br>Sets end date         | Modify permission                                 | Close user and set end date |
| Delete          | Show confirmation message<br>On Yes, delete | Delete permission<br>Delete allowed on status     | Delete user record          |

---

### 1.1e. DataTable (Users List) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
| ------ | --------- | ---------- | --------------- | -------------- | ------ | ------- |
| Status | Multi Select | – | All Available Statuses | All Active Statuses | status_ids | Filter users by status |
| Application | Multi Select | – | All Applications | All | application_ids | Filter users by application |
| App Object | Multi Select | Application | All App Objects | All | object_ids | Filter users by application object |
| Role | Multi Select | Application | All Roles | All | role_ids | Filter users by assigned role |

---

# 2. User Entry Page UI

Fields in the main form are grouped into different sections.

## 2.1 Main Form Sections

- Basic Details
- Roles

---

## 2.1.1 Section (Basic Details)

> This section is always read-only and cannot be modified.

| Name/Label | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations                         | On Change | Description                        | Tooltip              |
| ---------- | ----------- | -------------- | ------------------------------ | -------- | --------- | ----------------------------------- | --------- | ---------------------------------- | -------------------- |
| UserId     | user_id     | Text           | -                              | Yes      | Yes       | - Unique<br>- Length 2-25           | –         | Code/Short Name                    | User Code            |
| Name       | name        | Text           | -                              | Yes      | Yes       | - Length 3-100                      | –         | Name                               | Full Name            |
| Mobile     | mobile      | Text           | Mobile Number                  | No       | Yes       | - Valid Mobile Format<br>- Optional | –         | Mobile                             | User Mobile          |
| Email      | email       | Text           | Email Address                  | No       | Yes       | - Valid Email Format<br>- Optional  | –         | Email                              | Email Address        |
| Start Date | start_date  | Date Picker    | DD/MM/YYYY                     | Yes      | Yes       | -                                   | –         | Date, when it was started/launched | Effective Start Date |
| Remarks    | remarks     | Text Area      | Multi-line                     | No       | Yes       | Max Length 1000                     | –         | Additional Notes                   | Remarks              |

---

## 2.1.2 Section (Roles)

**Component Type:** DataTable

> - This section displays all role assignments for the selected user. 
> - This section contains data in tabular format

---
### 2.1.2a DataTable (Roles) - Columns  
  
| Name/Label | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                             | Tooltip                |     |
| ---------- | ----------- | -------------- | ------------------------------ | -------- | --------- | ----------- | --------- | --------------------------------------- | ---------------------- | --- |
| Role       | role_id     | Dropdown       | Select Role                    | Yes      | No        | -           | –         | Name of the AppRole                     | User Role              |     |
| Start Date | start_date  | Date Picker    | DD/MM/YYYY                     | Yes      | No        | -           | –         | Date, when it was assigned to this user | Effective From         |     |
| End Date   | end_date    | Date Picker    | DD/MM/YYYY                     | No       | No        | -           | –         | Date, when the role assignment ends     | Effective Until        |     |
| Actions    | –           | Action         | Remove Row                     | –        | –         | –           | –         | Remove assigned role                    | Delete Role Assignment |     |

---

### 2.1.2b DataTable (Roles) - Toolbar Config

| Feature | Settings | On Click |
| -------- | -------- | -------- |
| Search | No | N/A |
| Filter | No | N/A |
| Export | No | N/A |
| Full Screen | No | N/A |
| Add | Yes | Add a new Role Assignment row |

---

### 1.1c. DataTable (Roles) - Config

| Feature        | Settings              |
| -------------- | --------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | No                   |
| Column Resize  | No                   |
| Column Pinning | No                   |
| Sorting        | No                   |
| Pagination     | No |

---

### 2.1.2d DataTable (Roles) - RowAction Menu

| Name | Action | Visibility Criteria | Icon | Tooltip |
| ------ | ------ | ------------------- | ---- | ------- |
| Remove | Remove Assigned Role | Visible for all non-system roles | delete | Remove Role Assignment |

--- 

### 2.1.2e DataTable (Roles) - Filter fields
| Name        | Component    | Depends On | Possible Values  | Default Values | Output          | Tooltip                                                                  |
| ----------- | ------------ | ---------- | ---------------- | -------------- | --------------- | ------------------------------------------------------------------------ |
| - | - | –          | - | -            | - | - |
