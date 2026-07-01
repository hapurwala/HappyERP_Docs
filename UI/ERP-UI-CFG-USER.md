# Users UI/UX Documentation

This document gives UI details of all pages used in Users module.

# Related Reference files

specs file : -

> **Revisions**
> 
> Checked by NKS on 27-Jun-2026

---

# Pages

| S. No. | Name        | Type | Purpose                                                                |
| ------ | ----------- | ---- | ---------------------------------------------------------------------- |
| 1.     | Users List  | Page | - View all Users<br/>- Search/filter Users<br/>- Perform quick actions |
| 2.     | Modify User | Page | - View user details<br/>- Modify role assignments                     |

---

# 1. Users List Page UI

This page shows list of users in tabular format. The users appearing in the list depend upon current user's permissions and applied filters.

### 1.1a. DataTable (Users List) - Columns

| Header  | Data Source                                           | Format      | On Click           | Card Placement | Tooltip / On Hover | Inline Edit Component |
| ------- | ----------------------------------------------------- | ----------- | ------------------ | -------------- | ------------------ | --------------------- |
| Avatar  | m_party,attachments[].`url` where `is_primary` = True | Image       | Zoom image         | Image          | -                  | -                     |
| User Id | m_party.`short_name`                                  | String      | Open page `user`   | Title          | -                  | -                     |
| Name    | m_party.`display_name`                                | String      | –                  | Subtitle       | -                  | -                     |
| Mobile  | m_party.`mobile`                                      | String      | –                  | Body           | -                  | -                     |
| Email   | m_party.`email`                                       | String      | –                  | Body           | -                  | -                     |
| Active  | m_party.login.`is_enabled`                            | String      | –                  | Body           | -                  | -                     |
| Stage   | Current Stage                                         | Stage Badge | Show Stage history | Body           | -                  | -                     |

---

### 1.1b. DataTable (Users List) - Toolbar Config

| Feature          | Settings                       | On Click                                               |
| ---------------- | ------------------------------- | -------------------------------------------------------- |
| Search           | Yes                              | -                                                          |
| View Toggle      | Yes                              | -                                                          |
| Column Selection | Yes                              | -                                                          |
| Group By         | Yes                              | -                                                          |
| Filter           | Yes                              | -                                                          |
| Export           | Yes<br/>Filename: `users-list`  | Export displayed records using filename `users-list`.     |
| Share            | No                                | -                                                          |
| Full Screen      | Yes                              | -                                                          |
| Add              | No                                | -                                                          |

---

### 1.1c. DataTable (Users List) - Table Config

| Feature        | Settings              |
| --------------- | ---------------------- |
| Row Selection   | No                     |
| Bulk Actions    | No                     |
| Sticky Header   | Yes                    |
| Column Resize   | Yes                    |
| Column Pinning  | Yes                    |
| Sorting         | Yes                    |
| Pagination      | Yes<br/>Page Size: 10  |

---

### 1.1d. DataTable (Users List) - RowAction Menu

| Name        | Action                                    | Visibility Criteria                                                  | Icon   | Tooltip |
| ------------ | ------------------------------------------- | ------------------------------------------------------------------- | ------ | ------- |
| View        | Open page: `user`                          | - View Permission<br/>- No Modify Permission (Due to Role or Stage) | eye    | -       |
| Modify      | Open page: `user`                          | - Modify permission<br>- Modify allowed on status                   | pencil | -       |
| Set as <Stage_1> | Set status to Stage_1                  | - Allowed stage based on current stage                              | -      | -       |
| Set as <Stage_2> | Set status to Stage_2                  | - Allowed stage based on current stage                              | -      | -       |
| Undo Stage  | Move back to last stage                     | Permission to rollback current stage                                  | undo   | -       |
| Disable     | Sets m_party.login.`is_enabled` = False     | Modify permission<br/>m_party.login.`is_enabled` = True              | lock   | -       |
| Enable      | Sets m_party.login.`is_enabled` = True      | Modify permission<br/>m_party.login.`is_enabled` = False             | unlock | -       |

---

### 1.1e. DataTable (Users List) - Filters Fields

| Name        | Component    | Depends On  | Possible Values        | Default Values       | Output            | Required | Tooltip |
| ------------ | ------------- | ------------ | ------------------------ | ---------------------- | ------------------- | -------- | ------- |
| Stage       | Multi Select | –            | All Available Statuses  | All Active Statuses   | status_ids         | -        | -       |
| Application | Multi Select | –            | All Applications        | All                    | application_ids    | -        | -       |
| App Object  | Multi Select | Application  | All App Objects         | All                    | object_ids          | -        | -       |
| Role        | Multi Select | Application  | All Roles                | All                    | role_ids            | -        | -       |

