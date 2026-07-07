# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Accounts
- **Module**: Payment Adjustment

This document contains database design details of Payment Adjustment module. The data includes:

- General Information
- Organisation Information
- Vendor Information
- Currency Information
- Payment References
- Adjustment Details
- Attachments
- Summary Fields
- Adjustment Summary
- Vendor Summary
- Workflow Summary

---

## Collection: payment_adjustment

Contains Vendor Payment Adjustment transaction data.

---

# General Information

| Name                | Type      | Optional | Default Value  | Key     | Reference   | Remarks                                                                                                                  |
| ------------------- | --------- | -------- | -------------- | ------- | ----------- | ------------------------------------------------------------------------------------------------------------------------ |
| `id`                | String    | –        | Auto Generated | Primary | –           | Document Id                                                                                                              |
| `adjustment_number` | String    | –        | Auto Generated | Unique  | –           | PMTADJ Number                                                                                                            |
| `adjustment_date`   | Timestamp | –        | Current Date   | –       | –           | Adjustment Transaction Date                                                                                              |
| `adjustment_type`   | String    | –        | Manual         | –       | –           | Excess Payment / Short Payment / Purchase Return / Credit Note / Debit Note / Manual / Round Off / Ledger Reconciliation |
| `adjustment_status` | String    | Yes      | Pending        | –       | –           | Pending / Partial / Completed / Cancelled                                                                                |
| `stage`             | Map       | –        | Draft          | –       | `Stage` Map | Current Workflow Stage                                                                                                   |
| `remarks`           | String    | Yes      | –              | –       | –           | General Remarks                                                                                                          |

---

# Organisation Information

| Name                | Type   | Optional | Default Value | Key     | Reference           | Remarks           |
| ------------------- | ------ | -------- | ------------- | ------- | ------------------- | ----------------- |
| `organisation_id`   | String | –        | –             | Foreign | m_organisation.`id` | Organisation Id   |
| `organisation_name` | String | –        | –             | –       | –                   | Organisation Name |
| `branch_id`         | String | –        | –             | Foreign | m_branch.`id`       | Branch Id         |
| `branch_name`       | String | –        | –             | –       | –                   | Branch Name       |

---

# Vendor Information

| Name            | Type   | Optional | Default Value | Key     | Reference     | Remarks              |
| --------------- | ------ | -------- | ------------- | ------- | ------------- | -------------------- |
| `vendor_id`     | String | –        | –             | Foreign | m_vendor.`id` | Vendor Id            |
| `vendor_code`   | String | Yes      | –             | –       | –             | Vendor Code          |
| `vendor_name`   | String | –        | –             | –       | –             | Vendor Name          |
| `vendor_mobile` | String | Yes      | –             | –       | –             | Vendor Mobile Number |
| `vendor_email`  | String | Yes      | –             | –       | –             | Vendor Email Address |
| `vendor_gstin`  | String | Yes      | –             | –       | –             | Vendor GST Number    |

---

# Currency Information

| Name              | Type   | Optional | Default Value         | Key     | Reference       | Remarks         |
| ----------------- | ------ | -------- | --------------------- | ------- | --------------- | --------------- |
| `currency_id`     | String | –        | Organisation Currency | Foreign | m_currency.`id` | Currency Id     |
| `currency_name`   | String | Yes      | –                     | –       | –               | Currency Name   |
| `currency_symbol` | String | Yes      | –                     | –       | –               | Currency Symbol |
| `exchange_rate`   | Double | Yes      | 1                     | –       | –               | Exchange Rate   |

# Payment References

| Name                                            | Type      | Optional | Default Value | Key     | Reference         | Remarks                            |
| ----------------------------------------------- | --------- | -------- | ------------- | ------- | ----------------- | ---------------------------------- |
| `payment_references`                            | Array Map | Yes      | –             | –       | –                 | Linked Vendor Payments             |
| payment_references[].`id`                       | String    | –        | –             | –       | –                 | Row Id                             |
| payment_references[].`sequence`                 | Int64     | –        | 0             | –       | –                 | Display Sequence                   |
| payment_references[].`vendor_payment_id`        | String    | Yes      | –             | Foreign | make_payment.`id` | Vendor Payment Id                  |
| payment_references[].`vendor_payment_number`    | String    | Yes      | –             | –       | –                 | Vendor Payment Number              |
| payment_references[].`payment_date`             | Timestamp | Yes      | –             | –       | –                 | Vendor Payment Date                |
| payment_references[].`payment_type`             | String    | Yes      | –             | –       | –                 | Invoice Payment / Advance / Mixed  |
| payment_references[].`total_payment_value`      | Double    | Yes      | 0             | –       | –                 | Vendor Payment Amount              |
| payment_references[].`adjusted_value`           | Double    | Yes      | 0             | –       | –                 | Previously Adjusted Amount         |
| payment_references[].`available_balance`        | Double    | Yes      | 0             | –       | –                 | Available Balance                  |
| payment_references[].`current_adjustment_value` | Double    | Yes      | 0             | –       | –                 | Adjustment Value In Current PMTADJ |
| payment_references[].`remarks`                  | String    | Yes      | –             | –       | –                 | Payment Remarks                    |

