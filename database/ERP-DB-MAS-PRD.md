# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Masters
- **Module**: Product Master

Following collections are maintained for Product Master

* **m_brand**: contains data of the brands
* **m_product_group**: contains data of product groups
* **m_product**: contains data of products 
* **m_product_variety**: contains data of product varierties
* **m_product_pack**: contains data of product packs
* **m_uom**: contains data of UoM (unit of measurement)
* **m_product_attribute**: contains data of product attributes (color, size, width etc)
* **m_product_attribute_value**: conatins values of above product attributes

## Collection: m_brand

| Name           | Type      | Optional | Default Value | Key     | Reference        | Remarks                    |
|:-------------- |:--------- |:-------- |:------------- |:------- |:---------------- |:-------------------------- |
| `id`           | String    | –        | –             | Primary | –                | Document Id                |
| `name`         | String    | –        | –             | –       | –                |                            |
| `short_name`   | String    | –        | –             | –       | –                |                            |
| `is_own_brand` | Boolean   | -        | False         | -       | -                | Whether it is own brand    |
| `attachments`  | Array Map | Yes      | –             | –       | `Attachment` Map | Documents                  |
| `start_date`   | Timestamp | –        | –             | –       | –                |                            |
| `end_date`     | Timestamp | Yes      | –             | –       | –                |                            |
| `reason_end`   | String    | Yes      | –             | –       | –                |                            |
| `stage`        | Map       | –        | –             | –       | `Stage` Map      | Current Stage              |
| `stage_logs`   | Array Map | Yes      | –             | –       | `Stage` Map      | Workflow History           |
| `notes`        | Array Map | -        | -             | -       | `Note` Map       | It is an array of Note map |

## Collection: m_product_group

| Name                      | Type      | Optional | Default Value | Key     | Reference            | Remarks                         |
|:------------------------- |:--------- |:-------- |:------------- |:------- |:-------------------- |:------------------------------- |
| `id`                      | String    | –        | –             | Primary | –                    | Document Id                     |
| `name`                    | String    | –        | –             | –       | –                    |                                 |
| `short_name`              | String    | –        | –             | –       | –                    |                                 |
| `uom_id`                  | Int64     | -        | -             | Foreign | m_uom.`id`           | UoM used for this product group |
| `parent_product_group_id` | String    | Yes      | -             | Foreign | m_product_group.`id` | Product Group hierarchy         |
| `start_date`              | Timestamp | –        | –             | –       | –                    |                                 |
| `end_date`                | Timestamp | Yes      | –             | –       | –                    |                                 |
| `reason_end`              | String    | Yes      | –             | –       | –                    |                                 |
| `stage`                   | Map       | –        | –             | –       | `Stage` Map          | Current Stage                   |
| `stage_logs`              | Array Map | Yes      | –             | –       | `Stage` Map          | Workflow History                |
| `notes`                   | Array Map | -        | -             | -       | `Note` Map           | It is an array of Note map      |

## Collection: m_product

