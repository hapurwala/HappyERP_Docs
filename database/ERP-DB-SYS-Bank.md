# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: System
- **Module**: Bank

This module manages the global list of major banks. It serves as a master lookup for bank names to maintain reporting integrity and support future API integrations (like automated bank feeds).

> [!Note]
> Branch addresses and specific routing codes (like IFSC or account numbers) are **not** stored here. They are maintained directly on the Customer, Vendor, or Employee records as text fields, populated via a frontend Geocoding/Routing API.

## Collection: s_bank

This collection keeps record of all global banks. 

| Name                 | Type    | Optional | Default Value | Key     | Reference      | Remarks                                                                 |
| -------------------- | ------- | -------- | ------------- | ------- | -------------- | ----------------------------------------------------------------------- |
| `id`                 | String  | –        | –             | Primary | –              | 4-letter routing prefix or slug (e.g., HDFC, SBIN)                      |
| `name`               | String  | –        | –             | Unique  | –              | Full official name. Regional entities must have region appended (e.g., SBI (UK)) |
| `country_id`         | String  | –        | –             | Foreign | s_country.`id` | The country this bank operates in (e.g., IN)                            |
| `swift_code`         | String  | Yes      | –             | –       | –              | Primary global SWIFT/BIC code for international wires                   |
| `integration_id`     | String  | Yes      | –             | –       | –              | ID used by third-party aggregators (Plaid, SaltEdge) for automated feeds|
| `website`            | String  | Yes      | –             | –       | –              | Official website URL                                                    |
| `support_phone`      | String  | Yes      | –             | –       | –              | Customer care number                                                    |
| `logo_url`           | String  | Yes      | –             | –       | –              | URL to the bank's icon for UI display                                   |
| `is_active`          | Boolean | –        | True          | –       | –              | –                                                                       |
