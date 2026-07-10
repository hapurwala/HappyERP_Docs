# HappyERP Purchase Order (PO) Test Cases

This document contains functional, validation, workflow, database, integration and security test cases for the Purchase Order module.

---

# Module Information

| Property                 | Value          |
| ------------------------ | -------------- |
| Software                 | HappyERP       |
| Application              | Purchase       |
| Module                   | Purchase Order |
| Document                 | Test Cases     |
| Version                  | 1.0            |
| Status                   | Draft          |
| Total Planned Test Cases | 225+           |

---

# Test Strategy

This document validates the complete Purchase Order lifecycle including:

- UI Validation
- Field Validation
- Business Rules
- Workflow Validation
- Database Validation
- Calculation Validation
- Security & Permission Validation
- Integration Validation
- Audit Validation
- Performance Validation
- Negative Test Cases
- Edge Cases
- Regression Test Cases

---

# Test Case Priority

| Priority | Description                        |
| -------- | ---------------------------------- |
| Critical | Business cannot continue if failed |
| High     | Major functionality affected       |
| Medium   | Minor functionality affected       |
| Low      | Cosmetic or optional feature       |

---

# Test Execution Type

| Value     | Description                                |
| --------- | ------------------------------------------ |
| Manual    | Executed manually by tester                |
| Automated | Executed by automation framework           |
| Both      | Can be executed manually and automatically |

---

# Test Case Template

| TC ID | Module | Feature | Category | Priority | Execute By | Page | Preconditions | Test Scenario | Test Steps | Test Data | Expected Result | Actual Result | Status | Remarks |
| ----- | ------ | ------- | -------- | -------- | ---------- | ---- | ------------- | ------------- | ---------- | --------- | --------------- | ------------- | ------ | ------- |

---

# General Information

| TC ID   | Module         | Feature      | Category   | Priority | Execute By | Page           | Preconditions            | Test Scenario                    | Test Steps                | Test Data   | Expected Result                        | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------ | ---------- | -------- | ---------- | -------------- | ------------------------ | -------------------------------- | ------------------------- | ----------- | -------------------------------------- | ------------- | ------------ | ------- |
| TC-0001 | Purchase Order | Create PO    | Functional | Critical | Manual     | Purchase Order | User Logged In           | Verify Purchase Order page opens | Open Purchase → Click Add | —           | Purchase Order page opens successfully |               | Not Executed |         |
| TC-0002 | Purchase Order | PO Number    | Functional | Critical | Manual     | Purchase Order | Number Series Configured | Verify PO Number auto generation | Create New PO             | —           | Unique Purchase Order Number generated |               | Not Executed |         |
| TC-0003 | Purchase Order | PO Date      | Validation | High     | Manual     | Purchase Order | Create PO Page Open      | Verify current date populated    | Open Add PO               | —           | Current system date displayed          |               | Not Executed |         |
| TC-0004 | Purchase Order | PO Date      | Validation | High     | Manual     | Purchase Order | Create PO Page Open      | Verify future date restriction   | Select future date        | Tomorrow    | Validation message displayed           |               | Not Executed |         |
| TC-0005 | Purchase Order | Organisation | Validation | Critical | Manual     | Purchase Order | Create PO Page Open      | Verify Organisation mandatory    | Leave blank and Save      | Blank       | Validation message displayed           |               | Not Executed |         |
| TC-0006 | Purchase Order | Branch       | Validation | High     | Manual     | Purchase Order | Organisation Selected    | Verify Branch list filtered      | Select Organisation       | Org A       | Only Branches of Org A displayed       |               | Not Executed |         |
| TC-0007 | Purchase Order | Vendor       | Validation | Critical | Manual     | Purchase Order | Create PO Page Open      | Verify Vendor mandatory          | Leave blank               | Blank       | Validation displayed                   |               | Not Executed |         |
| TC-0008 | Purchase Order | Vendor       | Functional | High     | Manual     | Purchase Order | Vendor Exists            | Verify Vendor search             | Search Vendor             | ABC Traders | Vendor appears in lookup               |               | Not Executed |         |
| TC-0009 | Purchase Order | Currency     | Functional | Medium   | Manual     | Purchase Order | Organisation Selected    | Verify default currency          | Select Organisation       | INR         | Default currency populated             |               | Not Executed |         |
| TC-0010 | Purchase Order | Buyer        | Functional | Medium   | Manual     | Purchase Order | Buyer Exists             | Verify Buyer selection           | Select Buyer              | User A      | Buyer assigned successfully            |               | Not Executed |         |

---

# Product Details

| TC ID   | Module         | Feature     | Category   | Priority | Execute By | Page            | Preconditions      | Test Scenario                          | Test Steps          | Test Data | Expected Result                    | Actual Result | Status       | Remarks |
| ------- | -------------- | ----------- | ---------- | -------- | ---------- | --------------- | ------------------ | -------------------------------------- | ------------------- | --------- | ---------------------------------- | ------------- | ------------ | ------- |
| TC-0011 | Purchase Order | Add Product | Functional | Critical | Manual     | Product Details | Vendor Selected    | Verify product can be added            | Click Add Product   | Product A | Product added successfully         |               | Not Executed |         |
| TC-0012 | Purchase Order | Product     | Validation | Critical | Manual     | Product Details | Product Row Exists | Verify Product mandatory               | Leave Product blank | Blank     | Validation displayed               |               | Not Executed |         |
| TC-0013 | Purchase Order | Variety     | Functional | Medium   | Manual     | Product Details | Product Selected   | Verify Variety filtered                | Select Product      | Product A | Only Product A varieties displayed |               | Not Executed |         |
| TC-0014 | Purchase Order | Quantity    | Validation | Critical | Manual     | Product Details | Product Selected   | Verify Quantity mandatory              | Leave blank         | Blank     | Validation displayed               |               | Not Executed |         |
| TC-0015 | Purchase Order | Quantity    | Validation | Critical | Manual     | Product Details | Product Selected   | Verify zero quantity restriction       | Enter 0             | 0         | Validation displayed               |               | Not Executed |         |
| TC-0016 | Purchase Order | Quantity    | Validation | High     | Manual     | Product Details | Product Selected   | Verify negative quantity restriction   | Enter -5            | -5        | Validation displayed               |               | Not Executed |         |
| TC-0017 | Purchase Order | Rate        | Validation | Critical | Manual     | Product Details | Product Selected   | Verify Rate mandatory                  | Blank Rate          | Blank     | Validation displayed               |               | Not Executed |         |
| TC-0018 | Purchase Order | Rate        | Validation | High     | Manual     | Product Details | Product Selected   | Verify negative rate restriction       | Enter -100          | -100      | Validation displayed               |               | Not Executed |         |
| TC-0019 | Purchase Order | Discount    | Functional | High     | Manual     | Product Details | Product Added      | Verify Percentage discount calculation | Qty 10 × ₹100       | 10%       | Discount calculated correctly      |               | Not Executed |         |
| TC-0020 | Purchase Order | Discount    | Functional | High     | Manual     | Product Details | Product Added      | Verify Fixed discount calculation      | ₹500                | Fixed     | Correct discount applied           |               | Not Executed |         |

---

# Tax & Amount Calculation

