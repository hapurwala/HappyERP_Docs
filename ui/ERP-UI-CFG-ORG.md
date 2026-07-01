# Organisation UI/UX Documentation

This document gives UI details of all pages used in Organisation module.

# Pages

| S. No. | Name                    | Type  | Purpose                                                                                |
| ------ | ----------------------- | ----- | -------------------------------------------------------------------------------------- |
| 1.     | Organisation List       | Page  | - View all organisations<br/>- Search/filter organisations<br/>- Perform quick actions |
| 2.     | Add/Modify Organisation | Page  | - View organisation details<br/>- Add/modify organisation information                  |
| 3.     | Add/Modify Branch       | Popup | - Add/modify branch information under organisation                                     |

---

# 1. Organisation List Page UI

This page shows list of Organisations in tabular format. The Organisations appearing in the list depend upon current user's permissions and applied filters.

---

### 1.1a. DataTable (Organisation List) - Columns

| Name       | Content                 | Type   | On Click                                          | Card Placement | Tooltip / On Hover          |
|:---------- |:----------------------- |:------ |:------------------------------------------------- |:-------------- |:--------------------------- |
| Logo       | Organisation Logo       | Image  | Zoom Image                                        | Leading        | View Organisation Logo      |
| Name       | Organisation Name       | Text   | Page to open: `organisation`<br/>Mode : View/Edit | Title          | View Organisation Details   |
| Short Name | Organisation Short Name | Text   | –                                                 | Subtitle       | Organisation Short Name     |
| City       | City                    | Text   | –                                                 | Body           | Organisation City           |
| Since      | Start Date              | Date   | –                                                 | Body           | Date Of Incorporation       |
| Status     | Current Status          | Status | Open Workflow Timeline                            | Trailing       | Current Organisation Status |

---

### 1.1b. DataTable (Organisation List) - Toolbar Config

| Feature          | Settings                                           | On Click                                                                               |
|:---------------- |:-------------------------------------------------- |:-------------------------------------------------------------------------------------- |
| Search           | Yes                                                | Search organisation records based on the entered keyword(s).                           |
| View Toggle      | Yes                                                | Switch between available DataTable view layouts.                                       |
| Column Selection | Yes                                                | Open column selector to show or hide DataTable columns.                                |
| Group By         | Yes                                                | Group DataTable records based on the selected column.                                  |
| Filter           | Yes                                                | Open filter panel to apply additional filtering criteria.                              |
| Export           | Yes<br/>Filename: `organisation-list`              | Export the displayed organisation records using the filename `organisation-list`.      |
| Share            | Yes                                                | Generate and share the current DataTable view, including applied filters and settings. |
| Full Screen      | Yes                                                | Toggle the DataTable between normal and full-screen view.                              |
| Add              | Yes<br/>Page to open: `organisation`<br/>Mode: Add | Open the `organisation` page in Add mode to create a new organisation record.          |

---

### 1.1c. DataTable (Organisation List) - Config

| Feature        | Settings              |
|:-------------- |:--------------------- |
| Row Selection  | No                    |
| Bulk Actions   | No                    |
| Sticky Header  | Yes                   |
| Column Resize  | Yes                   |
| Column Pinning | Yes                   |
| Sorting        | Yes                   |
| Pagination     | Yes<br/>Page Size: 20 |

---

### 1.1d. DataTable (Organisation List) - RowAction Menu

| Name             | Action                                         | Visibility Criteria                             | Icon               | Tooltip |
|:---------------- |:---------------------------------------------- |:----------------------------------------------- |:------------------ |:------- |
| View             | Open page: `organisation`<br/>Mode : View      | No Modify Permission                            | eye                | -       |
| Modify           | Open page: `organisation`<br/><br/>Mode : Edit | Modify Permission<br/>- Modify allowed on stage | pencil             | -       |
| Set as <Stage_1> | Set stage to Stage_1                           | Allowed stage based on current stage            | arrow-right-circle | -       |
| Set as <Stage_2> | Set stage to Stage_2                           | Allowed stage based on current stage            | arrow-right-circle | -       |
| Undo Stage       | Move back to last stage                        | Permission to rollback                          | undo-2             | -       |
| Mark Closed      | Ask End Date and Close Organisation            | Modify Permission                               | archive            | -       |
| Delete           | Show Confirmation → Delete                     | Delete Permission                               | trash-2            | -       |

---

### 1.1e. DataTable (Organisation List) - Filters Fields