| Name                              | Type      | Optional | Default Value | Key     | Reference                  | Remarks                                        |
|:--------------------------------- |:--------- |:-------- |:------------- |:------- |:-------------------------- |:---------------------------------------------- |
| `id`                              | String    | –        | –             | Primary | –                          | Document Id                                    |
| `brand_id`                        | String    | -        | -             | Foreign | m_brand.`id`               |                                                |
| `product_group_id`                | String    | -        | -             | Foreign | m_product_group.`id`       |                                                |
| `name`                            | String    | –        | –             | –       | –                          |                                                |
| `short_name`                      | String    | –        | –             | –       | –                          |                                                |
| `display_name`                    | String    | –        | –             | –       | –                          |                                                |
| `description`                     | String    | Yes      |               | -       |                            |                                                |
| `uom_id`                          | String    | -        |               | Foreign | m_uom.`id`                 |                                                |
| `hsn_code`                        | String    | Yes      |               | -       |                            | Can this be shifted to tax details?            |
| `sequence`                        | Int64     | -        | -             | -       |                            |                                                |
| `usage_cateogry`                  | String    | -        | 0             | -       | s_cateogry.`product_usage` | Raw Material, Finished Product etc             |
| `to_purchase`                     | Boolean   | -        | 0             | -       |                            |                                                |
| `to_produce`                      | Boolean   | -        | 0             | -       |                            |                                                |
| `to_sale`                         | Boolean   | -        | 0             | -       |                            |                                                |
| `is_goods`                        | Boolean   | -        | 0             | -       |                            |                                                |
| `maintain_stock`                  | Boolean   | -        | 1             | -       |                            |                                                |
| `maintain_batch`                  | Boolean   | -        | 0             | -       |                            |                                                |
| `maintain_expiry`                 | Boolean   | -        | 0             | -       |                            | Applicable when `maintain_stock` = True        |
| `minimum_stock_quantity`          | Double    | -        | 0             | -       |                            | Applicable when `maintain_stock` = True        |
| `maximum_stock_quantity`          | Double    | -        | 0             | -       |                            |                                                |
| `reorder_stock_quantity`          | Double    | -        | 0             | -       |                            |                                                |
| `purchase_minimum_order_quantity` | Double    | -        | 0             | -       |                            |                                                |
| `purchase_maximum_order_quantity` | Double    | -        | 0             | -       |                            |                                                |
| `purchase_minimum_delivery_days`  | Double    | -        | 0             | -       |                            |                                                |
| `sales_minimum_order_quantity`    | Double    | -        | 0             | -       |                            |                                                |
| `sales_maximum_order_quantity`    | Double    | -        | 0             | -       |                            |                                                |
| `sales_minimum_delivery_days`     | Double    | -        | 0             | -       |                            |                                                |
| `payment_terms_id`                | String    | Yes      | -             | Foreign | m_payment_terms.`id`       | Default Payment Terms for this product         |
| `start_date`                      | Timestamp | –        | –             | –       | –                          |                                                |
| `end_date`                        | Timestamp | Yes      | –             | –       | –                          |                                                |
| `reason_end`                      | String    | Yes      | –             | –       | –                          |                                                |
| `organisations`                   | Array Map | Yes      | –             | –       | –                          | Organisations that deals in this product       |
| organisations.`organisation_id`   | String    | -        | –             | Foreign | m_organisation.`id`        |                                                |
| organisations.`start_date`        | Timestamp | –        | –             | –       | –                          |                                                |
| organisations.`end_date`          | Timestamp | Yes      | –             | –       | –                          |                                                |
| organisations.`reason_end`        | String    | Yes      | –             | –       | –                          |                                                |
| `branches`                        | Array Map | Yes      | –             | –       | –                          | Organisations that deals in this product       |
| branches.`branch_id`              | String    | -        | –             | Foreign | m_branch.`id`              |                                                |
| branches.`is_production`          | Boolean   | -        | 0             | -       |                            | Is this product produced here?                 |
| branches.`is_warehouse`           | Boolean   | -        | 0             | -       |                            | Is this product stroed here?                   |
| branches.`is_dispatch`            | Boolean   | -        | 0             | -       |                            | Is this product dispatched/invoiced from here? |
| branches.`is_default`             | Boolean   | -        | 0             | -       |                            | Is this default branch for this product?       |
| branches.`minimum_stock_quantity` | Double    | -        | 0             | -       |                            | Applicable when `maintain_stock` = True        |
| branches.`maximum_stock_quantity` | Double    | -        | 0             | -       |                            |                                                |
| branches.`start_date`             | Timestamp | –        | –             | –       | –                          |                                                |
| branches.`end_date`               | Timestamp | Yes      | –             | –       | –                          |                                                |
| branches.`reason_end`             | String    | Yes      | –             | –       | –                          |                                                |
| `attachments`                     | Array Map | Yes      | –             | –       | `Attachment` Map           | Branch Documents                               |
| `stage`                           | Map       | –        | –             | –       | `Stage` Map                | Current Stage                                  |
| `stage_logs`                      | Array Map | Yes      | –             | –       | `Stage` Map                | Workflow History                               |
| `notes`                           | Array Map | -        | -             | -       | `Note` Map                 | It is an array of Note map                     |

