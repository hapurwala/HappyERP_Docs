#  Product Rate UI/UX Documentation

This document provides UI details of all pages used in the ***MAS-PRD's*** Product Rate module.

---

# 1. Product Rate List Page UI 

This page displays a list of Product Rate  in tabular format.

---

### 1.1a. DataTable (Product Rate List) - Columns

| Name         | Content                        | Format      | On Click              | Card Placement | Tooltip/On Hover |
| ------------ | ------------------------------ | ----------- | --------------------- | -------------- | ---------------- |
| Name         | s_prd_rate_list.`name`         | String      | Open View/Modify Page | Title          | –                |
| Short Name   | s_prd_rate_list.`short_name`   | String      | –                     | Subtitle       | –                |
| Rate Type    | s_prd_rate_list.`rate_type`    | String      | –                     | Body           | –                |
| Process Type | s_prd_rate_list.`process_type` | String      | –                     | Body           | –                |
| Start        | s_prd_rate_list.`start_date`   | Date        | –                     | Body           | –                |
| Stage        | s_prd_rate_list_stage.`name`   | Stage Badge | -                     | Body           | –                |

---


### 1.1b. DataTable (Product Rate List) - Toolbar Config

| Feature          | Settings | On Click                                                                                              |
| ---------------- | -------- | ----------------------------------------------------------------------------------------------------- |
| Search           | Yes      | Search  records based on the entered keyword(s).                                                      |
| View Toggle      | Yes      | Switch between available DataTable view layouts.                                                      |
| Column Selection | Yes      | Open column selector to show or hide DataTable columns.                                               |
| Group By         | Yes      | Group DataTable records based on the selected column.                                                 |
| Filter           | Yes      | Open filter panel to apply additional filtering criteria.                                             |
| Export           | Yes      | Export the displayed records.                                                                         |
| Share            | No       | N/A                                                                                                   |
| Full Screen      | Yes      | Toggle the DataTable between normal and full-screen view.                                             |
| Add              | Yes      | Open Add Product Rate Page. Visible only when `s_app_role_permission.can_add = true` for Product Rate |

---

### 1.1c. DataTable (Product Rate List) - Config

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

### 1.1d. DataTable (Product Rate List) - RowAction Menu

| Name           | Action                                          | Visibility Criteria                               | Tooltip |
| -------------- | ----------------------------------------------- | ------------------------------------------------- | ------- |
| Modify         | Open page `rate`                                | - Modify permission<br>- Modify allowed on status | -       |
| Stage_1        | Set status to Stage_1                           | - Allowed status based on current status          | -       |
| Stage_2        | Set status to Stage_2                           | - Allowed status based on current status          | -       |
| Undo Status    | Move back to last status                        | - Permission to set current status                | -       |
| Mark as Closed | - Ask end/close date<br>- Sets end date         | - Modify permission                               | -       |
| Delete         | - Show confirmation message<br>- On Yes, delete | - Delete permission<br>- Delete allowed on status | -       |

---

### 1.1e. DataTable (Product Rate List) - Filters Fields

| Name       | Type         | Possible Values   | Default         |
| ---------- | ------------ | ----------------- | --------------- |
| Status     | Multi Select | Possible statuses | All Open Status |

---

# 2. Product Rate Entry Page UI

Fields in the main form are grouped into different sections.

## Main Form Sections

- Basic Details
- Rate

### 2.1 Section (Basic Details)

| Name/Label             | Data Source                    | Component             | Component Specific Information                                                                                                                                                 | Required | Read Only | Validations   | On Change                | Description                                             | Tooltip                                                     |
| ---------------------- | ------------------------------ | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- | --------- | ------------- | ------------------------ | ------------------------------------------------------- | ----------------------------------------------------------- |
| Name                   | s_prd_rate_list.`name`         | Text                  | Length 3-100                                                                                                                                                                   | Yes      | No        | Length: 3-100 | –                        | Name of the Rate List                                   | -                                                           |
| Short Name             | s_prd_rate_list.`short_name`   | Text                  | Length 2-25                                                                                                                                                                    | Yes      | No        | Length: 2-25  | –                        | Code/Short Name of the Rate List                        | -                                                           |
| Rate Type              | s_prd_rate_list.`rate_type`    | Dropdown              | Select Rate Type (MRP/Rate)                                                                                                                                                    | Yes      | No        | –             | –                        | Type of Rate maintained in this Rate List               | -                                                           |
| Process Type           | s_prd_rate_list.`process_type` | Dropdown              | Purchase, Sale, Stock Transfer                                                                                                                                                 | Yes      | No        | –             | –                        | Business process for which this Rate List is applicable | The Business process for which this Rate List is applicable |
| Customer/Supplier Type | s_prd_rate_list.`party_type`   | Dropdown              | Customer Type or Supplier Type based on Process Type<br><br>Sales: Customer Type, Customer<br><br>Purchase: Supplier Type, Supplier<br><br>Stock Transfer: Not implemented<br> | No       | No        | –             | Update available Parties | Applicable Customer/Supplier Type                       | Party Type                                                  |
| Supplier(s)            | s_prd_rate_list_supplier_ids   | Multi Select Dropdown | Select one or more  Specific Suppliers for this rate list                                                                                                                      | No       | No        | –             | Update Supplier Mapping  | Suppliers for which this Rate List is applicable        | Applicable Suppliers                                        |
| Agent(s)               | s_prd_rate_list_agent_ids      | Multi Select Dropdown  Select one or more Agents                                                                                                                                                      | No       | No        | –             | Update Agent Mapping     | Agents for which this Rate List is applicable           | Applicable Agents                                           |
| Start Date             | s_prd_rate_list.`start_date`   | Date Picker           | DD/MM/YYYY                                                                                                                                                                     | Yes      | No        | Only Date     | –                        | Date from which this Rate List becomes applicable       | Effective From                                              |



