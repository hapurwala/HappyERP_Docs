# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Purchase
- **Module**: Purchase Order

This document contains database design details of Purchase Order module. The data includes:

- General Information / Header Fields
- Product Details
- Delivery Schedule
- Payment Terms
- Summary Fields

## Collection: purchase_order

Contains Purchase Order transaction data.

### General Information

| Name                   | Type      | Optional | Default Value | Key                         | Reference                    | Remarks                                                                         |
| ---------------------- | --------- | -------- | ------------- | --------------------------- | ---------------------------- | ------------------------------------------------------------------------------- |
| `id`                   | String    | –        | –             | Primary                     | –                            | Document Id                                                                     |
| `date`                 | Timestamp | –        | Current Date  | –                           | –                            | Purchase Order Date                                                             |
| `financial_year`       | Int64     | -        | -             | -                           | -                            | Financial Year                                                                  |
| `organisation_id`      | String    | -        | -             | Foreign                     | m_organisation.`id`          | Source Organisation Id                                                          |
| `branch_id`            | String    | -        | -             | Foreign                     | m_branch.`id`                | Source Branch Id                                                                |
| `purpose`              | String    | -        | -             | -                           | s_category.`purpose`         | Purchase Order, Job Order etc.                                                  |
| `number_series_id`     | String    | -        | -             | Foreign                     | m_number_series.`id`         | Number Series used                                                              |
| `number`               | String    | –        | –             | Unique within Number Series | –                            | Purchase Order Number                                                           |
| `formatted_number`     | String    | -        | -             | -                           | -                            | PO Number with prefix and suffix                                                |
| `party_id`             | String    | –        | –             | Foreign                     | m_party.`id`                 | Vendor Id                                                                       |
| `party_display_name`   | String    | –        | –             | –                           | –                            | Vendor's Display Name                                                           |
| `party_quotation_ref`  | String    | Yes      | -             | -                           | -                            | Quotation No or any other reference                                             |
| `agent_party_id`       | String    | –        | –             | Foreign                     | m_party.`id`                 | Agent Id                                                                        |
| `agent_display_name`   | String    | –        | –             | –                           | –                            | Agent's Display Name                                                            |
| `currency_id`          | String    | –        | –             | Foreign                     | m_currency.`id`              | Currency                                                                        |
| `currency_short_name`  | String    | –        | –             | –                           | –                            | Currency Short Name                                                             |
| `buyer_user_id`        | String    | Yes      | -             | Foreign                     | m_party.`id`                 | Employee who is responsible for this purchase                                   |
| `buyer_user_name`      | String    | Yes      | -             | -                           | -                            | Responsible Employee Name                                                       |
| `purchase_type`        | String    | -        | -             | -                           | -                            | Purchase Type: Local, Central, Export                                           |
| `delivery_mode`        | String    |          |               |                             | s_category.`delivery_mode`   | Delivery Type: Road, Rail, Air, Sea, Courier, Hand Delivery, Pipeline, Pickup   |
| `delivery_charge_type` | String    | Yes      | -             | –                           | s_category.`delivery_charge` | Who pays the delivery charges: Paid, To Pay, To Bill, Inclusive, Not Applicable |
| `attachments`          | Array Map | Yes      | –             | –                           | `Attachment` Map             | Branch Documents                                                                |
| `stage`                | Map       | –        | –             | –                           | `Stage` Map                  | Current Stage                                                                   |
| `stage_logs`           | Array Map | Yes      | –             | –                           | `Stage` Map                  | Workflow History                                                                |
| `notes`                | Array Map | -        | -             | -                           | `Note` Map                   | It is an array of Note map                                                      |

### Product Details

