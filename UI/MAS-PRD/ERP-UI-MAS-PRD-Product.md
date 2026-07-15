#  Product  UI/UX Documentation

This document provides UI details of all pages used in the ***MAS-PRD's*** Product  module.

# Related Reference files

specs file : [ERP-DES-PRD-Product](/specs/ERP-DES-PRD-Product.md)

---

# Pages

| S. No. | Name             | Type | Purpose                                                                                  |
| ------ | ---------------- | ---- | ----------------------------------------------------------------------------------------- |
| 1.     | Product List     | Page | - View all products<br/>- Search/filter products<br/>- Perform quick actions on product(s) |
| 2.     | Add/Modify Product | Page | - View details of the product<br/>- Add/modify product data                             |

---

# 1. Product  List Page UI 

This page displays a list of Product   in tabular format.

---

### 1.1a. DataTable (Product  List) - Columns

| Header        | Data Source                   | Format      | On Click              | Card Placement | Tooltip/On Hover | Inline Edit Component |
| ------------- | ----------------------------- | ----------- | --------------------- | -------------- | ---------------- | --------------------- |
| Product Image | s_prd_product.`primary_image` | Image       | Zoom Image            | Image          | –                | -                     |
| Product Group | s_prd_product_group.`name`    | String      | –                     | Body           | –                | -                     |
| Name          | s_prd_product.`name`          | String      | Open View/Modify Page | Title          | –                | -                     |
| Short Name    | s_prd_product.`short_name`    | String      | –                     | Subtitle       | –                | -                     |
| UoM           | s_prd_product_uom.`name`      | String      | –                     | Body           | –                | -                     |
| Brand         | s_prd_product_brand.`name`    | String      | –                     | Body           | –                | -                     |
| Usage         | s_prd_product.`usage`         | String      | –                     | Body           | –                | -                     |
| Start Date    | s_prd_product.`start_date`    | Date        | –                     | Body           | –                | -                     |
| Stage         | s_prd_product_stage.`name`    | Stage Badge | –                     | Body           | –                | -                     |

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
| Add              | when  s_app_role_permission.`can_add = true` for Product. | Page to Open: `product`                                    |

---

### 1.1c. DataTable (Product  List) - Table Config

| Feature        | Settings             |
| -------------- | -------------------- |
| Row Selection  | No                   |
| Bulk Actions   | No                   |
| Sticky Header  | Yes                  |
| Column Resize  | Yes                  |
| Column Pinning | Yes                  |
| Sorting        | Yes                  |
| Pagination     | Yes<br>Page Size: 20 |

---

### 1.1d. DataTable (Product  List) - RowAction Menu

| Name           | Action                                                  | Visibility Criteria                                         | Icon   | Tooltip |
| -------------- | ------------------------------------------------------- | ----------------------------------------------------------- | ------ | ------- |
| Modify         | Open page `Product detail page`                         | - Modify permission<br>- Modify allowed on status           | pencil | -       |
| Stage_1        | Set status to Stage_1                                   | - Allowed status based on current status                    | -      | -       |
| Stage_2        | Set status to Stage_2                                   | - Allowed status based on current status                    | -      | -       |
| Undo Stage     | Move back to last Stage                                 | - Permission to set current stage                           | undo   | -       |
| Mark as Closed | - Ask end/close date<br>- Sets s_prd_product.`end_date` | - Modify permission<br>- If s_prd_product.`end_date` = Null | lock   | -       |
| Mark as Opened | - Sets s_prd_product.`end_date` = Null                  | - Modify permission<br/>- s_prd_product.`end_date` <> Null  | unlock | -       |
| Delete         | - Show confirmation message<br>- On Yes, delete         | - Delete permission<br>- Delete allowed on status           | trash  | -       |

---

### 1.1e. DataTable (Product  List) - Filters Fields

| Name          | Component    | Depends On | Possible Values    | Default Values | Output            | Required | Tooltip |
| ------------- | ------------ | ---------- | ------------------ | -------------- | ----------------- | -------- | ------- |
| Stage         | Multi Select | –          | Possible Stages    | All Open Stage | stage_ids         | -        | -       |
| Product Group | Multi Select | –          | All Product Groups | All            | product_group_ids | -        | -       |

---

# 2. Product  Entry Page UI

Fields in the main form are grouped into different sections.

## Main Form Sections

