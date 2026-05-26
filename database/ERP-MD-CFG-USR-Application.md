This document contains meta data for s_application collection.

### Collection: s\_application

| Name                  | Type    | Optional | Default Value | Key     | Reference            | Remarks     |
| :-------------------- | :------ | :------- | :------------ | :------ | :------------------- | :---------- |
| id                    | String  | --       | --            | Primary | --                   | Document Id |
| name                  | String  | --       | --            | --      | --                   |             |
| short_name            | String  | --       | --            | --      | --                   |             |
| parent_application_id | String  | Yes      | --            | Foreign | `s_application`.`id` |             |
| is_active             | Boolean | --       | False         | --      | --                   |             |

---

### Data

| id   | name      | short_name | parent_application_id | is_active |
| ---- | --------- | ---------- | --------------------- | --------- |
| 100  | Masters   | MAS        | --                    | Yes       |
| 400  | Purchase  | PUR        | --                    | Yes       |
|      | Sales     | SAL        | --                    | Yes       |
|      | Inventory | INV        | --                    | Yes       |
|      | Accounts  | ACCT       |                       |           |

---

