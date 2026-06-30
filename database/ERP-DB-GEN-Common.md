# 1. Common Fields in all collections/tables

Following common fields will be maintained in all collections/tables. 

> [!Notes]
> 
> Common fields mentioned here are for general reference. These fields will not be specifically mentioned in all collection definitions. It is assumed that these will be created in collection automatically by the system while creating the document.

| Name          | Type      | Optional | Default Value | Key     | Reference    | Remarks |
|:------------- |:--------- |:-------- |:------------- |:------- |:------------ |:------- |
| `remarks`     | String    | Yes      | –             | -       | -            | -       |
| `created_at`  | Timestamp | -        | now()         | -       | -            | -       |
| `created_by`  | String    | -        | –             | Foreign | m_party.`id` | -       |
| `modified_at` | Timestamp | Yes      | -             | -       | -            | -       |
| `modified_by` | String    | Yes      | -             | Foreign | m_party.`id` | -       |
| `is_deleted`  | Boolean   | -        | False         | -       | -            | -       |

---

# 2. Address related fields (Address Map)

Wherever address is maintained, it is maintained in a Map. Address Map and its fields are given below.

| Name                     | Type      | Optional | Default Value | Key     | Reference      | Remarks               |
|:------------------------ |:--------- |:-------- |:------------- |:------- |:-------------- |:--------------------- |
| `address`                | map       | Yes      | –             | –       | –              | -                     |
| address.`type`           | String    | Yes      | –             | –       | –              | Name given to address |
| address.`same_as_main`   | Boolean   | -        | False         | –       | –              | -                     |
| address.`street`         | String    | Yes      | –             | –       | –              | -                     |
| address.`city_id`        | String    | Yes      | –             | Foreign | m_city.`id`    | -                     |
| address.`city`           | String    | Yes      | –             | –       | –              | -                     |
| address.`pin`            | String    | Yes      | –             | –       | –              | -                     |
| address.`state_id`       | String    | Yes      | –             | Foreign | m_state.`id`   | -                     |
| address.`state`          | String    | Yes      | –             | –       | –              | -                     |
| address.`country_id`     | String    | Yes      | –             | Foreign | m_country.`id` | -                     |
| address.`country`        | String    | Yes      | –             | –       | –              | -                     |
| address.`landmark`       | String    | Yes      | –             | –       | –              | Landmark              |
| address.`location`       | Geopoint  | Yes      | –             | –       | –              | -                     |
| address.`tagged_by`      | String    | Yes      | –             | Foreign | m_party.`id`   | -                     |
| address.`tagged_by_name` | String    | Yes      | –             |         |                | User name             |
| address.`tagged_on`      | Timestamp | Yes      | –             | –       | –              | -                     |

---

# 3. Workflow Stage related fields (Stage Map)

Workflow stages are maintained in almost all **HappyERP** objects. Current stage and its history will be maintained for each object. Stage data will be maintained in a Map. The history of stages will be maintained in an Array of Stage Map.

## 3.1. Stage Map

Following fields are used for keeping details of stage of an object.

| Name                | Type      | Optional | Default Value | Key     | Reference               | Remarks   |
|:------------------- |:--------- |:-------- |:------------- |:------- |:----------------------- |:--------- |
| `stage`             | Map       | -        | -             | -       | -                       | Stage     |
| stage.`id`          | String    | -        | -             | Foreign | s_app_object_stage.`id` |           |
| stage.`name`        | String    | -        | -             | -       |                         |           |
| stage.`remarks`     | String    | Yes      | -             | -       |                         |           |
| stage.`media_url`   | String    | Yes      | -             | -       |                         |           |
| stage.`set_on`      | Timestamp | -        | -             | -       |                         |           |
| stage.`set_by`      | String    | -        |               | Foreign | m_party.`id`            |           |
| stage.`set_by_name` | String    | -        |               |         |                         | User name |

## 3.2. Stage Log Array Map

An array if Stage Map is used for keeping history of workflow stages.

