# UI Field Validations

This document defines common field validations found in **HappyERP UI** documents. It should be used while preparing UI field tables for entry pages, popups, filter panels and editable DataTables.

This file is based on validation patterns used in current UI documents such as Purchase Order, Purchase Invoice, GRN, Make Payment, Payment Adjustment, Organisation, User Configuration and Product Master screens.

# 1. Purpose

| Purpose                   | Description                                                                                                      |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Standard validation names | Use the same validation wording across all UI files.                                                             |
| Common field rules        | Keep common rules for fields like Name, Short Name, Date, Quantity, Rate, Amount, Mobile, Email and Attachments. |
| Better error messages     | Give clear messages to users when entered data is invalid.                                                       |
| UI and API alignment      | UI validation should match API and database validation wherever applicable.                                      |

# 2. Validation Columns in UI Documents

UI field tables should continue to use the following columns.

| Column                         | Description                                          | Example                 |
| ------------------------------ | ---------------------------------------------------- | ----------------------- |
| Required                       | Defines whether user must enter/select the value.    | Yes                     |
| Read Only                      | Defines whether user can modify the value.           | No                      |
| Validations                    | Defines validation rules to apply on the field.      | Required, Length: 3-100 |
| Component Specific Information | Defines input behavior, format or component options. | Decimal Allowed, Min=0  |
| On Change                      | Defines recalculation or dependent field behavior.   | Recalculate Amounts     |

# 3. Common Validation Types

| Validation Type               | Description                                         | Example in UI Files                               | Standard Message                               |
| ----------------------------- | --------------------------------------------------- | ------------------------------------------------- | ---------------------------------------------- |
| Required                      | Value must be entered or selected.                  | `Required`, `Mandatory`                           | This field is required.                        |
| Length                        | Text length must be within allowed range.           | `Length: 3-100`                                   | Value must be between 3 and 100 characters.    |
| Maximum Length                | Text cannot exceed allowed length.                  | `Max 500 Characters`                              | Value cannot be more than 500 characters.      |
| Numeric                       | Value must be a valid number.                       | `Numeric Value`                                   | Enter a valid number.                          |
| Integer                       | Value must be a whole number.                       | `Integer Only`, `Positive Integer`                | Enter a valid whole number.                    |
| Decimal                       | Value can contain decimal places.                   | `Decimal Allowed`, `Decimal Value`                | Enter a valid decimal value.                   |
| Greater Than Zero             | Number must be more than zero.                      | `Greater Than Zero`                               | Value must be greater than zero.               |
| Greater Than Or Equal To Zero | Number cannot be negative.                          | `Must Be Greater Than Or Equal To 0`              | Value cannot be negative.                      |
| Range                         | Number must be between minimum and maximum values.  | `0-100%`                                          | Value must be within allowed range.            |
| Unique                        | Value must not already exist in the selected scope. | `Must Be Unique`, `Unique Short Name`             | Value already exists.                          |
| Valid Date                    | Value must be a valid date.                         | `Only Date`, `Valid Date`                         | Enter a valid date.                            |
| Future Date Not Allowed       | Date cannot be after current date.                  | `Cannot Be Future Date`                           | Date cannot be in future.                      |
| Date Compare                  | Date must be compared with another date.            | `Must Be Greater Than Or Equal To PO Date`        | Date is not valid as per related date.         |
| Valid Option                  | Value must be selected from allowed list.           | `Valid Option Required`, `Valid Payment Mode`     | Select a valid option.                         |
| Active Lookup                 | Selected record must be active.                     | `Vendor Must Be Active`, `Active Vendor Required` | Select an active record.                       |
| Conditional Required          | Field is required only when condition is true.      | `Required If TDS Applicable`                      | This field is required for selected condition. |
| Minimum One Row               | At least one row is required in table.              | `Minimum One Row Required`                        | Add at least one row.                          |
| Cannot Exceed                 | Value cannot be greater than related value.         | `Cannot Exceed Pending Amount`                    | Value cannot exceed allowed amount.            |
| Formula Validation            | Calculated field must match formula.                | `Formula Validation`                              | Calculated value is not valid.                 |
| File Validation               | Uploaded file must match file rules.                | `Valid File Types`                                | Uploaded file is not valid.                    |

