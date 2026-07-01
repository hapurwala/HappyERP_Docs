
# Related Reference files

specs file : -

---

# Pages

| S. No. | Name                         | Type | Purpose                                                                                                       |
| ------ | ----------------------------- | ---- | --------------------------------------------------------------------------------------------------------------- |
| 1.     | Product Variety List          | Page | - View all product varieties<br/>- Search/filter product varieties<br/>- Perform quick actions on product variety(s) |
| 2.     | Add/Modify Product Variety    | Page | - View details of the product variety<br/>- Add/modify product variety data                                    |

---

# 1. Product Variety List Page UI

This page shows the list of Product Varieties in tabular format. The varieties appearing in the list depend upon current user's permissions and applied filters.

### 1.1a. DataTable (Product Variety List) - Columns

| Header          | Data Source                            | Format      | On Click              | Card Placement | Tooltip / On Hover                    | Inline Edit Component |
| --------------- | ---------------------------------------- | ----------- | --------------------- | -------------- | ---------------------------------------- | ---------------------- |
| Image           | s_prd_product_variety.`primary_image`  | Image       | Zoom Image            | Image          | –                                          | -                      |
| Product         | s_prd_product.`name`                   | String      | Open View/Modify Page | Title          | –                                          | -                      |
| Attributes      | Dynamic Product Attribute Values       | String      | –                     | Body           | Displays configured Attribute Values for the Product Variety | -                      |
| Varieties Count | Derived Count                          | Integer     | –                     | Body           | Number of varieties for the Product       | -                      |
| Start Date      | s_prd_product_variety.`start_date`     | Date        | –                     | Body           | –                                          | -                      |
| Stage           | s_prd_product_variety_stage.`name`     | Stage Badge | Show Stage History    | Body           | –                                          | -                      |

---

### 1.1b. DataTable (Product Variety List) - Toolbar Config

| Feature          | Settings                                                                | On Click                                                   |
| ---------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------- |
| Search           | Yes                                                                      | Search records based on the entered keyword(s).             |
| View Toggle      | Yes                                                                      | Switch between available DataTable view layouts.             |
| Column Selection | Yes                                                                      | Open column selector to show or hide DataTable columns.      |
| Group By         | Yes                                                                      | Group DataTable records based on the selected column.        |
| Filter           | Yes                                                                      | Open filter panel to apply additional filtering criteria.     |
| Export           | Yes                                                                      | Export the displayed records.                                |
| Share            | No                                                                       | N/A                                                          |
| Full Screen      | Yes                                                                      | Toggle the DataTable between normal and full-screen view.    |
| Add              | when  s_app_role_permission.`can_add = true` for Product Variety.       | Page to Open: `product_variety`                              |

---

### 1.1c. DataTable (Product Variety List) - Table Config

| Feature        | Settings              |
| --------------- | ---------------------- |
| Row Selection   | No                     |
| Bulk Actions    | No                     |
| Sticky Header   | Yes                    |
| Column Resize   | Yes                    |
| Column Pinning  | Yes                    |
| Sorting         | Yes                    |
| Pagination      | Yes<br/>Page Size: 10  |

---

### 1.1d. DataTable (Product Variety List) - RowAction Menu

| Name           | Action                                                              | Visibility Criteria                                                  | Icon   | Tooltip            |
| -------------- | ---------------------------------------------------------------------- | -------------------------------------------------------------------------- | ------ | -------------------- |
| View           | Open page `product_variety`                                          | View permission                                                          | eye    | -                     |
| Modify         | Open page `product_variety`                                          | - Modify permission<br>- Modify allowed on status                        | pencil | -                     |
| BoM            | Open page `product_variety_bom`                                      | Modify permission                                                         | list   | Bill of Materials     |
| Stage_1        | Set status to Stage_1                                                 | - Allowed status based on current status                                  | -      | -                     |
| Stage_2        | Set status to Stage_2                                                 | - Allowed status based on current status                                  | -      | -                     |
| Undo Stage     | Move back to last Stage                                                | - Permission to set current stage                                         | undo   | -                     |
| Mark as Closed | - Ask end/close date<br>- Sets s_prd_product_variety.`end_date`        | - Modify permission<br>- If s_prd_product_variety.`end_date` = Null       | lock   | -                     |
| Mark as Opened | - Sets s_prd_product_variety.`end_date` = Null                        | - Modify permission<br/>- s_prd_product_variety.`end_date` <> Null        | unlock | -                     |
| Delete         | - Show confirmation message<br>- On Yes, delete                        | - Delete permission<br>- Delete allowed on status                          | trash  | -                     |

