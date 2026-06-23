#  Product  UI/UX Documentation

This document provides UI details of all pages used in the ***MAS-PRD's*** Product  module.

---

# 1. Product  List Page UI 

This page displays a list of Product   in tabular format.

---

### 1.1a. DataTable (Product  List) - Columns

| Name          | Content                       | Format      | On Click              | Card Placement | Tooltip/On Hover |
| ------------- | ----------------------------- | ----------- | --------------------- | -------------- | ---------------- |
| –             | s_prd_product.`primary_image` | Image       | Zoom Image            | Thumbnail      | –                |
| Product Group | s_prd_product_group.`name`    | String      | –                     | Body           | –                |
| Name          | s_prd_product.`name`          | String      | Open View/Modify Page | Title          | –                |
| Short Name    | s_prd_product.`short_name`    | String      | –                     | Subtitle       | –                |
| UoM           | s_prd_product_uom.`name`      | String      | –                     | Body           | –                |
| Brand         | s_prd_product_brand.`name`    | String      | –                     | Body           | –                |
| Usage         | s_prd_product.`usage`         | String      | –                     | Body           | –                |
| Start         | s_prd_product.`start_date`    | Date        | –                     | Body           | –                |
| Stage         | s_prd_product_stage.`name`    | Stage Badge | –                     | Body           | –                |

---



### 1.1b. DataTable (Product  List) - Toolbar Config

| Feature          | Settings | On Click                                                                                    |
| ---------------- | -------- | ------------------------------------------------------------------------------------------- |
| Search           | Yes      | Search  records based on the entered keyword(s).                                            |
| View Toggle      | Yes      | Switch between available DataTable view layouts.                                            |
| Column Selection | Yes      | Open column selector to show or hide DataTable columns.                                     |
| Group By         | Yes      | Group DataTable records based on the selected column.                                       |
| Filter           | Yes      | Open filter panel to apply additional filtering criteria.                                   |
| Export           | Yes      | Export the displayed records.                                                               |
| Share            | No       | N/A                                                                                         |
| Full Screen      | Yes      | Toggle the DataTable between normal and full-screen view.                                   |
| Add              | Yes      | Open Add Product Page. Visible only when `s_app_role_permission.can_add = true` for Product |

---

### 1.1c. DataTable (Product  List) - Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | Yes      |
| Pagination     | Yes      |

---

### 1.1d. DataTable (Product  List) - RowAction Menu

| Name           | Action                                          | Visibility Criteria                               | Tooltip |
| -------------- | ----------------------------------------------- | ------------------------------------------------- | ------- |
| Modify         | Open page `Product detail pahe`                 | - Modify permission<br>- Modify allowed on status | -       |
| Stage_1        | Set status to Stage_1                           | - Allowed status based on current status          | -       |
| Stage_2        | Set status to Stage_2                           | - Allowed status based on current status          | -       |
| Undo Status    | Move back to last status                        | - Permission to set current status                | -       |
| Mark as Closed | - Ask end/close date<br>- Sets end date         | - Modify permission                               | -       |
| Delete         | - Show confirmation message<br>- On Yes, delete | - Delete permission<br>- Delete allowed on status | -       |

---

### 1.1e. DataTable (Product  List) - Filters Fields

| Name   | Type         | Possible Values   | Default         |
| ------ | ------------ | ----------------- | --------------- |
| Status | Multi Select | Possible statuses | All Open Status |
| Product Group | Multi Select Dropdown | All Product Groups | All |

---

# 2. Product  Entry Page UI

Fields in the main form are grouped into different sections.

## Main Form Sections

### 2.1 Left Sections

- Basic Details
- Functions
- Inventory Management
- Media
- Packs

### 2.2 Right Sections 

- Primary Image


---

### 2.1.1  Section (Basic Details)

