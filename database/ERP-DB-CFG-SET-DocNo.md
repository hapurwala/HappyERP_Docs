# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Configuration
- **Module**: Settings (Document Numbering)

Document numbering in **HappyERP** allows user to configure how to generate number of transaction documents like challan, sales invoice, purchase order etc.  

Almost all transaction documents are identified by their numbers. These numbers must be unique for individual document types across multiple financial years (i.e. throughout software life). Here are the key features:

- Number should be unique for a given document type like Sales Order, Purchase Order

- Multiple number series may be created for same document type.

- Separate number series may be created for organisations, branches, employees and/or clients.

- Each number series may start from 1 from financial year, month or day.

## Collection: m_number_series_config

This collection is used to store number series configuration.

| Name               | Type      | Optional | Default Value | Key     | Reference                                         | Remarks                                                                                                                                  |
| ------------------ | --------- | -------- | ------------- | ------- | ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `id`               | Auto      | -        | -             | Primary | -                                                 | Document Id                                                                                                                              |
| `name`             | String    | -        | -             | Unique  | -                                                 | Name of the series                                                                                                                       |
| `app_object_id`    | String    | -        | -             | Foreign | s_app_object.`id`                                 |                                                                                                                                          |
| `is_primary`       | Boolean   | -        | False         | -       | -                                                 | Primary series in its category (AppObjectId, Org, Branch, Party etc)                                                                     |
| `restart_duration` | String    | -        | FY            | -       | s_category.`duration`<br/>Allowed values: fy, mth | Duration when number will restart from 1. Default is FY i.e. number will start start from 1 on every new FY.                             |
| `min_number`       | Int64     | -        | 1             | -       | -                                                 | Number range for this number series                                                                                                      |
| `max_number`       | Int64     | -        | -             | -       | -                                                 | Number range for this number series                                                                                                      |
| `number_format`    | String    | -        | -             | -       | -                                                 | Format to be used for display number of the document in this series. Format may include keywords like OrgShortName, BranchShortName etc. |
| `is_org_wise`      | Boolean   | -        | True          | -       | -                                                 | Separate number for every organisation                                                                                                   |
| `allowed_org`      | Array     | -        | [0]           | Foreign | m_organisation.`'id`'                             | - Applicable when `is_org_wise` is true.<br/>- Organisations using this number series. <br/>- 0 means all.                               |
| `is_branch_wise`   | Boolean   | -        | False         | -       | -                                                 | Separate number for every branch                                                                                                         |
| `allowed_branch`   | Array     | -        | [0]           | Foreign | m_branch.`'id`'                                   | - Applicable when `is_branch_wise` is true.<br/>- Branches using this number series. <br/>- 0 means all.                                 |
| `is_party_wise`    | Boolean   | -        | False         | -       | -                                                 | Separate number for every party. This is useful to have customer or vendor wise                                                          |
| `allowed_party`    | Array     | -        | [0]           | Foreign | m_organsation.`'id`'                              | - Applicable when `is_party_wise` is true.<br/>- Parties using this number series. <br/>- 0 means all.                                   |
| `is_emp_wise`      | Boolean   | -        | False         | -       | -                                                 | Separate number for every employee                                                                                                       |
| `allowed_employee` | Array     | -        | [0]           | Foreign | m_party.`'id`'                                    | - Applicable when `is_emp_wise` is true.<br/>- Employees using this number series. <br/>- 0 means all.                                   |
| `start_date`       | Timestamp | –        | –             | –       | –                                                 | -                                                                                                                                        |
| `end_date`         | Timestamp | Yes      | –             | –       | –                                                 | -                                                                                                                                        |
| `reason_end`       | String    | Yes      | –             | –       | –                                                 | -                                                                                                                                        |
| `stage`            | Map       | –        | –             | –       | `Stage` Map                                       | Current Stage                                                                                                                            |
| `stage_logs`       | Array Map | Yes      | –             | –       | `Stage` Map                                       | Workflow History                                                                                                                         |
| `notes`            | Array Map | -        | -             | -       | `Note` Map                                        | It is an array of Note map                                                                                                               |

## Collection: m_number_series

This collection is used to store number series generated based on above configuration. This collection will be updated automatically in following cases:

- Changes in Number Series Configuration

- Start/End an organisation

- Start/End a branch

- Start/End a Party

- Start/End an Employee

| Name               | Type      | Optional | Default Value | Key     | Reference                                         | Remarks                                                                                                                                  |
| ------------------ | --------- | -------- | ------------- | ------- | ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `id`               | Auto      | -        | -             | Primary | -                                                 | Document Id                                                                                                                              |
| `name`             | String    | -        | -             | -       | -                                                 | Name of the series                                                                                                                       |
| `app_object_id`    | String    | -        | -             | Foreign | s_app_object.`id`                                 | -                                                                                                                                        |
| `organisation_id`  | String    | -        | Yes           | Foreign | m_organisation.`'id`                              | -                                                                                                                                        |
| `branch_id`        | String    | -        | Yes           | Foreign | m_branch.`'id`                                    | -                                                                                                                                        |
| `party_id`         | String    | -        | Yes           | Foreign | m_party.`'id`                                     | -                                                                                                                                        |
| `employee_id`      | String    | -        | Yes           | Foreign | m_party.`'id`                                     | -                                                                                                                                        |
| `is_primary`       | Boolean   | -        | False         | -       | -                                                 | Primary series in its category (AppObjectId, Org, Branch, Party etc)                                                                     |
| `restart_duration` | String    | -        | FY            | -       | s_category.`duration`<br/>Allowed values: fy, mth | Duration when number will restart from 1. Default is FY i.e. number will start start from 1 on every new FY.                             |
| `min_number`       | Int64     | -        | 1             | -       | -                                                 | Number range for this number series                                                                                                      |
| `max_number`       | Int64     | -        | -             | -       | -                                                 | Number range for this number series                                                                                                      |
| `number_format`    | String    | -        | -             | -       | -                                                 | Format to be used for display number of the document in this series. Format may include keywords like OrgShortName, BranchShortName etc. |
| `start_date`       | Timestamp | –        | –             | –       | –                                                 | -                                                                                                                                        |
| `end_date`         | Timestamp | Yes      | –             | –       | –                                                 | -                                                                                                                                        |
| `reason_end`       | String    | Yes      | –             | –       | –                                                 | -                                                                                                                                        |
