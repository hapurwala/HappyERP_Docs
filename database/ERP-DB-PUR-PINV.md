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

---

## General Information

| Name                    | Type      | Optional | Default Value | Key                         | Reference            | Remarks                                  |
| ----------------------- | --------- | -------- | ------------- | --------------------------- | -------------------- | ---------------------------------------- |
| `id`                    | String    | –        | –             | Primary                     | –                    | Document Id                              |
| `date`                  | Timestamp | –        | Current Date  | –                           | –                    | Purchase Invoice Date                    |
| `financial_year`        | Int64     | –        | –             | –                           | –                    | Financial Year                           |
| `organisation_id`       | String    | –        | –             | Foreign                     | m_organisation.`id`  | Source Organisation Id                   |
| `branch_id`             | String    | –        | –             | Foreign                     | m_branch.`id`        | Source Branch Id                         |
| `number_series_id`      | String    | –        | –             | Foreign                     | m_number_series.`id` | Number Series Used                       |
| `number`                | String    | –        | –             | Unique within Number Series | –                    | Purchase Invoice Number                  |
| `formatted_number`      | String    | –        | –             | –                           | –                    | PI Number With Prefix And Suffix         |
| `vendor_invoice_number` | String    | Yes      | –             | –                           | –                    | Vendor Invoice Number                    |
| `vendor_invoice_date`   | Timestamp | Yes      | –             | –                           | –                    | Vendor Invoice Date                      |
| `party_id`              | String    | –        | –             | Foreign                     | m_party.`id`         | Vendor Id                                |
| `party_display_name`    | String    | –        | –             | –                           | –                    | Vendor Display Name                      |
| `party_invoice_ref`     | String    | Yes      | –             | –                           | –                    | Vendor Reference Number                  |
| `agent_party_id`        | String    | Yes      | –             | Foreign                     | m_party.`id`         | Agent Id                                 |
| `agent_display_name`    | String    | Yes      | –             | –                           | –                    | Agent Display Name                       |
| `currency_id`           | String    | –        | –             | Foreign                     | m_currency.`id`      | Currency                                 |
| `currency_short_name`   | String    | –        | –             | –                           | –                    | Currency Short Name                      |
| `buyer_user_id`         | String    | Yes      | –             | Foreign                     | m_user.`id`          | Responsible Employee                     |
| `buyer_user_name`       | String    | Yes      | –             | –                           | –                    | Responsible Employee Name                |
| `purchase_type`         | String    | Yes      | Local         | –                           | –                    | Local / Central / Import / Export        |
| `reference_type`        | String    | –        | PO            | –                           | –                    | PO / GRN / Direct Purchase / Mixed       |
| `allow_direct_purchase` | Boolean   | –        | False         | –                           | –                    | Indicates Direct Purchase Products Exist |
| `direct_purchase_stage` | String    | Yes      | Pending       | –                           | –                    | Pending Approval / Approved / Rejected   |
| `attachments`           | Array Map | Yes      | –             | –                           | `Attachment` Map     | Invoice Attachments                      |
| `stage`                 | Map       | –        | –             | –                           | `Stage` Map          | Current Workflow Stage                   |
| `stage_logs`            | Array Map | Yes      | –             | –                           | `Stage` Map          | Workflow History                         |
| `notes`                 | Array Map | Yes      | –             | –                           | `Note` Map           | User Notes                               |

---

## Reference Documents

| Name                          | Type      | Optional | Default Value | Key     | Reference           | Remarks                |
| ----------------------------- | --------- | -------- | ------------- | ------- | ------------------- | ---------------------- |
| `purchase_orders`             | Array Map | Yes      | –             | –       | –                   | Linked Purchase Orders |
| purchase_orders[].`id`        | String    | –        | –             | –       | –                   | Row Id                 |
| purchase_orders[].`po_id`     | String    | –        | –             | Foreign | purchase_order.`id` | Purchase Order Id      |
| purchase_orders[].`po_number` | String    | –        | –             | –       | –                   | Purchase Order Number  |
| purchase_orders[].`po_date`   | Timestamp | Yes      | –             | –       | –                   | Purchase Order Date    |
| purchase_orders[].`net_value` | Double    | Yes      | 0             | –       | –                   | PO Net Value           |