| Name/Label    | Data Source                      | Component   | Component Specific Information                                                                            | Required | Read Only | Validations        | On Change             | Description                                 | Tooltip             |
| ------------- | -------------------------------- | ----------- | --------------------------------------------------------------------------------------------------------- | -------- | --------- | ------------------ | --------------------- | ------------------------------------------- | ------------------- |
| Name          | s_prd_product.`name`             | Text        | -                                                                                                         | Yes      | No        | Length: 3-100      | –                     | Name of the Product                         | –                   |
| Short Name    | s_prd_product.`short_name`       | Text        | -                                                                                                         | Yes      | No        | Length: 2-25       | –                     | Code/Short Name of the Product              | –                   |
| Brand         | s_prd_product.`brand_id`         | Dropdown    | -                                                                                                         | No       | No        | –                  | –                     | Brand associated with this Product          | Brand               |
| Product Group | s_prd_product.`product_group_id` | Dropdown    | -                                                                                                         | Yes      | No        | –                  | Update available UoMs | Product Group to which this Product belongs | Product Group       |
| UoM           | s_prd_product.`uom_id`           | Dropdown    | Unit of Measurement for this Product. It may be different from the UoM of the corresponding Product Group | Yes      | No        | –                  | –                     | UoM associated with this Product            | Unit of Measurement |
| HSN Code      | s_prd_product.`hsn_code`         | Text        | -                                                                                                         | No       | No        | Length: 3-50       | –                     | HSN Code as per GST                         | HSN Code            |
| Description   | s_prd_product.`description`      | Text Area   | Multi-line                                                                                                | No       | No        | –                  | –                     | Product Description                         | Description         |
| Start Date    | s_prd_product.`start_date`       | Date Picker | DD/MM/YYYY                                                                                                | Yes      | No        | Only Date          | –                     | Date when the Product was started/launched  | Effective From      |
| End Date      | s_prd_product.`end_date`         | Date Picker | DD/MM/YYYY                                                                                                | No       | No        | Only Date          | –                     | Date when the Product was closed            | Effective Until     |
| Remarks       | s_prd_product.`remarks`          | Text Area   | Multi-line                                                                                                | No       | No        | Max 255 Characters | –                     | Additional Notes                            | Remarks             |

---

### 2.1.2 Section (Functions)


| Name/Label              | Data Source                                  | Component | Component Specific Information | Required | Read Only | Validations   | On Change                               | Description                                                                                                                     | Tooltip                   |
| ----------------------- | -------------------------------------------- | --------- | ------------------------------ | -------- | --------- | ------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------- |
| Do You Sale             | s_prd_product.`is_sale`                      | Checkox   | -                              | Yes      | No        | –             | Enable/Disable Sale Configuration       | Indicates whether this Product is available for sale. Normally finished goods are marked for sale.                              | Sale Product              |
| Minimum Order Quantity  | s_prd_product.`sale_min_order_qty`           | Number    | Positive Integer               | No       | No        | Numeric Value | –                                       | Minimum quantity required in a single sale order                                                                                | Minimum Sale Quantity     |
| Maximum Order Quantity  | s_prd_product.`sale_max_order_qty`           | Number    | Positive Integer               | No       | No        | Numeric Value | –                                       | Maximum quantity allowed in a single sale order                                                                                 | Maximum Sale Quantity     |
| Minimum Delivery Period | s_prd_product.`sale_min_delivery_period`     | Number    | Number of Days                 | No       | No        | Numeric Value | –                                       | Minimum number of days required to deliver this Product for sale                                                                | Sale Delivery Period      |
| Do You Purchase         | s_prd_product.`is_purchase`                  | Checkbox  | -                              | Yes      | No        | –             | Enable/Disable Purchase Configuration   | Indicates whether this Product is available for purchase. Normally Raw Materials and Packing Materials are marked for purchase. | Purchase Product          |
| Minimum Order Quantity  | s_prd_product.`purchase_min_order_qty`       | Number    | Positive Integer               | No       | No        | Numeric Value | –                                       | Minimum quantity required in a single purchase order                                                                            | Minimum Purchase Quantity |
| Maximum Order Quantity  | s_prd_product.`purchase_max_order_qty`       | Number    | Positive Integer               | No       | No        | Numeric Value | –                                       | Maximum quantity allowed in a single purchase order                                                                             | Maximum Purchase Quantity |
| Minimum Delivery Period | s_prd_product.`purchase_min_delivery_period` | Number    | Number of Days                 | No       | No        | Numeric Value | –                                       | Minimum number of days required to receive this Product after purchase                                                          | Purchase Delivery Period  |
| Do You Produce          | s_prd_product.`is_produce`                   | Checkbox  | Yes / No                       | Yes      | No        | –             | Enable/Disable Production Configuration | Indicates whether this Product is produced by the company. Normally Finished and Semi-Finished Goods are produced.              | Produced Product          |

---

### 2.1.3 Section (Inventory Management)