---

# 2. User Entry Page UI

## Main Form Sections

```
2.1. Left: Main Form
- Basic Details
- Roles

2.2. Right: System Information
- System Information
```

---

## 2.1 Left Section

### 2.1.1 Basic Details

> This section is always read-only and cannot be modified.

| Name       | Data Source            | Component    | Component Specific Information | Required | Read Only | Validations       | On Change | Description                          | Tooltip |
| ----------- | ------------------------ | ------------- | --------------------------------- | -------- | --------- | -------------------- | --------- | --------------------------------------- | ------- |
| User Id    | m_party.`user_id`      | Text         | -                                  | Yes      | Yes       | -                     | –         | Code/Short Name                        | -       |
| Name       | m_party.`name`         | Text         | -                                  | Yes      | Yes       | -                     | –         | Name                                    | -       |
| Mobile     | m_party.`mobile`       | Text         | Mobile Number                      | No       | Yes       | -                     | –         | Mobile                                  | -       |
| Email      | m_party.`email`        | Text         | Email Address                      | No       | Yes       | -                     | –         | Email                                   | -       |
| Start Date | m_party.`start_date`   | DatePicker   | -                                  | Yes      | Yes       | -                     | –         | Date, when it was started/launched      | -       |
| Remarks    | m_party.`remarks`      | RichTextBox  | -                                  | No       | Yes       | Max Length 1000      | –         | Additional Notes                        | -       |

---

### 2.1.2 Roles

- This section displays all role assignments for the selected user.
- This section contains data in tabular format.

**Component Type:** DataTable

### 2.1.2a. DataTable (Roles) - Columns

| Header     | Data Source   | Format | On Click | Card Placement | Tooltip / On Hover | Inline Edit Component                     |
| ----------- | -------------- | ------ | -------- | ----------------- | --------------------- | -------------------------------------------- |
| Role       | `role_id`     | String | –        | Title             | -                      | Select                                        |
| Start Date | `start_date`  | Date   | –        | -                  | -                      | DatePicker                                    |
| End Date   | `end_date`    | Date   | –        | -                  | -                      | DatePicker<br/>- Should be >= Start Date     |

---

### 2.1.2b. DataTable (Roles) - Toolbar Config

| Feature          | Settings | On Click                       |
| ---------------- | -------- | -------------------------------- |
| Search           | No       | -                                 |
| View Toggle      | No       | -                                 |
| Column Selection | No       | -                                 |
| Group By         | No       | -                                 |
| Filter           | No       | -                                 |
| Export           | No       | -                                 |
| Share            | No       | -                                 |
| Full Screen      | No       | -                                 |
| Add              | Yes      | Add a new Role Assignment row    |

---

### 2.1.2c. DataTable (Roles) - Table Config

| Feature        | Settings |
| --------------- | -------- |
| Row Selection   | No       |
| Bulk Actions    | No       |
| Sticky Header   | No       |
| Column Resize   | No       |
| Column Pinning  | No       |
| Sorting         | No       |
| Pagination      | No       |

---

### 2.1.2d. DataTable (Roles) - RowAction Menu

| Name   | Action       | Visibility Criteria | Icon   | Tooltip |
| ------ | ------------- | ---------------------- | ------ | ------- |
| Modify | Inline Edit  | Modify permission      | pencil | -       |

---

### 2.1.2e. DataTable (Roles) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Required | Tooltip |
| ---- | --------- | ---------- | ------------------ | ------------------ | -------- | --------- | ------- |
| -    | -         | –          | -                   | -                   | -        | -         | -       |

---

## 2.2 Right Section

### 2.2.1 System Information

| Name        | Data Source        | Component  | Component Specific Information | Required | Read Only | Validations | On Change | Description                                            | Tooltip |
| ------------ | -------------------- | ----------- | ---------------------------------- | -------- | --------- | -------------- | --------- | ---------------------------------------------------------- | ------- |
| Created On  | System Generated    | Date Time  | DD/MM/YYYY HH:mm                    | No       | Yes       | –              | –         | Date and Time when the User record was created             | -       |
| Modified On | System Generated    | Date Time  | DD/MM/YYYY HH:mm                    | No       | Yes       | –              | –         | Date and Time when the User record was last modified       | -       |
