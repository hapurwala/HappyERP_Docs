#  Product Rate UI/UX Documentation

This document provides UI details of all pages used in the ***MAS-PRD's*** Product Rate module.

# Related Reference files

specs file : -

---

### Currency Behaviour

- Each Product Rate List is associated with exactly one Currency.
- All Product Rates within the Rate List use the selected Currency.
- Currency cannot be changed once Product Rates have been defined.
- Exchange Rate management is maintained separately.

---

# Pages

| S. No. | Name                      | Type  | Purpose                                                                                                       |
| ------ | -------------------------- | ----- | ----------------------------------------------------------------------------------------------------------- |
| 1.     | Product Rate List          | Page  | - View all Product Rate Lists<br/>- Search/filter Product Rate Lists<br/>- Perform quick actions on Product Rate List(s) |
| 2.     | Add/Modify Product Rate    | Page  | - View details of the Product Rate List<br/>- Add/modify Product Rate List data<br/>- Maintain Rates for Products/Product Packs |
| 3.     | Product Rate History Timeline Popup | Popup | - View complete rate history of a Product/Product Pack<br/>- Add a new rate with an effective start date |

---

# 1. Product Rate List Page UI 

This page displays a list of Product Rate  in tabular format.

---

### 1.1a. DataTable (Product Rate List) - Columns

| Header       | Data Source                      | Format      | On Click              | Card Placement | Tooltip / On Hover                     | Inline Edit Component |
| ------------ | ---------------------------------- | ----------- | --------------------- | -------------- | ----------------------------------------- | ---------------------- |
| Name         | s_prd_rate_list.`name`             | String      | Open View/Modify Page | Title          | –                                          | -                      |
| Short Name   | s_prd_rate_list.`short_name`       | String      | –                     | Subtitle       | –                                          | -                      |
| Rate Type    | s_prd_rate_list.`rate_type`        | String      | –                     | Body           | –                                          | -                      |
| Currency     | s_prd_rate_list.`currency_code`    | String      | –                     | Body           | Currency applicable for this Rate List     | -                      |
| Process Type | s_prd_rate_list.`process_type`     | String      | –                     | Body           | –                                          | -                      |
| Start        | s_prd_rate_list.`start_date`       | Date        | –                     | Body           | –                                          | -                      |
| Stage        | s_prd_rate_list_stage.`name`       | Stage Badge | -                     | Body           | –                                          | -                      |


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
| Add              | when  s_app_role_permission.`can_add = true` for Product Rate. | Page to Open: `rate` |

---

### 1.1c. DataTable (Product Rate List) - Table Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | Yes      |
| Pagination     | Yes<br>Page Size: 20 |

---

### 1.1d. DataTable (Product Rate List) - RowAction Menu

| Name           | Action                                                       | Visibility Criteria                                            | Icon   | Tooltip |
| -------------- | --------------------------------------------------------------- | ------------------------------------------------------------------ | ------ | ------- |
| Modify         | Open page `rate`                                                | - Modify permission<br>- Modify allowed on status                   | pencil | -       |
| Stage_1        | Set status to Stage_1                                            | - Allowed status based on current status                            | -      | -       |
| Stage_2        | Set status to Stage_2                                            | - Allowed status based on current status                            | -      | -       |
| Undo Stage     | Move back to last Stage                                          | - Permission to set current stage                                   | undo   | -       |
| Mark as Closed | - Ask end/close date<br>- Sets s_prd_rate_list.`end_date`        | - Modify permission<br>- If s_prd_rate_list.`end_date` = Null       | lock   | -       |
| Mark as Opened | - Sets s_prd_rate_list.`end_date` = Null                         | - Modify permission<br/>- s_prd_rate_list.`end_date` <> Null        | unlock | -       |
| Delete         | - Show confirmation message<br>- On Yes, delete                  | - Delete permission<br>- Delete allowed on status                    | trash  | -       |

---

### 1.1e. DataTable (Product Rate List) - Filters Fields

| Name     | Component    | Depends On | Possible Values       | Default Values | Output      | Required | Tooltip |
| -------- | ------------ | ---------- | ------------------------ | ----------------- | ------------- | -------- | ------- |
| Stage    | Multi Select | –          | Possible Stages          | All Open Stage     | stage_ids     | -        | -       |
| Currency | Multi Select | –          | Configured Currencies    | All                | currency_ids  | -        | -       |

