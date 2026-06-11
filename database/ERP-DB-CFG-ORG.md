# Collection: m_organisation

Contains Organisation master data.

## General Information

| Name                         | Type      | Optional | Default Value | Key     | Reference       | Remarks                    |
| ---------------------------- | --------- | -------- | ------------- | ------- | --------------- | -------------------------- |
| `id`                         | String    | –        | –             | Primary | –               | Document Id                |
| `name`                       | String    | –        | –             | Unique  | –               | Organisation Name          |
| `short_name`                 | String    | –        | –             | Unique  | –               | Organisation Short Name    |
| `display_name`               | String    | Yes      | –             | –       | –               | Display Name               |
| `website`                    | String    | Yes      | –             | –       | –               | Website URL                |
| `email`                      | String    | Yes      | –             | –       | –               | Official Email             |
| `mobile`                     | String    | Yes      | –             | –       | –               | Primary Mobile             |
| `mobile_whatsapp`            | Boolean   | –        | False         | –       | –               | WhatsApp Available         |
| `mobile_2`                   | String    | Yes      | –             | –       | –               | Secondary Mobile           |
| `mobile_2_whatsapp`          | Boolean   | –        | False         | –       | –               | WhatsApp Available         |
| `base_currency_id`           | String    | Yes      | –             | Foreign | m_currency.`id` | Base Currency              |
| `base_currency_short_name`   | String    | Yes      | –             | –       | –               | Currency Short Name        |
| `financial_year_start_month` | Int64     | –        | 4             | –       | –               | Financial Year Start Month |
| `financial_year_start_day`   | Int64     | –        | 1             | –       | –               | Financial Year Start Day   |
| `remarks`                    | String    | Yes      | –             | –       | –               | General Remarks            |
| `logo_url`                   | String    | Yes      | –             | –       | –               | Organisation Logo URL      |
| `start_date`                 | Timestamp | –        | Current Date  | –       | –               | Active From                |
| `end_date`                   | Timestamp | Yes      | –             | –       | –               | Active Till                |
| `reason_end`                 | String    | Yes      | –             | –       | –               | Reason For Closure         |

---

## Address

| Name                    | Type      | Optional | Default Value | Key     | Reference       | Remarks              |
| ----------------------- | --------- | -------- | ------------- | ------- | --------------- | -------------------- |
| `address`               | Map       | Yes      | –             | –       | –               | Organisation Address |
| address.`street`        | String    | Yes      | –             | –       | –               | Street Address       |
| address.`city_id`       | String    | Yes      | –             | Foreign | m_city.`id`     | City                 |
| address.`city_name`     | String    | Yes      | –             | –       | –               | City Name            |
| address.`pin`           | String    | Yes      | –             | –       | –               | PIN / ZIP Code       |
| address.`district_id`   | String    | Yes      | –             | Foreign | m_district.`id` | District             |
| address.`district_name` | String    | Yes      | –             | –       | –               | District Name        |
| address.`state_id`      | String    | Yes      | –             | Foreign | m_state.`id`    | State                |
| address.`state_name`    | String    | Yes      | –             | –       | –               | State Name           |
| address.`country_id`    | String    | Yes      | –             | Foreign | m_country.`id`  | Country              |
| address.`country_name`  | String    | Yes      | –             | –       | –               | Country Name         |
| address.`landmark`      | String    | Yes      | –             | –       | –               | Landmark             |
| address.`location`      | Geopoint  | Yes      | –             | –       | –               | Geo Location         |
| address.`tagged_by`     | String    | Yes      | –             | -       | m_user.`id`     | Tagged By            |
| address.`tagged_on`     | Timestamp | Yes      | –             | –       | –               | Tagged On            |

---

## Regulatory IDs

| Name                              | Type      | Optional | Default Value | Key | Reference         | Remarks                |
| --------------------------------- | --------- | -------- | ------------- | --- | ----------------- | ---------------------- |
| `regulatory_ids`                  | Array Map | Yes      | –             | –   | –                 | Registration Documents |
| regulatory_ids[].`id`             | String    | –        | –             | –   | –                 | Row Id                 |
| regulatory_ids[].`reg_class_id`   | String    | –        | –             | -   | s_category.reg_id | PAN/GSTIN/CIN/FSSAI    |
| regulatory_ids[].`reg_class_name` | String    | –        | –             | –   | –                 | Registration Type      |
| regulatory_ids[].`number`         | String    | –        | –             | –   | –                 | Registration Number    |
| regulatory_ids[].`issue_date`     | Timestamp | Yes      | –             | –   | –                 | Issue Date             |
| regulatory_ids[].`valid_from`     | Timestamp | Yes      | –             | –   | –                 | Valid From             |
| regulatory_ids[].`valid_upto`     | Timestamp | Yes      | –             | –   | –                 | Valid Till             |

