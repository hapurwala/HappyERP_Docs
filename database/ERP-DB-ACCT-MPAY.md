- **Software**: **HappyERP**
- **Application**: Accounts
- **Module**: Make Payment

This document contains database design details of Purchase Invoice module. The data includes:

- General Information / Header Fields
- Reference Documents
- Product Details
- Tax Details
- Additional Charges
- Vendor Payment References
- Purchase Return References
- Attachment
- Summary Fields
- Payment Summary
- Vendor Summary
- Workflow Summary

## Collection: payment_out

Contains Payment transaction data.
---

## General Information

| Name             | Type      | Optional | Default Value   | Key     | Reference   | Remarks                                              |
| ---------------- | --------- | -------- | --------------- | ------- | ----------- | ---------------------------------------------------- |
| `id`             | String    | –        | Auto Generated  | Primary | –           | Document Id                                          |
| `payment_number` | String    | –        | Auto Generated  | Unique  | –           | MPAY Number                                          |
| `payment_date`   | Timestamp | –        | Current Date    | –       | –           | Payment Transaction Date                             |
| `payment_type`   | String    | –        | Invoice Payment | –       | –           | Invoice Payment / Advance / Security Deposit / Mixed |
| `payment_status` | String    | Yes      | Pending         | –       | –           | Pending / Partial / Completed / Failed               |
| `stage`          | Map       | –        | Draft           | –       | `Stage` Map | Current Workflow Stage                               |
| `remarks`        | String    | Yes      | –               | –       | –           | General Remarks                                      |

---

## Organisation Information

| Name                | Type   | Optional | Default Value | Key     | Reference           | Remarks           |
| ------------------- | ------ | -------- | ------------- | ------- | ------------------- | ----------------- |
| `organisation_id`   | String | –        | –             | Foreign | m_organisation.`id` | Organisation Id   |
| `organisation_name` | String | –        | –             | –       | –                   | Organisation Name |
| `branch_id`         | String | –        | –             | Foreign | m_branch.`id`       | Branch Id         |
| `branch_name`       | String | –        | –             | –       | –                   | Branch Name       |

---

## Vendor Information

| Name            | Type   | Optional | Default Value | Key     | Reference     | Remarks       |
| --------------- | ------ | -------- | ------------- | ------- | ------------- | ------------- |
| `vendor_id`     | String | –        | –             | Foreign | m_vendor.`id` | Vendor Id     |
| `vendor_code`   | String | Yes      | –             | –       | –             | Vendor Code   |
| `vendor_name`   | String | –        | –             | –       | –             | Vendor Name   |
| `vendor_mobile` | String | Yes      | –             | –       | –             | Mobile Number |
| `vendor_email`  | String | Yes      | –             | –       | –             | Email Address |
| `vendor_gstin`  | String | Yes      | –             | –       | –             | GST Number    |

---

## Currency Information

| Name              | Type   | Optional | Default Value         | Key     | Reference       | Remarks         |
| ----------------- | ------ | -------- | --------------------- | ------- | --------------- | --------------- |
| `currency_id`     | String | –        | Organisation Currency | Foreign | m_currency.`id` | Currency Id     |
| `currency_name`   | String | Yes      | –                     | –       | –               | Currency Name   |
| `currency_symbol` | String | Yes      | –                     | –       | –               | Currency Symbol |
| `exchange_rate`   | Double | Yes      | 1                     | –       | –               | Exchange Rate   |

---

## Purchase Invoice References

