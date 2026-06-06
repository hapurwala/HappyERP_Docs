# Collection: purchase_order

Contains Purchase Order transaction data.

---

## General Information

| Name            | Type      | Optional | Default Value | Key     | Reference     | Remarks                |
| --------------- | --------- | -------- | ------------- | ------- | ------------- | ---------------------- |
| id              | String    | –        | –             | Primary | –             | Document Id            |
| `po_number`     | String    | –        | –             | Unique  | –             | Purchase Order Number  |
| `vendor_id`     | String    | –        | –             | Foreign | m_party.id    | Vendor Id              |
| `vendor_name`   | String    | –        | –             | –       | –             | Vendor Name            |
| `po_date`       | Timestamp | –        | Current Date  | –       | –             | Purchase Order Date    |
| `currency_id`   | String    | –        | –             | Foreign | m_currency.id | Currency               |
| `currency_name` | String    | –        | –             | –       | –             | Currency Name          |
| `freight_type`  | String    | Yes      | Company Paid  | –       | –             | Freight Responsibility |
| `remarks`       | String    | Yes      | –             | –       | –             | General Remarks        |

---

## Line Items

| Name                                  | Type      | Optional | Default Value | Key     | Reference            | Remarks                    |
| ------------------------------------- | --------- | -------- | ------------- | ------- | -------------------- | -------------------------- |
| line_items                            | Array Map | –        | –             | –       | –                    | Product Lines              |
| line_items[].id                       | String    | –        | –             | –       | –                    | Row Id                     |
| line_items[].`sequence`               | Int64     | –        | 0             | –       | –                    | Display Sequence           |
| line_items[].`product_id`             | String    | –        | –             | Foreign | m_product.id         | Product                    |
| line_items[].`product_name`           | String    | –        | –             | –       | –                    | Product Name               |
| line_items[].`product_short_name`     | String    | Yes      | –             | –       | –                    | Product short name         |
| line_items[].`variety_id`             | String    | Yes      | –             | Foreign | m_product_variety.id | Variety                    |
| line_items[].`variety_name`           | String    | Yes      | –             | –       | –                    | Variety Name               |
| line_items[].`pack_id`                | String    | Yes      | –             | Foreign | m_pack.id            | Pack                       |
| line_items[].`pack_name`              | String    | Yes      | –             | –       | –                    | Pack Name                  |
| line_items[].`quantity`               | Double    | –        | 0             | –       | –                    | Ordered Quantity           |
| line_items[].`uom_id`                 | String    | –        | –             | Foreign | m_uom.id             | UOM                        |
| line_items[].`uom_short_name`         | String    | –        | –             | –       | –                    | UOM Short Name             |
| line_items[].`rate`                   | Double    | –        | 0             | –       | –                    | Purchase Rate              |
| line_items[].`discount_type`          | String    | Yes      | Fixed         | –       | –                    | Unit / Percentage / Fixed  |
| line_items[].`discount_rate`          | Double    | Yes      | 0             | –       | –                    | Entered Discount Value     |
| line_items[].`discount_amount`        | Double    | Yes      | 0             | –       | –                    | Calculated Discount Amount |
| line_items[].`product_amount`         | Double    | –        | 0             | –       | –                    | Quantity × Rate            |
| line_items[].`taxes`                  | Array Map | Yes      | –             | –       | –                    | Tax Breakup                |
| line_items[].taxes[].`tax_id`         | String    | –        | –             | Foreign | m_tax.id             | Tax Id                     |
| line_items[].taxes[].`tax_name`       | String    | –        | –             | –       | –                    | Tax Name                   |
| line_items[].`taxable_amount`         | Double    | –        | 0             | –       | –                    | Amount After Discount      |
| line_items[].taxes[].`tax_percentage` | Double    | –        | 0             | –       | –                    | Tax Percentage             |
| line_items[].taxes[].`tax_amount`     | Double    | –        | 0             | –       | –                    | Tax Amount                 |
| line_items[].`tax_amount`             | Double    | Yes      | 0             | –       | –                    | Total Tax Amount           |
| line_items[].`line_total`             | Double    | –        | 0             | –       | –                    | Final Line Amount          |
| line_items[].`received_quantity`      | Double    | Yes      | 0             | –       | –                    | Updated Through GRN        |
| line_items[].`pending_quantity`       | Double    | Yes      | 0             | –       | –                    | Pending Receipt Quantity   |
| line_items[].`remarks`                | String    | Yes      | –             | –       | –                    | Item Remarks               |

---

