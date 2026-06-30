## Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Purchase
- **Module**: GRN

This document contains database design details of Purchase GRN module. The data includes:

- General Information 
- Purchase Order Reference
- Reference Summary
- Product Details
- Batch Lot Details
- Quality Inspection
- Attachment
- Additional Charges
- Summary Fields
- Receipt Summary
- Batch Summary
- Quality Summary
- Inventory Posting Summary
- Workflow Summary
- Integration Summary

## Collection: goods_receive

Contains GRN transaction data.

---

## General Information

| Name                    | Type      | Optional | Default Value   | Key                         | Reference            | Remarks                                  |
| ----------------------- | --------- | -------- | --------------- | --------------------------- | -------------------- | ---------------------------------------- |
| `id`                    | String    | –        | –               | Primary                     | –                    | Document Id                              |
| `date`                  | Timestamp | –        | Current Date    | –                           | –                    | GRN Date                                 |
| `financial_year`        | Int64     | –        | –               | –                           | –                    | Financial Year                           |
| `organisation_id`       | String    | –        | –               | Foreign                     | m_organisation.`id`  | Organisation Id                          |
| `branch_id`             | String    | –        | –               | Foreign                     | m_branch.`id`        | Branch Id                                |
| `number_series_id`      | String    | –        | –               | Foreign                     | m_number_series.`id` | Number Series Used                       |
| `number`                | String    | –        | –               | Unique Within Number Series | –                    | GRN Number                               |
| `formatted_number`      | String    | –        | –               | –                           | –                    | GRN Number With Prefix And Suffix        |
| `receipt_type`          | String    | –        | Regular Receipt | –                           | –                    | Regular / Returnable / Sample / Transfer |
| `party_id`              | String    | –        | –               | Foreign                     | m_party.`id`         | Vendor Id                                |
| `party_display_name`    | String    | –        | –               | –                           | –                    | Vendor Display Name                      |
| `vehicle_number`        | String    | Yes      | –               | –                           | –                    | Vehicle Number                           |
| `lr_number`             | String    | Yes      | –               | –                           | –                    | Lorry Receipt Number                     |
| `transporter_name`      | String    | Yes      | –               | –                           | –                    | Transport Company Name                   |
| `vendor_invoice_number` | String    | Yes      | –               | –                           | –                    | Vendor Invoice Number                    |
| `vendor_invoice_date`   | Timestamp | Yes      | –               | –                           | –                    | Vendor Invoice Date                      |
| `currency_id`           | String    | –        | –               | Foreign                     | m_currency.`id`      | Currency Id                              |
| `currency_short_name`   | String    | –        | –               | –                           | –                    | Currency Short Name                      |
| `remarks`               | String    | Yes      | –               | –                           | –                    | Additional Remarks                       |
| `attachments`           | Array Map | Yes      | –               | –                           | `Attachment` Map     | GRN Attachments                          |
| `stage`                 | Map       | –        | –               | –                           | `Stage` Map          | Current Workflow Stage                   |
| `stage_logs`            | Array Map | Yes      | –               | –                           | `Stage` Map          | Workflow History                         |
| `notes`                 | Array Map | Yes      | –               | –                           | `Note` Map           | User Notes                               |

---

## Purchase Order References

| Name                                    | Type      | Optional | Default Value | Key     | Reference           | Remarks                      |
| --------------------------------------- | --------- | -------- | ------------- | ------- | ------------------- | ---------------------------- |
| `purchase_orders`                       | Array Map | Yes      | –             | –       | –                   | Linked Purchase Orders       |
| purchase_orders[].`id`                  | String    | –        | –             | –       | –                   | Row Id                       |
| purchase_orders[].`po_id`               | String    | –        | –             | Foreign | purchase_order.`id` | Purchase Order Id            |
| purchase_orders[].`po_number`           | String    | –        | –             | –       | –                   | Purchase Order Number        |
| purchase_orders[].`po_date`             | Timestamp | Yes      | –             | –       | –                   | Purchase Order Date          |
| purchase_orders[].`po_stage`            | String    | Yes      | –             | –       | –                   | Current PO Stage             |
| purchase_orders[].`currency_id`         | String    | Yes      | –             | Foreign | m_currency.`id`     | Currency Id                  |
| purchase_orders[].`currency_short_name` | String    | Yes      | –             | –       | –                   | Currency Short Name          |
| purchase_orders[].`total_quantity`      | Double    | Yes      | 0             | –       | –                   | Total Ordered Quantity       |
| purchase_orders[].`received_quantity`   | Double    | Yes      | 0             | –       | –                   | Previously Received Quantity |
| purchase_orders[].`pending_quantity`    | Double    | Yes      | 0             | –       | –                   | Pending Quantity             |
| purchase_orders[].`net_value`           | Double    | Yes      | 0             | –       | –                   | PO Net Value                 |

