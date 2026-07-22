# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Configuration
- **Module**: Organisations

**HappyERP** supports multiple organisations of the user. It also supports multiple branches of an organisation. This document gives details of collections used to maintain data of Organisation(s) and their Branch(es).

## Collection: m_organisation

This collection keeps record of the organisations of the user. Following information is maintained for an organisation:

- **General Information**: Name, short name, website and contact information etc
- **Address**: Single address is maintained with organisation. The address is maintained using Address map given in [ERP-DB-Gen-Common.md](./ERP-DB-Gen-Common.md) 
- **Regulatory Ids**: IDs like PAN, TAN, GSTIN etc. The ID details are maintained using RegulatoryID map given in  [ERP-DB-Gen-Common.md](./ERP-DB-Gen-Common.md)
- **Attachments**: Multiple attachments are allowed with an organisation. The attachment details are kept using the Attachment map given in [ERP-DB-GEN-Common.md](./ERP-DB-GEN-Common.md)
- **Stage Log**: Current stage and stage log is maintained for an organisation. Stage information is kept using Stage Map given in [ERP-DB-Gen-Common.md](./ERP-DB-Gen-Common.md)
- **Notes**: Multiple notes can also be maintained with an organisation. Note information is maintained using Note map given in [ERP-DB-Gen-Common.md](./ERP-DB-Gen-Common.md)

| Name                         | Type      | Optional | Default Value | Key     | Reference        | Remarks                    |
| ---------------------------- | --------- | -------- | ------------- | ------- | ---------------- | -------------------------- |
| `id`                         | String    | –        | –             | Primary | –                | Document Id                |
| `name`                       | String    | –        | –             | Unique  | –                | Organisation Name          |
| `short_name`                 | String    | –        | –             | Unique  | –                | Organisation Short Name    |
| `display_name`               | String    | Yes      | –             | –       | –                | Display Name               |
| `address`                    | Map       | Yes      | –             | –       | `Address` Map    | Organisation Address       |
| `website`                    | String    | Yes      | –             | –       | –                | Website URL                |
| `email`                      | String    | Yes      | –             | –       | –                | Official Email             |
| `mobile`                     | String    | Yes      | –             | –       | –                | Primary Mobile             |
| `mobile_whatsapp`            | Boolean   | –        | False         | –       | –                | WhatsApp Available         |
| `mobile_2`                   | String    | Yes      | –             | –       | –                | Secondary Mobile           |
| `mobile_2_whatsapp`          | Boolean   | –        | False         | –       | –                | WhatsApp Available         |
| `base_currency_id`           | String    | Yes      | –             | Foreign | m_currency.`id`  | Home Currency              |
| `base_currency_short_name`   | String    | Yes      | –             | –       | –                | Home Currency Short Name   |
| `financial_year_start_month` | Int64     | –        | 4             | –       | –                | Financial Year Start Month |
| `financial_year_start_day`   | Int64     | –        | 1             | –       | –                | Financial Year Start Day   |
| `logo_url`                   | String    | Yes      | –             | –       | –                | Organisation Logo URL      |
| `start_date`                 | Timestamp | –        | Current Date  | –       | –                | Active From                |
| `end_date`                   | Timestamp | Yes      | –             | –       | –                | Active Till                |
| `reason_end`                 | String    | Yes      | –             | –       | –                | Reason For Closure         |
| `tax_systems`                | Array Map | Yes      | –             | –       | `TaxSystem` Map  |                            |
| `regulatory_ids`             | Array Map | Yes      | –             | –       | –                | Ids                        |
| `attachments`                | Array Map | Yes      | –             | –       | `Attachment` Map | Branch Documents           |
| `stage`                      | Map       | –        | –             | –       | `Stage` Map      | Current Stage              |
| `stage_logs`                 | Array Map | Yes      | –             | –       | `Stage` Map      | Workflow History           |
| `notes`                      | Array Map | -        | -             | -       | `Note` Map       | It is an array of Note map |