| TC ID   | Module         | Feature       | Category    | Priority | Execute By | Page            | Preconditions      | Test Scenario                    | Test Steps      | Test Data | Expected Result       | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------- | ----------- | -------- | ---------- | --------------- | ------------------ | -------------------------------- | --------------- | --------- | --------------------- | ------------- | ------------ | ------- |
| TC-0021 | Purchase Order | Product Value | Calculation | Critical | Manual     | Product Details | Qty & Rate Entered | Verify Product Value calculation | Qty=10 Rate=100 | 10×100    | Product Value = ₹1000 |               | Not Executed |         |
| TC-0022 | Purchase Order | Discount      | Calculation | High     | Manual     | Product Details | Discount Applied   | Verify Amount After Discount     | ₹1000−₹100      |           | ₹900                  |               | Not Executed |         |
| TC-0023 | Purchase Order | Tax           | Calculation | Critical | Manual     | Product Details | Tax Configured     | Verify GST calculation           | GST 18%         | ₹900      | ₹162 calculated       |               | Not Executed |         |
| TC-0024 | Purchase Order | Net Amount    | Calculation | Critical | Manual     | Product Details | Tax Applied        | Verify Net Amount                | ₹900+₹162       |           | ₹1062                 |               | Not Executed |         |
| TC-0025 | Purchase Order | Round Off     | Calculation | Medium   | Manual     | Summary         | Decimal Amount     | Verify Round Off                 | ₹1062.49        |           | Rounded correctly     |               | Not Executed |         |

---

# Delivery Schedule

| TC ID   | Module         | Feature           | Category    | Priority | Execute By | Page              | Preconditions | Test Scenario                  | Test Steps      | Test Data | Expected Result      | Actual Result | Status       | Remarks |
| ------- | -------------- | ----------------- | ----------- | -------- | ---------- | ----------------- | ------------- | ------------------------------ | --------------- | --------- | -------------------- | ------------- | ------------ | ------- |
| TC-0026 | Purchase Order | Delivery Schedule | Functional  | High     | Manual     | Delivery Schedule | Product Added | Verify Delivery Schedule row   | Add Schedule    | Qty 100   | Row added            |               | Not Executed |         |
| TC-0027 | Purchase Order | Warehouse         | Validation  | High     | Manual     | Delivery Schedule | Schedule Row  | Verify Warehouse mandatory     | Blank Warehouse | Blank     | Validation displayed |               | Not Executed |         |
| TC-0028 | Purchase Order | Quantity          | Validation  | High     | Manual     | Delivery Schedule | Schedule Row  | Verify Quantity mandatory      | Blank Qty       | Blank     | Validation displayed |               | Not Executed |         |
| TC-0029 | Purchase Order | Delivery Date     | Validation  | High     | Manual     | Delivery Schedule | Schedule Row  | Verify Delivery Date mandatory | Blank Date      | Blank     | Validation displayed |               | Not Executed |         |
| TC-0030 | Purchase Order | Pending Qty       | Calculation | Medium   | Manual     | Delivery Schedule | Partial GRN   | Verify Pending Quantity        | Receive 60/100  |           | Pending = 40         |               | Not Executed |         |

---

# Payment Terms

| TC ID   | Module         | Feature             | Category   | Priority | Execute By | Page           | Preconditions             | Test Scenario                           | Test Steps        | Test Data                                        | Expected Result      | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------------- | ---------- | -------- | ---------- | -------------- | ------------------------- | --------------------------------------- | ----------------- | ------------------------------------------------ | -------------------- | ------------- | ------------ | ------- |
| TC-0031 | Purchase Order | Payment Mode        | Functional | High     | Manual     | Payment Terms  | PO Created                | Verify Multiple Payment Modes           | Select Cash + UPI |                                                  | Saved successfully   |               | Not Executed |         |
| TC-0032 | Purchase Order | Credit Days         | Validation | Medium   | Manual     | Payment Terms  | Payment Terms Open        | Verify negative credit days restriction | Enter -5          | -5                                               | Validation displayed |               | Not Executed |         |
| TC-0033 | Purchase Order | TDS                 | Functional | High     | Manual     | Payment Terms  | TDS Applicable            | Verify TDS fields enabled               | Enable TDS        | Yes                                              | TDS fields displayed |               | Not Executed |         |
| TC-0034 | Purchase Order | Payment Schedule    | Functional | High     | Manual     | Payment Terms  | Credit Days Entered       | Verify Due Date calculation             | 30 Days           |                                                  | Due Date calculated  |               | Not Executed |         |
| TC-0035 | Purchase Order | Save Purchase Order | Functional | Critical | Manual     | Purchase Order | All Mandatory Data Filled | Save Purchase Order                     | Valid Data        | Purchase Order saved successfully and visible in |                      | Not Executed  |              |         |

# Workflow & Summary

| TC ID   | Module         | Feature    | Category   | Priority | Execute By | Page                | Preconditions           | Test Scenario            | Test Steps          | Test Data        | Expected Result                 | Actual Result | Status       | Remarks |
| ------- | -------------- | ---------- | ---------- | -------- | ---------- | ------------------- | ----------------------- | ------------------------ | ------------------- | ---------------- | ------------------------------- | ------------- | ------------ | ------- |
| TC-0036 | Purchase Order | Draft Save | Functional | Critical | Manual     | Purchase Order      | Mandatory fields filled | Verify Draft Save        | Click Save as Draft | Valid Data       | PO saved in Draft stage         |               | Not Executed |         |
| TC-0037 | Purchase Order | Submit     | Workflow   | Critical | Manual     | Purchase Order      | Draft PO exists         | Submit Purchase Order    | Click Submit        | Draft PO         | Stage changes to Submitted      |               | Not Executed |         |
| TC-0038 | Purchase Order | Approve    | Workflow   | Critical | Manual     | Purchase Order      | Submitted PO            | Approve Purchase Order   | Click Approve       | Submitted PO     | Stage changes to Approved       |               | Not Executed |         |
| TC-0039 | Purchase Order | Reject     | Workflow   | High     | Manual     | Purchase Order      | Submitted PO            | Reject Purchase Order    | Click Reject        | Remarks Required | Stage changes to Rejected       |               | Not Executed |         |
| TC-0040 | Purchase Order | Rollback   | Workflow   | High     | Manual     | Purchase Order      | Approved PO             | Undo Stage               | Click Undo          | Approved PO      | Previous Stage Restored         |               | Not Executed |         |
| TC-0041 | Purchase Order | Cancel     | Workflow   | High     | Manual     | Purchase Order      | Draft PO                | Cancel Purchase Order    | Click Cancel        | Draft PO         | Stage changes to Cancelled      |               | Not Executed |         |
| TC-0042 | Purchase Order | Delete     | Functional | High     | Manual     | Purchase Order      | Draft PO                | Delete Purchase Order    | Click Delete        | Draft PO         | PO deleted successfully         |               | Not Executed |         |
| TC-0043 | Purchase Order | Duplicate  | Functional | High     | Manual     | Purchase Order List | Existing PO             | Duplicate Purchase Order | Click Duplicate     | Existing PO      | New PO created with copied data |               | Not Executed |         |
| TC-0044 | Purchase Order | View       | Functional | Medium   | Manual     | Purchase Order List | Existing PO             | Open Purchase Order      | Click View          | Existing PO      | PO opens in read-only mode      |               | Not Executed |         |
| TC-0045 | Purchase Order | Modify     | Functional | High     | Manual     | Purchase Order      | Editable Stage          | Edit Purchase Order      | Modify Quantity     | Qty 20           | Changes saved successfully      |               | Not Executed |         |

---

# Summary Calculations