| Name                | Type      | Optional | Default Value | Key     | Reference | Remarks                    |
| ------------------- | --------- | -------- | ------------- | ------- | --------- | -------------------------- |
| `grns`              | Array Map | Yes      | –             | –       | –         | Linked Goods Receipt Notes |
| grns[].`id`         | String    | –        | –             | –       | –         | Row Id                     |
| grns[].`grn_id`     | String    | –        | –             | Foreign | .`id`     | GRN Id                     |
| grns[].`grn_number` | String    | –        | –             | –       | –         | GRN Number                 |
| grns[].`grn_date`   | Timestamp | Yes      | –             | –       | –         | GRN Date                   |
| grns[].`net_value`  | Double    | Yes      | 0             | –       | –         | GRN Net Value              |

| Name                                                  | Type      | Optional | Default Value | Key     | Reference      | Remarks                           |
| ----------------------------------------------------- | --------- | -------- | ------------- | ------- | -------------- | --------------------------------- |
| `direct_purchase_products`                            | Array Map | Yes      | –             | –       | –              | Products Purchased Without PO/GRN |
| direct_purchase_products[].`id`                       | String    | –        | –             | –       | –              | Row Id                            |
| direct_purchase_products[].`product_id`               | String    | –        | –             | Foreign | m_product.`id` | Product Id                        |
| direct_purchase_products[].`product_name`             | String    | –        | –             | –       | –              | Product Name                      |
| direct_purchase_products[].`direct_purchase_quantity` | Double    | –        | 0             | –       | –              | Direct Purchase Quantity          |
| direct_purchase_products[].`uom_id`                   | String    | –        | –             | Foreign | m_uom.`id`     | UOM Id                            |
| direct_purchase_products[].`rate`                     | Double    | –        | 0             | –       | –              | Purchase Rate                     |
| direct_purchase_products[].`net_value`                | Double    | –        | 0             | –       | –              | Total Value                       |
| direct_purchase_products[].`remarks`                  | String    | Yes      | –             | –       | –              | Remarks                           |

## Product Details

| Name                                                              | Type      | Optional | Default Value | Key     | Reference              | Remarks                             |
| ----------------------------------------------------------------- | --------- | -------- | ------------- | ------- | ---------------------- | ----------------------------------- |
| `products`                                                        | Array Map | –        | –             | –       | –                      | Invoice Product Lines               |
| products[].`id`                                                   | String    | –        | –             | –       | –                      | Row Id                              |
| products[].`sequence`                                             | Int64     | –        | 0             | –       | –                      | Display Sequence                    |
| products[].`receipt_source`                                       | String    | –        | PO            | –       | –                      | PO / GRN / Direct Purchase          |
| products[].`reference_id`                                         | String    | Yes      | –             | –       | –                      | Source Document Id                  |
| products[].`reference_number`                                     | String    | Yes      | –             | –       | –                      | PO Number / GRN Number              |
| products[].`product_id`                                           | String    | –        | –             | Foreign | m_product.`id`         | Product                             |
| products[].`product_name`                                         | String    | –        | –             | –       | –                      | Product Name                        |
| products[].`product_short_name`                                   | String    | Yes      | –             | –       | –                      | Product Short Name                  |
| products[].`variety_id`                                           | String    | Yes      | –             | Foreign | m_product_variety.`id` | Product Variety                     |
| products[].`variety_display_name`                                 | String    | Yes      | –             | –       | –                      | Variety Name                        |
| products[].`product_pack_id`                                      | String    | Yes      | –             | Foreign | m_product_pack.`id`    | Product Pack Id                     |
| products[].`product_pack_name`                                    | String    | Yes      | –             | –       | –                      | Product Pack Name                   |
| products[].`product_pack_short_name`                              | String    | Yes      | –             | –       | –                      | Product Pack Short Name             |
| products[].`count`                                                | Int64     | Yes      | 0             | –       | –                      | Number Of Packs                     |
| products[].`invoice_quantity`<br/>`current_invoiced_quantity`(AI) | Double    | –        | 0             | –       | –                      | Invoice Quantity                    |
| products[].`uom_id`                                               | String    | –        | –             | Foreign | m_uom.`id`             | UOM                                 |
| products[].`uom_short_name`                                       | String    | –        | –             | –       | –                      | UOM Short Name                      |
| products[].`rate`                                                 | Double    | –        | 0             | –       | –                      | Purchase Rate                       |
| products[].`product_value_before_discount`                        | Double    | –        | 0             | –       | –                      | Quantity × Rate                     |
| products[].`discount_type`                                        | String    | Yes      | Fixed         | –       | –                      | Per Unit / Percent / Fixed          |
| products[].`discount_rate`                                        | Double    | Yes      | 0             | –       | –                      | Entered Discount                    |
| products[].`discount_value`                                       | Double    | Yes      | 0             | –       | –                      | Calculated Discount Value           |
| products[].`product_value_after_discount`                         | Double    | –        | 0             | –       | –                      | Product Value After Discount        |
| products[].`taxes`                                                | Array Map | Yes      | –             | –       | –                      | Tax Breakup                         |
| products[].taxes[].`tax_id`                                       | String    | –        | –             | Foreign | m_tax.`id`             | Tax Id                              |
| products[].taxes[].`tax_name`                                     | String    | –        | –             | –       | –                      | Tax Name                            |
| products[].taxes[].`taxable_value`                                | Double    | –        | 0             | –       | –                      | Amount After Discount               |
| products[].taxes[].`tax_rate_percent`                             | Double    | –        | 0             | –       | –                      | Tax Rate Percentage                 |
| products[].taxes[].`tax_value`                                    | Double    | –        | 0             | –       | –                      | Tax Amount                          |
| products[].`tax_value`                                            | Double    | Yes      | 0             | –       | –                      | Total Tax Amount                    |
| products[].`net_value`                                            | Double    | –        | 0             | –       | –                      | Final Line Amount                   |
| products[].`received_quantity`<br/>`total_received_quantity`      | Double    | Yes      | 0             | –       | –                      | Quantity Received Through GRN       |
| products[].`invoiced_quantity`                                    | Double    | Yes      | 0             | –       | –                      | Quantity Covered In Current Invoice |
| products[].`pending_invoice_quantity`                             | Double    | Yes      | 0             | –       | –                      | Remaining Quantity To Be Invoiced   |
| products[].`remarks`                                              | String    | Yes      | –             | –       | –                      | Item Remarks                        |