---

# Adjustment Details

| Name                                     | Type      | Optional | Default Value | Key | Reference | Remarks                                                                                                                  |
| ---------------------------------------- | --------- | -------- | ------------- | --- | --------- | ------------------------------------------------------------------------------------------------------------------------ |
| `adjustments`                            | Array Map | Yes      | –             | –   | –         | Adjustment Details                                                                                                       |
| adjustments[].`id`                       | String    | –        | –             | –   | –         | Row Id                                                                                                                   |
| adjustments[].`sequence`                 | Int64     | –        | 0             | –   | –         | Display Sequence                                                                                                         |
| adjustments[].`adjustment_type`          | String    | –        | Manual        | –   | –         | Purchase Return / Credit Note / Debit Note / Excess Payment / Short Payment / Manual / Round Off / Ledger Reconciliation |
| adjustments[].`reference_id`             | String    | Yes      | –             | –   | –         | Reference Document Id                                                                                                    |
| adjustments[].`reference_number`         | String    | Yes      | –             | –   | –         | Reference Number                                                                                                         |
| adjustments[].`reference_date`           | Timestamp | Yes      | –             | –   | –         | Reference Date                                                                                                           |
| adjustments[].`original_value`           | Double    | Yes      | 0             | –   | –         | Original Document Value                                                                                                  |
| adjustments[].`adjusted_value`           | Double    | Yes      | 0             | –   | –         | Previously Adjusted Value                                                                                                |
| adjustments[].`available_balance`        | Double    | Yes      | 0             | –   | –         | Available Balance                                                                                                        |
| adjustments[].`current_adjustment_value` | Double    | Yes      | 0             | –   | –         | Adjustment Value In Current PMTADJ                                                                                       |
| adjustments[].`adjustment_reason`        | String    | Yes      | –             | –   | –         | Adjustment Reason                                                                                                        |
| adjustments[].`remarks`                  | String    | Yes      | –             | –   | –         | Remarks                                                                                                                  |

---

# Attachments

| Name                        | Type      | Optional | Default Value | Key     | Reference   | Remarks              |
| --------------------------- | --------- | -------- | ------------- | ------- | ----------- | -------------------- |
| `attachments`               | Array Map | Yes      | –             | –       | –           | Supporting Documents |
| attachments[].`id`          | String    | –        | –             | –       | –           | Row Id               |
| attachments[].`file_name`   | String    | –        | –             | –       | –           | File Name            |
| attachments[].`file_type`   | String    | –        | –             | –       | –           | File Type            |
| attachments[].`file_url`    | String    | –        | –             | –       | –           | File Storage URL     |
| attachments[].`file_size`   | Int64     | Yes      | 0             | –       | –           | File Size (Bytes)    |
| attachments[].`uploaded_by` | String    | Yes      | –             | Foreign | m_user.`id` | Uploaded By User     |
| attachments[].`uploaded_at` | Timestamp | Yes      | –             | –       | –           | Upload Date Time     |
| attachments[].`remarks`     | String    | Yes      | –             | –       | –           | Attachment Remarks   |

---

# Summary Fields

