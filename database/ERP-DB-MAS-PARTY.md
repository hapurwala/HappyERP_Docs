# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Masters
- **Module**: Party (Customer, Vendor, Employee)

Party master is a common collection in **HappyERP** that keeps record of: 

- Employees
- Customers
- Vendors

We keep all these in same collection because there are some common attributes among these 3 entities. For example a customer may be also be a vendor and vice versa. Similarly an Employee may also be a customer in some cases. Additionally, all these are the users of our software. Their login related related information is also kept here.

Following collections are maintained for Party Master

* **m_party**: contains data of the parties - customers, vendors and employees
* **m_party_type**: contains data of customer classification. It also allows hierarchical relations between types (of same type).

## Collection: m_party

| Name                                     | Type      | Optional | Default Value | Key     | Reference           | Remarks                          |
|:---------------------------------------- |:--------- |:-------- |:------------- |:------- |:------------------- |:-------------------------------- |
| `id`                                     | String    | –        | –             | Primary | –                   | Document Id                      |
| `name`                                   | String    | –        | –             | –       | –                   |                                  |
| `short_name`                             | String    | –        | –             | –       | –                   |                                  |
| `identifier`                             | String    | Yes      | –             | –       | –                   |                                  |
| `display_name`                           | String    | –        | –             | –       | –                   |                                  |
| `mobile`                                 | String    | –        | –             | –       | –                   |                                  |
| `mobile_whatsapp`                        | Boolean   | –        | False         | –       | –                   |                                  |
| `mobile_2`                               | String    | Yes      | –             | –       | –                   |                                  |
| `mobile_2_whatsapp`                      | Boolean   | –        | False         | –       | –                   |                                  |
| `email`                                  | String    | Yes      | –             | –       | –                   |                                  |
| `email_personal`                         | String    | Yes      | –             | –       | –                   |                                  |
| `is_employee`                            | Boolean   | -        | False         | -       | -                   |                                  |
| `is_customer`                            | Boolean   | –        | False         | –       | –                   |                                  |
| `is_vendor`                              | Boolean   | –        | False         | –       | –                   |                                  |
| `main_address`                           | Map       | Yes      | –             | –       | Address Map         |                                  |
| `other_addresses`                        | Array Map | Yes      | –             | –       | Address Map         |                                  |
| `employee_data`                          | Map       | Yes      | –             | –       | –                   | Applicable for Employee only     |
| employee_data.`first_name`               | String    | -        |               | -       |                     |                                  |
| employee_data.`middle_name`              | String    | Yes      |               | -       |                     |                                  |
| employee_data.`last_name`                | String    | -        |               | -       |                     |                                  |
| employee_data.`gender`                   | Number    | -        |               | -       |                     |                                  |
| employee_data.`birth_date`               | Timestamp | -        |               | -       |                     |                                  |
| employee_data.`marital_status`           | Number    | -        |               | -       |                     |                                  |
| employee_data.`marriage_date`            | Timestamp | Yes      |               | -       |                     |                                  |
| employee_data.`father_name`              | String    | -        |               | -       |                     |                                  |
| employee_data.`mother_name`              | String    | -        |               | -       |                     |                                  |
| employee_data.`spouse_name`              | String    | Yes      |               | -       |                     |                                  |
| employee_data.`bloodgroup`               | Number    | Yes      |               | -       |                     |                                  |
| employee_data.`employee_code`            | String    | -        |               | Unique  |                     | Unique among active employees    |
| employee_data.`joining_date`             | Timestamp | -        |               | -       |                     |                                  |
| employee_data.`relieve_date`             | Timestamp | Yes      |               | -       |                     |                                  |
| employee_data.`organisation_id`          | Number    | -        |               | Foreign |                     |                                  |
| employee_data.`branch_id`                | Number    | -        |               | Foreign |                     |                                  |
| employee_data.`department_id`            | Number    | -        |               | Foreign |                     |                                  |
| employee_data.`designation`              | Number    | -        |               | Foreign |                     |                                  |
| employee_data.`reporting_to_employee_id` | Number    | Yes      |               | Foreign |                     |                                  |
| `party_data`                             | Map       | Yes      | –             | –       | –                   | Applicable for Customer / Vendor |
| party_data.`customer_type`               | String    | Yes      | –             | Foreign | m_party_category.id |                                  |
| party_data.`vendor_type`                 | String    | Yes      | –             | Foreign | m_party_category.id |                                  |
| party_data.`contact_person`              | String    | Yes      | –             | –       |                     |                                  |
| party_data.`parent_party_id`             | String    | Yes      | –             | Foreign | m_party.id          |                                  |
| party_data.`credit_limit`                | Int64     | Yes      | –             | –       | –                   |                                  |
| party_data.`organisations`               | Array Map | Yes      | –             | –       | –                   |                                  |
| party_data.organisations[].`id`          | String    | –        | –             | Foreign | m_organisation.id   |                                  |
| party_data.organisations[].`start_date`  | Timestamp | –        | –             | –       | –                   |                                  |
| party_data.organisations[].`end_date`    | Timestamp | Yes      | –             | –       | –                   |                                  |
| party_data.organisations[].`reason_end`  | String    | Yes      | –             | –       | –                   |                                  |
| `login`                                  | Map       | Yes      | -             | -       | -                   |                                  |
| login.`auth_user_id`                     | Number    | -        | -             | Foreign | -                   |                                  |
| login.`login_method`                     | Number    | -        | -             | -       | -                   | Mobile, Email, Google            |
| login.`password`                         | String    | -        | -             | -       | -                   |                                  |
| login.`is_logged_in`                     | Boolean   | -        | False         | -       | -                   |                                  |
| login.`is_enabled`                       | Boolean   | -        | True          | -       | -                   |                                  |
| login.`disable_remarks`                  | String    | Yes      | -             | -       | -                   |                                  |
| `roles`                                  | Array Map | Yes      | -             | -       | -                   |                                  |
| roles[].`app_role_id`                    | String    | -        | -             | Foreign | app_role.`id`       |                                  |
| roles[].`start_date`                     | Timestamp | -        | -             | -       | -                   |                                  |
| roles[].`end_date`                       | Timestamp | Yes      | -             | -       | -                   |                                  |
| `regulatory_ids`                         | Array Map | Yes      | –             | –       | `RegulatoryID` Map  | Ids                              |
| `attachments`                            | Array Map | Yes      | –             | –       | `Attachment` Map    | Branch Documents                 |
| `stage`                                  | Map       | –        | –             | –       | `Stage` Map         | Current Stage                    |
| `stage_logs`                             | Array Map | Yes      | –             | –       | `Stage` Map         | Workflow History                 |
| `notes`                                  | Array Map | -        | -             | -       | `Note` Map          | It is an array of Note map       |

## Collection: m_party_type

| Name                       | Type    | Optional | Default Value | Key     | Reference           | Remarks       |
|:-------------------------- |:------- |:-------- |:------------- |:------- |:------------------- |:------------- |
| `id`                       | String  | –        | –             | Primary | –                   | Document Id   |
| `name`                     | String  | –        | –             | –       | –                   |               |
| `parent_party_category_id` | String  | Yes      | –             | Foreign | m_party_category.id | Party Channel |
| `maintain_account_head`    | Boolean | –        | False         | –       | –                   |               |
| `is_employee`              | Boolean | –        | False         | –       | –                   |               |
| `is_customer`              | Boolean | –        | False         | –       | –                   |               |
| `is_vendor`                | Boolean | –        | False         | –       | –                   |               |