---

### 1.1e. DataTable (Product Variety List) - Filters Fields

| Name      | Component    | Depends On | Possible Values        | Default Values | Output         | Required | Tooltip |
| --------- | ------------ | ---------- | -------------------------- | ----------------- | ---------------- | -------- | ------- |
| Product   | Multi Select | –          | All Products                | All                | product_ids      | -        | -       |
| Attribute | Multi Select | Product    | Configured Attribute Values | All                | attribute_ids    | -        | -       |
| Stage     | Multi Select | –          | Possible Stages              | All Open Stage     | stage_ids        | -        | -       |

---

# 2. Product Variety Entry Page UI

## Main Form Sections

```
2.1. Left: Main Form
- Basic Details

2.2. Right: Primary Image and Summary
- Primary Image
- System Information
```

---

## 2.1 Left Section

### 2.1.1 Basic Details

| Name               | Data Source                          | Component           | Component Specific Information                            | Required | Read Only | Validations                                | On Change                    | Description                                                            | Tooltip |
| ------------------- | --------------------------------------- | ---------------------- | ------------------------------------------------------------- | -------- | --------- | -------------------------------------------- | ------------------------------- | --------------------------------------------------------------------------- | ------- |
| Product            | s_prd_product_variety.`product_id`   | Dropdown             | Select Product                                              | Yes      | No        | –                                            | Load Product Attributes        | Product for which variety is created                                     | -       |
| Dynamic Attributes | s_prd_product_variety_attributes[]   | Dynamic Dropdown(s)  | Generated based on Product Attribute configuration          | Yes      | No        | Attribute Value required where applicable    | Reload when Product changes    | Displays all applicable Product Attributes and their allowed values       | -       |
| Start Date         | s_prd_product_variety.`start_date`   | Date Picker          | DD/MM/YYYY                                                  | Yes      | No        | Only Date                                    | –                               | Effective Start Date                                                       | -       |
| Remarks            | s_prd_product_variety.`remarks`      | Text Area            | Multi-line                                                  | No       | No        | Max 255 Characters                           | –                               | Additional Notes                                                            | -       |

---

## 2.2 Right Section

### 2.2.1 Primary Image

| Name          | Data Source                            | Component    | Component Specific Information | Required | Read Only | Validations              | On Change               | Description                       | Tooltip |
| ------------- | ----------------------------------------- | ------------- | --------------------------------- | -------- | --------- | --------------------------- | -------------------------- | ------------------------------------ | ------- |
| Primary Image | s_prd_product_variety.`primary_image`  | Image Upload | Upload with Preview              | No       | No        | Supported Image Formats    | Preview Uploaded Image    | Primary Image of Product Variety    | -       |

---

### 2.2.2 System Information

| Name        | Data Source      | Component | Component Specific Information | Required | Read Only | Validations | On Change | Description                                              | Tooltip |
| ----------- | ------------------ | --------- | --------------------------------- | -------- | --------- | ------------ | --------- | ---------------------------------------------------------- | ------- |
| Created By  | System Generated  | Text      | User Name                          | No       | Yes       | –            | –         | User who created the Product Variety                        | -       |
| Created On  | System Generated  | Date Time | DD/MM/YYYY HH:mm                   | No       | Yes       | –            | –         | Date and Time when the Product Variety was created          | -       |
| Modified By | System Generated  | Text      | User Name                          | No       | Yes       | –            | –         | User who last modified the Product Variety                  | -       |
| Modified On | System Generated  | Date Time | DD/MM/YYYY HH:mm                   | No       | Yes       | –            | –         | Date and Time when the Product Variety was last modified    | -       |