| Name                                       | Type      | Optional | Default Value | Key     | Reference              | Remarks                         |
| ------------------------------------------ | --------- | -------- | ------------- | ------- | ---------------------- | ------------------------------- |
| `products`                                 | Array Map | –        | –             | –       | –                      | Product Lines                   |
| products[].`id`                            | String    | –        | –             | –       | –                      | Row Id                          |
| products[].`sequence`                      | Int64     | –        | 0             | –       | –                      | Display Sequence                |
| products[].`product_id`                    | String    | –        | –             | Foreign | m_product.`id`         | Product                         |
| products[].`product_name`                  | String    | –        | –             | –       | –                      | Product Name                    |
| products[].`product_short_name`            | String    | -        | –             | –       | –                      | Product short name              |
| products[].`variety_id`                    | String    | Yes      | –             | Foreign | m_product_variety.`id` | Variety                         |
| products[].`variety_display_name`          | String    | Yes      | –             | –       | –                      | Variety Name                    |
| products[].`product_pack_id`               | String    | Yes      | –             | Foreign | m_product_pack.`id`    | Product Pack Id                 |
| products[].`product_pack_name`             | String    | Yes      | –             | –       | –                      | Pack Name                       |
| products[].`product_pack_short_name`       | String    | Yes      | -             | -       | -                      | Pack Short Name                 |
| products[].`count`                         | Int64     | -        | 0             | -       | -                      | Ordered Number of Packs         |
| products[].`quantity`                      | Double    | –        | 0             | –       | –                      | Ordered Quantity                |
| products[].`uom_id`                        | String    | –        | –             | Foreign | m_uom.`id`             | UOM                             |
| products[].`uom_short_name`                | String    | –        | –             | –       | –                      | UOM Short Name                  |
| products[].`rate`                          | Double    | –        | 0             | –       | –                      | Purchase Rate                   |
| products[].`product_value_before_discount` | Double    | -        | 0             | -       | -                      | Product Value = Quantity × Rate |
| products[].`discount_rate_type`            | String    | Yes      | Fixed         | –       | s_category.`rate_type` | Per Unit / Percent / Fixed      |
| products[].`discount_rate`                 | Double    | Yes      | 0             | –       | –                      | Entered Discount                |
| products[].`discount_value`                | Double    | Yes      | 0             | –       | –                      | Calculated Discount Value       |
| products[].`product_value_after_discount`  | Double    | –        | 0             | –       | –                      | (Quantity × Rate) - Discount    |
| products[].`taxes`                         | Array Map | Yes      | –             | –       | –                      | Tax Breakup                     |
| products[].taxes[].`tax_id`                | String    | –        | –             | Foreign | m_tax.`id`             | Tax Id                          |
| products[].taxes[].`tax_name`              | String    | –        | –             | –       | –                      | Tax Name                        |
| products[].taxes[].`taxable_value`         | Double    | –        | 0             | –       | –                      | Amount After Discount           |
| products[].taxes[].`tax_rate_percent`      | Double    | –        | 0             | –       | –                      | Tax Rate in  Percent            |
| products[].taxes[].`tax_value`             | Double    | –        | 0             | –       | –                      | Tax Amount                      |
| products[].`tax_value`                     | Double    | Yes      | 0             | –       | –                      | Total Tax Amount                |
| products[].`net_value`                     | Double    | –        | 0             | –       | –                      | Final Line Amount               |
| products[].`received_quantity`             | Double    | Yes      | 0             | –       | –                      | Updated Through GRN             |
| products[].`pending_quantity`              | Double    | Yes      | 0             | –       | –                      | Pending Receipt Quantity        |
| products[].`remarks`                       | String    | Yes      | –             | –       | –                      | Item Remarks                    |

## Delivery Schedule