| TC ID   | Module         | Feature             | Category    | Priority | Execute By | Page    | Preconditions               | Test Scenario              | Test Steps         | Test Data   | Expected Result           | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------------- | ----------- | -------- | ---------- | ------- | --------------------------- | -------------------------- | ------------------ | ----------- | ------------------------- | ------------- | ------------ | ------- |
| TC-0046 | Purchase Order | Total Items         | Calculation | High     | Manual     | Summary | Multiple Products           | Verify Total Items         | Add 5 Products     | 5 Products  | Total Items = 5           |               | Not Executed |         |
| TC-0047 | Purchase Order | Total Quantity      | Calculation | High     | Manual     | Summary | Multiple Products           | Verify Quantity Total      | Qty 10+20          | 30          | Total Quantity = 30       |               | Not Executed |         |
| TC-0048 | Purchase Order | Total Weight        | Calculation | Medium   | Manual     | Summary | Weight Configured           | Verify Weight Calculation  | Add Products       | Weight Data | Total Weight calculated   |               | Not Executed |         |
| TC-0049 | Purchase Order | Product Total       | Calculation | Critical | Manual     | Summary | Products Added              | Verify Product Total       | Multiple Products  | Values      | Correct Product Total     |               | Not Executed |         |
| TC-0050 | Purchase Order | Discount Total      | Calculation | High     | Manual     | Summary | Discounts Applied           | Verify Discount Total      | Multiple Discounts | Values      | Total Discount calculated |               | Not Executed |         |
| TC-0051 | Purchase Order | Tax Total           | Calculation | Critical | Manual     | Summary | Taxes Applied               | Verify Total Tax           | Multiple GST       | Values      | Correct Tax Total         |               | Not Executed |         |
| TC-0052 | Purchase Order | Charges Total       | Calculation | High     | Manual     | Summary | Charges Added               | Verify Charges             | Freight etc.       | Charges     | Correct Charges Total     |               | Not Executed |         |
| TC-0053 | Purchase Order | Additional Discount | Calculation | Medium   | Manual     | Summary | Additional Discount Applied | Verify Additional Discount | ₹500               | 500         | Deducted Successfully     |               | Not Executed |         |
| TC-0054 | Purchase Order | Adjustment Value    | Calculation | Medium   | Manual     | Summary | Adjustment Entered          | Verify Adjustment          | ₹50                | 50          | Included in Net Amount    |               | Not Executed |         |
| TC-0055 | Purchase Order | Net Amount          | Calculation | Critical | Manual     | Summary | Complete PO                 | Verify Final Net Amount    | Valid PO           | All Data    | Net Amount Correct        |               | Not Executed |         |

---

# Attachments

| TC ID   | Module         | Feature       | Category   | Priority | Execute By | Page       | Preconditions | Test Scenario            | Test Steps     | Test Data     | Expected Result       | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------- | ---------- | -------- | ---------- | ---------- | ------------- | ------------------------ | -------------- | ------------- | --------------------- | ------------- | ------------ | ------- |
| TC-0056 | Purchase Order | Upload PDF    | Functional | Medium   | Manual     | Attachment | PO Open       | Upload PDF               | Select PDF     | sample.pdf    | Uploaded Successfully |               | Not Executed |         |
| TC-0057 | Purchase Order | Upload Image  | Functional | Medium   | Manual     | Attachment | PO Open       | Upload Image             | Select JPG     | image.jpg     | Uploaded Successfully |               | Not Executed |         |
| TC-0058 | Purchase Order | Invalid File  | Validation | Medium   | Manual     | Attachment | PO Open       | Upload Invalid Extension | .exe           | test.exe      | Validation Displayed  |               | Not Executed |         |
| TC-0059 | Purchase Order | Download File | Functional | Medium   | Manual     | Attachment | File Uploaded | Download Attachment      | Click Download | Uploaded File | File Downloaded       |               | Not Executed |         |
| TC-0060 | Purchase Order | Delete File   | Functional | Medium   | Manual     | Attachment | File Uploaded | Delete Attachment        | Click Delete   | Uploaded File | Attachment Deleted    |               | Not Executed |         |

---

# Search & Filters

| TC ID   | Module         | Feature             | Category   | Priority | Execute By | Page    | Preconditions          | Test Scenario       | Test Steps   | Test Data     | Expected Result                 | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------------- | ---------- | -------- | ---------- | ------- | ---------------------- | ------------------- | ------------ | ------------- | ------------------------------- | ------------- | ------------ | ------- |
| TC-0061 | Purchase Order | Search Number       | Functional | High     | Manual     | PO List | Existing PO            | Search By PO Number | Enter Number | PO-001        | Correct Record Found            |               | Not Executed |         |
| TC-0062 | Purchase Order | Search Vendor       | Functional | Medium   | Manual     | PO List | Existing Vendor        | Search Vendor       | Vendor Name  | ABC Traders   | Correct Records Displayed       |               | Not Executed |         |
| TC-0063 | Purchase Order | Filter Date         | Functional | Medium   | Manual     | PO List | Existing Records       | Filter By Date      | Date Range   | Current Month | Correct Records Displayed       |               | Not Executed |         |
| TC-0064 | Purchase Order | Filter Stage        | Functional | Medium   | Manual     | PO List | Existing Records       | Filter By Stage     | Approved     | Approved      | Only Approved Records Displayed |               | Not Executed |         |
| TC-0065 | Purchase Order | Filter Organisation | Functional | Medium   | Manual     | PO List | Multiple Organisations | Filter Organisation | Org A        | Org A         | Only Org A Records Displayed    |               | Not Executed |         |

---

# Export & UI

| TC ID   | Module         | Feature          | Category   | Priority | Execute By | Page    | Preconditions     | Test Scenario      | Test Steps   | Test Data | Expected Result | Actual Result | Status       | Remarks |
| ------- | -------------- | ---------------- | ---------- | -------- | ---------- | ------- | ----------------- | ------------------ | ------------ | --------- | --------------- | ------------- | ------------ | ------- |
| TC-0066 | Purchase Order | Export Excel     | Functional | Medium   | Manual     | PO List | Records Available | Export Excel       | Click Export | Excel     | Excel Generated |               | Not Executed |         |
| TC-0067 | Purchase Order | Column Selection | UI         | Low      | Manual     | PO List | PO List Open      | Hide Columns       | Hide Vendor  | Vendor    | Column Hidden   |               | Not Executed |         |
| TC-0068 | Purchase Order | Full Screen      | UI         | Low      | Manual     | PO List | PO List Open      | Toggle Full Screen | Click Icon   | —         | Screen Expanded |               | Not Executed |         |
| TC-0069 | Purchase Order | Sorting          | UI         | Medium   | Manual     | PO List | Records Available | Sort By Date       | Click Header | Date      | Records Sorted  |               | Not Executed |         |
| TC-0070 | Purchase Order | Pagination       | UI         | Low      | Manual     | PO List | 50+ Records       | Verify Pagination  | Next Page    | —         | Records Loaded  |               | Not Executed |         |

---

# Audit & Timeline

| TC ID   | Module         | Feature       | Category   | Priority | Execute By | Page     | Preconditions    | Test Scenario            | Test Steps   | Test Data   | Expected Result      | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------- | ---------- | -------- | ---------- | -------- | ---------------- | ------------------------ | ------------ | ----------- | -------------------- | ------------- | ------------ | ------- |
| TC-0071 | Purchase Order | Timeline      | Functional | Medium   | Manual     | Workflow | PO Modified      | Verify Timeline Entry    | Save Changes | Qty Changed | Timeline Updated     |               | Not Executed |         |
| TC-0072 | Purchase Order | Stage History | Functional | Medium   | Manual     | Workflow | Workflow Updated | Verify Stage Logs        | Submit PO    | —           | History Recorded     |               | Not Executed |         |
| TC-0073 | Purchase Order | Audit Log     | Database   | High     | Manual     | Database | PO Updated       | Verify Audit Entry       | Modify PO    | Qty Changed | Audit Record Created |               | Not Executed |         |
| TC-0074 | Purchase Order | Created By    | Database   | Medium   | Manual     | Database | New PO           | Verify Created User      | Save PO      | User A      | Correct User Stored  |               | Not Executed |         |
| TC-0075 | Purchase Order | Updated At    | Database   | Medium   | Manual     | Database | Modify PO        | Verify Updated Timestamp | Save Changes | —           | Timestamp Updated    |               | Not Executed |         |