```
2.1. Left: Main Form
- Basic Details
- Functions
- Inventory Management
- Media
- Packs

2.2. Right: Images and Summary
- Images
- System Information
```


---

## 2.1 Left Section

### 2.1.1 Basic Details

| Name          | Data Source                      | Component   | Component Specific Information                                                                            | Required | Read Only | Validations        | On Change             | Description                                 | Tooltip |
| ------------- | -------------------------------- | ----------- | --------------------------------------------------------------------------------------------------------- | -------- | --------- | ------------------ | --------------------- | ------------------------------------------- | ------- |
| Name          | s_prd_product.`name`             | Text        | -                                                                                                         | Yes      | No        | Length: 3-100      | –                     | Name of the Product                         | –       |
| Short Name    | s_prd_product.`short_name`       | Text        | -                                                                                                         | Yes      | No        | Length: 2-25       | –                     | Code/Short Name of the Product              | –       |
| Brand         | s_prd_product.`brand_id`         | Dropdown    | -                                                                                                         | No       | No        | –                  | –                     | Brand associated with this Product          | -       |
| Product Group | s_prd_product.`product_group_id` | Dropdown    | -                                                                                                         | Yes      | No        | –                  | Update available UoMs | Product Group to which this Product belongs | -       |
| UoM           | s_prd_product.`uom_id`           | Dropdown    | Unit of Measurement for this Product. It may be different from the UoM of the corresponding Product Group | Yes      | No        | –                  | –                     | UoM associated with this Product            | -       |
| HSN Code      | s_prd_product.`hsn_code`         | Text        | -                                                                                                         | No       | No        | Length: 3-50       | –                     | HSN Code as per GST                         | -       |
| Description   | s_prd_product.`description`      | Text Area   | Multi-line                                                                                                | No       | No        | –                  | –                     | Product Description                         | -       |
| Start Date    | s_prd_product.`start_date`       | Date Picker | DD/MM/YYYY                                                                                                | Yes      | No        | Only Date          | –                     | Date when the Product was started/launched  | -       |
| End Date      | s_prd_product.`end_date`         | Date Picker | DD/MM/YYYY                                                                                                | No       | No        | Only Date          | –                     | Date when the Product was closed            | -       |
| Remarks       | s_prd_product.`remarks`          | Text Area   | Multi-line                                                                                                | No       | No        | Max 255 Characters | –                     | Additional Notes                            | -       |

---

### 2.1.2 Functions


| Name                    | Data Source                                  | Component | Component Specific Information | Required | Read Only | Validations   | On Change                               | Description                                                                                                                     | Tooltip                   |
| ----------------------- | -------------------------------------------- | --------- | ------------------------------ | -------- | --------- | ------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------- |
| Do You Sale             | s_prd_product.`is_sale`                      | Checkox   | -                              | Yes      | No        | –             | Enable/Disable Sale Configuration       | Indicates whether this Product is available for sale. Normally finished goods are marked for sale.                              | -              |
| Minimum Order Quantity  | s_prd_product.`sale_min_order_qty`           | Number    | Positive Integer               | No       | No        | Numeric Value | –                                       | Minimum quantity required in a single sale order                                                                                | -     |
| Maximum Order Quantity  | s_prd_product.`sale_max_order_qty`           | Number    | Positive Integer               | No       | No        | Numeric Value | –                                       | Maximum quantity allowed in a single sale order                                                                                 | -     |
| Minimum Delivery Period | s_prd_product.`sale_min_delivery_period`     | Number    | Number of Days                 | No       | No        | Numeric Value | –                                       | Minimum number of days required to deliver this Product for sale                                                                | -      |
| Do You Purchase         | s_prd_product.`is_purchase`                  | Checkbox  | -                              | Yes      | No        | –             | Enable/Disable Purchase Configuration   | Indicates whether this Product is available for purchase. Normally Raw Materials and Packing Materials are marked for purchase. | -          |
| Minimum Order Quantity  | s_prd_product.`purchase_min_order_qty`       | Number    | Positive Integer               | No       | No        | Numeric Value | –                                       | Minimum quantity required in a single purchase order                                                                            | -|
| Maximum Order Quantity  | s_prd_product.`purchase_max_order_qty`       | Number    | Positive Integer               | No       | No        | Numeric Value | –                                       | Maximum quantity allowed in a single purchase order                                                                             | -|
| Minimum Delivery Period | s_prd_product.`purchase_min_delivery_period` | Number    | Number of Days                 | No       | No        | Numeric Value | –                                       | Minimum number of days required to receive this Product after purchase                                                          | -  |
| Do You Produce          | s_prd_product.`is_produce`                   | Checkbox  | Yes / No                       | Yes      | No        | –             | Enable/Disable Production Configuration | Indicates whether this Product is produced by the company. Normally Finished and Semi-Finished Goods are produced.              | -          |