| Name         | Type              | Optional | Default Value | Key | Reference | Remarks                     |
|:------------ |:----------------- |:-------- |:------------- |:--- |:--------- |:--------------------------- |
| `stage_logs` | Array Map (Stage) | -        | -             | -   | -         | It is an array of Stage map |

---

# 4. Notes related fields (Note Map)

User notes are maintained in almost all **HappyERP** objects. Notes are Read Only remarks/ comment/ queries entered by user on an object. Normally Notes can be used as follow up chat on that object. Single Note is maintained in a map. An array of Notes is maintained with objects.

## 4.1. Note Map

Following fields are used for keeping a note in an object.

| Name               | Type      | Optional | Default Value | Key     | Reference    | Remarks                                                       |
|:------------------ |:--------- |:-------- |:------------- |:------- |:------------ |:------------------------------------------------------------- |
| `note`             | Map       | Yes      | -             | -       | -            | -                                                             |
| note.`id`          | String    | -        | -             | -       | -            | -                                                             |
| note.`ref_note_id` | String    | Yes      | -             | -       | -            | Current Note is response of this Note                         |
| note.`text`        | String    | -        | -             | -       | -            | -                                                             |
| note.`media_url`   | String    | Yes      | -             | -       | -            | -                                                             |
| note.`is_public`   | Boolean   | -        | False         | -       | -            | Is it visible to external users (customer, vendor etc.) also? |
| note.`created_by`  | String    | -        | -             | Foreign | m_party.`id` | -                                                             |
| note.`created_at`  | Timestamp | -        | now()         | -       | -            | -                                                             |

## 4.2. Notes Array Map

An array of Note Map is used for keeping complete chat history.

| Name    | Type             | Optional | Default Value | Key | Reference | Remarks                    |
|:------- |:---------------- |:-------- |:------------- |:--- |:--------- |:-------------------------- |
| `notes` | Array Map (Note) | -        | -             | -   | -         | It is an array of Note map |

---

# 5. Attachments/Media related fields (Attachment Map)

Attachments are maintained with almost all **HappyERP** objects. Attachments can be images, PDF, Documents or any other files. Normally one file will have a limit of 2 MB. If a bigger file is attached, system will try to compress it to fit in 2 MB. Details of an attachment is maintained in a map. An array of attachments is maintained with objects.

## 5.1. Attachment Map

Following fields are used for keeping details of an Attachment in an object.

| Name                          | Type      | Optional | Default Value | Key     | Reference   | Remarks                           |
|:----------------------------- |:--------- |:-------- |:------------- |:------- |:----------- |:--------------------------------- |
| `attachment`                  | Map       | –        | –             | –       | –           | Attachment Map                    |
| attachment.`name`             | String    | Yes      | –             | –       | –           | Display Name                      |
| attachment.`description`      | String    | Yes      | –             | –       | –           | Attachment Description            |
| attachment.`file_name`        | String    | –        | –             | –       | –           | Original File Name                |
| attachment.`file_extension`   | String    | Yes      | –             | –       | –           | pdf, jpg, png, xlsx, docx         |
| attachment.`file_type`        | String    | –        | –             | –       | –           | PDF, image, Document, Spreadsheet |
| attachment.`url`              | String    | –        | –             | –       | –           | URL on Server                     |
| attachment.`file_size`        | Int64     | Yes      | 0             | –       | –           | File size in Bytes                |
| attachment.`is_primary`       | Boolean   | –        | False         | –       | –           | Primary Attachment?               |
| attachment.`sequence`         | Int64     | –        | 0             | –       | –           | Display Sequence                  |
| attachment.`start_date`       | Timestamp | Yes      | –             | –       | –           | Valid From                        |
| attachment.`end_date`         | Timestamp | Yes      | –             | –       | –           | Valid Till                        |
| attachment.`uploaded_by`      | String    | Yes      | –             | Foreign | m_user.`id` | Uploaded By                       |
| attachment.`uploaded_by_name` | String    | Yes      | –             | –       | –           | Uploading User Name               |
| attachment.`uploaded_at`      | Timestamp | Yes      | –             | –       | –           | Uploaded Date                     |