## Delivery Schedule

| Name                                         | Type      | Optional | Default Value | Key     | Reference      | Remarks                       |
| -------------------------------------------- | --------- | -------- | ------------- | ------- | -------------- | ----------------------------- |
| delivery_schedule                            | Array Map | Yes      | –             | –       | –              | Delivery Planning             |
| delivery_schedule[].id                       | String    | –        | –             | –       | –              | Row Id                        |
| delivery_schedule[].`sequence`               | Int64     | –        | 0             | –       | –              | Display Sequence              |
| delivery_schedule[].`line_item_id`           | String    | Yes      | –             | –       | –              | Linked PO Line Item           |
| delivery_schedule[].`product_pack_id`        | String    | Yes      | –             | Foreign | m_product.id   | Product                       |
| delivery_schedule[].`product_name`           | String    | Yes      | –             | –       | –              | Product Name                  |
| delivery_schedule[].`warehouse_id`           | String    | –        | –             | Foreign | m_warehouse.id | Warehouse                     |
| delivery_schedule[].`warehouse_name`         | String    | –        | –             | –       | –              | Warehouse Name                |
| delivery_schedule[].`location_id`            | String    | Yes      | –             | Foreign | m_location.id  | Storage Location              |
| delivery_schedule[].`location_name`          | String    | Yes      | –             | –       | –              | Location Name                 |
| delivery_schedule[].`quantity`               | Double    | –        | 0             | –       | –              | Planned Delivery Quantity     |
| delivery_schedule[].`received_quantity`      | Double    | Yes      | 0             | –       | –              | Quantity Received Through GRN |
| delivery_schedule[].`pending_quantity`       | Double    | Yes      | 0             | –       | –              | Pending Quantity              |
| delivery_schedule[].`expected_delivery_date` | Timestamp | Yes      | –             | –       | –              | Planned Delivery Date         |
| delivery_schedule[].`actual_delivery_date`   | Timestamp | Yes      | –             | –       | –              | Actual Latest Delivery Date   |
| delivery_schedule[].`delivery_status`        | String    | –        | Pending       | –       | –              | Pending / Partial / Completed |
| delivery_schedule[].`priority`               | String    | Yes      | Medium        | –       | –              | High / Medium / Low           |
| delivery_schedule[].`remarks`                | String    | Yes      | –             | –       | –              | Delivery Remarks              |
| delivery_schedule[].`last_grn_id`            | String    | Yes      | –             | Foreign | t_grn.id       | Last Linked GRN               |
| delivery_schedule[].`updated_on`             | Timestamp | Yes      | –             | –       | –              | Last Updated On               |

---

## Payment Terms