| Name/Label             | Data Source                       | Component    | Component Specific Information | Required | Read Only | Validations   | On Change | Description                                                     | Tooltip                |
| ---------------------- | --------------------------------- | ------------ | ------------------------------ | -------- | --------- | ------------- | --------- | --------------------------------------------------------------- | ---------------------- |
| Is Goods?              | s_prd_product.`is_goods_type`     | Radio Button | -                              | Yes      | No        | –             | –         | Indicates whether the Product is a Goods item or a Service      | Goods Type             |
| Maintain Stock?        | s_prd_product.`maintain_stock`    | Radio Button | -                              | Yes      | No        | –             | –         | Indicates whether stock is maintained for this Product          | Maintain Stock         |
| Maintain Batch?        | s_prd_product.`maintain_batch`    | Radio Button | -                              | Yes      | No        | –             | –         | Indicates whether Batch Numbers are maintained for this Product | Batch Management       |
| Maintain Expiry?       | s_prd_product.`maintain_expiry`   | Radio Button | -                              | Yes      | No        | –             | –         | Indicates whether Expiry Dates are maintained for this Product  | Expiry Management      |
| Minimum Stock Quantity | s_prd_product.`min_stock_qty`     | Number       | Positive Integer               | No       | No        | Numeric Value | –         | Minimum stock quantity to be maintained                         | Minimum Stock Quantity |
| Maximum Stock Quantity | s_prd_product.`max_stock_qty`     | Number       | Positive Integer               | No       | No        | Numeric Value | –         | Maximum stock quantity allowed                                  | Maximum Stock Quantity |
| Reorder Stock Quantity | s_prd_product.`reorder_stock_qty` | Number       | Positive Integer               | No       | No        | Numeric Value | –         | Stock quantity at which the Product should be reordered         | Reorder Stock Quantity |

---

### 2.1.4a Section (Media)

**Component Type:** DataTable

|Name/Label|Data Source|Type/Component|Component Specific Information|Required|Read Only|Validations|On Change|Description|Tooltip|
|---|---|---|---|---|---|---|---|---|---|
|Media Class|s_prd_product_media[].`media_class_id`|Dropdown|List of Media Classes|Yes|No|–|–|Classification of Media|Media Class|
|Name|s_prd_product_media[].`name`|Textbox|Length 3-100|Yes|No|Length: 3-100|–|Name of the Media|Media Name|
|Description|s_prd_product_media[].`description`|Text Area|Multi-line|No|No|–|–|Description of the Media|Media Description|
|Filename|s_prd_product_media[].`file_name`|Text|System Generated|No|Yes|–|–|Uploaded File Name|File Name|
|Image URL|s_prd_product_media[].`image_url`|File Upload|Image Upload with Preview|Yes|No|Supported Image Formats|Preview Uploaded Image|URL/File associated with the Media|Image URL|
|Is Primary|s_prd_product_media[].`is_primary`|Checkbox|Yes / No|No|No|–|–|Indicates whether this is the Primary Media|Primary Media|
|Sequence|s_prd_product_media[].`sequence`|Number|Positive Integer|No|No|Numeric Value|–|Display Order of Media|Sequence|
|Start Date|s_prd_product_media[].`start_date`|Date Picker|DD/MM/YYYY|No|No|Only Date|–|Date from which the Media is applicable|Effective From|
|End Date|s_prd_product_media[].`end_date`|Date Picker|DD/MM/YYYY|No|No|Only Date|–|Date until which the Media is applicable|Effective Until|

---
### 2.1.4b. DataTable (Media) - Toolbar Config

| Feature          | Settings | On Click |
| ---------------- | -------- | -------- |
| Search           | No       | -        |
| View Toggle      | No       | -        |
| Column Selection | No       | -        |
| Group By         | No       | -        |
| Filter           | No       | -        |
| Export           | No       | -        |
| Share            | No       | -        |
| Full Screen      | No       | -        |

---

### 2.1.4c. DataTable (Media) - Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | No       |
| Pagination     | No       |

#### 2.1.4d DataTable (Media) - RowAction Menu

| Name | Action | Visibility Criteria | Tooltip |
| ---- | ------ | ------------------- | ------- |
| -    | -      | -                   | -       |
|      |        |                     |         |

### 2.1.4e. DataTable (Media) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
| ---- | --------- | ---------- | --------------- | -------------- | ------ | ------- |
| -    | -         | –          | -               | -              | -      | -       |


---

### 2.1.5a Section (Packs)