## Collection: m_product_variety

| Name                     | Type      | Optional | Default Value | Key     | Reference                | Remarks                    |
|:------------------------ |:--------- |:-------- |:------------- |:------- |:------------------------ |:-------------------------- |
| `id`                     | String    | –        | –             | Primary | –                        | Document Id                |
| `product_id`             | String    | -        | -             | Foreign | m_product.`id`           |                            |
| `product_attribute_1_id` | String    | -        | -             | Foreign | m_product_attribute.`id` |                            |
| `product_attribute_2_id` | String    | -        | -             | Foreign | m_product_attribute.`id` |                            |
| `product_attribute_3_id` | String    | -        | -             | Foreign | m_product_attribute.`id` |                            |
| `product_attribute_4_id` | String    | -        | -             | Foreign | m_product_attribute.`id` |                            |
| `product_attribute_5_id` | String    | -        | -             | Foreign | m_product_attribute.`id` |                            |
| `product_attribute_6_id` | String    | -        | -             | Foreign | m_product_attribute.`id` |                            |
| `display_name`           | String    | –        | –             | –       | –                        |                            |
| `description`            | String    | Yes      |               | -       |                          |                            |
| `start_date`             | Timestamp | –        | –             | –       | –                        |                            |
| `end_date`               | Timestamp | Yes      | –             | –       | –                        |                            |
| `reason_end`             | String    | Yes      | –             | –       | –                        |                            |
| `attachments`            | Array Map | Yes      | –             | –       | `Attachment` Map         | Branch Documents           |
| `stage`                  | Map       | –        | –             | –       | `Stage` Map              | Current Stage              |
| `stage_logs`             | Array Map | Yes      | –             | –       | `Stage` Map              | Workflow History           |
| `notes`                  | Array Map | -        | -             | -       | `Note` Map               | It is an array of Note map |

## Collection: m_product_pack

