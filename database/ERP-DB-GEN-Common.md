# Common Fields in all collections/tables

Following common fields will be maintained in all collections/tables. 

> [!Notes]
> 
> Common fields mentioned here are for general reference. These fields will not be specifically mentioned in all collection definitions. It is assumed that these will be created in collection automatically by the system while creating the document.

| Name          | Type      | Optional | Default Value | Key     | Reference  | Remarks |
|:------------- |:--------- |:-------- |:------------- |:------- |:---------- |:------- |
| `remarks`     | String    | Yes      | –             | -       | -          | -       |
| `created_at`  | Timestamp | -        | now()         | -       | -          | -       |
| `created_by`  | String    | -        | –             | Foreign | m_party.id | -       |
| `modified_at` | Timestamp | Yes      | -             | -       | -          | -       |
| `modified_by` | String    | Yes      | -             | Foreign | m_party.id | -       |
| `is_deleted`  | Boolean   | -        | False         | -       | -          | -       |

# Address related fields (Address Map)

Wherever address is maintained, it is maintained in a Map. Address Map and its fields are given below.

| Name                     | Type      | Optional | Default Value | Key     | Reference      | Remarks   |
|:------------------------ |:--------- |:-------- |:------------- |:------- |:-------------- |:--------- |
| `address`                | map       | Yes      | –             | –       | –              | -         |
| address.`street`         | String    | Yes      | –             | –       | –              | -         |
| address.`city_id`        | String    | Yes      | –             | Foreign | m_city.`id`    | -         |
| address.`city`           | String    | Yes      | –             | –       | –              | -         |
| address.`pin`            | String    | Yes      | –             | –       | –              | -         |
| address.`state_id`       | String    | Yes      | –             | Foreign | m_state.`id`   | -         |
| address.`state`          | String    | Yes      | –             | –       | –              | -         |
| address.`country_id`     | String    | Yes      | –             | Foreign | m_country.`id` | -         |
| address.`country`        | String    | Yes      | –             | –       | –              | -         |
| address.`location`       | Geopoint  | Yes      | –             | –       | –              | -         |
| address.`tagged_by`      | String    | Yes      | –             | Foreign | m_party.`id`   | -         |
| address.`tagged_by_name` | String    | Yes      | –             |         |                | User name |
| address.`tagged_on`      | Timestamp | Yes      | –             | –       | –              | -         |

# Workflow Stage related fields (Stage Map)

Workflow stages are maintained in almost all **HappyERP** objects. Current stage and its history will be maintained for each object. Stage data will be maintained in a Map. The history of stages will be maintained in an Array of Stage Map.

## Stage Map

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

## Stage Log Array Map

An array if Stage Map is used for keeping history of workflow stages.

| Name         | Type              | Optional | Default Value | Key | Reference | Remarks                     |
|:------------ |:----------------- |:-------- |:------------- |:--- |:--------- |:--------------------------- |
| `stage_logs` | Array Map (Stage) | -        | -             | -   | -         | It is an array of Stage map |

# Notes related fields (Note Map)

User notes are maintained in almost all **HappyERP** objects. Notes are Read Only remarks/ comment/ queries entered by user on an object. Normally Notes can be used as follow up chat on that object. Single Note is maintained in a map. An array of Notes is maintained with objects.

## Note Map

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

## Notes Array Map

An array of Note Map is used for keeping complete chat history.

| Name    | Type             | Optional | Default Value | Key | Reference | Remarks                    |
|:------- |:---------------- |:-------- |:------------- |:--- |:--------- |:-------------------------- |
| `notes` | Array Map (Note) | -        | -             | -   | -         | It is an array of Note map |

# Attachments/Media related fields (Attachment Map)

Attachments are maintained with almost all **HappyERP** objects. Attachments can be images, PDF, Documents or any other files. Normally one file will have a limit of 2 MB. If a bigger file is attached, system will try to compress it to fit in 2 MB. Details of an attachment is maintained in a map. An array of attachments is maintained with objects.

## Attachment Map

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

## Attachment Array Map

An array of Attachment Map is used for keeping multiple attachments in an object.

| Name          | Type                   | Optional | Default Value | Key | Reference | Remarks                          |
|:------------- |:---------------------- |:-------- |:------------- |:--- |:--------- |:-------------------------------- |
| `attachments` | Array Map (Attachment) | -        | -             | -   | -         | It is an array of Attachment map |

# Regulatory IDs related fields (RegulatoryID Map)

Regulatory IDs are different types of IDs (PAN, Aadhaar etc) assigned to individuals and organisations. Multiple IDs may be maintained for one object. Details of a ID is maintained in a map. An array of Regulatory IDs is maintained with objects to keep multiple IDs.

## RegulatoryID Map

Following fields are used for keeping details of an ID in an object.

| Name                            | Type      | Optional | Default Value | Key | Reference         | Remarks                   |
| ------------------------------- | --------- | -------- | ------------- | --- | ----------------- | ------------------------- |
| `regulatory_id`                 | Array Map | Yes      | –             | –   | –                 | ID                        |
| regulatory_ids.`reg_class_id`   | String    | –        | –             | -   | s_category.reg_id | PAN / GSTIN / CIN / FSSAI |
| regulatory_ids.`reg_class_name` | String    | –        | –             | –   | –                 | Registration Type         |
| regulatory_ids.`number`         | String    | –        | –             | –   | –                 | Registration Number       |
| regulatory_ids.`issue_date`     | Timestamp | Yes      | –             | –   | –                 | Issue Date                |
| regulatory_ids.`valid_from`     | Timestamp | Yes      | –             | –   | –                 | Valid From                |
| regulatory_ids.`valid_upto`     | Timestamp | Yes      | –             | –   | –                 | Valid Till                |

## RegulatoryID Array Map

An array of regulatory_id Map is used for keeping multiple IDs in an object.

| Name             | Type      | Optional | Default Value | Key | Reference        | Remarks |
|:---------------- |:--------- |:-------- |:------------- |:--- |:---------------- |:------- |
| `regulatory_ids` | Array Map | Yes      | –             | –   | RegulatoryID Map | Ids     |
