
# 1. Product Variety List Page UI

This page shows the list of Product Varieties in tabular format. The varieties appearing in the list depend upon current user's permissions and applied filters.

### 1.1a. DataTable (Product Variety List) - Columns

| Name            | Content                               | Format      | On Click              | Card Placement | Tooltip/On Hover                    |
| --------------- | ------------------------------------- | ----------- | --------------------- | -------------- | ----------------------------------- |
| –               | s_prd_product_variety.`primary_image` | Image       | Zoom Image            | Thumbnail      | Primary Image                       |
| Product         | s_prd_product.`name`                  | String      | Open View/Modify Page | Title          | –                                   |
|Attributes|Dynamic Product Attribute Values|String|–|Body|Displays configured Attribute Values for the Product Variety|
| Varieties Count | Derived Count                         | Integer     | –                     | Body           | Number of varieties for the Product |
| Start Date      | s_prd_product_variety.`start_date`    | Date        | –                     | Body           | –                                   |
| Stage           | s_prd_product_variety_stage.`name`    | Stage Badge | Show Stage History    | Body           | –                                   |

---

### 1.1b. DataTable (Product Variety List) - Toolbar Config

| Feature          | Settings | On Click                                                                                                    |
| ---------------- | -------- | ---------------------------------------------------------------------------------------------------------- |
| Search           | Yes      |                                                                                                             |
| View Toggle      | Yes                                                                                                                    |
| Column Selection | Yes                                                                                                                    |
| Group By         | Yes                                                                                                                    |
| Filter           | Yes                                                                                                                    |
| Export           | Yes                                                                                                                    |
| Share            | No                                                                                                                     |
| Full Screen      | Yes                                                                                                                    |
| Add              |  Open Page: `product_variety`. Visible only when `s_app_role_permission.can_add = true` for Product Variety r Product  |

---

### 1.1c. DataTable (Product Variety List) - Config

| Feature | Settings |
|----------|----------|
| Row Selection | Yes |
| Bulk Actions | Yes |
| Sticky Header | Yes |
| Column Resize | Yes |
| Column Pinning | Yes |
| Sorting | Yes |
| Pagination | Yes<br/>Page Size: 10 |

---

### 1.1d. DataTable (Product Variety List) - RowAction Menu

| Name | Action | Visibility Criteria | Tooltip |
|--------|---------|---------|---------|
| View | Open page: `product_variety` | View Permission | - |
| Modify | Open page: `product_variety` | Modify Permission | - |
| BoM | Open page: `product_variety_bom` | Modify Permission | Bill of Materials |
| Mark as Closed | Ask End Date and set `end_date` | Modify Permission | - |
| Delete | Delete Product Variety | Delete Permission | - |

---

### 1.1e. DataTable (Product Variety List) - Filters Fields

| Name    | Type         | Possible Values | Default     |
| ------- | ------------ | --------------- | ----------- |
| Product | Multi Select | All Products    | All         |
| Attribute   | Multi Select | Diff atribute value      | All         |
| Stage   | Multi Select | All Stages      | Open Stages |

---

# 2. Product Variety Entry Page UI

## 2.1 Main Form

### 2.1 Left Section (Basic Details)

| Name/Label | Data Source                        | Component   | Component Specific Information | Required | Read Only | Validations        | On Change               | Description                          | Tooltip    |
| ---------- | ---------------------------------- | ----------- | ------------------------------ | -------- | --------- | ------------------ | ----------------------- | ------------------------------------ | ---------- |
| Product    | s_prd_product_variety.`product_id` | Dropdown    | Select Product                 | Yes      | No        | –                  | Load Product Attributes | Product for which variety is created | Product    |
|Dynamic Attributes|s_prd_product_variety_attributes[]|Dynamic Dropdown(s)|Generated based on Product Attribute configuration|Yes|No|Attribute Value required where applicable|Reload when Product changes|Displays all applicable Product Attributes and their allowed values|Product Attributes|
| Start Date | s_prd_product_variety.`start_date` | Date Picker | DD/MM/YYYY                     | Yes      | No        | Only Date          | –                       | Effective Start Date                 | Start Date |
| Remarks    | s_prd_product_variety.`remarks`    | Text Area   | Multi-line                     | No       | No        | Max 255 Characters | –                       | Additional Notes                     | Remarks    |

---

### 2.2 Right Section (Primary Image)

| Name/Label | Data Source | Type/Component | Component Specific Information | Required | Read Only | Validations | On Change | Description | Tooltip |
|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|
| Primary Image | s_prd_product_variety.`primary_image` | Image Upload | Upload with Preview | No | No | Supported Image Formats | Preview Uploaded Image | Primary Image of Product Variety | Primary Image |


---

### 2.3 Section (System Information)

|Name/Label|Data Source|Component|Component Specific Information|Required|Read Only|Validations|On Change|Description|Tooltip|
|---|---|---|---|---|---|---|---|---|---|
|Created By|System Generated|Text|User Name|No|Yes|–|–|User who created the Product Attribute|Created By|
|Created On|System Generated|Date Time|DD/MM/YYYY HH:mm|No|Yes|–|–|Date and Time when the Product Attribute was created|Created On|
|Modified By|System Generated|Text|User Name|No|Yes|–|–|User who last modified the Product Attribute|Modified By|
|Modified On|System Generated|Date Time|DD/MM/YYYY HH:mm|No|Yes|–|–|Date and Time when the Product Attribute was last modified|Modified On|
