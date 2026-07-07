# Financial Year Configuration

## Purpose

Financial Year configuration allows Happy ERP to support country-specific and organisation-specific accounting periods.

The Financial Year must never be hardcoded.

Examples:

| Country       | Financial Year   |
| ------------- | ---------------- |
| India         | 01-Apr to 31-Mar |
| USA           | 01-Jan to 31-Dec |
| Australia     | 01-Jul to 30-Jun |
| UK Government | 06-Apr to 05-Apr |

---

# Organisation Configuration

## Collection: m_organisation

### Financial Year Settings

| Name                           | Type    | Optional | Default Value | Key | Reference | Remarks                         |
| ------------------------------ | ------- | -------- | ------------- | --- | --------- | ------------------------------- |
| financial_year_start_month     | Int64   | –        | 4             | –   | –         | Financial Year Start Month      |
| financial_year_start_day       | Int64   | –        | 1             | –   | –         | Financial Year Start Day        |
| maintain_financial_year_master | Boolean | –        | True          | –   | –         | Maintain Financial Year Records |

---

# Financial Year Master

## Collection: m_financial_year

Contains all financial years for an organisation.

| Name            | Type      | Optional | Default Value | Key     | Reference         | Remarks                   |
| --------------- | --------- | -------- | ------------- | ------- | ----------------- | ------------------------- |
| id              | String    | –        | –             | Primary | –                 | Document Id               |
| organisation_id | String    | –        | –             | Foreign | m_organisation.id | Organisation              |
| name            | String    | –        | –             | –       | –                 | FY Name                   |
| code            | String    | –        | –             | Unique  | –                 | Financial Year Code       |
| start_date      | Timestamp | –        | –             | –       | –                 | Financial Year Start Date |
| end_date        | Timestamp | –        | –             | –       | –                 | Financial Year End Date   |
| is_current      | Boolean   | –        | False         | –       | –                 | Current Financial Year    |
| is_locked       | Boolean   | –        | False         | –       | –                 | Prevent New Transactions  |
| remarks         | String    | Yes      | –             | –       | –                 | Notes                     |
| created_by      | String    | Yes      | –             | Foreign | m_user.id         | Created By                |
| created_at      | Timestamp | Yes      | –             | –       | –                 | Created On                |
| updated_by      | String    | Yes      | –             | Foreign | m_user.id         | Updated By                |
| updated_at      | Timestamp | Yes      | –             | –       | –                 | Updated On                |

---

# Example Records

## India

| Name       | Start Date  | End Date    |
| ---------- | ----------- | ----------- |
| FY 2024-25 | 01-Apr-2024 | 31-Mar-2025 |
| FY 2025-26 | 01-Apr-2025 | 31-Mar-2026 |
| FY 2026-27 | 01-Apr-2026 | 31-Mar-2027 |

---

## Australia

| Name       | Start Date  | End Date    |
| ---------- | ----------- | ----------- |
| FY 2025-26 | 01-Jul-2025 | 30-Jun-2026 |
| FY 2026-27 | 01-Jul-2026 | 30-Jun-2027 |

---

# Transaction Storage

All transaction collections should store Financial Year information.

Example:

## purchase_order

| Name                | Type   | Optional | Default Value | Key     | Reference           | Remarks             |
| ------------------- | ------ | -------- | ------------- | ------- | ------------------- | ------------------- |
| financial_year_id   | String | –        | –             | Foreign | m_financial_year.id | Financial Year      |
| financial_year_name | String | –        | –             | –       | –                   | Financial Year Name |

---

# Financial Year Resolution Logic

When a transaction is created:

1. Read transaction date.
2. Find Financial Year where:

```text
transaction_date >= start_date
AND
transaction_date <= end_date
```

3. Store:

```text
financial_year_id
financial_year_name
```

inside the transaction.

---

# Example

Transaction Date:

```text
01-Mar-2026
```

Matched Financial Year:

```text
FY 2025-26
```

Stored:

```json
{
  "financial_year_id": "FY2025-26",
  "financial_year_name": "FY 2025-26"
}
```

---

Transaction Date:

```text
09-Jun-2026
```

Matched Financial Year:

```text
FY 2026-27
```

Stored:

```json
{
  "financial_year_id": "FY2026-27",
  "financial_year_name": "FY 2026-27"
}
```

---

# Benefits

* Country Independent
* Organisation Specific
* Supports Multiple Financial Calendars
* No Hardcoded Accounting Logic
* Easier Reporting
* Easier Year-End Closing
* Supports Future Expansion