| Name                                                              | Type         | Optional | Default Value | Key     | Reference         | Remarks                                 |
| ----------------------------------------------------------------- | ------------ | -------- | ------------- | ------- | ----------------- | --------------------------------------- |
| payment_terms                                                     | Map          | Yes      | –             | –       | –                 | Payment Information                     |
| payment_terms.`payment_terms_date`                                | Timestamp    | Yes      | Current Date  | –       | –                 | Base Date For Payment Calculation       |
| payment_terms.`payment_basis`                                     | String       | Yes      | Invoice Date  | –       | –                 | Invoice Date / GRN Date / Delivery Date |
| payment_terms.`credit_days`                                       | Int64        | Yes      | 0             | –       | –                 | Credit Period                           |
| payment_terms.`advance_payment_percentage`                        | Double       | Yes      | 0             | –       | –                 | Advance Payment Percentage              |
| payment_terms.`advance_payment_amount`                            | Double       | Yes      | 0             | –       | –                 | Calculated Advance Amount               |
| payment_terms.`grace_days`                                        | Int64        | Yes      | 0             | –       | –                 | Grace Days                              |
| payment_terms.`payment_mode`                                      | Array String | Yes      | –             | –       | –                 | Allowed Payment Modes                   |
| payment_terms.`tds_applicable`                                    | Boolean      | –        | False         | –       | –                 | TDS Applicable                          |
| payment_terms.`tds_category_id`                                   | String       | Yes      | –             | Foreign | m_tds_category.id | TDS Category                            |
| payment_terms.`tds_category_name`                                 | String       | Yes      | –             | –       | –                 | TDS Category Name                       |
| payment_terms.`tds_percentage`                                    | Double       | Yes      | 0             | –       | –                 | TDS Rate                                |
| payment_terms.`tds_amount`                                        | Double       | Yes      | 0             | –       | –                 | Calculated TDS Amount                   |
| payment_terms.`payment_due_date`                                  | Timestamp    | Yes      | –             | –       | –                 | Due Date                                |
| payment_terms.`early_payment_discount_enabled`                    | Boolean      | –        | False         | –       | –                 | Enable Early Payment Discount           |
| payment_terms.`early_payment_discount`                            | Array Map    | Yes      | –             | –       | –                 | Applicable When Enabled                 |
| payment_terms.early_payment_discount.`discount_type`              | String       | Yes      | Percentage    | –       | –                 | Percentage / Fixed Amount               |
| payment_terms.early_payment_discount.`discount_value`             | Double       | Yes      | 0             | –       | –                 | Discount Value                          |
| payment_terms.early_payment_discount.`days_before_due`            | Int64        | Yes      | 0             | –       | –                 | Eligible Days Before Due Date           |
| payment_terms.early_payment_discount.`max_discount_amount`        | Double       | Yes      | 0             | –       | –                 | Maximum Discount Allowed                |
| payment_terms.early_payment_discount.`calculated_discount_amount` | Double       | Yes      | 0             | –       | –                 | Calculated Discount Amount              |
| payment_terms.`late_payment_penalty_enabled`                      | Boolean      | –        | False         | –       | –                 | Enable Late Payment Penalty             |
| payment_terms.`late_payment_penalty`                              | Map          | Yes      | –             | –       | –                 | Applicable When Enabled                 |
| payment_terms.late_payment_penalty.`penalty_type`                 | String       | Yes      | Percentage    | –       | –                 | Percentage / Fixed Amount               |
| payment_terms.late_payment_penalty.`penalty_value`                | Double       | Yes      | 0             | –       | –                 | Penalty Value                           |
| payment_terms.late_payment_penalty.`grace_days`                   | Int64        | Yes      | 0             | –       | –                 | Grace Period Before Penalty             |
| payment_terms.late_payment_penalty.`max_penalty_amount`           | Double       | Yes      | 0             | –       | –                 | Maximum Penalty Allowed                 |
| payment_terms.late_payment_penalty.`calculated_penalty_amount`    | Double       | Yes      | 0             | –       | –                 | Calculated Penalty Amount               |

---

## Attachments

| Name                             | Type      | Optional | Default Value | Key     | Reference | Remarks                           |
| -------------------------------- | --------- | -------- | ------------- | ------- | --------- | --------------------------------- |
| attachments                      | Array Map | Yes      | –             | –       | –         | Uploaded Documents                |
| attachments[].id                 | String    | –        | –             | –       | –         | Attachment Id                     |
| attachments[].`name`             | String    | Yes      | –             | –       | –         | Display Name                      |
| attachments[].`description`      | String    | Yes      | –             | –       | –         | Attachment Description            |
| attachments[].`file_name`        | String    | –        | –             | –       | –         | Original File Name                |
| attachments[].`file_extension`   | String    | Yes      | –             | –       | –         | pdf, jpg, png, xlsx, DOCX         |
| attachments[].`file_type`        | String    | –        | –             | –       | –         | PDF, image, Document, Spreadsheet |
| attachments[].`url`              | String    | –        | –             | –       | –         | File URL                          |
| attachments[].`file_size`        | Int64     | Yes      | 0             | –       | –         | File Size In Bytes                |
| attachments[].`is_primary`       | Boolean   | –        | False         | –       | –         | Primary Attachment                |
| attachments[].`sequence`         | Int64     | –        | 0             | –       | –         | Display Sequence                  |
| attachments[].`start_date`       | Timestamp | Yes      | –             | –       | –         | Valid From                        |
| attachments[].`end_date`         | Timestamp | Yes      | –             | –       | –         | Valid Till                        |
| attachments[].`uploaded_by`      | String    | Yes      | –             | Foreign | m_user.id | Uploaded By                       |
| attachments[].`uploaded_by_name` | String    | Yes      | –             | –       | –         | Uploaded User Name                |
| attachments[].`uploaded_at`      | Timestamp | Yes      | –             | –       | –         | Uploaded Date                     |
| attachments[].`last_modified_at` | Timestamp | Yes      | –             | –       | –         | Last Modified Date                |

---

## Workflow Timeline