| Name                                           | Type      | Optional | Default Value | Key     | Reference             | Remarks                               |
| ---------------------------------------------- | --------- | -------- | ------------- | ------- | --------------------- | ------------------------------------- |
| `invoice_references`                           | Array Map | Yes      | –             | –       | –                     | Linked Purchase Invoices              |
| invoice_references[].`id`                      | String    | –        | –             | –       | –                     | Row Id                                |
| invoice_references[].`sequence`                | Int64     | –        | 0             | –       | –                     | Display Sequence                      |
| invoice_references[].`purchase_invoice_id`     | String    | Yes      | –             | Foreign | purchase_invoice.`id` | Purchase Invoice Id                   |
| invoice_references[].`purchase_invoice_number` | String    | Yes      | –             | –       | –                     | Purchase Invoice Number               |
| invoice_references[].`invoice_date`            | Timestamp | Yes      | –             | –       | –                     | Invoice Date                          |
| invoice_references[].`invoice_amount`          | Double    | Yes      | 0             | –       | –                     | Invoice Amount                        |
| invoice_references[].`paid_amount`             | Double    | Yes      | 0             | –       | –                     | Total Paid Before Current Payment     |
| invoice_references[].`pending_amount`          | Double    | Yes      | 0             | –       | –                     | Pending Amount Before Current Payment |
| invoice_references[].`current_payment_amount`  | Double    | Yes      | 0             | –       | –                     | Payment Amount In Current MPAY        |
| invoice_references[].`adjustment_value`        | Double    | Yes      | 0             | –       | –                     | Purchase Return / Credit Adjustment   |
| invoice_references[].`net_payment_value`       | Double    | Yes      | 0             | –       | –                     | Final Payable Amount                  |
| invoice_references[].`remarks`                 | String    | Yes      | –             | –       | –                     | Reference Remarks                     |

---

## Advance Payment Information

| Name                       | Type    | Optional | Default Value | Key | Reference | Remarks                         |
| -------------------------- | ------- | -------- | ------------- | --- | --------- | ------------------------------- |
| `allow_advance_payment`    | Boolean | Yes      | False         | –   | –         | Payment Without Invoice         |
| `allow_mixed_payment`      | Boolean | Yes      | False         | –   | –         | Invoice + Advance Payment       |
| `advance_payment_value`    | Double  | Yes      | 0             | –   | –         | Advance Payment Amount          |
| `advance_adjustment_value` | Double  | Yes      | 0             | –   | –         | Adjusted Against Future Invoice |
| `advance_balance_value`    | Double  | Yes      | 0             | –   | –         | Remaining Advance Balance       |

---

## Payment Details

| Name                           | Type      | Optional | Default Value | Key     | Reference           | Remarks                                |
| ------------------------------ | --------- | -------- | ------------- | ------- | ------------------- | -------------------------------------- |
| `payment_modes`                | Array     | –        | –             | –       | –                   | Selected Payment Modes                 |
| `base_date`                    | String    | –        | Invoice Date  | –       | –                   | Invoice Date / Due Date / Current Date |
| `credit_days`                  | Int64     | Yes      | 0             | –       | –                   | Vendor Credit Days                     |
| `due_date`                     | Timestamp | Yes      | –             | –       | –                   | Payment Due Date                       |
| `tds_applicable`               | Boolean   | Yes      | False         | –       | –                   | TDS Applicable                         |
| `tds_category_id`              | String    | Yes      | –             | Foreign | m_tds_category.`id` | TDS Category                           |
| `tds_category_name`            | String    | Yes      | –             | –       | –                   | TDS Category Name                      |
| `tds_percentage`               | Double    | Yes      | 0             | –       | –                   | TDS Percentage                         |
| `tds_value`                    | Double    | Yes      | 0             | –       | –                   | TDS Amount                             |
| `early_payment_discount_value` | Double    | Yes      | 0             | –       | –                   | Early Payment Discount                 |
| `late_payment_penalty_value`   | Double    | Yes      | 0             | –       | –                   | Late Payment Penalty                   |
| `adjustment_value`             | Double    | Yes      | 0             | –       | –                   | Total Adjustment Value                 |
| `round_off_value`              | Double    | Yes      | 0             | –       | –                   | Round Off Amount                       |
| `total_payment_value`          | Double    | Yes      | 0             | –       | –                   | Gross Payment Value                    |
| `total_net_value`              | Double    | Yes      | 0             | –       | –                   | Final Payment Value                    |

---

## Payment Schedule

