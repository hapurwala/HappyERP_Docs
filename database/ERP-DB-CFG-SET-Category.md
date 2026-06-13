# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Configuration
- **Module**: Settings

**HappyERP** allows user to configure some settings that control the working, look and feel of the software. All such information will be saved in the database. This document gives details of the collections that keep the configuration data. 

## Collection: s_category

While using the software there are requirements of using several categories. These categories may be simple informative data or may also lead to some decision making. Each category has some predefined values. For example, User Category may define if user is an internal user or external user.

All such categories are kept together in this collection. In effect, this collection is a group of several small collections. Each inner collection defines a category. 

| Name              | Type        | Optional | Default Value | Key     | Reference | Remarks                                |
| ----------------- | ----------- | -------- | ------------- | ------- | --------- | -------------------------------------- |
| `id`              | Auto        | -        | -             | Primary | -         | Document Id                            |
| `key`             | String      | -        | -             | Unique  | -         | Internal Key Name                      |
| `name`            | String      | -        | -             | Unique  | -         | Key Name to Display                    |
| `description`     | String      | Yes      | -             | Unique  | -         |                                        |
| `values`          | Array (Map) | -        | -             | -       | -         |                                        |
| values.`id`       | Number      | -        | -             | Unique  | -         | Unique within current Key              |
| values.`value`    | String      | -        | -             | Unique  | -         |                                        |
| values.`is_fixed` | String      | -        | False         | Unique  | -         | If Fixed, user cannot modify/delete it |
| `is_internal`     | Boolean     | -        | False         | Unique  | -         | System defined category                |

## Collection: s_config

These are configuration settings done by admin at software/organisation level. Settings saved here will be used thouggout the software. For example format to display date etc may be saved here. Some of the settings saved here may affect the working of the software e.g. rounding to be used in sales invoice may change the final value of the invoice.

| Name            | Type    | Optional | Default Value | Key     | Reference | Remarks                      |
| --------------- | ------- | -------- | ------------- | ------- | --------- | ---------------------------- |
| `id`            | Auto    | -        | -             | Primary | -         | Document Id                  |
| `section`       | String  | -        | -             | -       | -         |                              |
| `section_name`  | String  | -        | -             | -       | -         |                              |
| `key`           | String  | -        | -             | -       | -         |                              |
| `key_name`      | String  | -        | -             | -       | -         |                              |
| `value`         | String  | -        | -             | -       | -         |                              |
| `default_value` | String  | -        | -             | -       | -         |                              |
| `lov`           | Array   | -        | -             | -       | -         |                              |
| `is_internal`   | Boolean | -        | False         | Unique  | -         | System defined configuration |

## Collection: s_preference

This collection keeps record of user preferences that are saved and used while using the software. For example UI theme selected by user or filter used in a report may be kept here. The data in this collection is for internal purpose only and will never be shown to the user.

| Name              | Type   | Optional | Default Value | Key     | Reference    | Remarks                                |
| ----------------- | ------ | -------- | ------------- | ------- | ------------ | -------------------------------------- |
| `id`              | Auto   | -        | -             | Primary | -            | Document Id                            |
| `user_id`         | String | -        | -             | Foreign | m_party.`id` |                                        |
| `ref_key`         | String | -        | -             | -       | -            | Key for reference                      |
| `preference_data` | String | -        | -             | -       | -            | Preference data is kept in JSON format |