| Name                                | Type      | Optional | Default Value | Key     | Reference             | Remarks                            |
| ----------------------------------- | --------- | -------- | ------------- | ------- | --------------------- | ---------------------------------- |
| stage                               | Map       | –        | –             | –       | –                     | Current Workflow Stage             |
| stage.id                            | String    | –        | –             | Foreign | m_app_object_stage.id | Stage Id                           |
| stage.`name`                        | String    | –        | –             | –       | –                     | Stage Name                         |
| stage.`badge_variant`               | String    | –        | –             | –       | –                     | UI Badge                           |
| stage.`remarks`                     | String    | Yes      | –             | –       | –                     | Stage Remarks                      |
| stage.`set_by`                      | String    | Yes      | –             | Foreign | m_user.id             | Updated By                         |
| stage.`set_by_name`                 | String    | Yes      | –             | –       | –                     | Updated By Name                    |
| stage.`set_on`                      | Timestamp | Yes      | –             | –       | –                     | Updated On                         |
| stage.`url`                         | String    | Yes      | –             | –       | –                     | Stage Attachment URL               |
| `workflow_logs`                     | Array Map | Yes      | –             | –       | –                     | Workflow History                   |
| workflow_logs[].id                  | String    | –        | –             | –       | –                     | Log Id                             |
| workflow_logs[].`stage_id`          | String    | –        | –             | Foreign | m_app_object_stage.id | Stage Id                           |
| workflow_logs[].`stage_name`        | String    | –        | –             | –       | –                     | Stage Name                         |
| workflow_logs[].`action`            | String    | –        | –             | –       | –                     | Submit / Approve / Reject / Cancel |
| workflow_logs[].`remarks`           | String    | Yes      | –             | –       | –                     | Remarks                            |
| workflow_logs[].`attachment_url`    | String    | Yes      | –             | –       | –                     | Supporting Document                |
| workflow_logs[].`action_by`         | String    | Yes      | –             | Foreign | m_user.id             | Action By                          |
| workflow_logs[].`action_by_name`    | String    | Yes      | –             | –       | –                     | Action By Name                     |
| workflow_logs[].`action_on`         | Timestamp | Yes      | –             | –       | –                     | Action Date                        |
| `approval_logs`                     | Array Map | Yes      | –             | –       | –                     | Approval / Rejection Logs          |
| approval_logs[].id                  | String    | –        | –             | –       | –                     | Approval Log Id                    |
| approval_logs[].`stage_id`          | String    | –        | –             | Foreign | m_app_object_stage.id | Related Stage                      |
| approval_logs[].`status`            | String    | –        | –             | –       | –                     | Approved / Rejected                |
| approval_logs[].`remarks`           | String    | Yes      | –             | –       | –                     | Approval Remarks                   |
| approval_logs[].`approved_by`       | String    | Yes      | –             | Foreign | m_user.id             | Approved By                        |
| approval_logs[].`approved_by_name`  | String    | Yes      | –             | –       | –                     | Approved By Name                   |
| approval_logs[].`approved_on`       | Timestamp | Yes      | –             | –       | –                     | Approval Date                      |
| `activity_logs`                     | Array Map | Yes      | –             | –       | –                     | User Activity History              |
| activity_logs[].id                  | String    | –        | –             | –       | –                     | Activity Id                        |
| activity_logs[].`activity_type`     | String    | –        | –             | –       | –                     | Create / Modify / Delete / Print   |
| activity_logs[].`field_name`        | String    | Yes      | –             | –       | –                     | Modified Field                     |
| activity_logs[].`old_value`         | String    | Yes      | –             | –       | –                     | Previous Value                     |
| activity_logs[].`new_value`         | String    | Yes      | –             | –       | –                     | Updated Value                      |
| activity_logs[].`performed_by`      | String    | Yes      | –             | Foreign | m_user.id             | Performed By                       |
| activity_logs[].`performed_by_name` | String    | Yes      | –             | –       | –                     | User Name                          |
| activity_logs[].`performed_on`      | Timestamp | Yes      | –             | –       | –                     | Activity Date                      |
| `audit_logs`                        | Array Map | Yes      | –             | –       | –                     | Compliance Audit Logs              |
| audit_logs[].id                     | String    | –        | –             | –       | –                     | Audit Id                           |
| audit_logs[].`event_type`           | String    | –        | –             | –       | –                     | Create / Update / Delete           |
| audit_logs[].`remarks`              | String    | Yes      | –             | –       | –                     | Audit Remarks                      |
| audit_logs[].`created_by`           | String    | Yes      | –             | Foreign | m_user.id             | Audit User                         |
| audit_logs[].`created_by_name`      | String    | Yes      | –             | –       | –                     | Audit User Name                    |
| audit_logs[].`created_on`           | Timestamp | Yes      | –             | –       | –                     | Audit Date                         |