**Component Type:** DataTable

|Name/Label|Data Source|Type/Component|Component Specific Information|Required|Read Only|Validations|On Change|Description|Tooltip|
|---|---|---|---|---|---|---|---|---|---|
|Name|s_prd_product_pack[].`name`|Textbox|Length 3-100|Yes|No|Length: 3-100|–|Name of the Product Pack|Pack Name|
|Short Name|s_prd_product_pack[].`short_name`|Textbox|Length 2-25|Yes|No|Length: 2-25|–|Code/Short Name of the Product Pack|Pack Code|
|UoM|s_prd_product_pack[].`uom_id`|Dropdown|Select Unit of Measurement|Yes|No|–|–|Standard UoM for this Product Pack|Unit of Measurement|
|HSN Code|s_prd_product_pack[].`hsn_code`|Textbox|Length 3-50|No|No|Length: 3-50|–|HSN Code as per GST|HSN Code|
|Qty in Pack|s_prd_product_pack[].`qty_in_pack`|Number|Decimal Value|Yes|No|Numeric Value|–|Quantity and UoM of the content of the Pack|Quantity in Pack|
|Qty in Product UoM|s_prd_product_pack[].`qty_in_product_uom`|Number|Decimal Value|No|No|Numeric Value|–|Equivalent quantity in Product UoM|Product UoM Quantity|
|Start Date|s_prd_product_pack[].`start_date`|Date Picker|DD/MM/YYYY|Yes|No|Only Date|–|Date when the Product Pack was started/launched|Effective From|
|Remarks|s_prd_product_pack[].`remarks`|Text Area|Multi-line|No|No|Max 255 Characters|–|Additional Notes|Remarks|

---
### 2.1.5b. DataTable (Packs) - Toolbar Config

| Feature          | Settings | On Click |
| ---------------- | -------- | -------- |
| Search           | No       | -        |
| View Toggle      | No       | -        |
| Column Selection | No       | -        |
| Group By         | No       | -        |
| Filter           | No       | -        |
| Export           | No       | -        |
| Share            | No       | -        |
| Full Screen      | No       | -        |

---

### 2.1.5c. DataTable (Packs) - Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | No       |
| Pagination     | No       |

#### 2.1.5d DataTable (Packs) - RowAction Menu

| Name | Action | Visibility Criteria | Tooltip |
| ---- | ------ | ------------------- | ------- |
| -    | -      | -                   | -       |
|      |        |                     |         |

### 2.1.5e. DataTable (Packs) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Tooltip |
| ---- | --------- | ---------- | --------------- | -------------- | ------ | ------- |
| -    | -         | –          | -               | -              | -      | -       |

---

### 2.2.1 Section (Images)

| Name/Label    | Data Source                   | Component             | Component Specific Information     | Required | Read Only | Validations                                                            | On Change               | Description                                   | Tooltip       |
| ------------- | ----------------------------- | --------------------- | ---------------------------------- | -------- | --------- | ---------------------------------------------------------------------- | ----------------------- | --------------------------------------------- | ------------- |
| Primary Image | s_prd_product.`primary_image` | Image Upload          | Single Image Upload with Preview   | No       | No        | - Supported Image Formats (JPG/PNG/JPEG) <br>- Maximum File Size: 2 MB | Preview Uploaded Image  | Primary image of the Product                  | Primary Image |
| Other Images  | s_prd_product.`other_images`  | Multiple Image Upload | Multiple Image Upload with Preview | No       | No        | - Supported Image Formats (JPG/PNG/JPEG) <br>- Maximum File Size: 2 MB | Preview Uploaded Images | Additional images associated with the Product | Other Images  |

---


### 2.3 Section (System Information)

|Name/Label|Data Source|Component|Component Specific Information|Required|Read Only|Validations|On Change|Description|Tooltip|
|---|---|---|---|---|---|---|---|---|---|
|Created By|System Generated|Text|User Name|No|Yes|–|–|User who created the Product Attribute|Created By|
|Created On|System Generated|Date Time|DD/MM/YYYY HH:mm|No|Yes|–|–|Date and Time when the Product Attribute was created|Created On|
|Modified By|System Generated|Text|User Name|No|Yes|–|–|User who last modified the Product Attribute|Modified By|
|Modified On|System Generated|Date Time|DD/MM/YYYY HH:mm|No|Yes|–|–|Date and Time when the Product Attribute was last modified|Modified On|
