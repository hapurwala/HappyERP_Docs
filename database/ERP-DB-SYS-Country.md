# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: System
- **Module**: Country, State & Currency

This module manages the global geographical and currency data used for address auto-completion and validation across the ERP.

> [!Note]
> As discussed, `City` is not maintained as a strict master collection to prevent database bloat. Instead, City is saved as a plain text string within the Address Map, utilizing a frontend geocoding API for autocomplete during data entry.

## Collection: s_country

This collection keeps record of all the countries. 

| Name                        | Type      | Optional | Default Value | Key     | Reference         | Remarks                                    |
| --------------------------- | --------- | -------- | ------------- | ------- | ----------------- | ------------------------------------------ |
| `id`                        | String    | –        | –             | Primary | –                 | ISO 2-letter Country Code (e.g., IN)       |
| `name`                      | String    | –        | –             | Unique  | –                 | Full Country Name                          |
| `dial_code`                 | String    | Yes      | –             | –       | –                 | Phone dial code (e.g., +91)                |
| `currency_id`               | String    | Yes      | –             | Foreign | s_currency.`id`   | Default currency for this country          |
| `tax_systems`               | Array Map | Yes      | –             | –       | –                 | -                                          |
| tax_systems.`tax_system_id` | String    | -        | –             | Foreign | m_tax_system.`id` | Applicable Tax System                      |
| tax_systems.`start_date`    | Timestamp | –        | Current Date  | –       | –                 | Active From                                |
| tax_systems.`end_date`      | Timestamp | Yes      | –             | –       | –                 | Active Till                                |
| tax_systems.`reason_end`    | String    | Yes      | –             | –       | –                 | Reason For Closure                         |
| `is_active`                 | Boolean   | -        | False         | -       | -                 | Active means this country is used by users |

## Collection: s_state

This collection keeps record of all the states/provinces. States are always linked to a parent country.

| Name                        | Type      | Optional | Default Value | Key     | Reference         | Remarks                                  |
| --------------------------- | --------- | -------- | ------------- | ------- | ----------------- | ---------------------------------------- |
| `id`                        | String    | –        | –             | Primary | –                 | Standard State Code (e.g., MH)           |
| `country_id`                | String    | –        | –             | Foreign | s_country.`id`    | Parent Country Code                      |
| `name`                      | String    | –        | –             | –       | –                 | Full State Name                          |
| `tax_systems`               | Array Map | Yes      | –             | –       | –                 | -                                        |
| tax_systems.`tax_system_id` | String    | -        | –             | Foreign | m_tax_system.`id` | Applicable Tax System                    |
| tax_systems.`start_date`    | Timestamp | –        | Current Date  | –       | –                 | Active From                              |
| tax_systems.`end_date`      | Timestamp | Yes      | –             | –       | –                 | Active Till                              |
| tax_systems.`reason_end`    | String    | Yes      | –             | –       | –                 | Reason For Closure                       |
| `is_active`                 | Boolean   | -        | False         | -       | -                 | Active means this state is used by users |

## Collection: s_currency

This collection keeps record of all global currencies. 

| Name             | Type      | Optional | Default Value | Key     | Reference | Remarks                                               |
| ---------------- | --------- | -------- | ------------- | ------- | --------- | ----------------------------------------------------- |
| `id`             | String    | –        | –             | Primary | –         | 3-Letter ISO Code (e.g., INR)                         |
| `name`           | String    | –        | –             | Unique  | –         | Full Currency Name                                    |
| `symbol`         | String    | Yes      | –             | –       | –         | Currency Symbol (e.g., ₹)                             |
| `decimal_places` | Number    | –        | 2             | –       | –         | Number of decimal places (e.g., 2 for USD, 0 for JPY) |
| `start_date`     | Timestamp | –        | Current Date  | –       | –         | Active From                                           |
| `end_date`       | Timestamp | Yes      | –             | –       | –         | Active Till                                           |
| `reason_end`     | String    | Yes      | –             | –       | –         | Reason For Closure                                    |
| `is_active`      | Boolean   | -        | False         | -       | -         | Active means this currency is used by users           |