---

## Reference Summary

| Name                     | Type   | Optional | Default Value | Key | Reference | Remarks                          |
| ------------------------ | ------ | -------- | ------------- | --- | --------- | -------------------------------- |
| `total_purchase_orders`  | Int64  | Yes      | 0             | –   | –         | Number Of Linked Purchase Orders |
| `total_po_quantity`      | Double | Yes      | 0             | –   | –         | Total Ordered Quantity           |
| `total_pending_quantity` | Double | Yes      | 0             | –   | –         | Total Pending Quantity           |
| `total_receipt_quantity` | Double | Yes      | 0             | –   | –         | Current GRN Receipt Quantity     |
| `total_po_value`         | Double | Yes      | 0             | –   | –         | Total Purchase Order Value       |
| `reference_type`         | String | –        | PO            | –   | –         | Single PO / Multiple PO / Mixed  |

## Product Details

| Name                                    | Type      | Optional | Default Value | Key     | Reference              | Remarks                          |
| --------------------------------------- | --------- | -------- | ------------- | ------- | ---------------------- | -------------------------------- |
| `products`                              | Array Map | –        | –             | –       | –                      | Received Products                |
| products[].`id`                         | String    | –        | –             | –       | –                      | Row Id                           |
| products[].`sequence`                   | Int64     | –        | 0             | –       | –                      | Display Sequence                 |
| products[].`receipt_source`             | String    | –        | PO            | –       | –                      | PO / Direct Receipt              |
| products[].`po_id`                      | String    | Yes      | –             | Foreign | purchase_order.`id`    | Purchase Order Id                |
| products[].`po_number`                  | String    | Yes      | –             | –       | –                      | Purchase Order Number            |
| products[].`po_product_id`              | String    | Yes      | –             | –       | –                      | Product Row Id In PO             |
| products[].`product_id`                 | String    | –        | –             | Foreign | m_product.`id`         | Product Id                       |
| products[].`product_name`               | String    | –        | –             | –       | –                      | Product Name                     |
| products[].`product_short_name`         | String    | Yes      | –             | –       | –                      | Product Short Name               |
| products[].`variety_id`                 | String    | Yes      | –             | Foreign | m_product_variety.`id` | Variety Id                       |
| products[].`variety_display_name`       | String    | Yes      | –             | –       | –                      | Variety Name                     |
| products[].`product_pack_id`            | String    | Yes      | –             | Foreign | m_product_pack.`id`    | Product Pack Id                  |
| products[].`product_pack_name`          | String    | Yes      | –             | –       | –                      | Product Pack Name                |
| products[].`product_pack_short_name`    | String    | Yes      | –             | –       | –                      | Product Pack Short Name          |
| products[].`count`                      | Int64     | Yes      | 0             | –       | –                      | Number Of Packs                  |
| products[].`ordered_quantity`           | Double    | Yes      | 0             | –       | –                      | Quantity Ordered In PO           |
| products[].`previous_received_quantity` | Double    | Yes      | 0             | –       | –                      | Quantity Received Earlier        |
| products[].`current_received_quantity`  | Double    | –        | 0             | –       | –                      | Quantity Received In Current GRN |
| products[].`pending_quantity`           | Double    | Yes      | 0             | –       | –                      | Remaining Quantity               |
| products[].`rejected_quantity`          | Double    | Yes      | 0             | –       | –                      | Rejected Quantity                |
| products[].`approved_quantity`          | Double    | Yes      | 0             | –       | –                      | Approved Quantity                |
| products[].`uom_id`                     | String    | –        | –             | Foreign | m_uom.`id`             | UOM Id                           |
| products[].`uom_short_name`             | String    | –        | –             | –       | –                      | UOM Short Name                   |
| products[].`rate`                       | Double    | Yes      | 0             | –       | –                      | Purchase Rate                    |
| products[].`product_value`              | Double    | Yes      | 0             | –       | –                      | Quantity × Rate                  |
| products[].`remarks`                    | String    | Yes      | –             | –       | –                      | Product Remarks                  |

