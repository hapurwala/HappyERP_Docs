# Common Fields in all collections/tables

Following common fields will be maintained in all collections/tables. 

> [!Notes]
> 
> Common fields mentioned here are for general reference. These fields will not be specifically given in all collection definition. It is assumed that these will be created in collection automatically by the system while creating the document.

| Name          | Type      | Optional | Default Value | Key     | Reference  | Remarks |
|:------------- |:--------- |:-------- |:------------- |:------- |:---------- |:------- |
| `remarks`     | String    | Yes      | –             | --      | --         | --      |
| `created_at`  | Timestamp | --       | now()         | --      | --         | --      |
| `created_by`  | String    | --       | –             | Foreign | m_party.id | --      |
| `modified_at` | Timestamp | Yes      | --            | --      | --         | --      |
| `modified_by` | String    | Yes      | --            | Foreign | m_party.id | --      |
| `is_deleted`  | Boolean   | --       | False         | --      | --         | --      |

# Workflow Stage related fields

Workflow stages are maintained in almost all **HappyERP** objects. Current stage and its history will be maintained for each object. Following fields will be used for keeping details of stage and its history.

| Name                        | Type      | Optional | Default Value | Key     | Reference             | Remarks                     |
| --------------------------- | --------- | -------- | ------------- | ------- | --------------------- | --------------------------- |
| stage                       | Map       | --       | --            | --      | --                    | Current Stage of the object |
| stage.`id`                  | String    | --       | --            | Foreign | s_app_object_stage.id |                             |
| stage.`name`                | String    | --       | --            | --      |                       |                             |
| stage.`remarks`             | String    | Yes      | --            | --      |                       |                             |
| stage.`media_url`           | String    | Yes      | --            | --      |                       |                             |
| stage.`set_on`              | Timestamp | --       | --            | --      |                       |                             |
| stage.`set_by`              | String    | --       |               | Foreign | m_party.id            |                             |
| `stage_history`             | array map | --       | --            | --      |                       |                             |
| stage_history[].`id`        | String    | --       |               | Foreign | s_app_object_stage.id |                             |
| stage_history[].`name`      | String    | --       | --            | --      |                       |                             |
| stage_history[].`remarks`   | String    | Yes      | --            | --      |                       |                             |
| stage_history[].`media_url` | String    | Yes      | --            | --      |                       |                             |
| stage_history[].`set_on`    | Timestamp | --       | now()         | --      |                       |                             |
| stage_history[].`set_by`    | Number    | --       | --            | --      | m_party.id            |                             |

# Address related fields

| Name                 | Type      | Optional | Default Value | Key     | Reference    | Remarks |
|:-------------------- |:--------- |:-------- |:------------- |:------- |:------------ |:------- |
| address              | map       | Yes      | –             | –       | –            |         |
| address.`street`     | String    | Yes      | –             | –       | –            |         |
| address.`city_id`    | String    | Yes      | –             | Foreign | m_city.id    |         |
| address.`city`       | String    | Yes      | –             | –       | –            |         |
| address.`pin`        | String    | Yes      | –             | –       | –            |         |
| address.`state_id`   | String    | Yes      | –             | Foreign | m_state.id   |         |
| address.`state`      | String    | Yes      | –             | –       | –            |         |
| address.`country_id` | String    | Yes      | –             | Foreign | m_country.id |         |
| address.`country`    | String    | Yes      | –             | –       | –            |         |
| address.`location`   | Geopoint  | Yes      | –             | –       | –            |         |
| address.`tagged_by`  | String    | Yes      | –             | Foreign | m_party.id   |         |
| address.`tagged_on`  | Timestamp | Yes      | –             | –       | –            |         |

# Notes related fields

| Name                 | Type      | Optional | Default Value | Key     | Reference  | Remarks |
| -------------------- | --------- | -------- | ------------- | ------- | ---------- | ------- |
| Notes                | Map       | Yes      | --            | --      | --         | --      |
| notes[].`text`       | String    | --       | --            | --      | --         | --      |
| notes[].`media_url`  | String    | Yes      | --            | --      | --         | --      |
| notes[].`created_by` | String    | --       | --            | Foreign | m_party.id | --      |
| notes[].`created_at` | Timestamp | --       | now()         | --      | --         | --      |

# Attachments/Media related fields

| Name               | Type      | Optional | Default Value | Key     | Reference   | Remarks                           |
| ------------------ | --------- | -------- | ------------- | ------- | ----------- | --------------------------------- |
| id                 | String    | –        | –             | –       | –           | Attachment Id                     |
| `name`             | String    | Yes      | –             | –       | –           | Display Name                      |
| `description`      | String    | Yes      | –             | –       | –           | Attachment Description            |
| `file_name`        | String    | –        | –             | –       | –           | Original File Name                |
| `file_extension`   | String    | Yes      | –             | –       | –           | pdf, jpg, png, xlsx, docx         |
| `file_type`        | String    | –        | –             | –       | –           | PDF, image, Document, Spreadsheet |
| `url`              | String    | –        | –             | –       | –           | URL on Server                     |
| `file_size`        | Int64     | Yes      | 0             | –       | –           | File size in Bytes                |
| `is_primary`       | Boolean   | –        | False         | –       | –           | Primary Attachment?               |
| `sequence`         | Int64     | –        | 0             | –       | –           | Display Sequence                  |
| `start_date`       | Timestamp | Yes      | –             | –       | –           | Valid From                        |
| `end_date`         | Timestamp | Yes      | –             | –       | –           | Valid Till                        |
| `uploaded_by`      | String    | Yes      | –             | Foreign | m_user.`id` | Uploaded By                       |
| `uploaded_by_name` | String    | Yes      | –             | –       | –           | Uploading User Name               |
| `uploaded_at`      | Timestamp | Yes      | –             | –       | –           | Uploaded Date                     |