| Name   | Component    | Depends On | Possible Values                      | Default Values  | Output             | Tooltip                                                                          |
|:------ |:------------ |:---------- |:------------------------------------ |:--------------- |:------------------ |:-------------------------------------------------------------------------------- |
| Status | Multi Select | –          | Active, Closed, Suspended, Cancelled | All Open Status | status_ids         | Select one or more organisation statuses to filter the list.                     |
| State  | Multi Select | –          | Active States                        | All             | state_ids          | Select one or more states to filter organisations by location.                   |
| City   | Multi Select | State      | Active Cities                        | All             | city_ids           | Select one or more cities. Available options are based on the selected state(s). |
| Since  | Date Range   | –          | Any Date Range                       | All             | from_date, to_date | Filter organisations created or active within the selected date range.           |

---

# 2. Organisation Entry Page UI

Fields in the main form are grouped in different sections.

## 2.1 Left: Main Form

* Basic Information
* Contact Information
* Regulatory IDs
* Branches
* Attachments

## 2.2 Right: Widgets

* Organisation Summary
* Workflow Timeline

---

## 2.1.1 Section (Basic Information)

| Name/Label          | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations       | On Change | Description                 | Tooltip                                                  |
| ------------------- | ----------- | -------------- | ------------------------------ | -------- | --------- | ----------------- | --------- | --------------------------- | -------------------------------------------------------- |
| Name                | name        | Text           | Length 3-100                   | Yes      | No        | Unique Name       | –         | Organisation Name           | Enter the legal name of the organisation                 |
| Short Name          | short_name  | Text           | Length 2-25                    | Yes      | No        | Unique Short Name | –         | Short Name                  | Enter a short identifier used across ERP                 |
| Start Date          | start_date  | Date Picker    | Single Date                    | Yes      | No        | Valid Date        | –         | Incorporation Date          | Date on which the organisation started operations        |
| Production Facility | production  | Toggle         | Yes/No                         | Yes      | No        | –                 | –         | Production Unit Available   | Indicates whether manufacturing activities are performed |
| Warehouse           | warehouse   | Toggle         | Yes/No                         | Yes      | No        | –                 | –         | Warehouse Available         | Indicates whether warehouse facilities are available     |
| Dispatch            | dispatch    | Toggle         | Yes/No                         | Yes      | No        | –                 | –         | Dispatch Facility Available | Indicates whether dispatch operations are managed        |
| Remarks             | remarks     | Text Area      | Multi-line                     | No       | No        | –                 | –         | General Remarks             | Additional notes or comments about the organisation      |

---

## 2.1.2 Section (Contact Information)

| Name/Label                | Data Source        | Type/Component | Component Specific Information | Required | Read Only | Validations   | On Change    | Description      | Tooltip                                                         |
| ------------------------- | ------------------ | -------------- | ------------------------------ | -------- | --------- | ------------- | ------------ | ---------------- | --------------------------------------------------------------- |
| Street Address            | address.street     | Text Area      | Length 2-255                   | No       | No        | –             | –            | Street Address   | Enter complete street address of the organisation               |
| City                      | address.city_id    | Lookup         | Active Cities                  | No       | No        | Valid City    | Load State   | City             | Select the city where the organisation is located               |
| State                     | address.state_id   | Lookup         | Active States                  | No       | No        | Valid State   | Load Country | State            | Select the state corresponding to the selected city             |
| Country                   | address.country_id | Lookup         | Active Countries               | No       | No        | Valid Country | –            | Country          | Select the country where the organisation operates              |
| Pincode                   | address.pin        | Text           | Length 2-10                    | No       | No        | Valid Pincode | –            | Postal Code      | Enter ZIP / Postal Code                                         |
| Geo Tag                   | address.location   | Geo Location   | Mobile Only                    | No       | No        | –             | Capture GPS  | Geo Location     | Capture GPS coordinates of the organisation location            |
| Mobile                    | mobile             | Text           | Length 3-25                    | No       | No        | –             | –            | Primary Mobile   | Primary contact number of the organisation                      |
| Mobile WhatsApp           | mobile_whatsapp    | Toggle         | Yes/No                         | No       | No        | –             | –            | WhatsApp Enabled | Indicates whether the primary mobile number supports WhatsApp   |
| Alternate Mobile          | mobile_2           | Text           | Length 3-25                    | No       | No        | –             | –            | Secondary Mobile | Secondary contact number of the organisation                    |
| Alternate Mobile WhatsApp | mobile_2_whatsapp  | Toggle         | Yes/No                         | No       | No        | –             | –            | WhatsApp Enabled | Indicates whether the alternate mobile number supports WhatsApp |
| Email                     | email              | Text           | Email Format                   | No       | No        | Valid Email   | –            | Email Address    | Official email address of the organisation                      |
| Website                   | website            | Text           | URL Format                     | No       | No        | Valid URL     | –            | Website URL      | Official website URL of the organisation                        |