---

## Batch / Lot Details

| Name                                 | Type      | Optional | Default Value  | Key     | Reference           | Remarks                   |
| ------------------------------------ | --------- | -------- | -------------- | ------- | ------------------- | ------------------------- |
| `batch_details`                      | Array Map | Yes      | –              | –       | –                   | Batch Information         |
| batch_details[].`id`                 | String    | –        | –              | –       | –                   | Row Id                    |
| batch_details[].`product_id`         | String    | –        | –              | Foreign | m_product.`id`      | Product Id                |
| batch_details[].`product_name`       | String    | –        | –              | –       | –                   | Product Name              |
| batch_details[].`po_id`              | String    | Yes      | –              | Foreign | purchase_order.`id` | Related PO                |
| batch_details[].`batch_number`       | String    | Yes      | Auto Generated | Unique  | –                   | Batch Number              |
| batch_details[].`lot_number`         | String    | Yes      | –              | –       | –                   | Vendor Lot Number         |
| batch_details[].`manufacturing_date` | Timestamp | Yes      | –              | –       | –                   | Manufacturing Date        |
| batch_details[].`expiry_date`        | Timestamp | Yes      | –              | –       | –                   | Expiry Date               |
| batch_details[].`received_quantity`  | Double    | –        | 0              | –       | –                   | Current Received Quantity |
| batch_details[].`approved_quantity`  | Double    | Yes      | 0              | –       | –                   | Approved Quantity         |
| batch_details[].`rejected_quantity`  | Double    | Yes      | 0              | –       | –                   | Rejected Quantity         |
| batch_details[].`warehouse_id`       | String    | Yes      | –              | Foreign | m_warehouse.`id`    | Warehouse                 |
| batch_details[].`rack_id`            | String    | Yes      | –              | Foreign | m_rack.`id`         | Rack                      |
| batch_details[].`bin_id`             | String    | Yes      | –              | Foreign | m_bin.`id`          | Bin                       |
| batch_details[].`remarks`            | String    | Yes      | –              | –       | –                   | Batch Remarks             |

---

## Quality Inspection

| Name                                        | Type      | Optional | Default Value | Key     | Reference           | Remarks                                 |
| ------------------------------------------- | --------- | -------- | ------------- | ------- | ------------------- | --------------------------------------- |
| `quality_inspections`                       | Array Map | Yes      | –             | –       | –                   | Product Wise Quality Inspection         |
| quality_inspections[].`id`                  | String    | –        | –             | –       | –                   | Row Id                                  |
| quality_inspections[].`po_id`               | String    | Yes      | –             | Foreign | purchase_order.`id` | Related PO                              |
| quality_inspections[].`po_number`           | String    | Yes      | –             | –       | –                   | Purchase Order Number                   |
| quality_inspections[].`product_id`          | String    | –        | –             | Foreign | m_product.`id`      | Product Id                              |
| quality_inspections[].`product_name`        | String    | –        | –             | –       | –                   | Product Name                            |
| quality_inspections[].`batch_number`        | String    | Yes      | –             | –       | –                   | Related Batch                           |
| quality_inspections[].`inspection_required` | Boolean   | –        | False         | –       | –                   | Inspection Required                     |
| quality_inspections[].`inspection_status`   | String    | Yes      | Pending       | –       | –                   | Pending / Approved / Rejected / Partial |
| quality_inspections[].`inspected_by`        | String    | Yes      | –             | Foreign | m_user.`id`         | Inspector                               |
| quality_inspections[].`inspection_date`     | Timestamp | Yes      | –             | –       | –                   | Inspection Date                         |
| quality_inspections[].`received_quantity`   | Double    | –        | 0             | –       | –                   | Current Received Quantity               |
| quality_inspections[].`approved_quantity`   | Double    | Yes      | 0             | –       | –                   | Approved Quantity                       |
| quality_inspections[].`rejected_quantity`   | Double    | Yes      | 0             | –       | –                   | Rejected Quantity                       |
| quality_inspections[].`remarks`             | String    | Yes      | –             | –       | –                   | Inspection Remarks                      |

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