# Security & Permissions

| TC ID   | Module         | Feature                  | Category | Priority | Execute By | Page           | Preconditions                    | Test Scenario                   | Test Steps   | Test Data       | Expected Result            | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------------------ | -------- | -------- | ---------- | -------------- | -------------------------------- | ------------------------------- | ------------ | --------------- | -------------------------- | ------------- | ------------ | ------- |
| TC-0076 | Purchase Order | View Permission          | Security | Critical | Manual     | PO List        | User Without View Permission     | Verify View Restriction         | Login        | Restricted User | Purchase Order Not Visible |               | Not Executed |         |
| TC-0077 | Purchase Order | Add Permission           | Security | Critical | Manual     | PO List        | User Without Add Permission      | Verify Add Restriction          | Click Add    | Restricted User | Add Button Hidden/Disabled |               | Not Executed |         |
| TC-0078 | Purchase Order | Modify Permission        | Security | Critical | Manual     | Purchase Order | User Without Modify Permission   | Verify Edit Restriction         | Open PO      | Restricted User | Edit Not Allowed           |               | Not Executed |         |
| TC-0079 | Purchase Order | Delete Permission        | Security | Critical | Manual     | Purchase Order | User Without Delete Permission   | Verify Delete Restriction       | Click Delete | Restricted User | Delete Not Allowed         |               | Not Executed |         |
| TC-0080 | Purchase Order | Cancel Permission        | Security | High     | Manual     | Purchase Order | User Without Cancel Permission   | Verify Cancel Restriction       | Click Cancel | Restricted User | Cancel Not Allowed         |               | Not Executed |         |
| TC-0081 | Purchase Order | Export Permission        | Security | Medium   | Manual     | PO List        | User Without Export Permission   | Verify Export Restriction       | Click Export | Restricted User | Export Not Allowed         |               | Not Executed |         |
| TC-0082 | Purchase Order | Stage Permission         | Workflow | Critical | Manual     | Purchase Order | User Has Limited Workflow Access | Verify Stage Change Restriction | Change Stage | Restricted User | Stage Change Denied        |               | Not Executed |         |
| TC-0083 | Purchase Order | Branch Restriction       | Security | High     | Manual     | PO List        | Multi Branch User                | Verify Branch Data Restriction  | Login        | Branch A        | Only Branch A Data Visible |               | Not Executed |         |
| TC-0084 | Purchase Order | Organisation Restriction | Security | High     | Manual     | PO List        | Multi Organisation User          | Verify Organisation Restriction | Login        | Org A           | Only Org A Data Visible    |               | Not Executed |         |
| TC-0085 | Purchase Order | API Authorization        | Security | Critical | Manual     | API            | Invalid Token                    | Verify Unauthorized Access      | Call API     | Invalid Token   | HTTP 401 Returned          |               | Not Executed |         |

---

# Database Validation

| TC ID   | Module         | Feature           | Category | Priority | Execute By | Page     | Preconditions        | Test Scenario            | Test Steps | Test Data     | Expected Result              | Actual Result | Status       | Remarks |
| ------- | -------------- | ----------------- | -------- | -------- | ---------- | -------- | -------------------- | ------------------------ | ---------- | ------------- | ---------------------------- | ------------- | ------------ | ------- |
| TC-0086 | Purchase Order | Header Data       | Database | Critical | Manual     | Database | PO Saved             | Verify Header Saved      | Save PO    | Valid Data    | Header Stored Correctly      |               | Not Executed |         |
| TC-0087 | Purchase Order | Product Array     | Database | Critical | Manual     | Database | Multiple Products    | Verify Product Array     | Save PO    | 5 Products    | All Product Rows Saved       |               | Not Executed |         |
| TC-0088 | Purchase Order | Delivery Schedule | Database | High     | Manual     | Database | Delivery Added       | Verify Delivery Schedule | Save PO    | Delivery Rows | Delivery Data Stored         |               | Not Executed |         |
| TC-0089 | Purchase Order | Payment Terms     | Database | High     | Manual     | Database | Payment Terms Filled | Verify Payment Map       | Save PO    | Payment Terms | Payment Data Stored          |               | Not Executed |         |
| TC-0090 | Purchase Order | Summary Fields    | Database | High     | Manual     | Database | PO Saved             | Verify Summary Values    | Save PO    | Valid PO      | Summary Calculated Correctly |               | Not Executed |         |

---

# Integration Testing

| TC ID   | Module         | Feature           | Category    | Priority | Execute By | Page             | Preconditions | Test Scenario             | Test Steps      | Test Data    | Expected Result              | Actual Result | Status       | Remarks |
| ------- | -------------- | ----------------- | ----------- | -------- | ---------- | ---------------- | ------------- | ------------------------- | --------------- | ------------ | ---------------------------- | ------------- | ------------ | ------- |
| TC-0091 | Purchase Order | GRN Integration   | Integration | Critical | Manual     | GRN              | Approved PO   | Create GRN                | Select PO       | Approved PO  | PO Available In GRN          |               | Not Executed |         |
| TC-0092 | Purchase Order | Pending Quantity  | Integration | Critical | Manual     | GRN              | Partial GRN   | Verify Pending Qty Update | Receive 50/100  | Qty 50       | Pending Qty Updated          |               | Not Executed |         |
| TC-0093 | Purchase Order | Received Quantity | Integration | Critical | Manual     | GRN              | Partial GRN   | Verify Received Qty       | Receive 50      | Qty 50       | Received Qty Updated         |               | Not Executed |         |
| TC-0094 | Purchase Order | Complete Receipt  | Integration | High     | Manual     | GRN              | Full Receipt  | Verify Full Receipt       | Receive All Qty | Qty 100      | Pending Qty = 0              |               | Not Executed |         |
| TC-0095 | Purchase Order | Purchase Invoice  | Integration | High     | Manual     | Purchase Invoice | GRN Completed | Verify Invoice Creation   | Create Invoice  | Approved GRN | Invoice Created Successfully |               | Not Executed |         |

---

# Negative Test Cases

| TC ID   | Module         | Feature           | Category | Priority | Execute By | Page           | Preconditions  | Test Scenario          | Test Steps   | Test Data                               | Expected Result                 | Actual Result | Status       | Remarks |
| ------- | -------------- | ----------------- | -------- | -------- | ---------- | -------------- | -------------- | ---------------------- | ------------ | --------------------------------------- | ------------------------------- | ------------- | ------------ | ------- |
| TC-0096 | Purchase Order | Blank Save        | Negative | Critical | Manual     | Purchase Order | New PO         | Save Without Data      | Click Save   | Blank                                   | Mandatory Validations Displayed |               | Not Executed |         |
| TC-0097 | Purchase Order | Invalid Quantity  | Negative | High     | Manual     | Product        | Product Added  | Enter Alphabet         | ABC          | Validation Displayed                    |                                 | Not Executed  |              |         |
| TC-0098 | Purchase Order | Invalid Rate      | Negative | High     | Manual     | Product        | Product Added  | Enter Text             | XYZ          | Validation Displayed                    |                                 | Not Executed  |              |         |
| TC-0099 | Purchase Order | Invalid Tax       | Negative | Medium   | Manual     | Product        | Product Added  | Remove Tax Config      | Invalid Tax  | Tax Validation Displayed                |                                 | Not Executed  |              |         |
| TC-0100 | Purchase Order | Duplicate Product | Negative | Medium   | Manual     | Product        | Product Exists | Add Same Product Again | Same Product | Duplicate Handling As Per Business Rule |                                 | Not Executed  |              |         |