---
### 2.3 Section (Rates)

**Component Type:** DataTable

---

#### 2.3a DataTable (Rates) - Columns


| Name/Label   | Data Source                              | Component   | Component Specific Information | Required | Read Only | Validations   | On Change                   | Description                                           | Tooltip                  |
| ------------ | ---------------------------------------- | ----------- | ------------------------------ | -------- | --------- | ------------- | --------------------------- | ----------------------------------------------------- | ------------------------ |
| Product      | s_prd_rate_list_detail.`product_id`      | Dropdown    | Select Product                 | Yes      | No        | –             | Update Product Pack Options | Product for which the rate is defined                 | -                        |
| Product Pack | s_prd_rate_list_detail.`product_pack_id` | Dropdown    | Select Product Pack            | No       | No        | –             | –                           | Product Pack for which the rate is defined            | -                        |
| Rate         | s_prd_rate_list_detail.`rate`            | Number      | Decimal Value                  | Yes      | No        | Numeric Value | –                           | Applicable rate for the selected Product/Product Pack | Show last Rate with date |
| Start Date   | s_prd_rate_list_detail.`start_date`      | Date Picker | DD/MM/YYYY                     | Yes      | No        | Only Date     | –                           | Date from which the rate becomes applicable           | -                        |
|              |                                          |             |                                |          |           |               |                             |                                                       |                          |

---

#### 2.3b DataTable (Rates) - Toolbar Config

| Feature     | Settings | On Click |
| ----------- | -------- | -------- |
| Search      | No       | N/A      |
| Filter      | No       | N/A      |
| Export      | No       | N/A      |
| Full Screen | No       | N/A      |
| Add         | No       | N/A      |

---

#### 2.3c DataTable (Rates) - Config

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

#### 2.3d DataTable (Rates) - RowAction Menu

| Name    | Action                          | Visibility Criteria | Icon | Tooltip |
| ------- | ------------------------------- | ------------------- | ---- | ------- |
| History | Open History TimeLine Popup     | -                   | -    | -       |

---

### 2.3e. DataTable (Rates) - Filters Fields

| Name       | Type         | Possible Values   | Default         |
| ---------- | ------------ | ----------------- | --------------- |
| -     | - | - | - |

---

# 3. Product Rate History Timeline Popup UI

This popup displays the complete rate history of a Product/Product Pack and allows users to add a new rate with an effective start date.


## 3.1 Rate Timeline Section

**Component Type:** Timeline List

> Displays all historical rates in chronological order.

### 3.1.1 Timeline Item Structure

| Name/Label      | Data Source                         | Component      | Read Only | Description                      | Tooltip         |
| --------------- | ----------------------------------- | -------------- | --------- | -------------------------------- | --------------- |
| Rate            | s_prd_rate_list_detail.`rate`       | Currency Label | Yes       | Applicable Rate                  | Product Rate    |
| Applicable From | s_prd_rate_list_detail.`start_date` | Date Label     | Yes       | Effective start date of the rate | Effective From  |
| Applicable Upto | s_prd_rate_list_detail.`end_date`   | Date Label     | Yes       | Effective end date of the rate   | Effective Until |
| Status          | s_prd_rate_list_detail.`status`     | Status Badge   | Yes       | Past / Current / Future          | Rate Status     |

---

## 3.4 Add New Rate Section

### 3.4.1 Section (Add New Rate)

| Name/Label      | Data Source                         | Type/Component | Component Specific Information | Required | Read Only | Validations   | On Change         | Description                                    | Tooltip        |
| --------------- | ----------------------------------- | -------------- | ------------------------------ | -------- | --------- | ------------- | ----------------- | ---------------------------------------------- | -------------- |
| Rate            | s_prd_rate_list_detail.`rate`       | Number         | Decimal Value                  | Yes      | No        | Numeric Value | –                 | New rate to be applied                         | Product Rate   |
| Applicable From | s_prd_rate_list_detail.`start_date` | Date Picker    | DD/MM/YYYY                     | Yes      | No        | Only Date     | Validate Timeline | Date from which the new rate becomes effective | Effective From |

---
