# Collection: t_purchase_order

Contains Purchase Order transaction data.

---

## General Information

| Name          | Type      | Optional | Default Value | Key     | Reference     | Remarks                |
| ------------- | --------- | -------- | ------------- | ------- | ------------- | ---------------------- |
| id            | String    | –        | –             | Primary | –             | Document Id            |
| po_number     | String    | –        | –             | Unique  | –             | Purchase Order Number  |
| vendor_id     | String    | –        | –             | Foreign | m_party.id    | Vendor Id              |
| vendor_name   | String    | –        | –             | –       | –             | Vendor Name            |
| po_date       | Timestamp | –        | Current Date  | –       | –             | Purchase Order Date    |
| currency_id   | String    | –        | –             | Foreign | m_currency.id | Currency               |
| currency_name | String    | –        | –             | –       | –             | Currency Name          |
| freight_type  | String    | Yes      | Company Paid  | –       | –             | Freight Responsibility |
| remarks       | String    | Yes      | –             | –       | –             | General Remarks        |

---

## Line Items

| Name                         | Type      | Optional | Default Value | Key     | Reference            | Remarks           |
| ---------------------------- | --------- | -------- | ------------- | ------- | -------------------- | ----------------- |
| line_items                   | Array Map | –        | –             | –       | –                    | Product Lines     |
| line_items[].id              | String    | –        | –             | –       | –                    | Row Id            |
| line_items[].product_id      | String    | –        | –             | Foreign | m_product.id         | Product           |
| line_items[].product_name    | String    | –        | –             | –       | –                    | Product Name      |
| line_items[].sku             | String    | Yes      | –             | –       | –                    | Product SKU       |
| line_items[].variety_id      | String    | Yes      | –             | Foreign | m_product_variety.id | Variety           |
| line_items[].variety_name    | String    | Yes      | –             | –       | –                    | Variety Name      |
| line_items[].pack_id         | String    | Yes      | –             | Foreign | m_pack.id            | Pack              |
| line_items[].pack_name       | String    | Yes      | –             | –       | –                    | Pack Name         |
| line_items[].quantity        | Double    | –        | 0             | –       | –                    | Ordered Quantity  |
| line_items[].uom_id          | String    | –        | –             | Foreign | m_uom.id             | UOM               |
| line_items[].uom_name        | String    | –        | –             | –       | –                    | UOM Name          |
| line_items[].rate            | Double    | –        | 0             | –       | –                    | Purchase Rate     |
| line_items[].discount_amount | Double    | Yes      | 0             | –       | –                    | Discount          |
| line_items[].tax_amount      | Double    | Yes      | 0             | –       | –                    | Tax Amount        |
| line_items[].line_total      | Double    | –        | 0             | –       | –                    | Final Line Amount |

---

## Delivery Schedule

| Name                                       | Type      | Optional | Default Value | Key     | Reference      | Remarks           |
| ------------------------------------------ | --------- | -------- | ------------- | ------- | -------------- | ----------------- |
| delivery_schedule                          | Array Map | Yes      | –             | –       | –              | Delivery Planning |
| delivery_schedule[].id                     | String    | –        | –             | –       | –              | Row Id            |
| delivery_schedule[].warehouse_id           | String    | –        | –             | Foreign | m_warehouse.id | Warehouse         |
| delivery_schedule[].warehouse_name         | String    | –        | –             | –       | –              | Warehouse Name    |
| delivery_schedule[].location_id            | String    | Yes      | –             | Foreign | m_location.id  | Storage Location  |
| delivery_schedule[].location_name          | String    | Yes      | –             | –       | –              | Location Name     |
| delivery_schedule[].quantity               | Double    | –        | 0             | –       | –              | Delivery Quantity |
| delivery_schedule[].expected_delivery_date | Timestamp | Yes      | –             | –       | –              | Expected Delivery |
| delivery_schedule[].priority               | String    | Yes      | Medium        | –       | –              | Delivery Priority |
| delivery_schedule[].remarks                | String    | Yes      | –             | –       | –              | Delivery Remarks  |

