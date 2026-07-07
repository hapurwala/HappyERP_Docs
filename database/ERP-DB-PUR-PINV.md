Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Purchase
- **Module**: Purchase Invoice

This document contains database design details of Purchase Invoice module. The data includes:

- General Information / Header Fields
- Reference Documents
- Product Details
- Tax Details
- Additional Charges
- Vendor Payment References
- Purchase Return References
- Attachment
- Summary Fields
- Payment Summary
- Direct Purchase Summary
- Reference Summary

## Collection: purchase_invoice

Contains Purchase Invoice transaction data.

## General Information

| Name                    | Type      | Optional | Default Value | Key                         | Reference            | Remarks                           |
| ----------------------- | --------- | -------- | ------------- | --------------------------- | -------------------- | --------------------------------- |
| `id`                    | String    | –        | –             | Primary                     | –                    | Document Id                       |
| `date`                  | Timestamp | –        | Current Date  | –                           | –                    | Purchase Invoice Date             |
| `financial_year`        | Int64     | –        | –             | –                           | –                    | Financial Year                    |
| `organisation_id`       | String    | –        | –             | Foreign                     | m_organisation.`id`  | Source Organisation Id            |
| `branch_id`             | String    | –        | –             | Foreign                     | m_branch.`id`        | Source Branch Id                  |
| `number_series_id`      | String    | –        | –             | Foreign                     | m_number_series.`id` | Number Series Used                |
| `number`                | String    | –        | –             | Unique within Number Series | –                    | Purchase Invoice Number           |
| `formatted_number`      | String    | –        | –             | –                           | –                    | PI Number With Prefix And Suffix  |
| `party_id`              | String    | –        | –             | Foreign                     | m_party.`id`         | Vendor Id                         |
| `party_display_name`    | String    | –        | –             | –                           | –                    | Vendor Display Name               |
| `vendor_invoice_number` | String    | Yes      | –             | –                           | –                    | Vendor Invoice Number             |
| `vendor_invoice_date`   | Timestamp | Yes      | –             | –                           | –                    | Vendor Invoice Date               |
| `agent_party_id`        | String    | Yes      | –             | Foreign                     | m_party.`id`         | Agent Id                          |
| `agent_display_name`    | String    | Yes      | –             | –                           | –                    | Agent Display Name                |
| `currency_id`           | String    | –        | –             | Foreign                     | m_currency.`id`      | Currency                          |
| `currency_short_name`   | String    | –        | –             | –                           | –                    | Currency Short Name               |
| `buyer_user_id`         | String    | Yes      | –             | Foreign                     | m_user.`id`          | Responsible Employee              |
| `buyer_user_name`       | String    | Yes      | –             | –                           | –                    | Responsible Employee Name         |
| `purchase_type`         | String    | Yes      | Local         | –                           | –                    | Local / Central / Import / Export |
| `attachments`           | Array Map | Yes      | –             | –                           | `Attachment` Map     | Invoice Attachments               |
| `stage`                 | Map       | –        | –             | –                           | `Stage` Map          | Current Workflow Stage            |
| `stage_logs`            | Array Map | Yes      | –             | –                           | `Stage` Map          | Workflow History                  |
| `notes`                 | Array Map | Yes      | –             | –                           | `Note` Map           | User Notes                        |

## Product Details