| Name                                   | Type      | Optional | Default Value | Key     | Reference           | Remarks                              |
| -------------------------------------- | --------- | -------- | ------------- | ------- | ------------------- | ------------------------------------ |
| `payment_schedule`                     | Array Map | Yes      | –             | –       | –                   | Payment Mode Wise Details            |
| payment_schedule[].`id`                | String    | –        | –             | –       | –                   | Row Id                               |
| payment_schedule[].`sequence`          | Int64     | –        | 0             | –       | –                   | Display Sequence                     |
| payment_schedule[].`payment_mode`      | String    | –        | –             | –       | –                   | Cash / UPI / Bank / Cheque           |
| payment_schedule[].`reference_number`  | String    | Yes      | –             | –       | –                   | UPI Ref / Cheque No / Transaction Id |
| payment_schedule[].`reference_date`    | Timestamp | Yes      | –             | –       | –                   | Transaction Date                     |
| payment_schedule[].`bank_account_id`   | String    | Yes      | –             | Foreign | m_bank_account.`id` | Organisation Bank Account            |
| payment_schedule[].`bank_account_name` | String    | Yes      | –             | –       | –                   | Bank Account Name                    |
| payment_schedule[].`amount`            | Double    | –        | 0             | –       | –                   | Payment Amount                       |
| payment_schedule[].`remarks`           | String    | Yes      | –             | –       | –                   | Payment Mode Remarks                 |

---

## Adjustments

| Name                             | Type      | Optional | Default Value   | Key | Reference | Remarks                                             |
| -------------------------------- | --------- | -------- | --------------- | --- | --------- | --------------------------------------------------- |
| `adjustments`                    | Array Map | Yes      | –               | –   | –         | Purchase Adjustments                                |
| adjustments[].`id`               | String    | –        | –               | –   | –         | Row Id                                              |
| adjustments[].`sequence`         | Int64     | –        | 0               | –   | –         | Display Sequence                                    |
| adjustments[].`adjustment_type`  | String    | –        | Purchase Return | –   | –         | Purchase Return / Credit Note / Debit Note / Manual |
| adjustments[].`reference_id`     | String    | Yes      | –               | –   | –         | Reference Document Id                               |
| adjustments[].`reference_number` | String    | Yes      | –               | –   | –         | Reference Number                                    |
| adjustments[].`reference_date`   | Timestamp | Yes      | –               | –   | –         | Reference Date                                      |
| adjustments[].`amount`           | Double    | –        | 0               | –   | –         | Adjustment Amount                                   |
| adjustments[].`remarks`          | String    | Yes      | –               | –   | –         | Adjustment Remarks                                  |

---

## Attachments

| Name                        | Type      | Optional | Default Value | Key     | Reference   | Remarks              |
| --------------------------- | --------- | -------- | ------------- | ------- | ----------- | -------------------- |
| `attachments`               | Array Map | Yes      | –             | –       | –           | Supporting Documents |
| attachments[].`id`          | String    | –        | –             | –       | –           | Row Id               |
| attachments[].`file_name`   | String    | –        | –             | –       | –           | File Name            |
| attachments[].`file_type`   | String    | –        | –             | –       | –           | File Type            |
| attachments[].`file_url`    | String    | –        | –             | –       | –           | File Storage URL     |
| attachments[].`file_size`   | Int64     | Yes      | 0             | –       | –           | File Size            |
| attachments[].`uploaded_by` | String    | Yes      | –             | Foreign | m_user.`id` | Uploaded User        |
| attachments[].`uploaded_at` | Timestamp | Yes      | –             | –       | –           | Upload Date          |
| attachments[].`remarks`     | String    | Yes      | –             | –       | –           | Attachment Remarks   |

---

## Summary Fields

| Name                                      | Type   | Optional | Default Value | Key | Reference | Remarks                                     |
| ----------------------------------------- | ------ | -------- | ------------- | --- | --------- | ------------------------------------------- |
| `total_invoice_count`                     | Int64  | Yes      | 0             | –   | –         | Total Linked Purchase Invoices              |
| `total_invoice_value`                     | Double | Yes      | 0             | –   | –         | Total Invoice Amount                        |
| `total_paid_value_before_current_payment` | Double | Yes      | 0             | –   | –         | Previously Paid Amount                      |
| `total_pending_payment_value`             | Double | Yes      | 0             | –   | –         | Total Pending Amount Before Current Payment |
| `total_current_payment_value`             | Double | Yes      | 0             | –   | –         | Current Payment Amount                      |
| `total_advance_payment_value`             | Double | Yes      | 0             | –   | –         | Total Advance Payment                       |
| `total_adjustment_value`                  | Double | Yes      | 0             | –   | –         | Total Adjustment Value                      |
| `total_tds_value`                         | Double | Yes      | 0             | –   | –         | Total TDS Deduction                         |
| `total_early_payment_discount_value`      | Double | Yes      | 0             | –   | –         | Total Early Payment Discount                |
| `total_late_payment_penalty_value`        | Double | Yes      | 0             | –   | –         | Total Late Payment Penalty                  |
| `round_off_value`                         | Double | Yes      | 0             | –   | –         | Round Off Value                             |
| `total_net_value`                         | Double | Yes      | 0             | –   | –         | Final Vendor Payment Value                  |