| Name                              | Type      | Optional | Default Value | Key     | Reference              | Remarks                                                                                  |
|:--------------------------------- |:--------- |:-------- |:------------- |:------- |:---------------------- |:---------------------------------------------------------------------------------------- |
| `id`                              | String    | –        | –             | Primary | –                      | Document Id                                                                              |
| `product_id`                      | String    | -        | -             | Foreign | m_product.`id`         |                                                                                          |
| `product_variety_id`              | String    | -        | -             | Foreign | m_product_variety.`id` |                                                                                          |
| `name`                            | String    | –        | –             | –       | –                      |                                                                                          |
| `short_name`                      | String    | –        | –             | –       | –                      |                                                                                          |
| `display_name`                    | String    | –        | –             | –       | –                      |                                                                                          |
| `sequence`                        | Int64     | -        | -             | -       |                        |                                                                                          |
| `is_loose`                        | Boolean   | -        | 0             |         |                        |                                                                                          |
| `rate_uom_id`                     | String    | -        | -             | Foreign | m_uom.`id`             | Sale/Purchase Rate is defined in this                                                    |
| `uom_alias`                       | String    | Yes      | -             | -       |                        | E.g. Pack, Pouch, Polypack, Bottle, Box, Drum, Carton To be used in Reports, Invoice etc |
| `content_quantity`                | Double    | -        | 0             | -       |                        | 0 in case of loose                                                                       |
| `content_uom_id`                  | String    | -        |               | Foreign | m_uom.`id`             |                                                                                          |
| `quantity_in_rate_uom`            | Double    | -        | 0             | -       |                        | Calculated quantity of 1 pack in rate uom. If rate uom is Nos, it will be 1.             |
| `product_level_quantity`          | Double    | -        | 0             | -       |                        |                                                                                          |
| `product_level_uom_id`            | String    | -        |               | Foreign | m_uom.`id`             | Use it from tbl_product. Disabled.                                                       |
| `minimum_stock_quantity`          | Double    | -        | 0             | -       |                        | In Rate Unit ID                                                                          |
| `maximum_stock_quantity`          | Double    | -        | 0             | -       |                        | In Rate Unit ID                                                                          |
| `reorder_level_stock_quantity`    | Double    | -        | 0             | -       |                        | In Rate Unit ID                                                                          |
| `purchase_minimum_order_quantity` | Double    | -        | 0             | -       |                        | In Rate Unit ID                                                                          |
| `purchase_maximum_order_quantity` | Double    | -        | 0             | -       |                        | In Rate Unit ID                                                                          |
| `purchase_minimum_delivery_days`  | Double    | -        | 0             | -       |                        | In Rate Unit ID                                                                          |
| `sales_minimum_order_quantity`    | Double    | -        | 0             | -       |                        | In Rate Unit ID                                                                          |
| `sales_maximum_order_quantity`    | Double    | -        | 0             | -       |                        | In Rate Unit ID                                                                          |
| `sales_minimum_delivery_days`     | Double    | -        | 0             | -       |                        | In Rate Unit ID                                                                          |
| `start_date`                      | Timestamp | –        | –             | –       | –                      |                                                                                          |
| `end_date`                        | Timestamp | Yes      | –             | –       | –                      |                                                                                          |
| `reason_end`                      | String    | Yes      | –             | –       | –                      |                                                                                          |
| `attachments`                     | Array Map | Yes      | –             | –       | `Attachment` Map       | Branch Documents                                                                         |
| `stage`                           | Map       | –        | –             | –       | `Stage` Map            | Current Stage                                                                            |
| `stage_logs`                      | Array Map | Yes      | –             | –       | `Stage` Map            | Workflow History                                                                         |
| `notes`                           | Array Map | -        | -             | -       | `Note` Map             | It is an array of Note map                                                               |

## Collection: m_production_process

| Name                        | Type      | Optional | Default Value | Key     | Reference              | Remarks                                                |
|:--------------------------- |:--------- |:-------- |:------------- |:------- |:---------------------- |:------------------------------------------------------ |
| `id`                        | String    | –        | –             | Primary | –                      | Document Id                                            |
| `name`                      | String    | -        | -             | -       | -                      | -                                                      |
| `product_id`                | String    | -        | -             | Foreign | m_product.`id`         | Main output product of this production process         |
| `product_variety_id`        | String    | Yes      | -             | Foreign | m_product_variety.`id` | Main output Product Variety of this production process |
| `product_pack_id`           | String    | Yes      | -             | Foreign | m_product_pack.`id`    | Main output Product Pack of this production process    |
| `is_primary`                | Boolean   | -        | False         | -       | -                      | Is it primary process                                  |
| `input`                     | Array Map | -        | -             | -       | -                      | Array having ingredients detail                        |
| input.`product_id`          | String    | -        | -             | Foreign | m_product.`id`         | Product being used                                     |
| input.`product_variety_id`  | String    | Yes      | -             | Foreign | m_product_variety.`id` | Product Variety being used                             |
| input.`product_pack_id`     | String    | Yes      | -             | Foreign | m_product_pack.`id`    | Product Pack being used                                |
| input.`quantity`            | Double    | -        | 0             | -       | -                      | Standard quantity used                                 |
| input.`quantity_min`        | Double    | -        | 0             | -       | -                      | Minimum quantity used                                  |
| input.`quantity_max`        | Double    | -        | 0             | -       | -                      | Maximum quantity used                                  |
| input.`quantity_additional` | Double    | -        | 0             | -       | -                      | In some cases company fills in some additional qty.    |
| input.`uom_id`              | String    | -        | -             | Foreign | m_uom_`id`             | -                                                      |
| `output`                    | Array Map | -        | -             | -       | -                      | Array having output product details                    |
| output.`product_id`         | String    | -        | -             | Foreign | m_product.`id`         | Product produced                                       |
| output.`product_variety_id` | String    | Yes      | -             | Foreign | m_product_variety.`id` | Product Variety produced                               |
| output.`product_pack_id`    | String    | Yes      | -             | Foreign | m_product_pack.`id`    | Product Pack produced                                  |
| output.`quantity`           | Double    | -        | 0             | -       | -                      | Standard quantity produced                             |
| output.`quantity_min`       | Double    | -        | 0             | -       | -                      | Minimum quantity produced                              |
| output.`quantity_max`       | Double    | -        | 0             | -       | -                      | Maximum quantity produced                              |
| output.`uom_id`             | String    | -        | -             | Foreign | m_uom_`id`             | -                                                      |
| `start_date`                | Timestamp | –        | –             | –       | –                      |                                                        |
| `end_date`                  | Timestamp | Yes      | –             | –       | –                      |                                                        |
| `reason_end`                | String    | Yes      | –             | –       | –                      |                                                        |
| `stage`                     | Map       | –        | –             | –       | `Stage` Map            | Current Stage                                          |
| `stage_logs`                | Array Map | Yes      | –             | –       | `Stage` Map            | Workflow History                                       |
| `notes`                     | Array Map | -        | -             | -       | `Note` Map             | It is an array of Note map                             |