## Collection: m_branch

This collection keeps record of the branches of an organisation. Following information is maintained for a branch:

- **General Information**: Name, shortname and contact information etc
- **Address**: Single address is maintained with a branch. The address is maintained using Address map given in [ERP-DB-Gen-Common.md](./ERP-DB-Gen-Common.md) 
- **Regulatory Ids**: Several IDs like PAN, GSTIN etc can be maintained. The ID details are maintained using RegulatoryID map given in  [ERP-DB-Gen-Common.md](./ERP-DB-Gen-Common.md)
- **Attachments**: Multiple attachments are allowed with a branch. The attachment details are kept using the Attachment map given in [ERP-DB-GEN-Common.md](./ERP-DB-GEN-Common.md)
- **Stage Log**: Current stage and stage log is maintained for a branch. Stage information is kept using Stage Map given in [ERP-DB-Gen-Common.md](./ERP-DB-Gen-Common.md)
- **Notes**: Multiple notes can also be maintained with a branch. Note information is maintained using Note map given in [ERP-DB-Gen-Common.md](./ERP-DB-Gen-Common.md)

## General Information

| Name                 | Type      | Optional | Default Value | Key     | Reference           | Remarks                                                     |
| -------------------- | --------- | -------- | ------------- | ------- | ------------------- | ----------------------------------------------------------- |
| `id`                 | String    | –        | –             | Primary | –                   | Document Id                                                 |
| `organisation_id`    | String    | –        | –             | Foreign | m_organisation.`id` | Parent Organisation                                         |
| `organisation_name`  | String    | –        | –             | –       | –                   | Parent Organisation Name                                    |
| `name`               | String    | –        | –             | Unique  | –                   | Branch Name                                                 |
| `short_name`         | String    | –        | –             | Unique  | –                   | Branch Short Name                                           |
| `display_name`       | String    | Yes      | –             | –       | –                   | Display Name                                                |
| `address`            | Map       | Yes      | –             | –       | `Address` Map       | Branch Address                                              |
| `mobile`             | String    | Yes      | –             | –       | –                   | Primary Mobile                                              |
| `mobile_whatsapp`    | Boolean   | –        | False         | –       | –                   | WhatsApp Available                                          |
| `mobile_2`           | String    | Yes      | –             | –       | –                   | Secondary Mobile                                            |
| `mobile_2_whatsapp`  | Boolean   | –        | False         | –       | –                   | WhatsApp Available                                          |
| `email`              | String    | Yes      | –             | –       | –                   | Branch Email                                                |
| `is_production_unit` | Boolean   | –        | False         | –       | –                   | Works as Production Unit                                    |
| `is_warehouse`       | Boolean   | –        | False         | –       | –                   | Works as Warehouse                                          |
| `is_dispatch_center` | Boolean   | –        | False         | –       | –                   | Dispatch allowed from here                                  |
| `start_date`         | Timestamp | –        | Current Date  | –       | –                   | Active From                                                 |
| `end_date`           | Timestamp | Yes      | –             | –       | –                   | Active Till                                                 |
| `reason_end`         | String    | Yes      | –             | –       | –                   | Reason For Closure                                          |
| `tax_systems`        | Array Map | Yes      | –             | –       | `TaxSystem` Map     | To be copied from Organisation but it may be different also |
| `regulatory_ids`     | Array Map | Yes      | –             | –       | `RegulatoryID` Map  | IDs                                                         |
| `attachments`        | Array Map | Yes      | –             | –       | `Attachment` Map    | Branch Documents                                            |
| `stage`              | Map       | –        | –             | –       | `Stage` Map         | Current Stage                                               |
| `stage_logs`         | Array Map | Yes      | –             | –       | `Stage` Map         | Workflow History                                            |
| `notes`              | Array Map | -        | -             | -       | `Note` Map          | It is an array of Note map                                  |