---

# Performance Testing

| TC ID   | Module         | Feature            | Category    | Priority | Execute By | Page           | Preconditions        | Test Scenario  | Test Steps   | Test Data                     | Expected Result | Actual Result | Status | Remarks |
| ------- | -------------- | ------------------ | ----------- | -------- | ---------- | -------------- | -------------------- | -------------- | ------------ | ----------------------------- | --------------- | ------------- | ------ | ------- |
| TC-0101 | Purchase Order | Save Performance   | Performance | Medium   | Manual     | Purchase Order | Valid PO             | Save PO        | Normal PO    | Saved Within SLA              |                 | Not Executed  |        |         |
| TC-0102 | Purchase Order | Product Loading    | Performance | Medium   | Manual     | Product Lookup | Large Product Master | Search Product | Product Name | Results Within SLA            |                 | Not Executed  |        |         |
| TC-0103 | Purchase Order | Vendor Lookup      | Performance | Medium   | Manual     | Vendor Lookup  | 10K Vendors          | Search Vendor  | Vendor Name  | Lookup Performs Efficiently   |                 | Not Executed  |        |         |
| TC-0104 | Purchase Order | List Loading       | Performance | Medium   | Manual     | PO List        | 100K Records         | Open List      | —            | List Loads Within SLA         |                 | Not Executed  |        |         |
| TC-0105 | Purchase Order | Export Performance | Performance | Low      | Manual     | PO List        | Large Dataset        | Export Excel   | 10K Records  | Export Completes Successfully |                 | Not Executed  |        |         |

---

# Edge Cases

| TC ID   | Module         | Feature          | Category  | Priority | Execute By | Page            | Preconditions   | Test Scenario             | Test Steps    | Test Data                   | Expected Result | Actual Result | Status | Remarks |
| ------- | -------------- | ---------------- | --------- | -------- | ---------- | --------------- | --------------- | ------------------------- | ------------- | --------------------------- | --------------- | ------------- | ------ | ------- |
| TC-0106 | Purchase Order | Maximum Products | Edge Case | High     | Manual     | Product Details | Add Products    | Add 1000 Products         | 1000 Products | System Handles Successfully |                 | Not Executed  |        |         |
| TC-0107 | Purchase Order | Maximum Quantity | Edge Case | High     | Manual     | Product Details | Product Added   | Enter Very Large Quantity | 999999999     | Saved Successfully          |                 | Not Executed  |        |         |
| TC-0108 | Purchase Order | Decimal Quantity | Edge Case | Medium   | Manual     | Product Details | Decimal Allowed | Enter Decimal Qty         | 10.555        | Saved Correctly             |                 | Not Executed  |        |         |
| TC-0109 | Purchase Order | Large Discount   | Edge Case | Medium   | Manual     | Product Details | Product Added   | Apply Maximum Discount    | 100%          | Business Rule Applied       |                 | Not Executed  |        |         |
| TC-0110 | Purchase Order | Large Attachment | Edge Case | Medium   | Manual     | Attachment      | Upload Allowed  | Upload Large File         | Max Size      | Upload Rule Applied         |                 | Not Executed  |        |         |

---

# Audit & Data Integrity

| TC ID   | Module         | Feature           | Category | Priority | Execute By | Page     | Preconditions    | Test Scenario                 | Test Steps      | Test Data              | Expected Result           | Actual Result | Status       | Remarks |
| ------- | -------------- | ----------------- | -------- | -------- | ---------- | -------- | ---------------- | ----------------------------- | --------------- | ---------------------- | ------------------------- | ------------- | ------------ | ------- |
| TC-0111 | Purchase Order | Audit Entry       | Audit    | High     | Manual     | Database | Modify PO        | Verify Audit Record           | Change Quantity | Qty Updated            | Audit Log Created         |               | Not Executed |         |
| TC-0112 | Purchase Order | Timeline Entry    | Audit    | Medium   | Manual     | Workflow | Stage Changed    | Verify Timeline               | Submit PO       | Approved               | Timeline Updated          |               | Not Executed |         |
| TC-0113 | Purchase Order | Created Timestamp | Database | Medium   | Manual     | Database | New PO           | Verify Created Time           | Save PO         | Valid PO               | Correct Timestamp Stored  |               | Not Executed |         |
| TC-0114 | Purchase Order | Updated Timestamp | Database | Medium   | Manual     | Database | Modify PO        | Verify Updated Time           | Save Changes    | Valid PO               | Updated Timestamp Changed |               | Not Executed |         |
| TC-0115 | Purchase Order | Data Consistency  | Database | Critical | Manual     | Database | Save Complete PO | Verify All Tables/Collections | Complete PO     | No Data Mismatch Found |                           | Not Executed  |              |         |

# Real World Business Scenarios

| TC ID   | Module         | Feature         | Category          | Priority | Execute By | Page | Preconditions | Test Scenario                  | Test Steps        | Test Data | Expected Result                 | Actual Result | Status       | Remarks |
| ------- | -------------- | --------------- | ----------------- | -------- | ---------- | ---- | ------------- | ------------------------------ | ----------------- | --------- | ------------------------------- | ------------- | ------------ | ------- |
| TC-0116 | Purchase Order | Partial Receipt | Business Scenario | Critical | Manual     | GRN  | Approved PO   | Receive Partial Quantity       | Receive 50 of 100 | Qty=50    | Received Qty=50, Pending Qty=50 |               | Not Executed |         |
| TC-0117 | Purchase Order | Full Receipt    | Business Scenario | Critical | Manual     | GRN  | Approved PO   | Receive Full Quantity          | Receive 100       | Qty=100   | Pending Qty becomes 0           |               | Not Executed |         |
| TC-0118 | Purchase Order | Multiple GRN    | Business Scenario | Critical | Manual     | GRN  | Approved PO   | Receive Goods In Multiple GRNs | Create 3 GRNs     | 30+30+40  | PO Updated Correctly            |               | Not Executed |         |
| TC-0119 | Purchase Order | Over Receipt    | Validation        | High     | Manual     | GRN  | Approved PO   | Receive More Than Ordered      | Receive 110       | Qty=110   | Validation Displayed            |               | Not Executed |         |
| TC-0120 | Purchase Order | Short Receipt   | Business Scenario | High     | Manual     | GRN  | Approved PO   | Receive Less Quantity          | Receive 95        | Qty=95    | Pending Qty=5                   |               | Not Executed |         |

---

# Purchase Invoice Integration

| TC ID   | Module         | Feature                  | Category    | Priority | Execute By | Page             | Preconditions    | Test Scenario            | Test Steps    | Test Data   | Expected Result                 | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------------------ | ----------- | -------- | ---------- | ---------------- | ---------------- | ------------------------ | ------------- | ----------- | ------------------------------- | ------------- | ------------ | ------- |
| TC-0121 | Purchase Order | Invoice Against PO       | Integration | Critical | Manual     | Purchase Invoice | GRN Completed    | Create Purchase Invoice  | Select PO     | Approved PO | Invoice Created Successfully    |               | Not Executed |         |
| TC-0122 | Purchase Order | Partial Invoice          | Integration | High     | Manual     | Purchase Invoice | Partial GRN      | Invoice Partial Quantity | Qty=50        | 50          | Invoice Created For Partial Qty |               | Not Executed |         |
| TC-0123 | Purchase Order | Multiple Invoice         | Integration | High     | Manual     | Purchase Invoice | Multiple GRNs    | Create Multiple Invoices | Multiple GRNs | Valid Data  | Invoice Linked Successfully     |               | Not Executed |         |
| TC-0124 | Purchase Order | Invoice Value Validation | Validation  | High     | Manual     | Purchase Invoice | Invoice Creation | Verify Invoice Amount    | Compare PO    | Values      | Amount Matches PO               |               | Not Executed |         |
| TC-0125 | Purchase Order | Closed PO                | Validation  | High     | Manual     | Purchase Invoice | Fully Received   | Try New Invoice          | Closed PO     | Existing PO | Validation Displayed            |               | Not Executed |         |

