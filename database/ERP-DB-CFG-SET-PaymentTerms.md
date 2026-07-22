# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Configuration
- **Module**: Settings (Payment Term)

Every industry has its own terms of payment. These terms vary with parties and products. **HappyERP** allows user to configure their Payment Terms for Sales and Purchase both.

Payment Terms has following components:

- **Credit Days**: A customer gets this period to make payment for the purchases.
- **Payment Schedule**: Applicable when customer is given felixibility to make payment in parts.
- **Early Payment Discount**: Discount given to customer on early payment (i.e. before due date). This is also known as cash discount (CD).
- **Late Payment Penalty**: Penalty imposed on payment after due date. In many cases it is waived off.

## Collection: m_payment_term

This collection is used to store number series configuration.

| Name                     | Type      | Optional | Default Value | Key | Reference                   | Remarks                                   |
| ------------------------ | --------- | -------- | ------------- | --- | --------------------------- | ----------------------------------------- |
| `id`                     | String    | -        | –             | –   | –                           | Document Id                               |
| `name`                   | String    | –        | –             | –   | –                           |                                           |
| `short_name`             | String    | –        | –             | –   | –                           |                                           |
| `on_sales`               | Boolean   | –        | False         | –   | –                           | Applicable on Sales?                      |
| `on_purchase`            | Boolean   | –        | False         | –   | –                           | Applicable on Purchase?                   |
| `is_default`             | Boolean   | –        | False         | –   | –                           | Default Terms                             |
| `credit_days`            | Int64     | -        | 0             | –   | –                           | Credit Period (Days)                      |
| `base_date`              | String    | -        | Invoice Date  | –   | s_category.`base_date`      | Invoice Date / Delivery Date              |
| `schedule`               | Array Map | -        | -             | -   | -                           | Payment Schedule                          |
| schedule[].`base_date`   | String    | -        | Order Date    | -   | s_category.`base_date`      | Invoice Date / Delivery Date / Order Date |
| schedule[].`days_within` | Int64     | Yes      | 0             | -   | -                           | Number of days after Base Date            |
| schedule[].`due_date`    | Timestamp | Yes      | -             | -   | -                           | Due Date for payment                      |
| schedule[].`percent`     | Double    | -        | 0             | -   | -                           | % of Order Amount                         |
| schedule[].`amount`      | Double    | -        | 0             | -   | -                           | Payment Amount                            |
| `discount`               | Array Map | Yes      | -             | -   | -                           | Discount on early payment                 |
| discount[].`base_date`   | String    | -        | Invoice Date  | -   | s_category.`base_date`      | Invoice Date / Delivery Date / Order Date |
| discount[].`days_within` | Int64     | -        | 0             | -   | -                           | Number of days after Base Date            |
| discount[].`rate_type`       | String    | -        | -             | -   | s_category.`rate_type` | Per Unit / Percent / Fixed                |
| discount[].`rate`        | Double    | -        | 0             | -   | -                           | Entered Discount                          |
| discount[].`max_value`   | Double    | -        | 0             | -   | -                           | Maximum Discount Allowed                  |
| `penalty`                | Array Map | Yes      | -             | -   | -                           | Penalty on late payment                   |
| penalty[].`base_date`    | String    | -        | Due Date      | -   | s_category.`base_date`      | Invoice Date / Delivery Date / Order Date |
| penalty[].`days_after`   | Int64     | -        | 0             | -   | -                           | Number of days after Base Date            |
| penalty[].`rate_type`        | String    | -        | -             | -   | s_category.`rate_type` | Per Unit / Percent / Fixed                |
| penalty[].`rate`         | Double    | -        | -             | -   | -                           | Entered Penalty                           |
| penalty[].`min_value`    | Double    | -        | -             | -   | -                           | Minimum Penalty Applicable                |
| penalty[].`max_value`    | Double    | -        | -             | -   | -                           | Maximum Penalty Applicable                |
| `start_date`             | Timestamp | –        | –             | –   | –                           |                                           |
| `end_date`               | Timestamp | Yes      | –             | –   | –                           |                                           |
| `reason_end`             | String    | Yes      | –             | –   | –                           |                                           |
| `stage`                  | Map       | –        | –             | –   | `Stage` Map                 | Current Stage                             |
| `stage_logs`             | Array Map | Yes      | –             | –   | `Stage` Map                 | Workflow History                          |
| `notes`                  | Array Map | -        | -             | -   | `Note` Map                  | It is an array of Note map                |
