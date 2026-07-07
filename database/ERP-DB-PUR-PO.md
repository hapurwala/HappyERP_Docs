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

| Name                   | Type      | Optional | Default Value | Key                         | Reference            | Remarks                                                                         |
| ---------------------- | --------- | -------- | ------------- | --------------------------- | -------------------- | ------------------------------------------------------------------------------- |
| `id`                   | String    | –        | –             | Primary                     | –                    | Document Id                                                                     |
| `date`                 | Timestamp | –        | Current Date  | –                           | –                    | Purchase Order Date                                                             |
| `financial_year`       | Int64     | -        | -             | -                           | -                    | Financial Year                                                                  |
| `organisation_id`      | String    | -        | -             | Foreign                     | m_organisation.`id`  | Source Organisation Id                                                          |
| `branch_id`            | String    | -        | -             | Foreign                     | m_branch.`id`        | Source Branch Id                                                                |
| `purpose_class`        | String    | -        | -             | -                           | -                    | Purchase Order, Job Order etc.                                                  |
| `number_series_id`     | String    | -        | -             | Foreign                     | m_number_series.`id` | Number Series used                                                              |
| `number`               | String    | –        | –             | Unique within Number Series | –                    | Purchase Order Number                                                           |
| `formatted_number`     | String    | -        | -             | -                           | -                    | PO Number with prefix and suffix as per Number Series                           |
| `party_id`             | String    | –        | –             | Foreign                     | m_party.`id`         | Vendor Id                                                                       |
| `party_display_name`   | String    | –        | –             | –                           | –                    | Vendor's Display Name                                                           |
| `party_quotation_ref`  | String    | Yes      | -             | -                           | -                    | Quotation No or any other reference                                             |
| `agent_party_id`       | String    | –        | –             | Foreign                     | m_party.`id`         | Agent Id                                                                        |
| `agent_display_name`   | String    | –        | –             | –                           | –                    | Agent's Display Name                                                            |
| `currency_id`          | String    | –        | –             | Foreign                     | m_currency.`id`      | Currency                                                                        |
| `currency_short_name`  | String    | –        | –             | –                           | –                    | Currency Short Name                                                             |
| `buyer_user_id`        | String    | Yes      | -             | Foreign                     | m_party.`id`         | Employee who is responsible for this purchase                                   |
| `buyer_user_name`      | String    | Yes      | -             | -                           | -                    | Responsible Employee Name                                                       |
| `purchase_type`        | String    | -        | -             | -                           | -                    | Purchase Type: Local, Central, Export                                           |
| `delivery_mode`        | String    |          |               |                             | s_category.`delivery_mode`                     | Delivery Type: Road, Rail, Air, Sea, Courier, Hand Delivery, Pipeline, Pickup   |
| `delivery_charge_type` | String    | Yes      | -             | –                           | s_category.`delivery_charge`                    | Who pays the delivery charges: Paid, To Pay, To Bill, Inclusive, Not Applicable |
| `attachments`          | Array Map | Yes      | –             | –                           | `Attachment` Map     | Branch Documents                                                                |
| `stage`                | Map       | –        | –             | –                           | `Stage` Map          | Current Stage                                                                   |
| `stage_logs`           | Array Map | Yes      | –             | –                           | `Stage` Map          | Workflow History                                                                |
| `notes`                | Array Map | -        | -             | -                           | `Note` Map           | It is an array of Note map                                                      |

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
| products[].`discount_type`                 | String    | Yes      | Fixed         | –       | –                      | Per Unit / Percent / Fixed      |
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

## Payment Terms

| Name                                   | Type         | Optional | Default Value | Key     | Reference           | Remarks                                   |
| -------------------------------------- | ------------ | -------- | ------------- | ------- | ------------------- | ----------------------------------------- |
| `payment_terms`                        | Map          | Yes      | –             | –       | –                   | Payment Information                       |
| payment_terms.`payment_mode`           | Array String | Yes      | –             | –       | –                   | Allowed Payment Modes                     |
| payment_terms.`base_date`              | String       | -        | Invoice Date  | –       | –                   | Invoice Date / Delivery Date              |
| payment_terms.`credit_days`            | Int64        | -        | 0             | –       | –                   | Credit Period (Days)                      |
| payment_terms.`tds_applicable`         | Boolean      | –        | False         | –       | –                   | TDS Applicable                            |
| payment_terms.`tds_category_id`        | String       | Yes      | –             | Foreign | m_tds_category.`id` | TDS Category                              |
| payment_terms.`tds_category_name`      | String       | Yes      | –             | –       | –                   | TDS Category Name                         |
| payment_terms.`tds_percentage`         | Double       | Yes      | 0             | –       | –                   | TDS Rate                                  |
| payment_terms.`tds_value`              | Double       | Yes      | 0             | –       | –                   | Calculated TDS Amount                     |
| payment_terms.`payment`                | Array Map    | -        | -             | -       | -                   | Payment Schedule                          |
| payment_terms.payment[].`base_date`    | String       | -        | Order Date    | -       | -                   | Invoice Date / Delivery Date / Order Date |
| payment_terms.payment[].`days_within`  | Int64        | Yes      | 0             | -       | -                   | Number of days within Base Date           |
| payment_terms.payment[].`due_date`     | Timestamp    | -        | -             | -       | -                   | Due Date for payment                      |
| payment_terms.payment[].`percent`      | Double       | -        | 0             | -       | -                   | % of Order Amount                         |
| payment_terms.payment[].`amount`       | Double       | -        | 0             | -       | -                   | Payment Amount                            |
| payment_terms.`discount`               | Array Map    | Yes      | -             | -       | -                   | Discount on early payment                 |
| payment_terms.discount[].`base_date`   | String       | -        | Invoice Date  | -       | -                   | Invoice Date / Delivery Date / Order Date |
| payment_terms.discount[].`days_within` | Int64        | -        | 0             | -       | -                   | Number of days within Base Date           |
| payment_terms.discount[].`type`        | String       | -        | -             | -       | -                   | Per Unit / Percent / Fixed                |
| payment_terms.discount[].`rate`        | Double       | -        | 0             | -       | -                   | Entered Discount                          |
| payment_terms.discount[].`max_value`   | Double       | -        | 0             | -       | -                   | Maximum Discount Allowed                  |
| payment_terms.`penalty`                | Array Map    | Yes      | -             | -       | -                   | Penalty on late payment                   |
| payment_terms.penalty[].`base_date`    | String       | -        | Due Date      | -       | -                   | Invoice Date / Delivery Date / Order Date |
| payment_terms.penalty[].`days_after`   | Int64        | -        | 0             | -       | -                   | Number of days after Base Date            |
| payment_terms.penalty[].`type`         | String       | -        | -             | -       | -                   | Per Unit / Percent / Fixed                |
| payment_terms.penalty[].`rate`         | Double       | -        | -             | -       | -                   | Entered Penalty                           |
| payment_terms.penalty[].`max_value`    | Double       | -        | -             | -       | -                   | Maximum Penalty Applicable                |

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