---

## Payment Terms

| Name                                         | Type         | Optional | Default Value | Key     | Reference         | Remarks               |
| -------------------------------------------- | ------------ | -------- | ------------- | ------- | ----------------- | --------------------- |
| payment_terms                                | Map          | Yes      | –             | –       | –                 | Payment Information   |
| payment_terms.payment_basis                  | String       | Yes      | Invoice Date  | –       | –                 | Payment Basis         |
| payment_terms.credit_days                    | Int64        | Yes      | 0             | –       | –                 | Credit Period         |
| payment_terms.advance_payment_percentage     | Double       | Yes      | 0             | –       | –                 | Advance Payment       |
| payment_terms.grace_days                     | Int64        | Yes      | 0             | –       | –                 | Grace Days            |
| payment_terms.payment_mode                   | Array String | Yes      | –             | –       | –                 | Allowed Payment Modes |
| payment_terms.tds_applicable                 | Boolean      | –        | False         | –       | –                 | TDS Applicable        |
| payment_terms.tds_category_id                | String       | Yes      | –             | Foreign | m_tds_category.id | TDS Category          |
| payment_terms.tds_percentage                 | Double       | Yes      | 0             | –       | –                 | TDS Rate              |
| payment_terms.payment_due_date               | Timestamp    | Yes      | –             | –       | –                 | Due Date              |
| payment_terms.early_payment_discount_enabled | Boolean      | –        | False         | –       | –                 | Early Discount        |
| payment_terms.late_payment_penalty_enabled   | Boolean      | –        | False         | –       | –                 | Late Penalty          |

---

## Attachments

| Name                      | Type      | Optional | Default Value | Key     | Reference | Remarks            |
| ------------------------- | --------- | -------- | ------------- | ------- | --------- | ------------------ |
| attachments               | Array Map | Yes      | –             | –       | –         | Uploaded Documents |
| attachments[].id          | String    | –        | –             | –       | –         | Attachment Id      |
| attachments[].file_name   | String    | –        | –             | –       | –         | File Name          |
| attachments[].file_type   | String    | –        | –             | –       | –         | File Type          |
| attachments[].url         | String    | –        | –             | –       | –         | File URL           |
| attachments[].uploaded_by | String    | Yes      | –             | Foreign | m_user.id | Uploaded By        |
| attachments[].uploaded_on | Timestamp | Yes      | –             | –       | –         | Uploaded Date      |

---

## Workflow Timeline

| Name                | Type      | Optional | Default Value | Key     | Reference             | Remarks                |
| ------------------- | --------- | -------- | ------------- | ------- | --------------------- | ---------------------- |
| stage               | Map       | –        | –             | –       | –                     | Current Workflow Stage |
| stage.id            | String    | –        | –             | Foreign | m_app_object_stage.id | Stage Id               |
| stage.name          | String    | –        | –             | –       | –                     | Stage Name             |
| stage.badge_variant | String    | –        | –             | –       | –                     | UI Badge               |
| stage.remarks       | String    | Yes      | –             | –       | –                     | Stage Remarks          |
| stage.set_by        | String    | Yes      | –             | Foreign | m_user.id             | Updated By             |
| stage.set_on        | Timestamp | Yes      | –             | –       | –                     | Updated On             |

| workflow_logs | Array Map | Yes | – | – | – | Workflow History |  
| workflow_logs[].stage_id | String | – | – | Foreign | m_app_object_stage.id | Stage Id |  
| workflow_logs[].stage_name | String | – | – | – | – | Stage Name |  
| workflow_logs[].remarks | String | Yes | – | – | – | Remarks |  
| workflow_logs[].set_by | String | Yes | – | Foreign | m_user.id | Action By |  
| workflow_logs[].set_on | Timestamp | Yes | – | – | – | Action Date |