---

## Tax Details

Tax information is stored inside each product line.

| Name                                  | Type      | Optional | Default Value | Key     | Reference  | Remarks                      |
| ------------------------------------- | --------- | -------- | ------------- | ------- | ---------- | ---------------------------- |
| products[].`taxes`                    | Array Map | Yes      | –             | –       | –          | Product Wise Tax Breakup     |
| products[].taxes[].`tax_id`           | String    | –        | –             | Foreign | m_tax.`id` | Tax Id                       |
| products[].taxes[].`tax_name`         | String    | –        | –             | –       | –          | Tax Name                     |
| products[].taxes[].`taxable_value`    | Double    | –        | 0             | –       | –          | Product Value After Discount |
| products[].taxes[].`tax_rate_percent` | Double    | –        | 0             | –       | –          | Tax Percentage               |
| products[].taxes[].`tax_value`        | Double    | –        | 0             | –       | –          | Tax Amount                   |
| products[].taxes[].`remarks`          | String    | Yes      | –             | –       | –          | Tax Remarks                  |

---

## Additional Charges

| Name                           | Type      | Optional | Default Value | Key | Reference | Remarks                                  |
| ------------------------------ | --------- | -------- | ------------- | --- | --------- | ---------------------------------------- |
| `additional_charges`           | Array Map | Yes      | –             | –   | –         | Freight, Packing, Loading, Other Charges |
| additional_charges[].`id`      | String    | –        | –             | –   | –         | Row Id                                   |
| additional_charges[].`name`    | String    | –        | –             | –   | –         | Charge Name                              |
| additional_charges[].`type`    | String    | –        | Fixed         | –   | –         | Fixed / Percent                          |
| additional_charges[].`rate`    | Double    | Yes      | 0             | –   | –         | Entered Rate                             |
| additional_charges[].`amount`  | Double    | –        | 0             | –   | –         | Calculated Charge Amount                 |
| additional_charges[].`remarks` | String    | Yes      | –             | –   | –         | Charge Remarks                           |

## Payment Terms