---

## Attachments

All fileds are inside [ERP-DB-GEN-Common.md](./ERP-DB-GEN-Common.md)

| Name          | Type      | Optional | Default Value | Key | Reference | Remarks          |
| ------------- | --------- | -------- | ------------- | --- | --------- | ---------------- |
| `attachments` | Array Map | Yes      | –             | –   | –         | Branch Documents |

---

## Workflow

| Name                       | Type      | Optional | Default Value | Key     | Reference               | Remarks          |
| -------------------------- | --------- | -------- | ------------- | ------- | ----------------------- | ---------------- |
| `stage`                    | Map       | –        | –             | –       | –                       | Current Stage    |
| stage.`id`                 | String    | –        | –             | Foreign | m_app_object_stage.`id` | Stage Id         |
| stage.`name`               | String    | –        | –             | –       | –                       | Stage Name       |
| stage.`badge_variant`      | String    | –        | –             | –       | –                       | UI Badge Variant |
| stage.`remarks`            | String    | Yes      | –             | –       | –                       | Remarks          |
| stage.`set_by`             | String    | Yes      | –             | Foreign | m_user.`id`             | Updated By       |
| stage.`set_by_name`        | String    | Yes      | –             | –       | –                       | Updated By Name  |
| stage.`set_at`             | Timestamp | Yes      | –             | –       | –                       | Updated On       |
| `stage_logs`               | Array Map | Yes      | –             | –       | –                       | Workflow History |
| stage_logs[].`stage_id`    | String    | –        | –             | Foreign | m_app_object_stage.`id` | Stage Id         |
| stage_logs[].`stage_name`  | String    | –        | –             | –       | –                       | Stage Name       |
| stage_logs[].`remarks`     | String    | Yes      | –             | –       | –                       | Remarks          |
| stage_logs[].`set_by`      | String    | Yes      | –             | Foreign | m_user.`id`             | Action By        |
| stage_logs[].`set_by_name` | String    | Yes      | –             | –       | –                       | Action By Name   |
| stage_logs[].`set_at`      | Timestamp | Yes      | –             | –       | –                       | Action Date      |

# Collection: m_branch

Contains Branch master data.

## General Information

| Name                 | Type      | Optional | Default Value | Key     | Reference           | Remarks                  |
| -------------------- | --------- | -------- | ------------- | ------- | ------------------- | ------------------------ |
| `id`                 | String    | –        | –             | Primary | –                   | Document Id              |
| `organisation_id`    | String    | –        | –             | Foreign | m_organisation.`id` | Parent Organisation      |
| `organisation_name`  | String    | –        | –             | –       | –                   | Parent Organisation Name |
| `name`               | String    | –        | –             | Unique  | –                   | Branch Name              |
| `short_name`         | String    | –        | –             | Unique  | –                   | Branch Short Name        |
| `display_name`       | String    | Yes      | –             | –       | –                   | Display Name             |
| `mobile`             | String    | Yes      | –             | –       | –                   | Primary Mobile           |
| `mobile_whatsapp`    | Boolean   | –        | False         | –       | –                   | WhatsApp Available       |
| `mobile_2`           | String    | Yes      | –             | –       | –                   | Secondary Mobile         |
| `mobile_2_whatsapp`  | Boolean   | –        | False         | –       | –                   | WhatsApp Available       |
| `email`              | String    | Yes      | –             | –       | –                   | Branch Email             |
| `is_production_unit` | Boolean   | –        | False         | –       | –                   | Production Branch        |
| `is_warehouse`       | Boolean   | –        | False         | –       | –                   | Warehouse Branch         |
| `is_dispatch_center` | Boolean   | –        | False         | –       | –                   | Dispatch Branch          |
| `remarks`            | String    | Yes      | –             | –       | –                   | General Remarks          |
| `start_date`         | Timestamp | –        | Current Date  | –       | –                   | Active From              |
| `end_date`           | Timestamp | Yes      | –             | –       | –                   | Active Till              |
| `reason_end`         | String    | Yes      | –             | –       | –                   | Reason For Closure       |