# 4. Standard Text Field Validations

| Field            | Required | Validation                       | Description                                        | Example                | Error Message                                      |
| ---------------- | -------- | -------------------------------- | -------------------------------------------------- | ---------------------- | -------------------------------------------------- |
| Name             | Yes      | `Required, Length: 3-100`        | Common name field used in masters and setup pages. | Product Name           | Name must be between 3 and 100 characters.         |
| Short Name       | Yes      | `Required, Length: 2-25, Unique` | Short code/name used for easy identification.      | PRD                    | Short Name must be between 2 and 25 characters.    |
| Code             | Yes      | `Required, Length: 1-30, Unique` | System or user-defined document/master code.       | PUR-PO                 | Code must be unique and cannot be blank.           |
| Description      | No       | `MaxLength: 500`                 | Longer business description.                       | Cotton fabric purchase | Description cannot be more than 500 characters.    |
| Remarks          | No       | `MaxLength: 500`                 | Notes entered by user on transaction or master.    | Urgent delivery        | Remarks cannot be more than 500 characters.        |
| Reference Number | No       | `Unique Within Document`         | Bank, cheque, UPI or external reference number.    | CHQ1025                | Reference Number already exists for this document. |
| File Name        | Yes      | `Required`                       | Auto-filled uploaded file name.                    | invoice.pdf            | File Name cannot be blank.                         |

# 5. Contact and Address Validations

| Field          | Required | Validation                                 | Description                                | Example                  | Error Message                                        |
| -------------- | -------- | ------------------------------------------ | ------------------------------------------ | ------------------------ | ---------------------------------------------------- |
| Street Address | No       | `Length: 2-255`                            | Organisation or party street address.      | Industrial Area, Phase 1 | Street Address must be between 2 and 255 characters. |
| City           | No       | `Valid City`                               | City selected from active city list.       | Jaipur                   | Select a valid city.                                 |
| State          | No       | `Valid State`                              | State selected from active state list.     | Rajasthan                | Select a valid state.                                |
| Country        | No       | `Valid Country`                            | Country selected from active country list. | India                    | Select a valid country.                              |
| Pincode        | No       | `Length: 2-10, Valid Pincode`              | Postal code or ZIP code.                   | 302001                   | Enter a valid pincode.                               |
| Mobile         | No       | `Length: 3-25, Valid Mobile Number Format` | Primary or alternate contact number.       | 9876543210               | Enter a valid mobile number.                         |
| Email          | No       | `Valid Email`                              | Email address.                             | accounts@example.com     | Enter a valid email address.                         |
| Website        | No       | `Valid URL`                                | Website URL.                               | https://www.happyerp.in  | Enter a valid website URL.                           |

# 6. Date Validations

| Field                  | Required | Validation                            | Description                              | Example            | Error Message                                          |
| ---------------------- | -------- | ------------------------------------- | ---------------------------------------- | ------------------ | ------------------------------------------------------ |
| Document Date          | Yes      | `Required, Only Date`                 | Main transaction date.                   | PO Date            | Document Date is required.                             |
| Payment Date           | Yes      | `Required, Cannot Be Future Date`     | Payment transaction date.                | Make Payment Date  | Payment Date cannot be in future.                      |
| Adjustment Date        | Yes      | `Required, Cannot Be Future Date`     | Payment adjustment date.                 | Adjustment Date    | Adjustment Date cannot be in future.                   |
| Reference Date         | No       | `Cannot Be Future Date`               | Bank, cheque or external reference date. | Cheque Date        | Reference Date cannot be in future.                    |
| Issue Date             | No       | `Cannot Be Greater Than Current Date` | Regulatory ID issue date.                | GSTIN Issue Date   | Issue Date cannot be greater than current date.        |
| Start Date             | Yes      | `Required, Only Date`                 | Effective start date of master or rate.  | Rate Start Date    | Start Date is required.                                |
| End Date               | No       | `Greater Than Or Equal To Start Date` | Effective closing date.                  | Valid Upto         | End Date cannot be earlier than Start Date.            |
| Expected Delivery Date | Yes      | `Greater Than Or Equal To PO Date`    | Planned delivery date in Purchase Order. | 20/07/2026         | Expected Delivery Date cannot be earlier than PO Date. |
| Date Range             | No       | `Valid Date Range`                    | Filter period with from and to dates.    | From Date, To Date | Date Range is not valid.                               |