## Collection: m_product_party

| Name                           | Type      | Optional | Default Value | Key     | Reference              | Remarks                              |
| ------------------------------ | --------- | -------- | ------------- | ------- | ---------------------- | ------------------------------------ |
| `id`                           | String    | -        | -             | Primary | -                      | Document Id                          |
| `product_id`                   | String    | -        | -             | Foreign | m_product.`id`         | -                                    |
| `product_variety_id`           | String    | -        | -             | Foreign | m_product_variety.`id` | -                                    |
| `product_pack_id`              | String    | Yes      | -             | Foreign | m_product_pack.`id`    | -                                    |
| `party_id`                     | String    | -        | -             | Foreign | m_party.`id`           | -                                    |
| `name_at_party`                | String    | Yes      | -             | -       | -                      | Product Name used by Party, if any   |
| `code_at_party`                | String    | Yes      | -             | -       | -                      | Code used by Party, if any           |
| `barcode_at_party`             | String    | Yes      | -             | -       | -                      | Unique barcode used by party, if any |
| `minimum_order_quantity`       | Number    | -        | 0             | -       | -                      | -                                    |
| `maximum_order_quantity`       | Number    | -        | 0             | -       | -                      | -                                    |
| `minimum_delivery_period_days` | Number    | -        | 0             | -       | -                      | -                                    |
| `start_date`                   | Timestamp | -        | -             | -       | -                      | -                                    |
| `end_date`                     | Timestamp | Yes      | -             | -       | -                      | -                                    |
| `reason_end`                   | String    | Yes      | -             | -       | -                      | -                                    |
| `stage`                        | Map       | –        | –             | –       | `Stage` Map            | Current Stage                        |
| `stage_logs`                   | Array Map | Yes      | –             | –       | `Stage` Map            | Workflow History                     |
| `notes`                        | Array Map | -        | -             | -       | `Note` Map             | It is an array of Note map           |

## Collection: m_product_rate_list