---

## Additional Charges

| Name                           | Type      | Optional | Default Value | Key | Reference | Remarks                             |
| ------------------------------ | --------- | -------- | ------------- | --- | --------- | ----------------------------------- |
| `additional_charges`           | Array Map | Yes      | –             | –   | –         | Freight, Loading, Unloading Charges |
| additional_charges[].`id`      | String    | –        | –             | –   | –         | Row Id                              |
| additional_charges[].`name`    | String    | –        | –             | –   | –         | Charge Name                         |
| additional_charges[].`type`    | String    | Yes      | Fixed         | –   | –         | Fixed / Percent                     |
| additional_charges[].`rate`    | Double    | Yes      | 0             | –   | –         | Entered Rate                        |
| additional_charges[].`amount`  | Double    | Yes      | 0             | –   | –         | Calculated Amount                   |
| additional_charges[].`remarks` | String    | Yes      | –             | –   | –         | Charge Remarks                      |

---

## Summary Fields

| Name                            | Type   | Optional | Default Value | Key     | Reference  | Remarks                  |
| ------------------------------- | ------ | -------- | ------------- | ------- | ---------- | ------------------------ |
| `total_items`                   | Int64  | Yes      | 0             | –       | –          | Total Products           |
| `total_count`                   | Int64  | Yes      | 0             | –       | –          | Total Product Packs      |
| `total_received_quantity`       | Double | Yes      | 0             | –       | –          | Total Received Quantity  |
| `total_approved_quantity`       | Double | Yes      | 0             | –       | –          | Total Approved Quantity  |
| `total_rejected_quantity`       | Double | Yes      | 0             | –       | –          | Total Rejected Quantity  |
| `total_pending_quantity`        | Double | Yes      | 0             | –       | –          | Total Pending Quantity   |
| `total_weight`                  | Double | Yes      | 0             | –       | –          | Total Weight             |
| `weight_uom_id`                 | String | Yes      | –             | Foreign | m_uom.`id` | Weight UOM               |
| `weight_uom_short_name`         | String | Yes      | –             | –       | –          | Weight UOM Short Name    |
| `total_product_value`           | Double | Yes      | 0             | –       | –          | Total Product Value      |
| `total_additional_charge_value` | Double | Yes      | 0             | –       | –          | Total Additional Charges |
| `round_off_value`               | Double | Yes      | 0             | –       | –          | Round Off Difference     |
| `total_net_value`               | Double | Yes      | 0             | –       | –          | Final GRN Value          |

---

## Receipt Summary

| Name                    | Type    | Optional | Default Value | Key | Reference | Remarks                        |
| ----------------------- | ------- | -------- | ------------- | --- | --------- | ------------------------------ |
| `receipt_type`          | String  | Yes      | Regular       | –   | –         | Regular / Sample / Returnable  |
| `allow_direct_receipt`  | Boolean | Yes      | False         | –   | –         | Non-PO Product Receipt Allowed |
| `direct_receipt_stage`  | String  | Yes      | Pending       | –   | –         | Pending / Approved / Rejected  |
| `total_direct_products` | Int64   | Yes      | 0             | –   | –         | Total Direct Receipt Products  |
| `total_direct_quantity` | Double  | Yes      | 0             | –   | –         | Direct Receipt Quantity        |

---

## Batch Summary

| Name                  | Type  | Optional | Default Value | Key | Reference | Remarks                 |
| --------------------- | ----- | -------- | ------------- | --- | --------- | ----------------------- |
| `total_batches`       | Int64 | Yes      | 0             | –   | –         | Total Batch Count       |
| `expired_batches`     | Int64 | Yes      | 0             | –   | –         | Expired Batches         |
| `near_expiry_batches` | Int64 | Yes      | 0             | –   | –         | Near Expiry Batch Count |
| `warehouse_count`     | Int64 | Yes      | 0             | –   | –         | Warehouses Used         |
| `rack_count`          | Int64 | Yes      | 0             | –   | –         | Racks Used              |
| `bin_count`           | Int64 | Yes      | 0             | –   | –         | Bins Used               |