# 7. Number, Quantity and Amount Validations

| Field              | Required | Validation                                         | Description                                     | Example  | Error Message                            |
| ------------------ | -------- | -------------------------------------------------- | ----------------------------------------------- | -------- | ---------------------------------------- |
| Quantity           | Yes      | `Required, Decimal, Greater Than Zero`             | Item quantity in transaction detail.            | 10.500   | Quantity must be greater than zero.      |
| Rate               | Yes      | `Required, Decimal, Greater Than Zero`             | Item rate or product rate.                      | 1250.75  | Rate must be greater than zero.          |
| Amount             | Yes      | `Required, Decimal, Greater Than Or Equal To Zero` | Transaction amount or schedule amount.          | 25000.00 | Amount cannot be negative.               |
| Discount           | No       | `Cannot Exceed Line Amount`                        | Discount at line or document level.             | 500.00   | Discount cannot exceed line amount.      |
| Discount Rate      | Yes      | `Greater Than Or Equal To Zero`                    | Discount percentage/rate/value.                 | 5.00     | Discount Rate cannot be negative.        |
| Max Discount Value | No       | `Greater Than Or Equal To Zero`                    | Maximum discount cap.                           | 1000.00  | Max Discount Value cannot be negative.   |
| Penalty Rate       | Yes      | `Greater Than Or Equal To Zero`                    | Penalty percentage/rate/value.                  | 2.00     | Penalty Rate cannot be negative.         |
| Max Penalty Value  | No       | `Greater Than Or Equal To Zero`                    | Maximum penalty cap.                            | 1000.00  | Max Penalty Value cannot be negative.    |
| Exchange Rate      | No       | `Decimal, Greater Than Zero`                       | Currency exchange rate.                         | 83.25    | Exchange Rate must be greater than zero. |
| Credit Days        | No       | `Integer, Greater Than Or Equal To Zero`           | Credit period in days.                          | 30       | Credit Days cannot be negative.          |
| Days Within        | Yes      | `Integer, Greater Than Or Equal To Zero`           | Days from base date for schedule or discount.   | 15       | Days Within cannot be negative.          |
| Days After         | Yes      | `Integer, Greater Than Or Equal To Zero`           | Grace days before penalty.                      | 7        | Days After cannot be negative.           |
| Percentage         | No       | `Range: 0-100, Decimal`                            | Percentage values like TDS or discount percent. | 18.00    | Percentage must be between 0 and 100.    |
| Sequence           | Yes      | `Integer, Unique Sequence`                         | Stage or display order.                         | 10       | Sequence must be unique.                 |
| Number of Decimals | Yes      | `Integer, Greater Than Or Equal To Zero`           | Decimal places for UOM.                         | 2        | Number of Decimals cannot be negative.   |

# 8. Lookup and Select Validations

| Field         | Required    | Validation                     | Description                                              | Example              | Error Message                                    |
| ------------- | ----------- | ------------------------------ | -------------------------------------------------------- | -------------------- | ------------------------------------------------ |
| Organisation  | Yes         | `Active Organisation Required` | Organisation selected by user or context.                | ABC Textiles         | Select a valid organisation.                     |
| Branch        | No          | `Valid Branch`                 | Branch must belong to selected organisation.             | Jaipur Branch        | Select a valid branch.                           |
| Vendor        | Yes         | `Active Vendor Required`       | Vendor selected in purchase/account documents.           | ABC Traders          | Select an active vendor.                         |
| Customer      | Yes         | `Active Customer Required`     | Customer selected in sales documents.                    | XYZ Retail           | Select an active customer.                       |
| Product       | Yes         | `Active Product Required`      | Product selected in item detail.                         | Cotton Fabric        | Select an active product.                        |
| UOM           | Yes         | `Valid UOM Required`           | Unit of measure selected for quantity.                   | Meter                | Select a valid UOM.                              |
| Currency      | Yes         | `Valid Currency`               | Transaction currency.                                    | INR                  | Select a valid currency.                         |
| Payment Mode  | Yes         | `Valid Payment Mode Required`  | Cash, UPI, Bank, Cheque, NEFT or RTGS.                   | Bank Transfer        | Select a valid payment mode.                     |
| Base Date     | Yes         | `Valid Option Required`        | Date base for due date, discount or penalty calculation. | Invoice Date         | Select a valid base date.                        |
| Discount Type | Yes         | `Valid Option Required`        | Per Unit, Percentage or Fixed.                           | Percentage           | Select a valid discount type.                    |
| Penalty Type  | Yes         | `Valid Option Required`        | Per Unit, Percentage or Fixed.                           | Fixed                | Select a valid penalty type.                     |
| Stage         | Yes         | `Valid Stage`                  | Workflow stage selected or displayed.                    | Draft                | Select a valid stage.                            |
| TDS Category  | Conditional | `Required If TDS Applicable`   | TDS category from TDS master.                            | Contractor           | TDS Category is required when TDS is applicable. |
| Bank Account  | Conditional | `Required For Bank Payments`   | Organisation bank account used for payment.              | HDFC Current Account | Bank Account is required for bank payment.       |