---

### 2.1.3 Inventory Management


| Name                   | Data Source                       | Component    | Component Specific Information | Required | Read Only | Validations   | On Change | Description                                                     | Tooltip                |
| ---------------------- | --------------------------------- | ------------ | ------------------------------ | -------- | --------- | ------------- | --------- | --------------------------------------------------------------- | ---------------------- |
| Is Goods?              | s_prd_product.`is_goods_type`     | Radio Button | -                              | Yes      | No        | –             | –         | Indicates whether the Product is a Goods item or a Service      | -             |
| Maintain Stock?        | s_prd_product.`maintain_stock`    | Radio Button | -                              | Yes      | No        | –             | –         | Indicates whether stock is maintained for this Product          | -         |
| Maintain Batch?        | s_prd_product.`maintain_batch`    | Radio Button | -                              | Yes      | No        | –             | –         | Indicates whether Batch Numbers are maintained for this Product | -       |
| Maintain Expiry?       | s_prd_product.`maintain_expiry`   | Radio Button | -                              | Yes      | No        | –             | –         | Indicates whether Expiry Dates are maintained for this Product  | -      |
| Minimum Stock Quantity | s_prd_product.`min_stock_qty`     | Number       | Positive Integer               | No       | No        | Numeric Value | –         | Minimum stock quantity to be maintained                         | - |
| Maximum Stock Quantity | s_prd_product.`max_stock_qty`     | Number       | Positive Integer               | No       | No        | Numeric Value | –         | Maximum stock quantity allowed                                  | - |
| Reorder Stock Quantity | s_prd_product.`reorder_stock_qty` | Number       | Positive Integer               | No       | No        | Numeric Value | –         | Stock quantity at which the Product should be reordered         | - |

---

### 2.1.4a. DataTable (Media) - Columns

| Header      | Data Source                            | Format  | On Click   | Card Placement | Tooltip / On Hover                          | Inline Edit Component                       |
| ----------- | -------------------------------------- | ------- | ---------- | -------------- | ------------------------------------------- | ------------------------------------------- |
| Media Class | s_prd_product_media[].`media_class_id` | String  | –          | -              | Classification of Media                     | Dropdown<br/>Options: List of Media Classes |
| Name        | s_prd_product_media[].`name`           | String  | –          | -              | -                                           | Textbox<br/>Length: 3-100                   |
| Description | s_prd_product_media[].`description`    | String  | –          | -              | -                                           | Text Area<br/>Multi-line                    |
| Filename    | s_prd_product_media[].`file_name`      | String  | –          | -              | -                                           | –                                           |
| Image       | s_prd_product_media[].`image_url`      | Image   | Zoom Image | -              | -                                           | -                                           |
| Is Primary  | s_prd_product_media[].`is_primary`     | -       | –          | -              | Indicates whether this is the Primary Media | Checkbox                                    |
| Sequence    | s_prd_product_media[].`sequence`       | Integer | –          | -              | Display Order of Media                      | Number<br/>Positive Integer                 |
| Start Date  | s_prd_product_media[].`start_date`     | Date    | –          | -              | -                                           | Date Picker<br/>DD/MM/YYYY                  |
| End Date    | s_prd_product_media[].`end_date`       | Date    | –          | -              | -                                           | Date Picker<br/>DD/MM/YYYY                  |

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

### 2.1.4c. DataTable (Media) - Table Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | No       |
| Pagination     | No       |

### 2.1.4d DataTable (Media) - RowAction Menu

| Name | Action | Visibility Criteria | Icon | Tooltip |
| ---- | ------ | ------------------- | ----- | ------- |
| -    | -      | -                   | -     | -       |

### 2.1.4e. DataTable (Media) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Required | Tooltip |
| ---- | --------- | ---------- | --------------- | -------------- | ------ | -------- | ------- |
| -    | -         | –          | -               | -              | -      | -        | -       |


---