---

## 2.1.3 Section (Regulatory IDs)

**Component Type:** DataTable

### 2.1.3a DataTable (Regulatory IDs) - Columns

| Name/Label         | Data Source | Type/Component | Component Specific Information                                             | Required | Read Only | Validations                                                         | On Change | Description                   | Tooltip                   |
|:------------------ |:----------- |:-------------- |:-------------------------------------------------------------------------- |:-------- |:--------- |:------------------------------------------------------------------- |:--------- |:----------------------------- |:------------------------- |
| Registration Class | reg_class   | Lookup         | Single Select, Searchable, Possible Values: PAN, GSTIN, CIN, Aadhaar, etc. | Yes      | No        | Registration Class Must Be Active                                   | -         | PAN, GSTIN, CIN, Aadhaar etc. | Select Registration Class |
| Number             | number      | Text Input     | Max Length As Per Registration Class Rules                                 | Yes      | No        | Must Be Unique Per Registration Class; Format Validation Applicable | -         | Registration Number           | Enter Registration Number |
| Issue Date         | issue_date  | Date Picker    | Date Selection                                                             | No       | No        | Cannot Be Greater Than Current Date                                 | -         | Issue Date                    | Select Issue Date         |
| Valid From         | valid_from  | Date Picker    | Date Selection                                                             | No       | No        | Must Be Less Than Or Equal To Valid Upto                            | -         | Valid From                    | Select Valid From Date    |
| Valid Upto         | valid_upto  | Date Picker    | Date Selection                                                             | No       | No        | Must Be Greater Than Or Equal To Valid From                         | -         | Valid Upto                    | Select Valid Upto Date    |

### 2.1.3b DataTable (Regulatory IDs) - Toolbar Config

| Feature     | Settings | On Click    |
| ----------- | -------- | ----------- |
| Search      | No       | -           |
| Filter      | No       | -           |
| Export      | No       | -           |
| Full Screen | Yes      | -           |
| Add         | Yes      | Add New Row |

### 2.1.3c DataTable (Regulatory IDs) - RowAction Menu

| Name      | Action                         | Visibility Criteria   | Icon   | Tooltip |
|:--------- |:------------------------------ |:--------------------- |:------ |:------- |
| Modify    | Change Row To Inline Edit Mode | - Page mode is modify | Pencil | -       |
| Duplicate | Copy Registration              | - Page mode is modify | Copy   | -       |
| Delete    | Delete Registration            | - Page Mode Is Modify | delete | -       |

## 2.1.4 Section (Branches)

**Component Type:** DataTable

### 2.1.4a DataTable (Branches) - Columns

| Name/Label | Data Source   | Type/Component | Component Specific Information | Required | Read Only | Validations                | On Change | Description                   | Tooltip                                                         |
|:---------- |:------------- |:-------------- |:------------------------------ |:-------- |:--------- |:-------------------------- |:--------- |:----------------------------- |:--------------------------------------------------------------- |
| Name       | branch_name   | Text           | -                              | Yes      | No        | Branch Name Must Be Unique | -         | Branch Name                   | Open branch details                                             |
| Short Name | short_name    | Text           | -                              | No       | No        | -                          | -         | Branch Short Name             | Branch short name                                               |
| City       | city_name     | Text           | -                              | No       | No        | -                          | -         | Branch City                   | Branch city                                                     |
| Mobile     | mobile        | Text           | -                              | No       | No        | Valid Mobile Number Format | -         | Branch Mobile Number          | Branch contact number                                           |
| Production | is_production | Status         | Possible Values: Yes, No       | No       | Yes       | -                          | -         | Production Facility Indicator | Indicates whether the branch is a production facility           |
| Warehouse  | is_warehouse  | Status         | Possible Values: Yes, No       | No       | Yes       | -                          | -         | Warehouse Indicator           | Indicates whether the branch operates as a warehouse            |
| Dispatch   | is_dispatch   | Status         | Possible Values: Yes, No       | No       | Yes       | -                          | -         | Dispatch Indicator            | Indicates whether the branch is enabled for dispatch operations |

### 2.1.4b DataTable (Branches) - Toolbar Config

| Feature     | Settings                | On Click                                                           |
| ----------- | ----------------------- | ------------------------------------------------------------------ |
| Search      | Yes                     | Search branch records based on the entered keyword(s).             |
| Filter      | No                      | N/A                                                                |
| Export      | No                      | N/A                                                                |
| Full Screen | Yes                     | Toggle the DataTable between normal and full-screen view.          |
| Add         | Yes<br/>Popup: `branch` | Open the `branch` popup in create mode to add a new branch record. |