---

# 2. Product Rate Entry Page UI

Fields in the main form are grouped into different sections.

## Main Form Sections

```
2.1. Left: Main Form
- Basic Details
- Rates

2.2. Right: System Information
- System Information
```

---

## 2.1 Left Section

### 2.1.1 Basic Details

| Name                   | Data Source                    | Component             | Component Specific Information                                                                                                                                                                    | Required | Read Only | Validations        | On Change                  | Description                                             | Tooltip |
| ----------------------- | ------------------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | --------- | ------------------- | --------------------------- | --------------------------------------------------------- | ------- |
| Name                   | s_prd_rate_list.`name`         | Text                  | Length 3-100                                                                                                                                                                                      | Yes      | No        | Length: 3-100      | –                          | Name of the Rate List                                   | -       |
| Short Name             | s_prd_rate_list.`short_name`   | Text                  | - Length 2-25<br>- Unique                                                                                                                                                                         | Yes      | No        | Length: 2-25       | –                          | Code/Short Name of the Rate List                        | -       |
| Rate Type              | s_prd_rate_list.`rate_type`    | Dropdown              | Select Rate Type (MRP/Rate)                                                                                                                                                                       | Yes      | No        | –                  | –                          | Type of Rate maintained in this Rate List               | -       |
| Process Type           | s_prd_rate_list.`process_type` | Dropdown              | Purchase, Sale, Stock Transfer                                                                                                                                                                    | Yes      | No        | –                  | –                          | Business process for which this Rate List is applicable | -       |
| Currency               | s_prd_rate_list.`currency_id`  | Dropdown              | Select Currency                                                                                                                                                                                   | Yes      | No        | –                  | Update Rate Display Format | Currency applicable for this Rate List                  | -       |
| Customer/Supplier Type | s_prd_rate_list.`party_type`   | Dropdown              | Customer Type or Supplier Type based on Process Type<br><br>Sales:<br>- Customer Type<br>- Customer<br><br>Purchase:<br>- Supplier Type<br>- Supplier<br><br>Stock Transfer:<br>- Not Implemented | No       | No        | –                  | Update Available Parties   | Applicable Customer/Supplier Type                       | -       |
| Supplier(s)            | s_prd_rate_list_supplier_ids   | Multi Select Dropdown | Select one or more Specific Suppliers for this Rate List                                                                                                                                          | No       | No        | –                  | Update Supplier Mapping    | Suppliers for which this Rate List is applicable        | -       |
| Agent(s)               | s_prd_rate_list_agent_ids      | Multi Select Dropdown | Select one or more Agents                                                                                                                                                                         | No       | No        | –                  | Update Agent Mapping       | Agents for which this Rate List is applicable           | -       |
| Start Date             | s_prd_rate_list.`start_date`   | Date Picker           | DD/MM/YYYY                                                                                                                                                                                        | Yes      | No        | Only Date          | –                          | Date from which this Rate List becomes applicable       | -       |
| End Date               | s_prd_rate_list.`end_date`     | Date Picker           | DD/MM/YYYY                                                                                                                                                                                        | No       | Yes       | Only Date          | –                          | Date on which this Rate List was closed                 | -       |
| Remarks                | s_prd_rate_list.`remarks`      | Text Area             | Multi-line                                                                                                                                                                                        | No       | No        | Max 255 Characters | –                          | Additional Notes                                        | -       |

---

### 2.1.2a. DataTable (Rates) - Columns

| Header       | Data Source                              | Format      | On Click | Card Placement | Tooltip / On Hover        | Inline Edit Component |
| ------------ | ------------------------------------------ | ----------- | -------- | ----------------- | ---------------------------- | ------------------------ |
| Product      | s_prd_rate_list_detail.`product_id`      | String      | –        | -                  | -                             | Dropdown<br/>Select Product       |
| Product Pack | s_prd_rate_list_detail.`product_pack_id` | String      | –        | -                  | -                             | Dropdown<br/>Select Product Pack  |
| Rate         | s_prd_rate_list_detail.`rate`            | Number      | –        | -                  | Show last Rate with date      | Number<br/>Decimal Value          |
| Start Date   | s_prd_rate_list_detail.`start_date`      | Date        | –        | -                  | -                             | Date Picker<br/>DD/MM/YYYY        |