| Name                             | Type   | Optional | Default Value | Key | Reference | Remarks                          |
| -------------------------------- | ------ | -------- | ------------- | --- | --------- | -------------------------------- |
| `total_payment_count`            | Int64  | Yes      | 0             | –   | –         | Total Linked Vendor Payments     |
| `total_payment_value`            | Double | Yes      | 0             | –   | –         | Total Payment Value              |
| `total_adjusted_value`           | Double | Yes      | 0             | –   | –         | Previously Adjusted Value        |
| `total_available_balance`        | Double | Yes      | 0             | –   | –         | Total Available Balance          |
| `total_current_adjustment_value` | Double | Yes      | 0             | –   | –         | Current Adjustment Value         |
| `total_purchase_return_value`    | Double | Yes      | 0             | –   | –         | Purchase Return Adjustment Value |
| `total_credit_note_value`        | Double | Yes      | 0             | –   | –         | Credit Note Adjustment Value     |
| `total_debit_note_value`         | Double | Yes      | 0             | –   | –         | Debit Note Adjustment Value      |
| `total_manual_adjustment_value`  | Double | Yes      | 0             | –   | –         | Manual Adjustment Value          |
| `round_off_value`                | Double | Yes      | 0             | –   | –         | Round Off Value                  |
| `total_net_value`                | Double | Yes      | 0             | –   | –         | Final Adjustment Value           |

---

# Adjustment Summary

| Name                          | Type  | Optional | Default Value | Key | Reference | Remarks                          |
| ----------------------------- | ----- | -------- | ------------- | --- | --------- | -------------------------------- |
| `purchase_return_count`       | Int64 | Yes      | 0             | –   | –         | Total Purchase Return References |
| `credit_note_count`           | Int64 | Yes      | 0             | –   | –         | Total Credit Note References     |
| `debit_note_count`            | Int64 | Yes      | 0             | –   | –         | Total Debit Note References      |
| `manual_adjustment_count`     | Int64 | Yes      | 0             | –   | –         | Manual Adjustment Count          |
| `ledger_reconciliation_count` | Int64 | Yes      | 0             | –   | –         | Ledger Reconciliation Count      |
| `adjustment_reason_count`     | Int64 | Yes      | 0             | –   | –         | Total Adjustment Reasons Used    |

---

# Vendor Summary

| Name                           | Type      | Optional | Default Value | Key | Reference | Remarks                              |
| ------------------------------ | --------- | -------- | ------------- | --- | --------- | ------------------------------------ |
| `vendor_outstanding_value`     | Double    | Yes      | 0             | –   | –         | Outstanding Amount Before Adjustment |
| `vendor_advance_balance_value` | Double    | Yes      | 0             | –   | –         | Vendor Advance Balance               |
| `vendor_credit_limit`          | Double    | Yes      | 0             | –   | –         | Vendor Credit Limit                  |
| `vendor_credit_days`           | Int64     | Yes      | 0             | –   | –         | Vendor Credit Days                   |
| `last_payment_date`            | Timestamp | Yes      | –             | –   | –         | Last Vendor Payment Date             |
| `last_adjustment_date`         | Timestamp | Yes      | –             | –   | –         | Previous Adjustment Date             |
| `last_adjustment_value`        | Double    | Yes      | 0             | –   | –         | Previous Adjustment Value            |

---

# Workflow Summary

| Name | Type | Optional | Default Value | Key | Reference | Remarks |
| -------------------------------- | --------- | -------- | ------------- | ------- | ---------------- | ---------------------------------------- |
| `stage` | Map | – | Draft | – | `Stage` Map | Current Workflow Stage |
| `stage_logs` | Array Map | Yes | – | – | `Stage` Map | Workflow History |
| `created_by` | String | Yes | – | Foreign | m_user.`id` | Created By User |
| `created_at` | Timestamp | Yes | Current Date | – | – | Creation Date |
| `updated_by` | String | Yes | – | Foreign | m_user.`id` | Last Updated User |
| `updated_at` | Timestamp | Yes | Current Date | – | – | Last Updated Date |
| `verified_by` | String | Yes | – | Foreign | m_user.`id` | Verified By User |
| `verified_at` | Timestamp | Yes | – | – | – | Verification Date |
| `approved_by` | String | Yes | – | Foreign | m_user.`id` | Approved By User |
| `approved_at` | Timestamp | Yes | – | – | – | Approval Date |
| `completed_by` | String | Yes | – | Foreign | m_user.`id` | Completed By User |
| `completed_at` | Timestamp | Yes | – | – | – | Completion Date |
| `closed_by` | String | Yes | – | Foreign | m_user.`id` | Closed By User |
| `closed_at` | Timestamp | Yes | – | – | – | Closing Date |
| `cancelled_by` | String | Yes | – | Foreign | m_user.`id` | Cancelled By User |
| `cancelled_at` | Timestamp | Yes | – | – | – | Cancellation Date |
| `notes` | Array Map | Yes | – | – | `Note` Map | User Notes |

---