---

## Quality Summary

| Name                         | Type    | Optional | Default Value | Key | Reference | Remarks                       |
| ---------------------------- | ------- | -------- | ------------- | --- | --------- | ----------------------------- |
| `inspection_required`        | Boolean | Yes      | False         | –   | –         | At Least One Product Needs QC |
| `inspection_status`          | String  | Yes      | Pending       | –   | –         | Pending / Approved / Rejected |
| `total_inspected_products`   | Int64   | Yes      | 0             | –   | –         | Total Inspected Products      |
| `total_approved_products`    | Int64   | Yes      | 0             | –   | –         | Products Approved             |
| `total_rejected_products`    | Int64   | Yes      | 0             | –   | –         | Products Rejected             |
| `quality_completion_percent` | Double  | Yes      | 0             | –   | –         | Quality Completion Percentage |

---

## Inventory Posting Summary

| Name                       | Type      | Optional | Default Value | Key     | Reference          | Remarks                 |
| -------------------------- | --------- | -------- | ------------- | ------- | ------------------ | ----------------------- |
| `stock_posted`             | Boolean   | Yes      | False         | –       | –                  | Inventory Updated       |
| `stock_posted_on`          | Timestamp | Yes      | –             | –       | –                  | Stock Posting Date      |
| `stock_posted_by`          | String    | Yes      | –             | Foreign | m_user.`id`        | User Who Posted Stock   |
| `inventory_voucher_id`     | String    | Yes      | –             | Foreign | stock_voucher.`id` | Generated Stock Voucher |
| `inventory_voucher_number` | String    | Yes      | –             | –       | –                  | Voucher Number          |

## Workflow Summary

| Name          | Type      | Optional | Default Value | Key     | Reference   | Remarks                |
| ------------- | --------- | -------- | ------------- | ------- | ----------- | ---------------------- |
| `stage`       | Map       | –        | –             | –       | `Stage` Map | Current Workflow Stage |
| `stage_logs`  | Array Map | Yes      | –             | –       | `Stage` Map | Workflow History       |
| `created_by`  | String    | Yes      | –             | Foreign | m_user.`id` | Created User           |
| `created_on`  | Timestamp | Yes      | Current Date  | –       | –           | Creation Date          |
| `updated_by`  | String    | Yes      | –             | Foreign | m_user.`id` | Last Updated User      |
| `updated_on`  | Timestamp | Yes      | Current Date  | –       | –           | Last Updated Date      |
| `verified_by` | String    | Yes      | –             | Foreign | m_user.`id` | GRN Verification User  |
| `verified_on` | Timestamp | Yes      | –             | –       | –           | Verification Date      |
| `closed_by`   | String    | Yes      | –             | Foreign | m_user.`id` | User Who Closed GRN    |
| `closed_on`   | Timestamp | Yes      | –             | –       | –           | GRN Closing Date       |
| `notes`       | Array Map | Yes      | –             | –       | `Note` Map  | User Notes             |

---

## Integration Summary

| Name                            | Type    | Optional | Default Value | Key | Reference | Remarks                               |
| ------------------------------- | ------- | -------- | ------------- | --- | --------- | ------------------------------------- |
| `linked_purchase_invoice_count` | Int64   | Yes      | 0             | –   | –         | Number Of Linked Purchase Invoices    |
| `linked_purchase_invoice_ids`   | Array   | Yes      | –             | –   | –         | Purchase Invoice Ids                  |
| `linked_purchase_return_count`  | Int64   | Yes      | 0             | –   | –         | Number Of Linked Purchase Returns     |
| `linked_purchase_return_ids`    | Array   | Yes      | –             | –   | –         | Purchase Return Ids                   |
| `is_fully_invoiced`             | Boolean | Yes      | False         | –   | –         | GRN Fully Covered By Purchase Invoice |
| `is_fully_returned`             | Boolean | Yes      | False         | –   | –         | All Received Goods Returned           |

---