| Name               | Type      | Optional | Default Value | Key     | Reference                  | Remarks                        |
| ------------------ | --------- | -------- | ------------- | ------- | -------------------------- | ------------------------------ |
| `id`               | String    | -        | -             | Primary | -                          | Document Id                    |
| `name`             | String    | -        | -             | -       | -                          | -                              |
| `rate_applies_to`  | String    | -        | -             | -       | -                          | Sale, Purchase, Stock Transfer |
| `rate_category`    | String    | -        | -             | -       | s_category.`rate_category` | MRP, Rate                      |
| `organisation_ids` | Array     | Yes      | -             | Foreign | m_organisation.`id`        | Shourld we keep it as array?   |
| `branch_ids`       | Array     | Yes      | -             | Foreign | m_branch.`id`              | Shourld we keep it as array?   |
| `party_ids`        | Array     | -        |               | Foreign | m_party.`id`               | -                              |
| `start_date`       | Timestamp | -        | -             | -       | -                          | -                              |
| `end_date`         | Timestamp | Yes      | -             | -       | -                          | -                              |
| `reason_end`       | String    | Yes      | -             | -       | -                          | -                              |
| `stage`            | Map       | –        | –             | –       | `Stage` Map                | Current Stage                  |
| `stage_logs`       | Array Map | Yes      | –             | –       | `Stage` Map                | Workflow History               |
| `notes`            | Array Map | -        | -             | -       | `Note` Map                 | It is an array of Note map     |

## Collection: m_product_rate_formula

| Name                 | Type      | Optional | Default Value | Key     | Reference                | Remarks                        |
| -------------------- | --------- | -------- | ------------- | ------- | ------------------------ | ------------------------------ |
| `id`                 | String    | -        | -             | Primary | -                        | Document Id                    |
| `rate_list_id`       | String    | -        | -             | Foreign | m_product_rate_list.`id` | -                              |
| `product_id`         | String    | -        | -             | Foreign | m_product.`id`           | -                              |
| `product_variety_id` | String    | -        | -             | Foreign | m_product_variety.`id`   | -                              |
| `product_pack_id`    | String    | -        | -             | Foreign | m_product_pack.`id`      | -                              |
| `applicable_from`    | Timestamp | -        | Today         | -       | -                        | -                              |
| `applicable_upto`    | Timestamp | Yes      | -             | -       | -                        | -                              |
| `rate`               | String    | -        | -             | -       | -                        | It can be a value or a formula |
| `stage`              | Map       | –        | –             | –       | `Stage` Map              | Current Stage                  |

## Collection: m_product_rate

This collection will have final rates of products. User will not make any direct entry in this collection. This table will be populated when rates are approved in rate list for a given period, party, product etc. 

| Name                 | Type      | Optional | Default Value | Key     | Reference              | Remarks                        |
| -------------------- | --------- | -------- | ------------- | ------- | ---------------------- | ------------------------------ |
| `id`                 | String    | -        | -             | Primary | -                      | Document Id                    |
| `rate_applies_to`    | String    | -        | -             | -       | -                      | Sale, Purchase, Stock Transfer |
| `organisation_id`    | String    | Yes      | -             | Foreign | m_organisation.`id`    | Shourld we keep it as array?   |
| `branch_id`          | String    | Yes      | -             | Foreign | m_branch.`id`          | Shourld we keep it as array?   |
| `party_id`           | String    | Yes      |               | Foreign | m_party.`id`           | -                              |
| `product_id`         | String    | -        | -             | Foreign | m_product.`id`         | -                              |
| `product_variety_id` | String    | -        | -             | Foreign | m_product_variety.`id` | -                              |
| `product_pack_id`    | String    | -        | -             | Foreign | m_product_pack.`id`    | -                              |
| `rate_mrp`           | Double    | -        | -             | -       | -                      |                                |
| `rate`               | Double    | -        | -             | -       | -                      |                                |
| `applicable_from`    | Timestamp | -        | -             | -       | -                      | -                              |
| `applicable_upto`    | Timestamp | Yes      | -             | -       | -                      | -                              |

## Collection: m_uom