# 9. Transaction and DataTable Validations

| Area              | Validation                                       | Description                                                     | Example Error Message                              |
| ----------------- | ------------------------------------------------ | --------------------------------------------------------------- | -------------------------------------------------- |
| Document Number   | `Required, Unique, Read Only`                    | Auto-generated number must be unique.                           | Document Number already exists.                    |
| Product Details   | `Minimum One Row Required`                       | At least one item row is required before saving/submitting.     | Add at least one product row.                      |
| Payment Modes     | `At Least One Payment Mode Required`             | At least one payment mode is required.                          | Select at least one payment mode.                  |
| Bills             | `Minimum One Bill Required`                      | At least one outstanding bill is required for adjustment.       | Select at least one bill.                          |
| Credit Documents  | `Minimum One Credit Document Required`           | At least one credit document is required for credit settlement. | Select at least one credit document.               |
| Same Vendor       | `Same Vendor Required`                           | Selected references must belong to one vendor.                  | All selected records must belong to same vendor.   |
| Same Currency     | `All References Must Have Same Currency`         | Selected references must use same currency.                     | All selected records must have same currency.      |
| Payment Amount    | `Cannot Exceed Pending Amount`                   | Payment cannot be more than pending amount.                     | Payment Amount cannot exceed Pending Amount.       |
| Adjustment Amount | `Cannot Exceed Available Balance`                | Adjustment cannot be more than available balance.               | Adjustment Amount cannot exceed Available Balance. |
| Received Quantity | `Cannot Exceed Planned Quantity`                 | Received quantity cannot be more than planned quantity.         | Received Quantity cannot exceed Planned Quantity.  |
| Delivery Quantity | `Total Delivery Quantity Must Match PO Quantity` | Delivery schedule quantity must match ordered quantity.         | Total delivery quantity must match PO Quantity.    |
| Tax               | `Valid Tax Configuration Required`               | Tax calculation requires valid tax setup.                       | Valid tax configuration is required.               |
| Formula Fields    | `Formula Validation`                             | Calculated field must match formula result.                     | Calculated value is not valid.                     |

# 10. File and Attachment Validations

| Field          | Required | Validation             | Description                                            | Example          | Error Message                       |
| -------------- | -------- | ---------------------- | ------------------------------------------------------ | ---------------- | ----------------------------------- |
| Attachments    | No       | `Valid File Types`     | Supporting documents uploaded by user.                 | PDF, Image, DOC  | Uploaded file type is not allowed.  |
| File Name      | Yes      | `Cannot Be Blank`      | Original or generated uploaded file name.              | invoice.pdf      | File Name cannot be blank.          |
| File Type      | Yes      | `Allowed File Type`    | Extension/type should be permitted.                    | PDF              | File type is not allowed.           |
| File Size      | No       | `MaxSize: 2 MB`        | File size should be within configured limit.           | 1.5 MB           | File size cannot be more than 2 MB. |
| Duplicate File | No       | `Unique Within Object` | Same file should not be uploaded twice for one object. | Same invoice PDF | This file is already uploaded.      |

# 11. Recommended Short Notation

Use this notation in UI field tables to keep validations short and consistent.

