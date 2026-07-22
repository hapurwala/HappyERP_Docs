# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Configuration
- **Module**: Settings (Sundry Adjustment/Info)

**HappyERP** allows user to configure Document Extras. It is divided into Financial components (Sundry Adjustments) and Informational components (Sundry Info) that can be dynamically attached to transactions like Invoices and Challans. Sundry Adjustments may be categorised further in Expense on self, Chargeable to party, Discount to party.

## Collection: m_sundry_adjustment

This collection handles all financial additions, deductions, and internal costs.

| Name                 | Type      | Optional | Default Value | Key     | Reference                           | Remarks                                                         |
| -------------------- | --------- | -------- | ------------- | ------- | ----------------------------------- | --------------------------------------------------------------- |
| `id`                 | String    | –        | –             | Primary | –                                   | Document Id                                                     |
| `name`               | String    | –        | –             | –       | –                                   |                                                                 |
| `display_name`       | String    | –        | –             | –       | –                                   | Name to be printed on document                                  |
| `base_adjustment_id` | String    | Yes      | –             | Foreign | m_sundry_adjustment.`id`            | Links a party-specific override to its global base rule.        |
| `app_object_id`      | String    | –        | –             | Foreign | s_app_object.`id`                   | E.g., Sales_Invoice.                                            |
| `party_id`           | String    | Yes      | –             | Foreign | m_party.`id`                        | If filled, this adjustment applies ONLY to this party.          |
| `type`               | String    | –        | –             | –       | s_category.`sundry_adjustment_type` | Charge, discount, expense                                       |
| `calculation_base`   | String    | –        | –             | –       | s_category.`calculation_base`       | Value, Quantity, Count etc                                      |
| `rate_type`          | String    | -        | -             | -       | s_category.`rate_type`              | Rate / Percent / Fixed                                          |
| `default_rate`       | Double    | –        | –             | –       | –                                   | The default value or percentage to apply.                       |
| `is_mandatory`       | Boolean   | –        | False         | –       | –                                   | If True, automatically added to the document.                   |
| `is_editable`        | Boolean   | –        | True          | –       | –                                   | If True, user can change rate at entry.                         |
| `is_taxable`         | Boolean   | –        | False         | –       | –                                   | If True, Tax is calculated on this adjustment.                  |
| `ledger_account_id`  | String    | –        | –             | Foreign | –                                   | Which general ledger account this hits (e.g., Freight Account). |
| `start_date`         | Timestamp | –        | –             | –       | –                                   | -                                                               |
| `end_date`           | Timestamp | Yes      | –             | –       | –                                   | -                                                               |
| `reason_end`         | String    | Yes      | –             | –       | –                                   | -                                                               |
| `stage`              | Map       | –        | –             | –       | `Stage` Map                         | Current Stage                                                   |
| `stage_logs`         | Array Map | Yes      | –             | –       | `Stage` Map                         | Workflow History                                                |
| `notes`              | Array Map | -        | -             | -       | `Note` Map                          | It is an array of Note map                                      |

## Collection: m_sundry_info

This collection handles purely informational fields.

| Name            | Type      | Optional | Default Value | Key     | Reference         | Remarks                                                      |
| --------------- | --------- | -------- | ------------- | ------- | ----------------- | ------------------------------------------------------------ |
| `id`            | String    | –        | –             | Primary | –                 | Document Id                                                  |
| `name`          | String    | –        | –             | Unique  | –                 | E.g., Vehicle Number                                         |
| `app_object_id` | String    | –        | –             | Foreign | s_app_object.`id` | 1:1 mapping.                                                 |
| `data_type`     | String    | –        | `text`        | –       | –                 | `text`, `number`, `date`, `boolean` (Used for UI rendering). |
| `is_mandatory`  | Boolean   | –        | False         | –       | –                 | If True, user cannot save document without it.               |
| `start_date`    | Timestamp | –        | –             | –       | –                 | -                                                            |
| `end_date`      | Timestamp | Yes      | –             | –       | –                 | -                                                            |
| `reason_end`    | String    | Yes      | –             | –       | –                 | -                                                            |
| `stage`         | Map       | –        | –             | –       | `Stage` Map       | Current Stage                                                |
| `stage_logs`    | Array Map | Yes      | –             | –       | `Stage` Map       | Workflow History                                             |
| `notes`         | Array Map | -        | -             | -       | `Note` Map        | It is an array of Note map                                   |