---

# Vendor Scenarios

| TC ID   | Module         | Feature         | Category      | Priority | Execute By | Page           | Preconditions           | Test Scenario      | Test Steps      | Test Data              | Expected Result       | Actual Result | Status       | Remarks |
| ------- | -------------- | --------------- | ------------- | -------- | ---------- | -------------- | ----------------------- | ------------------ | --------------- | ---------------------- | --------------------- | ------------- | ------------ | ------- |
| TC-0126 | Purchase Order | Inactive Vendor | Validation    | Critical | Manual     | Purchase Order | Vendor Inactive         | Select Vendor      | Inactive Vendor | Vendor                 | Selection Not Allowed |               | Not Executed |         |
| TC-0127 | Purchase Order | Blocked Vendor  | Validation    | Critical | Manual     | Purchase Order | Vendor Blocked          | Select Vendor      | Blocked Vendor  | Vendor                 | Validation Displayed  |               | Not Executed |         |
| TC-0128 | Purchase Order | Credit Limit    | Business Rule | High     | Manual     | Purchase Order | Vendor Has Credit Limit | Create Large PO    | ₹5,00,000       | Warning Displayed      |                       | Not Executed  |              |         |
| TC-0129 | Purchase Order | Credit Days     | Business Rule | Medium   | Manual     | Purchase Order | Vendor Configured       | Verify Credit Days | Vendor A        | Vendor                 | Credit Days Loaded    |               | Not Executed |         |
| TC-0130 | Purchase Order | Currency        | Functional    | Medium   | Manual     | Purchase Order | Foreign Vendor          | Select Vendor      | USD Vendor      | Currency Auto Selected |                       | Not Executed  |              |         |

---

# Product Scenarios

| TC ID   | Module         | Feature           | Category   | Priority | Execute By | Page            | Preconditions    | Test Scenario         | Test Steps       | Test Data | Expected Result          | Actual Result | Status       | Remarks |
| ------- | -------------- | ----------------- | ---------- | -------- | ---------- | --------------- | ---------------- | --------------------- | ---------------- | --------- | ------------------------ | ------------- | ------------ | ------- |
| TC-0131 | Purchase Order | Inactive Product  | Validation | Critical | Manual     | Product Details | Product Inactive | Select Product        | Inactive Product | Product   | Selection Not Allowed    |               | Not Executed |         |
| TC-0132 | Purchase Order | Product Variety   | Functional | Medium   | Manual     | Product Details | Product Selected | Verify Variety Filter | Select Product   | Product A | Correct Varieties Loaded |               | Not Executed |         |
| TC-0133 | Purchase Order | Product Pack      | Functional | Medium   | Manual     | Product Details | Product Selected | Verify Pack Selection | Select Pack      | Box       | Pack Stored Correctly    |               | Not Executed |         |
| TC-0134 | Purchase Order | UOM               | Functional | Medium   | Manual     | Product Details | Product Selected | Verify UOM            | Select Product   | KG        | UOM Displayed Correctly  |               | Not Executed |         |
| TC-0135 | Purchase Order | Tax Configuration | Validation | High     | Manual     | Product Details | Product Selected | Verify Product Tax    | GST Product      | GST       | Tax Loaded Automatically |               | Not Executed |         |

---

# Delivery Schedule Scenarios

| TC ID   | Module         | Feature             | Category      | Priority | Execute By | Page              | Preconditions   | Test Scenario                  | Test Steps      | Test Data     | Expected Result             | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------------- | ------------- | -------- | ---------- | ----------------- | --------------- | ------------------------------ | --------------- | ------------- | --------------------------- | ------------- | ------------ | ------- |
| TC-0136 | Purchase Order | Multiple Warehouses | Functional    | High     | Manual     | Delivery Schedule | Product Added   | Deliver To Multiple Warehouses | 3 Warehouses    | Different Qty | Schedule Saved Successfully |               | Not Executed |         |
| TC-0137 | Purchase Order | Delivery Priority   | Functional    | Medium   | Manual     | Delivery Schedule | Schedule Exists | Verify Priority                | High            | High          | Priority Saved              |               | Not Executed |         |
| TC-0138 | Purchase Order | Expected Date       | Validation    | High     | Manual     | Delivery Schedule | Schedule Exists | Verify Expected Date           | Select Date     | Future Date   | Saved Successfully          |               | Not Executed |         |
| TC-0139 | Purchase Order | Actual Delivery     | Integration   | Medium   | Manual     | GRN               | GRN Completed   | Verify Actual Date             | Create GRN      | Today         | Actual Date Updated         |               | Not Executed |         |
| TC-0140 | Purchase Order | Pending Schedule    | Business Rule | Medium   | Manual     | Delivery Schedule | Partial Receipt | Verify Pending Qty             | Receive Partial | Qty           | Pending Updated             |               | Not Executed |         |

---

# Multi Currency & Finance

| TC ID   | Module         | Feature                | Category    | Priority | Execute By | Page           | Preconditions       | Test Scenario         | Test Steps | Test Data    | Expected Result      | Actual Result | Status       | Remarks |
| ------- | -------------- | ---------------------- | ----------- | -------- | ---------- | -------------- | ------------------- | --------------------- | ---------- | ------------ | -------------------- | ------------- | ------------ | ------- |
| TC-0141 | Purchase Order | Exchange Rate          | Functional  | High     | Manual     | Purchase Order | Foreign Currency    | Enter Exchange Rate   | USD        | 86.50        | Net Value Updated    |               | Not Executed |         |
| TC-0142 | Purchase Order | Decimal Currency       | Calculation | Medium   | Manual     | Purchase Order | Foreign Currency    | Verify Decimal        | USD        | 123.456      | Rounded Correctly    |               | Not Executed |         |
| TC-0143 | Purchase Order | TDS                    | Calculation | High     | Manual     | Payment Terms  | TDS Applicable      | Calculate TDS         | 2%         | Valid Amount | TDS Calculated       |               | Not Executed |         |
| TC-0144 | Purchase Order | Payment Schedule       | Functional  | High     | Manual     | Payment Terms  | Multiple Schedule   | Add Multiple Schedule | 40%-60%    | Schedule     | Saved Successfully   |               | Not Executed |         |
| TC-0145 | Purchase Order | Early Payment Discount | Calculation | Medium   | Manual     | Payment Terms  | Discount Configured | Verify Discount       | 5%         | Discount     | Calculated Correctly |               | Not Executed |         |

---

# Approval Workflow

| TC ID   | Module         | Feature              | Category | Priority | Execute By | Page           | Preconditions  | Test Scenario     | Test Steps   | Test Data | Expected Result            | Actual Result | Status       | Remarks |
| ------- | -------------- | -------------------- | -------- | -------- | ---------- | -------------- | -------------- | ----------------- | ------------ | --------- | -------------------------- | ------------- | ------------ | ------- |
| TC-0146 | Purchase Order | Draft → Submitted    | Workflow | Critical | Manual     | Purchase Order | Draft Exists   | Submit PO         | Submit       | Draft     | Stage Updated              |               | Not Executed |         |
| TC-0147 | Purchase Order | Submitted → Approved | Workflow | Critical | Manual     | Purchase Order | Submitted      | Approve           | Approve      | Submitted | Approved Successfully      |               | Not Executed |         |
| TC-0148 | Purchase Order | Submitted → Rejected | Workflow | High     | Manual     | Purchase Order | Submitted      | Reject            | Remarks      | Submitted | Rejected Successfully      |               | Not Executed |         |
| TC-0149 | Purchase Order | Rollback             | Workflow | High     | Manual     | Purchase Order | Approved       | Rollback          | Undo         | Approved  | Previous Stage Restored    |               | Not Executed |         |
| TC-0150 | Purchase Order | Completed            | Workflow | Medium   | Manual     | Purchase Order | Fully Received | Verify Completion | Complete GRN | All Qty   | Stage Updated To Completed |               | Not Executed |         |