| Name                                          | Type      | Optional | Default Value | Key     | Reference           | Remarks                            |
| --------------------------------------------- | --------- | -------- | ------------- | ------- | ------------------- | ---------------------------------- |
| `delivery_schedule`                           | Array Map | Yes      | –             | –       | –                   | Delivery Planning                  |
| delivery_schedule[].id                        | String    | –        | –             | –       | –                   | Row Id                             |
| delivery_schedule[].`product_pack_id`         | String    | Yes      | –             | Foreign | m_product_pack.`id` | Product                            |
| delivery_schedule[].`product_pack_name`       | String    | Yes      | –             | –       | –                   | Product Pack Name                  |
| delivery_schedule[].`product_pack_short_name` | String    | Yes      | -             | -       | -                   | Product Pack Short Name            |
| delivery_schedule[].`branch_id`               | String    | Yes      | –             | Foreign | m_branch.`id`       | Warehouse                          |
| delivery_schedule[].`party_id`                | String    | Yes      | -             | Foreign | m_party.`id`        | Party (Vendor/ Customer/ Employee) |
| delivery_schedule[].`deliver_to_name`         | String    | –        | –             | –       | –                   | Warehouse/Party Name               |
| delivery_schedule[].`quantity`                | Double    | –        | 0             | –       | –                   | Planned Delivery Quantity          |
| delivery_schedule[].`priority`                | String    | Yes      | Medium        | –       | –                   | High / Medium / Low                |
| delivery_schedule[].`remarks`                 | String    | Yes      | –             | –       | –                   | Delivery Remarks                   |
| delivery_schedule[].`received_quantity`       | Double    | Yes      | 0             | –       | –                   | Quantity Received Through GRN      |
| delivery_schedule[].`pending_quantity`        | Double    | Yes      | 0             | –       | –                   | Pending Quantity                   |
| delivery_schedule[].`expected_delivery_date`  | Timestamp | Yes      | –             | –       | –                   | Planned Delivery Date              |
| delivery_schedule[].`actual_delivery_date`    | Timestamp | Yes      | –             | –       | –                   | Actual Latest Delivery Date        |

## Payment Schedule

| Name                                  | Type         | Optional | Default Value | Key     | Reference                 | Remarks                                                                            |
| ------------------------------------- | ------------ | -------- | ------------- | ------- | ------------------------- | ---------------------------------------------------------------------------------- |
| `payment_mode`                        | Array String | Yes      | –             | –       | s_category.`payment_mode` | Allowed Payment Mode(s)                                                            |
| `base_payment_term_id`                | String       | Yes      | –             | Foreign | m_payment_term.`id`       | Applicable Payment Terms Information                                               |
| `tds_applicable`                      | Boolean      | –        | False         | –       | –                         | TDS Applicable                                                                     |
| `tds_category_id`                     | String       | Yes      | –             | Foreign | m_tds_category.`id`       | TDS Category                                                                       |
| `tds_category_name`                   | String       | Yes      | –             | –       | –                         | TDS Category Name                                                                  |
| `tds_percentage`                      | Double       | Yes      | 0             | –       | –                         | TDS Rate                                                                           |
| `tds_value`                           | Double       | Yes      | 0             | –       | –                         | Calculated TDS Amount. Exact TDS amount will be calculated at the tine of payment. |
| `base_payment_term_name`              | String       | –        | –             | –       | –                         |                                                                                    |
| `payment_term`                        | Map          | Yes      | –             | –       | –                         | Payment Information                                                                |
| payment_term.`credit_days`            | Int64        | -        | 0             | –       | –                         | Credit Period (Days)                                                               |
| payment_term.`base_date`              | String       | -        | Invoice Date  | –       | s_category.`base_date`    | Invoice Date / Delivery Date                                                       |
| payment_term.`schedule`               | Array Map    | -        | -             | -       | -                         | Payment Schedule                                                                   |
| payment_term.schedule[].`base_date`   | String       | -        | Order Date    | -       | s_category.`base_date`    | Invoice Date / Delivery Date / Order Date                                          |
| payment_term.schedule[].`days_within` | Int64        | Yes      | 0             | -       | -                         | Number of days after Base Date                                                     |
| payment_term.schedule[].`due_date`    | Timestamp    | -        | -             | -       | -                         | Due Date for payment                                                               |
| payment_term.schedule[].`percent`     | Double       | -        | 0             | -       | -                         | % of Order Amount                                                                  |
| payment_term.schedule[].`amount`      | Double       | -        | 0             | -       | -                         | Payment Amount                                                                     |
| payment_term.`discount`               | Array Map    | Yes      | -             | -       | -                         | Discount on early payment                                                          |
| payment_term.discount[].`base_date`   | String       | -        | Invoice Date  | -       | s_category.`base_date`    | Invoice Date / Delivery Date / Order Date                                          |
| payment_term.discount[].`days_within` | Int64        | -        | 0             | -       | -                         | Number of days within Base Date                                                    |
| payment_term.discount[].`rate_type`   | String       | -        | -             | -       | s_category.`rate_type`    | Per Unit / Percent / Fixed                                                         |
| payment_term.discount[].`rate`        | Double       | -        | 0             | -       | -                         | Entered Discount                                                                   |
| payment_term.discount[].`max_value`   | Double       | -        | 0             | -       | -                         | Maximum Discount Allowed                                                           |
| payment_term.`penalty`                | Array Map    | Yes      | -             | -       | -                         | Penalty on late payment                                                            |
| payment_term.penalty[].`base_date`    | String       | -        | Due Date      | -       | s_category.`base_date`    | Invoice Date / Delivery Date / Order Date                                          |
| payment_term.penalty[].`days_after`   | Int64        | -        | 0             | -       | -                         | Number of days after Base Date                                                     |
| payment_term.penalty[].`rate_type`    | String       | -        | -             | -       | s_category.`rate_type`    | Per Unit / Percent / Fixed                                                         |
| payment_term.penalty[].`rate`         | Double       | -        | -             | -       | -                         | Entered Penalty                                                                    |
| payment_term.penalty[].`min_value`    | Double       | -        | -             | -       | -                         | Minimum Penalty Applicable                                                         |
| payment_term.penalty[].`max_value`    | Double       | -        | -             | -       | -                         | Maximum Penalty Applicable                                                         |