## 5.2. Attachment Array Map

An array of Attachment Map is used for keeping multiple attachments in an object.

| Name          | Type                   | Optional | Default Value | Key | Reference | Remarks                          |
|:------------- |:---------------------- |:-------- |:------------- |:--- |:--------- |:-------------------------------- |
| `attachments` | Array Map (Attachment) | -        | -             | -   | -         | It is an array of Attachment map |

---

# 6. Regulatory IDs related fields (RegulatoryID Map)

Regulatory IDs are different types of IDs (PAN, Aadhaar etc) assigned to individuals and organisations. Multiple IDs may be maintained for one object. Details of a ID is maintained in a map. An array of Regulatory IDs is maintained with objects to keep multiple IDs.

## 6.1. RegulatoryID Map

Following fields are used for keeping details of an ID in an object.

| Name                            | Type      | Optional | Default Value | Key | Reference           | Remarks                   |
| ------------------------------- | --------- | -------- | ------------- | --- | ------------------- | ------------------------- |
| `regulatory_id`                 | Array Map | Yes      | –             | –   | –                   | ID                        |
| regulatory_ids.`reg_class_id`   | String    | –        | –             | -   | s_category.`reg_id` | PAN / GSTIN / CIN / FSSAI |
| regulatory_ids.`reg_class_name` | String    | –        | –             | –   | –                   | Registration Type         |
| regulatory_ids.`number`         | String    | –        | –             | –   | –                   | Registration Number       |
| regulatory_ids.`issue_date`     | Timestamp | Yes      | –             | –   | –                   | Issue Date                |
| regulatory_ids.`valid_from`     | Timestamp | Yes      | –             | –   | –                   | Valid From                |
| regulatory_ids.`valid_upto`     | Timestamp | Yes      | –             | –   | –                   | Valid Till                |

## 6.2. RegulatoryID Array Map

An array of regulatory_id Map is used for keeping multiple IDs in an object.

| Name             | Type      | Optional | Default Value | Key | Reference        | Remarks |
|:---------------- |:--------- |:-------- |:------------- |:--- |:---------------- |:------- |
| `regulatory_ids` | Array Map | Yes      | –             | –   | RegulatoryID Map | Ids     |

---

# 7. Bank Account related fields (BankAccount Map)

Bank Account related information is maintained with parties (Customer, Vendor, Employee etc) and organisation/branches. Details of a Bank Account is maintained in a map. An array of attachments is maintained with objects.

## 7.1. BankAccount Map

Following fields are used for keeping details of a Bank Account.

| Name                            | Type    | Optional | Default Value | Key     | Reference                 | Remarks                      |
|:------------------------------- |:------- |:-------- |:------------- |:------- |:------------------------- |:---------------------------- |
| `bank_account`                  | Map     | -        | –             | –       | –                         | BankAccount Map              |
| bank_account.`bank_id`          | String  | -        | –             | Foreign | m_bank.`id`               |                              |
| bank_account.`bank_name`        | String  | -        | –             | –       | –                         |                              |
| bank_account.`bank_logo_url`    | String  | Yes      | –             | –       | –                         |                              |
| bank_account.`account_number`   | String  | -        | –             | –       | –                         |                              |
| bank_account.`account_category` | String  | -        | –             | –       | s_cateogry.`bank_account` | Savings/Current              |
| bank_account.`virtual_id`       | String  | Yes      | –             | –       | –                         | Virtual ID given by the bank |
| bank_account.`micr`             | String  | Yes      | 0             | –       | –                         |                              |
| bank_account.`is_primary`       | Boolean | –        | False         | –       | –                         | Primary Account?             |

## 7.2. BankAccount Array Map

An array of BankAccount Map is used for keeping details of multiple bank accounts.

| Name            | Type                   | Optional | Default Value | Key | Reference | Remarks                           |
|:--------------- |:---------------------- |:-------- |:------------- |:--- |:--------- |:--------------------------------- |
| `bank_accounts` | Array Map BankAccount) | -        | -             | -   | -         | It is an array of BankAccount map |