---

# Data Integrity

| TC ID   | Module         | Feature                 | Category | Priority | Execute By | Page     | Preconditions    | Test Scenario         | Test Steps        | Test Data   | Expected Result               | Actual Result | Status       | Remarks |
| ------- | -------------- | ----------------------- | -------- | -------- | ---------- | -------- | ---------------- | --------------------- | ----------------- | ----------- | ----------------------------- | ------------- | ------------ | ------- |
| TC-0151 | Purchase Order | Duplicate Number        | Database | Critical | Manual     | Database | Existing PO      | Force Duplicate       | Existing Number   | Duplicate   | Save Not Allowed              |               | Not Executed |         |
| TC-0152 | Purchase Order | Foreign Key             | Database | High     | Manual     | Database | Save PO          | Verify FK             | Vendor            | Vendor Id   | Valid Reference Stored        |               | Not Executed |         |
| TC-0153 | Purchase Order | Product Array Integrity | Database | High     | Manual     | Database | Save PO          | Verify Product Array  | Multiple Products | Products    | Data Stored Correctly         |               | Not Executed |         |
| TC-0154 | Purchase Order | Payment Map Integrity   | Database | High     | Manual     | Database | Save PO          | Verify Payment Terms  | Payment Data      | Payment Map | Stored Correctly              |               | Not Executed |         |
| TC-0155 | Purchase Order | Summary Integrity       | Database | Critical | Manual     | Database | Save Complete PO | Verify Summary Fields | Valid PO          | Summary     | All Totals Match Product Data |               | Not Executed |         |

# Amendment & Revision Testing

| TC ID   | Module         | Feature              | Category   | Priority | Execute By | Page           | Preconditions           | Test Scenario          | Test Steps      | Test Data                             | Expected Result                | Actual Result | Status       | Remarks |
| ------- | -------------- | -------------------- | ---------- | -------- | ---------- | -------------- | ----------------------- | ---------------------- | --------------- | ------------------------------------- | ------------------------------ | ------------- | ------------ | ------- |
| TC-0156 | Purchase Order | PO Amendment         | Functional | Critical | Manual     | Purchase Order | Approved PO             | Amend Approved PO      | Modify Quantity | Qty 120                               | Amendment Created Successfully |               | Not Executed |         |
| TC-0157 | Purchase Order | Revision Number      | Database   | High     | Manual     | Purchase Order | Amendment Created       | Verify Revision Number | Save Amendment  | Rev-2                                 | Revision Incremented           |               | Not Executed |         |
| TC-0158 | Purchase Order | Amendment History    | Audit      | High     | Manual     | Timeline       | PO Revised              | Verify Timeline        | Open Timeline   | Revised PO                            | Amendment History Recorded     |               | Not Executed |         |
| TC-0159 | Purchase Order | Amendment Permission | Security   | High     | Manual     | Purchase Order | User Without Permission | Amend PO               | Modify PO       | Restricted User                       | Amendment Not Allowed          |               | Not Executed |         |
| TC-0160 | Purchase Order | Amendment After GRN  | Validation | Critical | Manual     | Purchase Order | Partial GRN Exists      | Modify Ordered Qty     | Qty Reduced     | Validation According To Business Rule |                                | Not Executed  |              |         |

---

# Purchase Return Integration

| TC ID   | Module         | Feature                  | Category      | Priority | Execute By | Page            | Preconditions    | Test Scenario           | Test Steps     | Test Data            | Expected Result                   | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------------------ | ------------- | -------- | ---------- | --------------- | ---------------- | ----------------------- | -------------- | -------------------- | --------------------------------- | ------------- | ------------ | ------- |
| TC-0161 | Purchase Order | Purchase Return Link     | Integration   | Critical | Manual     | Purchase Return | GRN Exists       | Create Purchase Return  | Return Product | Qty 10               | Return Linked To Original PO      |               | Not Executed |         |
| TC-0162 | Purchase Order | Returned Quantity        | Integration   | High     | Manual     | Purchase Return | Existing Return  | Verify Returned Qty     | Return Qty     | Qty 20               | Returned Qty Updated              |               | Not Executed |         |
| TC-0163 | Purchase Order | Pending Qty After Return | Business Rule | High     | Manual     | Purchase Return | Partial Return   | Verify Pending Qty      | Return Qty     | Qty 10               | Pending Updated Correctly         |               | Not Executed |         |
| TC-0164 | Purchase Order | Multiple Returns         | Integration   | High     | Manual     | Purchase Return | Multiple Returns | Create Multiple Returns | Qty 5+5        | Total Return Updated |                                   | Not Executed  |              |         |
| TC-0165 | Purchase Order | Return Value             | Calculation   | High     | Manual     | Purchase Return | Return Created   | Verify Return Amount    | Qty × Rate     | Valid Data           | Return Value Calculated Correctly |               | Not Executed |         |

---

# Vendor Payment Integration

| TC ID   | Module         | Feature            | Category    | Priority | Execute By | Page           | Preconditions      | Test Scenario             | Test Steps           | Test Data         | Expected Result              | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------------ | ----------- | -------- | ---------- | -------------- | ------------------ | ------------------------- | -------------------- | ----------------- | ---------------------------- | ------------- | ------------ | ------- |
| TC-0166 | Purchase Order | Vendor Payment     | Integration | Critical | Manual     | Vendor Payment | Invoice Approved   | Create Vendor Payment     | Select Invoice       | Valid Invoice     | Payment Created Successfully |               | Not Executed |         |
| TC-0167 | Purchase Order | Outstanding Amount | Integration | High     | Manual     | Vendor Payment | Partial Payment    | Verify Outstanding Amount | Pay ₹50,000          | Invoice ₹1,00,000 | Outstanding ₹50,000          |               | Not Executed |         |
| TC-0168 | Purchase Order | Full Payment       | Integration | High     | Manual     | Vendor Payment | Outstanding Exists | Pay Remaining Amount      | ₹50,000              | Outstanding       | Outstanding Becomes Zero     |               | Not Executed |         |
| TC-0169 | Purchase Order | Payment Status     | Integration | Medium   | Manual     | Vendor Payment | Payment Completed  | Verify Status             | Refresh PO           | Paid              | Payment Status Updated       |               | Not Executed |         |
| TC-0170 | Purchase Order | Payment History    | Audit       | Medium   | Manual     | Vendor Payment | Multiple Payments  | Verify History            | Open Payment History | Multiple Payments | History Available            |               | Not Executed |         |

---

# Payment Adjustment Integration