---

## Payment Summary

| Name                   | Type   | Optional | Default Value | Key | Reference | Remarks                 |
| ---------------------- | ------ | -------- | ------------- | --- | --------- | ----------------------- |
| `payment_mode_count`   | Int64  | Yes      | 0             | –   | –         | Number Of Payment Modes |
| `cash_payment_value`   | Double | Yes      | 0             | –   | –         | Cash Payment Amount     |
| `upi_payment_value`    | Double | Yes      | 0             | –   | –         | UPI Payment Amount      |
| `bank_transfer_value`  | Double | Yes      | 0             | –   | –         | Bank Transfer Amount    |
| `cheque_payment_value` | Double | Yes      | 0             | –   | –         | Cheque Payment Amount   |
| `neft_payment_value`   | Double | Yes      | 0             | –   | –         | NEFT Payment Amount     |
| `rtgs_payment_value`   | Double | Yes      | 0             | –   | –         | RTGS Payment Amount     |

---

## Vendor Summary

| Name                       | Type      | Optional | Default Value | Key | Reference | Remarks                           |
| -------------------------- | --------- | -------- | ------------- | --- | --------- | --------------------------------- |
| `vendor_outstanding_value` | Double    | Yes      | 0             | –   | –         | Outstanding Amount Before Payment |
| `vendor_credit_limit`      | Double    | Yes      | 0             | –   | –         | Vendor Credit Limit               |
| `vendor_credit_days`       | Int64     | Yes      | 0             | –   | –         | Vendor Credit Days                |
| `last_payment_date`        | Timestamp | Yes      | –             | –   | –         | Last Vendor Payment Date          |
| `last_payment_value`       | Double    | Yes      | 0             | –   | –         | Previous Payment Amount           |

---

## Workflow Summary

| Name           | Type      | Optional | Default Value | Key     | Reference   | Remarks                |
| -------------- | --------- | -------- | ------------- | ------- | ----------- | ---------------------- |
| `stage`        | Map       | –        | Draft         | –       | `Stage` Map | Current Workflow Stage |
| `stage_logs`   | Array Map | Yes      | –             | –       | `Stage` Map | Workflow History       |
| `created_by`   | String    | Yes      | –             | Foreign | m_user.`id` | Created By User        |
| `created_at`   | Timestamp | Yes      | Current Date  | –       | –           | Creation Date          |
| `updated_by`   | String    | Yes      | –             | Foreign | m_user.`id` | Last Updated User      |
| `updated_at`   | Timestamp | Yes      | Current Date  | –       | –           | Last Updated Date      |
| `verified_by`  | String    | Yes      | –             | Foreign | m_user.`id` | Verified By User       |
| `verified_at`  | Timestamp | Yes      | –             | –       | –           | Verification Date      |
| `approved_by`  | String    | Yes      | –             | Foreign | m_user.`id` | Approved By User       |
| `approved_at`  | Timestamp | Yes      | –             | –       | –           | Approval Date          |
| `completed_by` | String    | Yes      | –             | Foreign | m_user.`id` | Completed By User      |
| `completed_at` | Timestamp | Yes      | –             | –       | –           | Completion Date        |
| `cancelled_by` | String    | Yes      | –             | Foreign | m_user.`id` | Cancelled By User      |
| `cancelled_at` | Timestamp | Yes      | –             | –       | –           | Cancellation Date      |
| `notes`        | Array Map | Yes      | –             | –       | `Note` Map  | User Notes             |

---