| Notation                           | Meaning                                                    | Example                         |
| ---------------------------------- | ---------------------------------------------------------- | ------------------------------- |
| `Required`                         | User must enter/select a value.                            | `Required`                      |
| `Length: 3-100`                    | Text length must be between 3 and 100 characters.          | `Required, Length: 3-100`       |
| `MaxLength: 500`                   | Text cannot exceed 500 characters.                         | `MaxLength: 500`                |
| `Integer`                          | Only whole number is allowed.                              | `Integer, >= 0`                 |
| `Decimal`                          | Decimal number is allowed.                                 | `Decimal, > 0`                  |
| `> 0`                              | Value must be greater than zero.                           | `Decimal, > 0`                  |
| `>= 0`                             | Value must be greater than or equal to zero.               | `Integer, >= 0`                 |
| `Range: 0-100`                     | Value must be between 0 and 100.                           | `Range: 0-100`                  |
| `Only Date`                        | Date without time is allowed.                              | `Required, Only Date`           |
| `No Future Date`                   | Future date is not allowed.                                | `Required, No Future Date`      |
| `AfterOrEqual: field_name`         | Date/value must be greater than or equal to another field. | `AfterOrEqual: start_date`      |
| `Unique`                           | Value must be unique in defined scope.                     | `Unique: organisation`          |
| `Active Lookup`                    | Selected lookup record must be active.                     | `Active Vendor Required`        |
| `Required If: condition`           | Required only when condition is true.                      | `Required If: TDS Applicable`   |
| `Cannot Exceed: field_name`        | Value cannot exceed another field value.                   | `Cannot Exceed: pending_amount` |
| `MinRows: 1`                       | Table must contain minimum one row.                        | `MinRows: 1`                    |
| `FileTypes: PDF,JPG,PNG,DOCX,XLSX` | Only listed file types are allowed.                        | `FileTypes: PDF,JPG,PNG`        |

# 12. Example UI Field Validation Table

| Name/Label    | Data Source   | Type/Component | Component Specific Information | Required | Read Only | Validations                         | On Change           | Description      | Tooltip              |
| ------------- | ------------- | -------------- | ------------------------------ | -------- | --------- | ----------------------------------- | ------------------- | ---------------- | -------------------- |
| Name          | name          | Text           | Length 3-100                   | Yes      | No        | Required, Length: 3-100             | -                   | Name of record   | Enter name           |
| Short Name    | short_name    | Text           | Length 2-25                    | Yes      | No        | Required, Length: 2-25, Unique      | -                   | Short identifier | Enter short name     |
| Document Date | document_date | Date Picker    | Single Date                    | Yes      | No        | Required, Only Date, No Future Date | -                   | Document date    | Select document date |
| Vendor        | vendor_id     | Lookup         | Active Vendors                 | Yes      | No        | Active Vendor Required              | Load Vendor Details | Vendor selection | Select vendor        |
| Quantity      | quantity      | Number Input   | Decimal Allowed, Min=0         | Yes      | No        | Decimal, > 0                        | Recalculate Amounts | Ordered quantity | Enter quantity       |
| Rate          | rate          | Currency Input | Decimal Allowed                | Yes      | No        | Decimal, > 0                        | Recalculate Amounts | Item rate        | Enter rate           |
| Discount      | discount      | Currency Input | Decimal Allowed                | No       | No        | Cannot Exceed: line_amount          | Recalculate Amounts | Discount amount  | Enter discount       |
| Remarks       | remarks       | Text Area      | Multi Line                     | No       | No        | MaxLength: 500                      | -                   | General remarks  | Enter remarks        |

# 13. Error Message Guidelines

| Guideline             | Description                                         | Good Example                                 |
| --------------------- | --------------------------------------------------- | -------------------------------------------- |
| Use field label       | Message should mention the same label shown on UI.  | Quantity must be greater than zero.          |
| Be specific           | Tell user exactly what is wrong.                    | Payment Amount cannot exceed Pending Amount. |
| Mention allowed range | If value has range, show that range.                | Percentage must be between 0 and 100.        |
| Avoid technical terms | Do not show database field names to user.           | Select an active vendor.                     |
| Keep message short    | Message should be readable near field.              | Date cannot be in future.                    |
| Use same wording      | Same validation should use same message everywhere. | Name must be between 3 and 100 characters.   |