---

## Address

| Name                    | Type      | Optional | Default Value | Key     | Reference       | Remarks        |
| ----------------------- | --------- | -------- | ------------- | ------- | --------------- | -------------- |
| `address`               | Map       | Yes      | –             | –       | –               | Branch Address |
| address.`street`        | String    | Yes      | –             | –       | –               | Street Address |
| address.`city_id`       | String    | Yes      | –             | Foreign | m_city.`id`     | City           |
| address.`city_name`     | String    | Yes      | –             | –       | –               | City Name      |
| address.`pin`           | String    | Yes      | –             | –       | –               | PIN / ZIP Code |
| address.`district_id`   | String    | Yes      | –             | Foreign | m_district.`id` | District       |
| address.`district_name` | String    | Yes      | –             | –       | –               | District Name  |
| address.`state_id`      | String    | Yes      | –             | Foreign | m_state.`id`    | State          |
| address.`state_name`    | String    | Yes      | –             | –       | –               | State Name     |
| address.`country_id`    | String    | Yes      | –             | Foreign | m_country.`id`  | Country        |
| address.`country_name`  | String    | Yes      | –             | –       | –               | Country Name   |
| address.`landmark`      | String    | Yes      | –             | –       | –               | Landmark       |
| address.`location`      | Geopoint  | Yes      | –             | –       | –               | Geo Location   |
| address.`tagged_by`     | String    | Yes      | –             | Foreign | m_user.`id`     | Tagged By      |
| address.`tagged_on`     | Timestamp | Yes      | –             | –       | –               | Tagged On      |

---

## Regulatory IDs

| Name                              | Type      | Optional | Default Value | Key | Reference | Remarks                |
| --------------------------------- | --------- | -------- | ------------- | --- | --------- | ---------------------- |
| `regulatory_ids`                  | Array Map | Yes      | –             | –   | –         | Registration Documents |
| regulatory_ids[].`id`             | String    | –        | –             | –   | –         | Row Id                 |
| regulatory_ids[].`reg_class_id`   | String    | –        | –             | -   | -         | PAN / GSTIN / FSSAI    |
| regulatory_ids[].`reg_class_name` | String    | –        | –             | –   | –         | Registration Type      |
| regulatory_ids[].`number`         | String    | –        | –             | –   | –         | Registration Number    |
| regulatory_ids[].`issue_date`     | Timestamp | Yes      | –             | –   | –         | Issue Date             |
| regulatory_ids[].`valid_from`     | Timestamp | Yes      | –             | –   | –         | Valid From             |
| regulatory_ids[].`valid_upto`     | Timestamp | Yes      | –             | –   | –         | Valid Till             |

---

## Attachments

All fileds are inside [ERP-DB-GEN-Common.md](./ERP-DB-GEN-Common.md)

| Name          | Type      | Optional | Default Value | Key | Reference | Remarks          |
| ------------- | --------- | -------- | ------------- | --- | --------- | ---------------- |
| `attachments` | Array Map | Yes      | –             | –   | –         | Branch Documents |

---

## Workflow

| Name                  | Type      | Optional | Default Value | Key     | Reference               | Remarks          |
| --------------------- | --------- | -------- | ------------- | ------- | ----------------------- | ---------------- |
| `stage`               | Map       | –        | –             | –       | –                       | Current Stage    |
| stage.`id`            | String    | –        | –             | Foreign | m_app_object_stage.`id` | Stage Id         |
| stage.`name`          | String    | –        | –             | –       | –                       | Stage Name       |
| stage.`badge_variant` | String    | –        | –             | –       | –                       | UI Badge Variant |
| stage.`remarks`       | String    | Yes      | –             | –       | –                       | Remarks          |
| stage.`set_by`        | String    | Yes      | –             | Foreign | m_user.`id`             | Updated By       |
| stage.`set_by_name`   | String    | Yes      | –             | –       | –                       | Updated By Name  |
| stage.`set_at`        | Timestamp | Yes      | –             | –       | –                       | Updated On       |
