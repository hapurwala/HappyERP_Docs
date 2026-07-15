
# ERP UI — Common Field Validations

> This is the single source of truth for field-level validation rules shared across all ERP modules. Module UI docs should link to this file instead of repeating these rules.

---

## Text Fields

| Field       | Type                                                                                 | Length | Values | Format      | Trim | Comma Format | Clear on Parent Change | Validation Trigger | Validation Type         | Error Message                        | Remarks                           |
| :---------- | :----------------------------------------------------------------------------------- | :----- | :----- | :---------- | :--- | :----------- | :--------------------- | :----------------- | :---------------------- | :----------------------------------- | :-------------------------------- |
| Name        | All                                                                                  | 3–100  | –      | –           | Yes  | –            | –                      | On Blur; On Submit | Min/Max Length; Unique  | "{Field} must be 3–100 characters"   | Unique within scope in most cases |
| Short Name  | All                                                                                  | 2–25   | –      | –           | Yes  | –            | –                      | On Blur; On Submit | Min/Max Length; Unique  | "{Field} must be 2–25 characters"    | Unique within scope in most cases |
| Remarks     | All                                                                                  | 0-500  | –      | –           | No   | –            | –                      | On Submit          | Max Length              | "Must not exceed {limit} characters" | Optional; multi-line              |
| Description | All                                                                                  | 0–1000 | –      | –           | No   | –            | –                      | On Submit          | Max Length              | "Must not exceed 1000 characters"    | Optional; multi-line              |
| Mobile      | Numeric<br>`^\d{3,25}$`                                                              | 3–25   | –      | Numeric     | Yes  | –            | –                      | On Blur; On Submit | Numeric; Min/Max Length | "Only numeric characters allowed"    | Numeric characters only           |
| Email       | Alphanumeric + Special Chars<br>`^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$` | –      | –      | -           | Yes  | –            | –                      | On Blur; On Submit | Format                  | "Enter a valid email address"        | Standard email format             |
| Pincode     | Numeric<br>`^\d{2,10}$`                                                              | 2–10   | –      | Numeric     | Yes  | –            | –                      | On Blur; On Submit | Numeric; Min/Max Length | "Only numeric characters allowed"    | –                                 |
| URL         | Alphanumeric + Special Chars<br>`^https?:\/\/.+$`                                    | –      | –      | https://... | Yes  | –            | –                      | On Blur; On Submit | Format                  | "Enter a valid URL"                  | Standard URL format               |

---

## Date Fields

| Field        | Type | Length | Values  | Format                  | Trim | Comma Format | Clear on Parent Change | Validation Trigger   | Validation Type         | Error Message                             | Remarks                                      |
| :----------- | :--- | :----- | :------ | :---------------------- | :--- | :----------- | :--------------------- | :------------------- | :---------------------- | :---------------------------------------- | :------------------------------------------- |
| Past Date    | –    | –      | < Today | DD/MM/YYYY              | –    | –            | –                      | On Change; On Submit | Date Constraint         | "Date must be in the past"                | Only dates before today are allowed          |
| Current Date | –    | –      | = Today | DD/MM/YYYY              | –    | –            | –                      | On Change; On Submit | Date Constraint         | "Date must be today's date"               | Only today's date is allowed                 |
| Future Date  | –    | –      | > Today | DD/MM/YYYY              | –    | –            | –                      | On Change; On Submit | Date Constraint         | "Date must be in the future"              | Only dates after today are allowed           |
| Start Date   | –    | –      | -       | DD/MM/YYYY              | –    | –            | –                      | On Change; On Submit | Format                  | "{Field} is required"                     | –                                            |
| End Date     | –    | –      | -       | DD/MM/YYYY              | –    | –            | –                      | On Change; On Submit | Format; Date Comparison | "End Date must be on or after Start Date" | Optional; must be ≥ Start Date               |
| Date Range   | –    | –      | –       | DD/MM/YYYY – DD/MM/YYYY | –    | –            | –                      | On Change            | Date Comparison         | "From Date must be on or before To Date"  | From Date must be ≤ To Date; used in filters |

---

## Number Fields