| Name                                       | Type      | Optional | Default Value | Key     | Reference                   | Remarks                      |
| ------------------------------------------ | --------- | -------- | ------------- | ------- | --------------------------- | ---------------------------- |
| `products`                                 | Array Map | –        | –             | –       | –                           | Invoice Product Lines        |
| products[].`id`                            | String    | –        | –             | –       | –                           | Row Id                       |
| products[].`sequence`                      | Int64     | –        | 0             | –       | –                           | Display Sequence             |
| products[].`purchase_order_id`             | String    | Yes      | –             | Foreign | purchase_order.`id`         | PO                           |
| products[].`po_number`                     | String    | Yes      | –             | -       | -                           | PO Number                    |
| products[].`goods_receive_id`              | String    | Yes      | –             | Foreign | goods_receive.`id`          | GRN                          |
| products[].`gr_number`                     | String    | Yes      | –             | -       | -                           | GR Number                    |
| products[].`product_id`                    | String    | –        | –             | Foreign | m_product.`id`              | Product                      |
| products[].`product_name`                  | String    | –        | –             | –       | –                           | Product Name                 |
| products[].`product_short_name`            | String    | Yes      | –             | –       | –                           | Product Short Name           |
| products[].`variety_id`                    | String    | Yes      | –             | Foreign | m_product_variety.`id`      | Product Variety              |
| products[].`variety_display_name`          | String    | Yes      | –             | –       | –                           | Variety Name                 |
| products[].`product_pack_id`               | String    | Yes      | –             | Foreign | m_product_pack.`id`         | Product Pack Id              |
| products[].`product_pack_name`             | String    | Yes      | –             | –       | –                           | Product Pack Name            |
| products[].`product_pack_short_name`       | String    | Yes      | –             | –       | –                           | Product Pack Short Name      |
| products[].`count`                         | Int64     | Yes      | 0             | –       | –                           | Number Of Packs              |
| products[].`quantity`                      | Double    | –        | 0             | –       | –                           | Invoice Quantity             |
| products[].`uom_id`                        | String    | –        | –             | Foreign | m_uom.`id`                  | UOM                          |
| products[].`uom_short_name`                | String    | –        | –             | –       | –                           | UOM Short Name               |
| products[].`rate`                          | Double    | –        | 0             | –       | –                           | Purchase Rate                |
| products[].`product_value_before_discount` | Double    | –        | 0             | –       | –                           | Quantity × Rate              |
| products[].`discount_basis`                | String    | Yes      | Fixed         | –       | s_category.`discount_basis` | Per Unit / Percent / Fixed   |
| products[].`discount_rate`                 | Double    | Yes      | 0             | –       | –                           | Entered Discount             |
| products[].`discount_value`                | Double    | Yes      | 0             | –       | –                           | Calculated Discount Value    |
| products[].`product_value_after_discount`  | Double    | –        | 0             | –       | –                           | Product Value After Discount |
| products[].`taxes`                         | Array Map | Yes      | –             | –       | –                           | Tax Breakup                  |
| products[].taxes[].`tax_id`                | String    | –        | –             | Foreign | m_tax.`id`                  | Tax Id                       |
| products[].taxes[].`tax_name`              | String    | –        | –             | –       | –                           | Tax Name                     |
| products[].taxes[].`taxable_value`         | Double    | –        | 0             | –       | –                           | Amount After Discount        |
| products[].taxes[].`tax_rate_percent`      | Double    | –        | 0             | –       | –                           | Tax Rate Percentage          |
| products[].taxes[].`tax_value`             | Double    | –        | 0             | –       | –                           | Tax Amount                   |
| products[].`tax_value`                     | Double    | Yes      | 0             | –       | –                           | Total Tax Amount             |
| products[].`net_value`                     | Double    | –        | 0             | –       | –                           | Final Line Amount            |
| products[].`paid_value`                    | Double    | Yes      | 0             | –       | –                           | Payment Made                 |
| products[].`pending_value`                 | Double    | Yes      | 0             | –       | –                           | Pending Payment              |
| products[].`remarks`                       | String    | Yes      | –             | –       | –                           |                              |

## Additional Charges

| Name                           | Type      | Optional | Default Value | Key | Reference                   | Remarks                                  |
| ------------------------------ | --------- | -------- | ------------- | --- | --------------------------- | ---------------------------------------- |
| `additional_charges`           | Array Map | Yes      | –             | –   | –                           | Freight, Packing, Loading, Other Charges |
| additional_charges[].`id`      | String    | –        | –             | –   | –                           | Charge Id                                |
| additional_charges[].`name`    | String    | –        | –             | –   | –                           | Charge Name                              |
| additional_charges[].`basis`   | String    | –        | Fixed         | –   | s_category.`discount_basis` | Fixed / Percent / Quantity               |
| additional_charges[].`rate`    | Double    | Yes      | 0             | –   | –                           | Entered Rate                             |
| additional_charges[].`value`   | Double    | –        | 0             | –   | –                           | Calculated Charge Value                  |
| additional_charges[].`remarks` | String    | Yes      | –             | –   | –                           | Charge Remarks                           |

## Payment Terms