### 2.1.4c DataTable (Branches) - RowAction Menu

| Name      | Action        | Visibility Criteria            | Icon         | Tooltip                              |
|:--------- |:------------- |:------------------------------ |:------------ |:------------------------------------ |
| Modify    | Edit Branch   | Visible for all branch records | edit         | Edit branch details                  |
| Duplicate | Copy Branch   | Visible for all branch records | content_copy | Create a copy of the selected branch |
| Delete    | Delete Branch | Visible for all branch records | delete       | Delete the selected branch           |

---

## 2.1.5 Section (Attachments)

| Name/Label | Data Source | Type/Component | Required | Read Only | Description                 |
| ---------- | ----------- | -------------- | -------- | --------- | --------------------------- |
| Preview    | –           | File Viewer    | No       | Yes       | Preview File                |
| File Name  | file_name   | Text Display   | Yes      | Yes       | File Name                   |
| File Type  | file_type   | Badge          | Yes      | Yes       | File Type                   |
| Actions    | –           | Action Menu    | No       | No        | Preview / Download / Delete |

---

## 2.2 Right Side Widgets

### 2.2.1 Organisation Summary

**Component to use:** Summary Card

| Name                 | Content                    | On Click                                                  | Tooltip / On Hover                                        |
|:-------------------- |:-------------------------- |:--------------------------------------------------------- |:--------------------------------------------------------- |
| Total Branches       | Count(branches)            | Open Branches list filtered for current organisation      | Total number of branches associated with the organisation |
| Production Branches  | Count(production=true)     | Open Branches list filtered by Production = Yes           | Number of branches configured as production facilities    |
| Warehouse Branches   | Count(warehouse=true)      | Open Branches list filtered by Warehouse = Yes            | Number of branches configured as warehouses               |
| Dispatch Branches    | Count(dispatch=true)       | Open Branches list filtered by Dispatch = Yes             | Number of branches configured for dispatch operations     |
| Active Registrations | Count(valid registrations) | Open Regulatory IDs list filtered to active registrations | Number of currently valid regulatory registrations        |
| Since                | Organisation Start Date    | N/A                                                       | Organisation establishment/start date                     |

---

### 2.2.2 Workflow Timeline

**Component to use:** StageHistoryViewer

| Name/Label | Data Source | Component                   | Tooltip                                                  |
|:---------- |:----------- |:--------------------------- |:-------------------------------------------------------- |
| Stage      | stage_name  | Text                        | Workflow stage assigned to the record                    |
| Set At     | set_at      | Date Time                   | Date and time when the stage was assigned                |
| Set By     | set_by      | User                        | User who assigned or changed the stage                   |
| Remarks    | remarks     | Text Area                   | Comments or remarks entered during the stage transition  |
| Attachment | attachment  | File Viewer / Download Link | Supporting document attached during the stage transition |

---

# 3. Branch Entry Popup UI

## 3.1 Section (Basic Information)

| Name                | Type      | Validation              |
| ------------------- | --------- | ----------------------- |
| Name                | Text      | Mandatory, Length 3-100 |
| Short Name          | Text      | Mandatory, Length 2-25  |
| Start Date          | Date      | Mandatory               |
| Production Facility | Toggle    | Mandatory               |
| Warehouse           | Toggle    | Mandatory               |
| Dispatch            | Toggle    | Mandatory               |
| Remarks             | Text Area | Optional                |

---

## 3.2 Section (Contact Information)

| Name                      | Type            | Validation    |
| ------------------------- | --------------- | ------------- |
| Street Address            | Text Area       | Optional      |
| City                      | Lookup          | Optional      |
| State                     | Lookup          | Optional      |
| Country                   | Lookup          | Optional      |
| Pincode                   | Text            | Optional      |
| Geo Tag                   | Location Picker | Mobile Only   |
| Mobile                    | Text            | Optional      |
| Mobile WhatsApp           | Toggle          | Default False |
| Alternate Mobile          | Text            | Optional      |
| Alternate Mobile WhatsApp | Toggle          | Default False |
| Email                     | Text            | Optional      |
| Website                   | Text            | Optional      |

---

## 3.3 Section (Regulatory IDs)

**Component Type:** DataTable

| Name               | Type   |
| ------------------ | ------ |
| Registration Class | Lookup |
| Number             | Text   |
| Issue Date         | Date   |
| Valid From         | Date   |
| Valid Upto         | Date   |