| Field       | Type             | Length | Values          | Format   | Trim | Comma Format    | Clear on Parent Change | Validation Trigger | Validation Type             | Error Message                                      | Remarks                                             |
| :---------- | :--------------- | :----- | :-------------- | :------- | :--- | :-------------- | :--------------------- | :----------------- | :-------------------------- | :------------------------------------------------- | :-------------------------------------------------- |
| Integer     | Integer          | –      | Module-specific | 0        | –    | Module-specific | –                      | On Blur; On Submit | Integer                     | "Enter a valid whole number"                       | Base type; no decimals allowed                      |
| Decimal     | Decimal          | –      | Module-specific | 0.00     | –    | Module-specific | –                      | On Blur; On Submit | Decimal (2dp)               | "Enter a valid number"                             | Base type; up to 2 decimal places                   |
| Quantity    | Integer, Decimal | –      | > 0             | 0 / 0.00 | –    | Yes             | –                      | On Blur; On Submit | Positive; Decimal           | "Quantity must be greater than 0"                  | Decimal allowed in most cases; integer-only in some |
| Amount      | Decimal          | –      | ≥ 0             | 0.00     | –    | Yes             | –                      | On Blur; On Submit | Non-negative; Decimal (2dp) | "Amount must be 0 or greater"                      | Decimal up to 2 places                              |
| Rate        | Decimal          | –      | > 0             | 0.00     | –    | Yes             | –                      | On Blur; On Submit | Positive; Decimal (2dp)     | "Rate must be greater than 0"                      | Decimal up to 2 places                              |
| Tax %       | Decimal          | –      | 0–100           | 0.00%    | –    | No              | –                      | On Blur; On Submit | Range; Decimal (2dp)        | "Tax must be between 0 and 100"                    | Percentage; decimal up to 2 places                  |
| Percentage  | Decimal          | –      | 0–100           | 0.00%    | –    | No              | –                      | On Blur; On Submit | Range; Decimal (2dp); Sum   | "Total percentage across all rows must equal 100%" | Total across rows must = 100%                       |
| Credit Days | Integer          | –      | ≥ 0             | 0        | –    | No              | –                      | On Blur; On Submit | Non-negative; Integer       | "Credit Days must be 0 or greater"                 | Whole number; no decimals                           |
| Sequence    | Integer          | –      | ≥ 1             | 0        | –    | No              | –                      | On Blur; On Submit | Positive; Integer           | "Sequence must be 1 or greater"                    | Controls display order; positive integer            |


---

## Select / Lookup Fields

| Field    | Type   | Length | Values              | Format | Trim | Comma Format | Clear on Parent Change | Validation Trigger | Validation Type | Error Message                    | Remarks                      |
| :------- | :----- | :----- | :------------------ | :----- | :--- | :----------- | :--------------------- | :----------------- | :-------------- | :------------------------------- | :--------------------------- |
| Priority | Select | –      | High / Medium / Low | –      | –    | –            | –                      | On Submit          | Enum            | "Select a valid priority"        | Fixed option list            |
| Currency | Lookup | –      | –                   | –      | –    | –            | –                      | On Submit          | Lookup          | "Select a valid currency"        | –                            |
| UoM      | Lookup | –      | –                   | –      | –    | –            | –                      | On Submit          | Lookup          | "Select a valid unit of measure" | –                            |
| City     | Lookup | –      | –                   | –      | –    | –            | Yes                    | On Submit          | Dependency      | "Please select a state first"    | Depends on State selection   |
| State    | Lookup | –      | –                   | –      | –    | –            | Yes                    | On Submit          | Dependency      | "Please select a country first"  | Depends on Country selection |
| Country  | Lookup | –      | –                   | –      | –    | –            | –                      | On Submit          | Lookup          | "Select a valid country"         | –                            |

---

## Attachments

| Field     | Type     | Length          | Values               | Format | Trim | Comma Format | Clear on Parent Change | Validation Trigger | Validation Type | Error Message                                 | Remarks                                              |
| :-------- | :------- | :-------------- | :------------------- | :----- | :--- | :----------- | :--------------------- | :----------------- | :-------------- | :-------------------------------------------- | :--------------------------------------------------- |
| Images    | Image    | Max 2 MB        | JPG, JPEG, PNG       | -      | –    | –            | –                      | On Upload          | Format; Size    | "Only JPG, JPEG, PNG files are allowed"       | Files are automatically compressed after upload      |
| Documents | Document | Max 2 MB        | PDF, DOCX, XLSX, CSV | -      | –    | –            | –                      | On Upload          | Format; Size    | "Only PDF, DOCX, XLSX, CSV files are allowed" | Files are automatically compressed after upload      |

---

## Boolean Fields

| Field  | Type    | Length | Values   | Format | Trim | Comma Format | Clear on Parent Change | Validation Trigger | Validation Type | Error Message | Remarks                       |
| :----- | :------ | :----- | :------- | :----- | :--- | :----------- | :--------------------- | :----------------- | :-------------- | :------------ | :---------------------------- |
| Active | Boolean | –      | Yes / No | –      | –    | –            | –                      | On Change          | Boolean         | –             | Defaults to Yes on creation   |

---