| Name                       | Type      | Optional | Default Value | Key     | Reference        | Remarks                                                                                                                                     |
|:-------------------------- |:--------- |:-------- |:------------- |:------- |:---------------- |:------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                       | String    | –        | –             | Primary | –                | Document Id                                                                                                                                 |
| `name`                     | String    | –        | –             | –       | –                |                                                                                                                                             |
| `short_name`               | String    | –        | –             | –       | –                |                                                                                                                                             |
| `uom_category`             | String    | –        | –             | –       | s_category.`uom` | Weight/Volumne/Length etc                                                                                                                   |
| `decimal_place`            | Int64     | –        | –             | –       | –                | Number of Decimal Places to maintain for this UoM                                                                                           |
| `format`                   | String    | –        | –             | –       | –                | Text used to format the quantity using this UoM                                                                                             |
| `stage`                    | Map       | –        | –             | –       | `Stage` Map      | Current Stage                                                                                                                               |
| `stage_logs`               | Array Map | Yes      | –             | –       | `Stage` Map      | Workflow History                                                                                                                            |
| `notes`                    | Array Map | -        | -             | -       | `Note` Map       | It is an array of Note map                                                                                                                  |
| `conversion`               | Array Map | Yes      | -             | -       | -                | Keeps factor to convert quantity from this UoM to another UoM                                                                               |
| conversion.`target_uom_id` | String    | -        | -             | Foreign | m_uom.`id`       | Target UoM, in which quantity is to be converted.                                                                                           |
| conversion.`product_id`    | String    | Yes      | -             | Foreign | m_product.`id`   | Specify product, if the factor is applicable for a product only. If product is not given, this factor would be applicable for all products. |
| conversion.`factor`        | Double    | -        | 0             | -       | -                | Factor. Multiply  quantity in source UoM with this factor to get quantity in target UoM.                                                    |

## Collection: m_product_attribute

| Name            | Type      | Optional | Default Value | Key     | Reference      | Remarks                                               |
|:--------------- |:--------- |:-------- |:------------- |:------- |:-------------- |:----------------------------------------------------- |
| `id`            | String    | –        | –             | Primary | –              | Document Id                                           |
| `name`          | String    | –        | –             | –       | –              |                                                       |
| `short_name`    | String    | –        | –             | –       | –              |                                                       |
| `sequence`      | Int64     | -        | max() + 1     | -       | -              | Sequence to be used while display                     |
| `stage`         | Map       | –        | –             | –       | `Stage` Map    | Current Stage                                         |
| `stage_logs`    | Array Map | Yes      | –             | –       | `Stage` Map    | Workflow History                                      |
| `notes`         | Array Map | -        | -             | -       | `Note` Map     | It is an array of Note map                            |
| `applicability` | Array     | Yes      | -             | Foreign | m_product.`id` | Ids of products to which this attribute is applicable |

## Collection: m_product_attribute_value

| Name                          | Type      | Optional | Default Value | Key     | Reference                | Remarks                           |
|:----------------------------- |:--------- |:-------- |:------------- |:------- |:------------------------ |:--------------------------------- |
| `id`                          | String    | –        | –             | Primary | –                        | Document Id                       |
| `product_attribute_id`        | String    | -        | -             | Foreign | m_product_attribute.`id` | -                                 |
| `value`                       | String    | -        | -             | Unique  | -                        | Unique in given Product Attribute |
| `parent_product_attribute_id` | String    | -        | -             | Foreign | m_product_attribute.`id` | Creates Hierarchy                 |
| `start_date`                  | Timestamp | –        | –             | –       | –                        |                                   |
| `end_date`                    | Timestamp | Yes      | –             | –       | –                        |                                   |
| `reason_end`                  | String    | Yes      | –             | –       | –                        |                                   |
| `stage`                       | Map       | –        | –             | –       | `Stage` Map              | Current Stage                     |
| `stage_logs`                  | Array Map | Yes      | –             | –       | `Stage` Map              | Workflow History                  |
| `notes`                       | Array Map | -        | -             | -       | `Note` Map               | It is an array of Note map        |