| TC ID   | Module         | Feature                    | Category      | Priority | Execute By | Page               | Preconditions          | Test Scenario        | Test Steps          | Test Data   | Expected Result             | Actual Result | Status       | Remarks |
| ------- | -------------- | -------------------------- | ------------- | -------- | ---------- | ------------------ | ---------------------- | -------------------- | ------------------- | ----------- | --------------------------- | ------------- | ------------ | ------- |
| TC-0171 | Purchase Order | Adjustment Against Invoice | Integration   | Critical | Manual     | Payment Adjustment | Vendor Credit Exists   | Create Adjustment    | Select Bills        | Valid Bills | Adjustment Saved            |               | Not Executed |         |
| TC-0172 | Purchase Order | Vendor Credit              | Integration   | High     | Manual     | Payment Adjustment | Purchase Return Exists | Adjust Credit        | Select Return       | Credit Note | Credit Applied Successfully |               | Not Executed |         |
| TC-0173 | Purchase Order | Net Payable                | Calculation   | High     | Manual     | Payment Adjustment | Credit Exists          | Verify Net Payable   | Create Adjustment   | Valid Data  | Net Payable Correct         |               | Not Executed |         |
| TC-0174 | Purchase Order | Settlement Status          | Business Rule | Medium   | Manual     | Payment Adjustment | Fully Adjusted         | Verify Settlement    | Complete Adjustment | ₹0 Balance  | Status Updated              |               | Not Executed |         |
| TC-0175 | Purchase Order | Ledger Update              | Integration   | Critical | Manual     | Payment Adjustment | Adjustment Saved       | Verify Vendor Ledger | Open Ledger         | Vendor      | Ledger Updated Successfully |               | Not Executed |         |

---

# Notification & Communication

| TC ID   | Module         | Feature                 | Category     | Priority | Execute By | Page           | Preconditions        | Test Scenario | Test Steps | Test Data         | Expected Result       | Actual Result | Status       | Remarks |
| ------- | -------------- | ----------------------- | ------------ | -------- | ---------- | -------------- | -------------------- | ------------- | ---------- | ----------------- | --------------------- | ------------- | ------------ | ------- |
| TC-0176 | Purchase Order | PO Created Notification | Notification | Medium   | Manual     | Purchase Order | Notification Enabled | Create PO     | Save PO    | Valid Data        | Notification Sent     |               | Not Executed |         |
| TC-0177 | Purchase Order | Approval Notification   | Notification | Medium   | Manual     | Workflow       | Submit PO            | Approve PO    | Approved   | Notification Sent |                       | Not Executed  |              |         |
| TC-0178 | Purchase Order | Rejection Notification  | Notification | Medium   | Manual     | Workflow       | Reject PO            | Reject        | Remarks    | Notification Sent |                       | Not Executed  |              |         |
| TC-0179 | Purchase Order | Email Template          | Notification | Medium   | Manual     | Notification   | Email Enabled        | Verify Email  | Create PO  | Valid Email       | Email Content Correct |               | Not Executed |         |
| TC-0180 | Purchase Order | SMS / WhatsApp          | Notification | Low      | Manual     | Notification   | SMS Enabled          | Verify SMS    | Create PO  | Mobile No         | SMS Delivered         |               | Not Executed |         |

---

# Concurrency Testing

| TC ID   | Module         | Feature             | Category    | Priority | Execute By | Page           | Preconditions            | Test Scenario          | Test Steps      | Test Data          | Expected Result            | Actual Result | Status       | Remarks |
| ------- | -------------- | ------------------- | ----------- | -------- | ---------- | -------------- | ------------------------ | ---------------------- | --------------- | ------------------ | -------------------------- | ------------- | ------------ | ------- |
| TC-0181 | Purchase Order | Concurrent Edit     | Concurrency | Critical | Manual     | Purchase Order | Same PO Opened Twice     | Modify Same PO         | User A & User B | Same PO            | Conflict Handled Correctly |               | Not Executed |         |
| TC-0182 | Purchase Order | Concurrent Approval | Concurrency | High     | Manual     | Workflow       | Two Approvers            | Approve Simultaneously | Two Users       | Same PO            | Single Approval Accepted   |               | Not Executed |         |
| TC-0183 | Purchase Order | Concurrent Delete   | Concurrency | High     | Manual     | Purchase Order | Two Users                | Delete Simultaneously  | Same PO         | Same Record        | Safe Handling              |               | Not Executed |         |
| TC-0184 | Purchase Order | Duplicate Save      | Concurrency | High     | Manual     | Purchase Order | Double Click Save        | Save Multiple Times    | Rapid Click     | One PO Created     |                            | Not Executed  |              |         |
| TC-0185 | Purchase Order | Auto Refresh        | Concurrency | Medium   | Manual     | Purchase Order | Another User Modified PO | Refresh Record         | Existing PO     | Latest Data Loaded |                            | Not Executed  |              |         |

---

# Data Recovery

| TC ID   | Module         | Feature         | Category | Priority | Execute By | Page           | Preconditions      | Test Scenario          | Test Steps   | Test Data         | Expected Result       | Actual Result | Status       | Remarks |
| ------- | -------------- | --------------- | -------- | -------- | ---------- | -------------- | ------------------ | ---------------------- | ------------ | ----------------- | --------------------- | ------------- | ------------ | ------- |
| TC-0186 | Purchase Order | Network Failure | Recovery | High     | Manual     | Purchase Order | Internet Connected | Disconnect During Save | Save PO      | Valid Data        | Proper Error Handling |               | Not Executed |         |
| TC-0187 | Purchase Order | Session Timeout | Recovery | Medium   | Manual     | Purchase Order | Idle Session       | Save After Timeout     | Valid Data   | Login Requested   |                       | Not Executed  |              |         |
| TC-0188 | Purchase Order | Browser Refresh | Recovery | Medium   | Manual     | Purchase Order | Unsaved Changes    | Refresh Browser        | Unsaved Data | Warning Displayed |                       | Not Executed  |              |         |
| TC-0189 | Purchase Order | Power Failure   | Recovery | Medium   | Manual     | Purchase Order | Draft Exists       | Unexpected Shutdown    | Draft PO     | Draft Recoverable |                       | Not Executed  |              |         |
| TC-0190 | Purchase Order | Server Restart  | Recovery | Medium   | Manual     | Purchase Order | Server Restarted   | Reopen PO              | Existing PO  | Data Intact       |                       | Not Executed  |              |         |

---

# Miscellaneous

| TC ID   | Module         | Feature               | Category   | Priority | Execute By | Page           | Preconditions   | Test Scenario                                                    | Test Steps                | Test Data          | Expected Result                                           | Actual Result | Status       | Remarks |
| ------- | -------------- | --------------------- | ---------- | -------- | ---------- | -------------- | --------------- | ---------------------------------------------------------------- | ------------------------- | ------------------ | --------------------------------------------------------- | ------------- | ------------ | ------- |
| TC-0191 | Purchase Order | Print PO              | Functional | Medium   | Manual     | Purchase Order | Approved PO     | Print Purchase Order                                             | Click Print               | Approved PO        | Print Preview Opens                                       |               | Not Executed |         |
| TC-0192 | Purchase Order | PDF Export            | Functional | Medium   | Manual     | Purchase Order | Approved PO     | Export PDF                                                       | Click Export PDF          | Approved PO        | PDF Generated                                             |               | Not Executed |         |
| TC-0193 | Purchase Order | Barcode               | Functional | Low      | Manual     | Purchase Order | Barcode Enabled | Verify Barcode                                                   | Open PO                   | Approved PO        | Barcode Generated                                         |               | Not Executed |         |
| TC-0194 | Purchase Order | QR Code               | Functional | Low      | Manual     | Purchase Order | QR Enabled      | Verify QR                                                        | Open PO                   | Approved PO        | QR Generated                                              |               | Not Executed |         |
| TC-0195 | Purchase Order | Complete PO Lifecycle | End-to-End | Critical | Manual     | Entire Module  | Fresh Database  | Create → Approve → GRN → Invoice → Payment → Adjustment → Return | Complete Procurement Flow | Real Business Data | Entire Lifecycle Completes Successfully Without Data Loss |               | Not Executed |         |