| Name                                   | Type         | Optional | Default Value | Key     | Reference                   | Remarks                                   |
| -------------------------------------- | ------------ | -------- | ------------- | ------- | --------------------------- | ----------------------------------------- |
| `payment_terms`                        | Map          | Yes      | –             | –       | –                           | Payment Information                       |
| payment_terms.`payment_mode`           | Array String | Yes      | –             | –       | s_category.`payment_mode`   | Allowed Payment Mode(s)                   |
| payment_terms.`base_date`              | String       | -        | Invoice Date  | –       | s_category.`base_date`      | Invoice Date / Delivery Date              |
| payment_terms.`credit_days`            | Int64        | -        | 0             | –       | –                           | Credit Period (Days)                      |
| payment_terms.`tds_applicable`         | Boolean      | –        | False         | –       | –                           | TDS Applicable                            |
| payment_terms.`tds_category_id`        | String       | Yes      | –             | Foreign | m_tds_category.`id`         | TDS Category                              |
| payment_terms.`tds_category_name`      | String       | Yes      | –             | –       | –                           | TDS Category Name                         |
| payment_terms.`tds_percentage`         | Double       | Yes      | 0             | –       | –                           | TDS Rate                                  |
| payment_terms.`tds_value`              | Double       | Yes      | 0             | –       | –                           | Calculated TDS Amount                     |
| payment_terms.`payment`                | Array Map    | -        | -             | -       | -                           | Payment Schedule                          |
| payment_terms.payment[].`base_date`    | String       | -        | Order Date    | -       | s_category.`base_date`      | Invoice Date / Delivery Date / Order Date |
| payment_terms.payment[].`days_within`  | Int64        | Yes      | 0             | -       | -                           | Number of days within Base Date           |
| payment_terms.payment[].`due_date`     | Timestamp    | -        | -             | -       | -                           | Due Date for payment                      |
| payment_terms.payment[].`percent`      | Double       | -        | 0             | -       | -                           | % of Order Amount                         |
| payment_terms.payment[].`amount`       | Double       | -        | 0             | -       | -                           | Payment Amount                            |
| payment_terms.`discount`               | Array Map    | Yes      | -             | -       | -                           | Discount on early payment                 |
| payment_terms.discount[].`base_date`   | String       | -        | Invoice Date  | -       | s_category.`base_date`      | Invoice Date / Delivery Date / Order Date |
| payment_terms.discount[].`days_within` | Int64        | -        | 0             | -       | -                           | Number of days within Base Date           |
| payment_terms.discount[].`basis`       | String       | -        | -             | -       | s_category.`discount_basis` | Per Unit / Percent / Fixed                |
| payment_terms.discount[].`rate`        | Double       | -        | 0             | -       | -                           | Entered Discount                          |
| payment_terms.discount[].`max_value`   | Double       | -        | 0             | -       | -                           | Maximum Discount Allowed                  |
| payment_terms.`penalty`                | Array Map    | Yes      | -             | -       | -                           | Penalty on late payment                   |
| payment_terms.penalty[].`base_date`    | String       | -        | Due Date      | -       | s_category.`base_date`      | Invoice Date / Delivery Date / Order Date |
| payment_terms.penalty[].`days_after`   | Int64        | -        | 0             | -       | -                           | Number of days after Base Date            |
| payment_terms.penalty[].`basis`        | String       | -        | -             | -       | s_category.`discount_basis` | Per Unit / Percent / Fixed                |
| payment_terms.penalty[].`rate`         | Double       | -        | -             | -       | -                           | Entered Penalty                           |
| payment_terms.penalty[].`max_value`    | Double       | -        | -             | -       | -                           | Maximum Penalty Applicable                |

## Summary Fields

| Name                                  | Type      | Optional | Default Value | Key     | Reference  | Remarks                             |
| ------------------------------------- | --------- | -------- | ------------- | ------- | ---------- | ----------------------------------- |
| `total_items`                         | Int64     | –        | 0             | –       | –          | Number Of Products                  |
| `total_count`                         | Int64     | Yes      | 0             | –       | –          | Sum Of Product Pack Count           |
| `total_quantity`                      | Double    | –        | 0             | –       | –          | Total Invoice Quantity              |
| `total_weight`                        | Double    | Yes      | 0             | –       | –          | Total Weight                        |
| `weight_uom_id`                       | String    | Yes      | –             | Foreign | m_uom.`id` | Weight UOM                          |
| `weight_uom_short_name`               | String    | Yes      | –             | –       | –          | Weight UOM Short Name               |
| `total_product_value_before_discount` | Double    | –        | 0             | –       | –          | Total Product Value Before Discount |
| `total_discount_value`                | Double    | Yes      | 0             | –       | –          | Total Discount Value                |
| `total_product_value_after_discount`  | Double    | –        | 0             | –       | –          | Product Value After Discount        |
| `total_tax_value`                     | Double    | Yes      | 0             | –       | –          | Total Tax Value                     |
| `total_tds_value`                     | Double    | Yes      | 0             | –       | –          | Total TDS Deduction                 |
| `total_expense_value`                 | Double    | Yes      | 0             | –       | –          | Additional Expenses                 |
| `total_charge_value`                  | Double    | Yes      | 0             | –       | –          | Total Additional Charges            |
| `total_freight_tpt_value`             | Double    | Yes      | 0             | –       | –          | Freight Paid To Transporter         |
| `total_freight_party_value`           | Double    | Yes      | 0             | –       | –          | Freight Paid By Vendor              |
| `additional_discount_value`           | Double    | Yes      | 0             | –       | –          | Additional Invoice Level Discount   |
| `adjustment_value`                    | Double    | Yes      | 0             | –       | –          | Round Off                           |
| `total_net_value`                     | Double    | –        | 0             | –       | –          | Final Invoice Value                 |
| `total_paid_value`                    | Double    | –        | 0             | –       | –          | Total amount paid                   |
| `pending_value`                       | Double    | –        | 0             | –       | –          | Pending Value to pay                |
| `last_payment_date`                   | Timestamp | Yes      | –             | –       | –          | Latest Vendor Payment Date          |
| `next_due_date`                       | Timestamp | Yes      | –             | –       | –          | Upcoming Payment Due Date           |