| Name                                   | Type         | Optional | Default Value | Key     | Reference           | Remarks                           |
| -------------------------------------- | ------------ | -------- | ------------- | ------- | ------------------- | --------------------------------- |
| `payment_terms`                        | Map          | Yes      | –             | –       | –                   | Payment Information               |
| payment_terms.`payment_mode`           | Array String | Yes      | –             | –       | –                   | Allowed Payment Modes             |
| payment_terms.`base_date`              | String       | –        | Invoice Date  | –       | –                   | Invoice Date / GRN Date / PO Date |
| payment_terms.`credit_days`            | Int64        | –        | 0             | –       | –                   | Credit Period (Days)              |
| payment_terms.`due_date`               | Timestamp    | Yes      | –             | –       | –                   | Calculated Due Date               |
| payment_terms.`tds_applicable`         | Boolean      | –        | False         | –       | –                   | TDS Applicable                    |
| payment_terms.`tds_category_id`        | String       | Yes      | –             | Foreign | m_tds_category.`id` | TDS Category                      |
| payment_terms.`tds_category_name`      | String       | Yes      | –             | –       | –                   | TDS Category Name                 |
| payment_terms.`tds_percentage`         | Double       | Yes      | 0             | –       | –                   | TDS Rate                          |
| payment_terms.`tds_value`              | Double       | Yes      | 0             | –       | –                   | Calculated TDS Amount             |
| payment_terms.`payment`                | Array Map    | Yes      | –             | –       | –                   | Payment Schedule                  |
| payment_terms.payment[].`base_date`    | String       | –        | Invoice Date  | –       | –                   | Invoice Date / GRN Date / PO Date |
| payment_terms.payment[].`days_within`  | Int64        | Yes      | 0             | –       | –                   | Number Of Days Within Base Date   |
| payment_terms.payment[].`due_date`     | Timestamp    | Yes      | –             | –       | –                   | Due Date For Payment              |
| payment_terms.payment[].`percent`      | Double       | Yes      | 0             | –       | –                   | % Of Invoice Amount               |
| payment_terms.payment[].`amount`       | Double       | Yes      | 0             | –       | –                   | Payment Amount                    |
| payment_terms.`discount`               | Array Map    | Yes      | –             | –       | –                   | Discount On Early Payment         |
| payment_terms.discount[].`base_date`   | String       | –        | Invoice Date  | –       | –                   | Invoice Date / GRN Date / PO Date |
| payment_terms.discount[].`days_within` | Int64        | Yes      | 0             | –       | –                   | Number Of Days Within Base Date   |
| payment_terms.discount[].`type`        | String       | Yes      | Fixed         | –       | –                   | Per Unit / Percent / Fixed        |
| payment_terms.discount[].`rate`        | Double       | Yes      | 0             | –       | –                   | Entered Discount                  |
| payment_terms.discount[].`max_value`   | Double       | Yes      | 0             | –       | –                   | Maximum Discount Allowed          |
| payment_terms.`penalty`                | Array Map    | Yes      | –             | –       | –                   | Penalty On Late Payment           |
| payment_terms.penalty[].`base_date`    | String       | –        | Due Date      | –       | –                   | Due Date / Invoice Date           |
| payment_terms.penalty[].`days_after`   | Int64        | Yes      | 0             | –       | –                   | Number Of Days After Due Date     |
| payment_terms.penalty[].`type`         | String       | Yes      | Fixed         | –       | –                   | Per Unit / Percent / Fixed        |
| payment_terms.penalty[].`rate`         | Double       | Yes      | 0             | –       | –                   | Entered Penalty                   |
| payment_terms.penalty[].`max_value`    | Double       | Yes      | 0             | –       | –                   | Maximum Penalty Applicable        |

---

## Vendor Payment References

| Name                               | Type      | Optional | Default Value | Key     | Reference           | Remarks                    |
| ---------------------------------- | --------- | -------- | ------------- | ------- | ------------------- | -------------------------- |
| `vendor_payments`                  | Array Map | Yes      | –             | –       | –                   | Linked Vendor Payments     |
| vendor_payments[].`id`             | String    | –        | –             | –       | –                   | Row Id                     |
| vendor_payments[].`payment_id`     | String    | –        | –             | Foreign | vendor_payment.`id` | Vendor Payment Id          |
| vendor_payments[].`payment_number` | String    | –        | –             | –       | –                   | Vendor Payment Number      |
| vendor_payments[].`payment_date`   | Timestamp | –        | –             | –       | –                   | Payment Date               |
| vendor_payments[].`payment_mode`   | String    | Yes      | –             | –       | –                   | Cash / UPI / Bank / Cheque |
| vendor_payments[].`amount`         | Double    | –        | 0             | –       | –                   | Paid Amount                |
| vendor_payments[].`remarks`        | String    | Yes      | –             | –       | –                   | Payment Remarks            |

