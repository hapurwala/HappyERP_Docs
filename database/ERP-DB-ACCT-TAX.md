# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Accounts
- **Module**: Taxation

Different countries have different tax system. Even one country may have multiple tax systems. **HappyERP** supports all such tax systems. This document gives details of the collections that are used to keep tax related data. The tax system includes:

- **Tax System/Regime**: Basic details of a tax system like Name, Applicability etc 
- **Tax**: The taxes applicable within a tax system and its applicability various parameters.
- **Tax Payer Type**: A tax system may support different Taxpayer registration types based on type of business or turn etc.

## Collection: m_tax_system

| Name                        | Type      | Optional | Default Value | Key     | Reference | Remarks                                                                         |
| --------------------------- | --------- | -------- | ------------- | ------- | --------- | ------------------------------------------------------------------------------- |
| `id`                        | String    | –        | –             | Primary | –         | Document Id                                                                     |
| `name`                      | String    | –        | –             | Unique  | –         |                                                                                 |
| `short_name`                | String    | –        | –             | Unique  | –         |                                                                                 |
| `input_credit_applicable`   | Boolean   | -        | False         | -       | -         | Whether input credit applicable in this tax system                              |
| `reverse_charge_applicable` | Boolean   | -        | False         | -       | -         | Whether reverse charge applicable on purchase if tax is not applied by supplier |
| `start_date`                | Timestamp | –        | Current Date  | –       | –         | Active From                                                                     |
| `end_date`                  | Timestamp | Yes      | –             | –       | –         | Active Till                                                                     |
| `reason_end`                | String    | Yes      | –             | –       | –         | Reason For Closure                                                              |
|                             |           |          |               |         |           |                                                                                 |

## Collection: m_tax

| Name                  | Type      | Optional | Default Value | Key     | Reference         | Remarks                                                                                         |
| --------------------- | --------- | -------- | ------------- | ------- | ----------------- | ----------------------------------------------------------------------------------------------- |
| `id`                  | String    | –        | –             | Primary | –                 | Document Id                                                                                     |
| `name`                | String    | –        | –             | Unique  | –                 | -                                                                                               |
| `short_name`          | String    | –        | –             | Unique  | –                 | -                                                                                               |
| `tax_system_id`       | String    | –        | –             | Foreign | m_tax_system.`id` | -                                                                                               |
| `round_decimal_place` | Int64     | -        | 2             | -       | -                 | Number of Decimal Places to use in rounding. A negative number indicates places before decimal. |
| `sequence`            | Int64     | -        | -             | -       | -                 | -                                                                                               |
| `start_date`          | Timestamp | –        | Current Date  | –       | –                 | Active From                                                                                     |
| `end_date`            | Timestamp | Yes      | –             | –       | –                 | Active Till                                                                                     |
| `reason_end`          | String    | Yes      | –             | –       | –                 | Reason For Closure                                                                              |

## Collection: m_taxpayer_type

| Name                                           | Type      | Optional | Default Value | Key     | Reference                  | Remarks            |
| ---------------------------------------------- | --------- | -------- | ------------- | ------- | -------------------------- | ------------------ |
| `id`                                           | String    | –        | –             | Primary | –                          | Document Id        |
| `name`                                         | String    | –        | –             | Unique  | –                          |                    |
| `short_name`                                   | String    | –        | –             | Unique  | –                          |                    |
| `tax_system_id`                                | String    | –        | –             | Foreign | m_tax_system.`id`          |                    |
| `sales_applicability`                          | Array Map | -        | -             | -       | -                          | -                  |
| sales_applicability[].`sales_type`             | String    | -        | -             | -       | s_category.`sales_type`    | -                  |
| sales_applicability[].`is_turnover_limit`      | Boolean   | -        | False         | -       | -                          | -                  |
| sales_applicability[].`minimum_turnover`       | Double    | -        | 0             | -       | -                          | -                  |
| sales_applicability[].`maximum_turnover`       | Double    | -        | 0             | -       | -                          | -                  |
| sales_applicability[].`Taxes`                  | Array     | -        | -             | Foreign | m_tax.`id`                 |                    |
| `purchase_applicability`                       | Array Map | -        | -             | -       | -                          | -                  |
| purchase_applicability[].`party_taxpayer_type` | String    | -        | -             | -       | m_taxpayer_type.`id`       | -                  |
| purchase_applicability[].`purchase_type`       | String    | -        | -             | -       | s_category.`purchase_type` | -                  |
| purchase_applicability[].`Taxes`               | Array     | -        | -             | Foreign | m_tax.`id`                 |                    |
| `start_date`                                   | Timestamp | –        | Current Date  | –       | –                          | Active From        |
| `end_date`                                     | Timestamp | Yes      | –             | –       | –                          | Active Till        |
| `reason_end`                                   | String    | Yes      | –             | –       | –                          | Reason For Closure |
