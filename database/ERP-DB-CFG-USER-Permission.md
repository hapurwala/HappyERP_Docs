# Database Design Details

This document gives database design details of following module.

- **Software**: **HappyERP**
- **Application**: Configuration
- **Module**: User Management and Permissions

**HappyERP** supports full fledged user management and permissions. 

This document gives database details of the User Management and Permissions module in **HappyERP**. User management is a critical part of the software and it ensures that only authorized users use the software. It also ensures that every user will be able to perform only authorized tasks.

**HappyERP** has following components for the purpose of User Management:

- **Application** & **Module**: Working of every department is put together in applications. These applications may further be divided in modules. These applications are designed in a manner that they may work individually as well as integrated with other applications. E.g. Masters, Sales etc. The applications are decided at the system design level.
- **AppObject**: Objects (referred as AppObject) are independent components within an Application. Every Object is a complete object in itself. Users are given permission at the Object level. E.g. Product, Customer, Sales Order etc. The Objects are decided at the system design level.
- **Workflow Stage**: **HappyERP** supports workflow stages with all AppObjects. These stages are the lifecycle of the objects.
- **AppRole** & **Permission**: A Role (referred as AppRole) is similar to the roles in the organisation. E.g. Sales Executive, Loader etc. Standard AppRoles are decided at the system design level. However Admin may add more roles at run time.
- **AppRole Permissions**: AppRoles are assigned permissions on several AppObjects. Admin may assign several permissions to AppRoles. 
- **User** & **Roles**: It is the user of **HappyERP**. An employee is an internal user of the application and customers, vendors are the external users of the application. A user may be assigned one or more roles and accordingly he/she gets the permissions on different AppObjects. Admin will add Users in the software and will assign required roles to each user.

## Collection: s_application

| Name                    | Type    | Optional | Default Value | Key     | Reference          | Remarks     |
|:----------------------- |:------- |:-------- |:------------- |:------- |:------------------ |:----------- |
| `id`                    | String  | -        | -             | Primary | -                  | Document Id |
| `name`                  | String  | -        | -             | -       | -                  |             |
| `short_name`            | String  | -        | -             | -       | -                  |             |
| `parent_application_id` | String  | Yes      | -             | Foreign | s_application.`id` |             |
| `is_active`             | Boolean | -        | False         | -       | -                  |             |

## Collection: s_app_object

| Name                    | Type    | Optional | Default Value | Key     | Reference | Remarks                       |
| ----------------------- | ------- | -------- | ------------- | ------- | --------- | ----------------------------- |
| `id`                    | String  | -        | -             | Primary | -         | Document Id                   |
| `application_id`        | String  | Yes      | -             | Foreign | -         |                               |
| `name`                  | String  | -        | -             | Unique  | -         |                               |
| `object_category`       | Number  | -        | -             | -       | -         | Form, SubForm, Report, Action |
| `sequence`              | Number  | -        | -             | -       | -         |                               |
| `is_internal`           | Boolean | -        | False         | -       | -         |                               |
| `to_link_acct_voucher`  | Boolean | -        | False         | -       | -         |                               |
| `to_link_stock_voucher` | Boolean | -        | False         | -       | -         |                               |
| `component_name`        | String  | Yes      | -             | -       | -         |                               |

## Collection: s_app_object_stage

```
Sample Workflow Stage for Masters: 
    Created
    ↓ 
    Verified
    ↓ 
    Approved (available for use)

Sample Workflow Stages for Transactions: 
    Created
    ↓ 
    Verified
    ↓ 
    Approved
    ↓ 
    Completed
```

| Name                           | Type    | Optional | Default Value | Key     | Reference                 | Remarks     |
| ------------------------------ | ------- | -------- | ------------- | ------- | ------------------------- | ----------- |
| `id`                           | String  | -        | -             | Primary | -                         | Document Id |
| `app_object_id`                | Number  | Yes      | -             | Foreign | s_app_object.`id`         |             |
| `sequence`                     | Number  | -        | -             | -       | -                         |             |
| `name`                         | String  | -        | -             | -       | -                         |             |
| `name_external`                | String  | -        | -             | -       | -                         |             |
| `description`                  | String  | Yes      | -             | -       | -                         |             |
| `badge_variant`                | String  | -        | -             | -       | -                         |             |
| `is_internal`               | Boolean | -        | True          | -       | -                         |             |
| `is_remarks_required`          | Boolean | -        | False         | -       | -                         |             |
| `validation_before_set`                | Array   | Yes      | -             | -       |  |             |
| `action_after_set`                | Array   | Yes      | -             | -       | s_category.`stage_action` |             |
| `allow_delete`                 | Boolean | -        | False         | -       | -                         |             |
| `allow_edit`                   | Boolean | -        | False         | -       | -                         |             |
| `allow_cancel`                 | Boolean | -        | False         | -       | -                         |             |
| `is_available_for_next_module` | Boolean | -        | False         | -       | -                         |             |

## Collection: s_app_role

| Name          | Type      | Optional | Default Value | Key     | Reference   | Remarks            |
| ------------- | --------- | -------- | ------------- | ------- | ----------- | ------------------ |
| `id`          | String    | -        | -             | Primary | -           | Document Id        |
| `name`        | String    | -        | -             | -       | -           |                    |
| `description` | String    | Yes      | -             | -       | -           |                    |
| `start_date`  | Timestamp | -        | Current Date  | -       | -           |                    |
| `end_date`    | Timestamp | Yes      | -             | -       | -           |                    |
| `reason_end`  | String    | Yes      | -             | -       | -           | Reason For Closure |
| `is_internal` | Boolean   | -        | False         | -       | -           |                    |
| `stage`       | Map       | -        | -             | -       | `Stage` Map | Current Stage      |
| `stage_logs`  | Array Map | Yes      | -             | -       | `Stage` Map | Workflow History   |

## Collection: s_app_role_permission

| Name                                    | Type      | Optional | Default Value | Key     | Reference          | Remarks                                  |
| --------------------------------------- | --------- | -------- | ------------- | ------- | ------------------ | ---------------------------------------- |
| `id`                                    | String    | -        | -             | Primary | -                  | Document Id                              |
| `app_role_id`                           | String    | -        | -             | Foreign | s_app_role.`id`    |                                          |
| `app_object_id`                         | String    | -        | -             | Foreign | s_app_object.`id`  |                                          |
| `can_add`                               | Boolean   | -        | False         | -       | -                  |                                          |
| `can_view`                              | Boolean   | -        | True          | -       | -                  |                                          |
| `view_scope_category`                   | String    | -        | -             | -       | s_category.`scope` |                                          |
| `stage_permissions`                     | Array Map | -        | -             | -       | -                  |                                          |
| stage_permissions.`app_object_stage_id` | Number    | -        | -             | Foreign | -                  |                                          |
| stage_permissions.`can_view`            | Boolean   | -        | False         | -       | -                  | Can view / share /print                  |
| stage_permissions.`can_edit`            | Boolean   | -        | False         | -       | -                  |                                          |
| stage_permissions.`can_delete`          | Boolean   | -        | False         | -       | -                  |                                          |
| stage_permissions.`can_cancel`          | Boolean   | -        | False         | -       | -                  |                                          |
| stage_permissions.`can_set`             | Boolean   | -        | False         | -       | -                  | Can set this stage (from previous stage) |
| stage_permissions.`can_rollback`        | Boolean   | -        | False         | -       | -                  | Applicable when can_set is True          |