---

## Purchase Return References

| Name                                 | Type      | Optional | Default Value | Key     | Reference            | Remarks                 |
| ------------------------------------ | --------- | -------- | ------------- | ------- | -------------------- | ----------------------- |
| `purchase_returns`                   | Array Map | Yes      | –             | –       | –                    | Linked Purchase Returns |
| purchase_returns[].`id`              | String    | –        | –             | –       | –                    | Row Id                  |
| purchase_returns[].`return_id`       | String    | –        | –             | Foreign | purchase_return.`id` | Purchase Return Id      |
| purchase_returns[].`return_number`   | String    | –        | –             | –       | –                    | Purchase Return Number  |
| purchase_returns[].`return_date`     | Timestamp | –        | –             | –       | –                    | Return Date             |
| purchase_returns[].`return_quantity` | Double    | Yes      | 0             | –       | –                    | Total Returned Quantity |
| purchase_returns[].`return_value`    | Double    | Yes      | 0             | –       | –                    | Total Return Value      |
| purchase_returns[].`remarks`         | String    | Yes      | –             | –       | –                    | Return Remarks          |

---

## Attachments

| Name                        | Type      | Optional | Default Value | Key | Reference        | Remarks                  |
| --------------------------- | --------- | -------- | ------------- | --- | ---------------- | ------------------------ |
| `attachments`               | Array Map | Yes      | –             | –   | `Attachment` Map | Supporting Documents     |
| attachments[].`id`          | String    | –        | –             | –   | –                | Row Id                   |
| attachments[].`file_name`   | String    | –        | –             | –   | –                | Original File Name       |
| attachments[].`file_type`   | String    | –        | –             | –   | –                | PDF, JPG, PNG, XLSX etc. |
| attachments[].`file_url`    | String    | –        | –             | –   | –                | File Storage URL         |
| attachments[].`file_size`   | Int64     | Yes      | 0             | –   | –                | File Size In Bytes       |
| attachments[].`uploaded_by` | String    | Yes      | –             | –   | m_user.`id`      | Uploaded User            |
| attachments[].`uploaded_on` | Timestamp | Yes      | –             | –   | –                | Upload Date & Time       |
| attachments[].`remarks`     | String    | Yes      | –             | –   | –                | Attachment Remarks       |

## Summary Fields

| Name                                                       | Type   | Optional | Default Value | Key     | Reference  | Remarks                                   |
| ---------------------------------------------------------- | ------ | -------- | ------------- | ------- | ---------- | ----------------------------------------- |
| `total_items`                                              | Int64  | –        | 0             | –       | –          | Number Of Products                        |
| `total_count`                                              | Int64  | Yes      | 0             | –       | –          | Sum Of Product Pack Count                 |
| `total_quantity`<br/>`total_current_invoiced_quantity`(AI) | Double | –        | 0             | –       | –          | Total Invoice Quantity                    |
| `total_received_quantity`                                  | Double | Yes      | 0             | –       | –          | Quantity Received Through GRNs            |
| `total_invoiced_quantity`                                  | Double | –        | 0             | –       | –          | Total Quantity Covered In Current Invoice |
| `total_pending_invoice_quantity`                           | Double | Yes      | 0             | –       | –          | Remaining Quantity To Be Invoiced         |
| `total_weight`                                             | Double | Yes      | 0             | –       | –          | Total Weight                              |
| `weight_uom_id`                                            | String | Yes      | –             | Foreign | m_uom.`id` | Weight UOM                                |
| `weight_uom_short_name`                                    | String | Yes      | –             | –       | –          | Weight UOM Short Name                     |
| `total_product_value_before_discount`                      | Double | –        | 0             | –       | –          | Total Product Value Before Discount       |
| `total_discount_value`                                     | Double | Yes      | 0             | –       | –          | Total Discount Value                      |
| `total_product_value_after_discount`                       | Double | –        | 0             | –       | –          | Product Value After Discount              |
| `total_tax_value`                                          | Double | Yes      | 0             | –       | –          | Total Tax Value                           |
| `total_tds_value`                                          | Double | Yes      | 0             | –       | –          | Total TDS Deduction                       |
| `total_expense_value`                                      | Double | Yes      | 0             | –       | –          | Additional Expenses                       |
| `total_charge_value`                                       | Double | Yes      | 0             | –       | –          | Total Additional Charges                  |
| `total_freight_tpt_value`                                  | Double | Yes      | 0             | –       | –          | Freight Paid To Transporter               |
| `total_freight_party_value`                                | Double | Yes      | 0             | –       | –          | Freight Paid By Vendor                    |
| `additional_discount_value`                                | Double | Yes      | 0             | –       | –          | Additional Invoice Level Discount         |
| `adjustment_value`                                         | Double | Yes      | 0             | –       | –          | Manual Adjustment                         |
| `round_off_value`                                          | Double | Yes      | 0             | –       | –          | Round Off Difference                      |
| `total_net_value`                                          | Double | –        | 0             | –       | –          | Final Invoice Value                       |