## Summary Fields

| Name                                  | Type      | Optional | Default Value | Key | Reference | Remarks                                                |
| ------------------------------------- | --------- | -------- | ------------- | --- | --------- | ------------------------------------------------------ |
| `expected_delivery_date`              | Timestamp | -        | -             | -   | -         | Earliest expected delivery date from Delivery Schedule |
| `total_items`                         | Int64     | -        | 0             | -   | -         | Number of Items                                        |
| `total_count`                         | Int64     | -        | 0             | -   | -         | Sum of Pack Count                                      |
| `total_quantity`                      | Double    | -        | 0             | -   | -         | Sum of Quantity                                        |
| `total_received_quantity`             | Double    | -        | 0             | -   | -         | Sum of Received Quantity                               |
| `total_pending_quantity`              | Double    | -        | 0             | -   | -         | Pending Quantity to Receive                            |
| `total_weight`                        | Double    | -        | 0             | -   | -         | Total Weight                                           |
| `weight_uom_id`                       | String    | Yes      | -             | -   | -         | Weight UOM                                             |
| `weight_uom_short_name`               | String    | Yes      | -             | -   | -         | Weight UOM Short Name                                  |
| `total_product_value_before_discount` | Double    | -        | 0             | -   | -         | Total Product Value                                    |
| `total_discount_value`                | Double    | -        | 0             | -   | -         | Total Discount                                         |
| `total_product_value_after_discount`  | Double    | -        | 0             | -   | -         | Total Product Value after Discount                     |
| `total_tax_value`                     | Double    | -        | 0             | -   | -         | Total Tax                                              |
| `total_expense_value`                 | Double    | -        | 0             | -   | -         | Total Expenses                                         |
| `total_charge_value`                  | Double    | -        | 0             | -   | -         | Total Charges                                          |
| `total_freight_tpt_value`             | Double    | -        | 0             | -   | -         | Freight (Transporter)                                  |
| `total_freight_party_value`           | Double    | -        | 0             | -   | -         | Freight (Party)                                        |
| `additional_discount_value`           | Double    | -        | 0             | -   | -         | Additional Discount on PO                              |
| `adjustment_value`                    | Double    | -        | 0             | -   | -         | Adjustment                                             |
| `total_net_value`                     | Double    | -        | 0             | -   | -         | Net PO Value                                           |