### 2.1.5a. DataTable (Packs) - Columns

| Header             | Data Source                               | Format  | On Click | Card Placement | Tooltip / On Hover                              | Inline Edit Component                   |
| ------------------ | ----------------------------------------- | ------- | -------- | -------------- | ----------------------------------------------- | --------------------------------------- |
| Name               | s_prd_product_pack[].`name`               | String  | –        | -              | Name of the Product Pack                        | Textbox<br/>Length: 3-100               |
| Short Name         | s_prd_product_pack[].`short_name`         | String  | –        | -              | Code/Short Name of the Product Pack             | Textbox<br/>Length: 2-25                |
| UoM                | s_prd_product_pack[].`uom_id`             | String  | –        | -              | Standard UoM for this Product Pack              | Dropdown<br/>Select Unit of Measurement |
| HSN Code           | s_prd_product_pack[].`hsn_code`           | String  | –        | -              | HSN Code as per GST                             | Textbox<br/>Length: 3-50                |
| Qty in Pack        | s_prd_product_pack[].`qty_in_pack`        | Decimal | –        | -              | Quantity and UoM of the content of the Pack     | Number<br/>Decimal Value                |
| Qty in Product UoM | s_prd_product_pack[].`qty_in_product_uom` | Decimal | –        | -              | Equivalent quantity in Product UoM              | Number<br/>Decimal Value                |
| Start Date         | s_prd_product_pack[].`start_date`         | Date    | –        | -              | Date when the Product Pack was started/launched | Date Picker<br/>DD/MM/YYYY              |
| Remarks            | s_prd_product_pack[].`remarks`            | String  | –        | -              | Additional Notes                                | Text Area<br/>Multi-line                |

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

### 2.1.5c. DataTable (Packs) - Table Config

| Feature        | Settings |
| -------------- | -------- |
| Row Selection  | No       |
| Bulk Actions   | No       |
| Sticky Header  | Yes      |
| Column Resize  | Yes      |
| Column Pinning | Yes      |
| Sorting        | No       |
| Pagination     | No       |

### 2.1.5d DataTable (Packs) - RowAction Menu

| Name | Action | Visibility Criteria | Icon | Tooltip |
| ---- | ------ | ------------------- | ----- | ------- |
| -    | -      | -                   | -     | -       |


### 2.1.5e. DataTable (Packs) - Filters Fields

| Name | Component | Depends On | Possible Values | Default Values | Output | Required | Tooltip |
| ---- | --------- | ---------- | --------------- | -------------- | ------ | -------- | ------- |
| -    | -         | –          | -               | -              | -      | -        | -       |

---

## 2.2 Right Section

### 2.2.1 Images

| Name          | Data Source                   | Component             | Component Specific Information     | Required | Read Only | Validations                                                            | On Change               | Description                                   | Tooltip |
| ------------- | ----------------------------- | --------------------- | ---------------------------------- | -------- | --------- | ---------------------------------------------------------------------- | ----------------------- | --------------------------------------------- | ------- |
| Primary Image | s_prd_product.`primary_image` | Image Upload          | Single Image Upload with Preview   | No       | No        | - Supported Image Formats (JPG/PNG/JPEG) <br>- Maximum File Size: 2 MB | Preview Uploaded Image  | Primary image of the Product                  | -       |
| Other Images  | s_prd_product.`other_images`  | Multiple Image Upload | Multiple Image Upload with Preview | No       | No        | - Supported Image Formats (JPG/PNG/JPEG) <br>- Maximum File Size: 2 MB | Preview Uploaded Images | Additional images associated with the Product | -       |

---


### 2.2.2 System Information

| Name        | Data Source      | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                                       | Tooltip |
| ----------- | ---------------- | --------- | ------------------------------ | -------- | --------- | ----------- | --------- | ------------------------------------------------- | ------- |
| Created By  | System Generated | Text      | User Name                      | No       | Yes       | –           | –         | User who created the Product                      | -       |
| Created On  | System Generated | Date Time | DD/MM/YYYY HH:mm               | No       | Yes       | –           | –         | Date and Time when the Product  was created       | -       |
| Modified By | System Generated | Text      | User Name                      | No       | Yes       | –           | –         | User who last modified the Product                | -       |
| Modified On | System Generated | Date Time | DD/MM/YYYY HH:mm               | No       | Yes       | –           | –         | Date and Time when the Product  was last modified | -       |