---

### 2.1.2b. DataTable (Rates) - Toolbar Config

| Feature          | Settings | On Click |
| ---------------- | -------- | -------- |
| Search           | No       | N/A      |
| View Toggle      | No       | N/A      |
| Column Selection | No       | N/A      |
| Group By         | No       | N/A      |
| Filter           | No       | N/A      |
| Export           | No       | N/A      |
| Share            | No       | N/A      |
| Full Screen      | No       | N/A      |
| Add              | No       | N/A      |

---

### 2.1.2c. DataTable (Rates) - Table Config

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

### 2.1.2d. DataTable (Rates) - RowAction Menu

| Name    | Action                        | Visibility Criteria | Icon | Tooltip |
| ------- | ----------------------------- | ------------------- | ---- | ------- |
| History | Open `History TimeLine` Popup | -                   | -    | -       |

---

### 2.1.2e. DataTable (Rates) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Required | Tooltip |
| ---- | --------- | ---------- | ----------------- | ----------------- | ------ | -------- | ------- |
| -    | -         | –          | -                  | -                  | -      | -        | -       |

---

## 2.2 Right Section

### 2.2.1 System Information

| Name        | Data Source       | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                                                  | Tooltip |
| ----------- | ------------------- | --------- | --------------------------------- | -------- | --------- | ------------ | --------- | ------------------------------------------------------------------ | ------- |
| Created By  | System Generated   | Text      | User Name                          | No       | Yes       | –            | –         | User who created the Product Rate List                              | -       |
| Created On  | System Generated   | Date Time | DD/MM/YYYY HH:mm                   | No       | Yes       | –            | –         | Date and Time when the Product Rate List was created                | -       |
| Modified By | System Generated   | Text      | User Name                          | No       | Yes       | –            | –         | User who last modified the Product Rate List                        | -       |
| Modified On | System Generated   | Date Time | DD/MM/YYYY HH:mm                   | No       | Yes       | –            | –         | Date and Time when the Product Rate List was last modified          | -       |

---

# 3. Product Rate History Timeline Popup UI

This popup displays the complete rate history of a Product/Product Pack and allows users to add a new rate with an effective start date.


## 3.1 Rate Timeline Section

**Component Type:** Timeline List

> Displays all historical rates in chronological order.

### 3.1.1 Timeline Item Structure

| Name             | Data Source                         | Component      | Component Specific Information | Required | Read Only | Validations | On Change | Description                                          | Tooltip |
| ----------------- | -------------------------------------- | -------------- | --------------------------------- | -------- | --------- | ------------ | --------- | ------------------------------------------------------- | ------- |
| Rate             | s_prd_rate_list_detail.`rate`       | Currency Label | System Generated                   | No       | Yes       | –            | –         | Applicable Rate in the selected Rate List Currency        | -       |
| Applicable From  | s_prd_rate_list_detail.`start_date` | Date Label     | System Generated                   | No       | Yes       | –            | –         | Effective start date of the rate                          | -       |
| Applicable Upto  | s_prd_rate_list_detail.`end_date`   | Date Label     | System Generated                   | No       | Yes       | –            | –         | Effective end date of the rate                            | -       |
| Status           | s_prd_rate_list_detail.`status`     | Status Badge   | Past / Current / Future            | No       | Yes       | –            | –         | Past / Current / Future                                   | -       |

---

## 3.2 Add New Rate Section

### 3.2.1 Add New Rate

| Name             | Data Source                         | Component   | Component Specific Information | Required | Read Only | Validations   | On Change         | Description                                    | Tooltip |
| ----------------- | -------------------------------------- | ----------- | ------------------------------- | -------- | --------- | ------------- | ------------------- | ------------------------------------------------- | ------- |
| Rate             | s_prd_rate_list_detail.`rate`       | Number      | Decimal Value                  | Yes      | No        | Numeric Value | –                  | New rate to be applied                            | -       |
| Applicable From  | s_prd_rate_list_detail.`start_date` | Date Picker | DD/MM/YYYY                     | Yes      | No        | Only Date     | Validate Timeline  | Date from which the new rate becomes effective    | -       |


---