---

## Payment Summary

| Name                          | Type      | Optional | Default Value | Key | Reference | Remarks                            |
| ----------------------------- | --------- | -------- | ------------- | --- | --------- | ---------------------------------- |
| `total_paid_value`            | Double    | Yes      | 0             | –   | –         | Total Paid Through Vendor Payments |
| `total_pending_payment_value` | Double    | Yes      | 0             | –   | –         | Remaining Amount To Be Paid        |
| `payment_status`              | String    | Yes      | Pending       | –   | –         | Pending / Partial / Paid           |
| `last_payment_date`           | Timestamp | Yes      | –             | –   | –         | Latest Vendor Payment Date         |
| `next_due_date`               | Timestamp | Yes      | –             | –   | –         | Upcoming Payment Due Date          |

---

## Direct Purchase Summary

| Name                    | Type    | Optional | Default Value | Key | Reference | Remarks                            |
| ----------------------- | ------- | -------- | ------------- | --- | --------- | ---------------------------------- |
| `allow_direct_purchase` | Boolean | Yes      | False         | –   | –         | Direct Purchase Allowed            |
| `direct_purchase_stage` | String  | Yes      | Pending       | –   | –         | Pending / Approved / Rejected      |
| `total_direct_products` | Int64   | Yes      | 0             | –   | –         | Number Of Direct Purchase Products |
| `total_direct_quantity` | Double  | Yes      | 0             | –   | –         | Total Direct Purchase Quantity     |
| `total_direct_value`    | Double  | Yes      | 0             | –   | –         | Total Direct Purchase Value        |

---

## Reference Summary

| Name                    | Type   | Optional | Default Value | Key | Reference | Remarks                            |
| ----------------------- | ------ | -------- | ------------- | --- | --------- | ---------------------------------- |
| `total_purchase_orders` | Int64  | Yes      | 0             | –   | –         | Number Of Linked Purchase Orders   |
| `total_grns`            | Int64  | Yes      | 0             | –   | –         | Number Of Linked GRNs              |
| `reference_type`        | String | –        | PO            | –   | –         | PO / GRN / Direct Purchase / Mixed |

---

## Workflow Summary

| Name         | Type      | Optional | Default Value | Key     | Reference   | Remarks                |
| ------------ | --------- | -------- | ------------- | ------- | ----------- | ---------------------- |
| `stage`      | Map       | –        | –             | –       | `Stage` Map | Current Workflow Stage |
| `stage_logs` | Array Map | Yes      | –             | –       | `Stage` Map | Workflow History       |
| `created_by` | String    | Yes      | –             | Foreign | m_user.`id` | Created User           |
| `created_on` | Timestamp | Yes      | Current Date  | –       | –           | Creation Date          |
| `updated_by` | String    | Yes      | –             | Foreign | m_user.`id` | Last Updated User      |
| `updated_on` | Timestamp | Yes      | Current Date  | –       | –           | Last Updated Date      |
| `notes`      | Array Map | Yes      | –             | –       | `Note` Map  | User Notes             |

---
