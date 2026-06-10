# Workflow Engine

**HappyERP** has implemented a comprehensive workflow management system to cater to the needs to different clients belonging to different industries. It provides complete control the use to workflows as per their needs. Following are the components of the Workflow Engine.

> [!NOTE]
> 
> **Meta Data** is a set of data that is prepared during system design phase and is pre-filled in user's database at the time of setting up the software.  

| Component           | Description                                                                                                                                                                                                                                                                                                                                                                            | Meta Data Source                | Can Admin Create? |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- | ----------------- |
| Application         | Normally every application performs tasks related to one particular department of an organisation. E.g. Sales, Purchase, Accounts etc.                                                                                                                                                                                                                                                 | `ERP-MD-CFG-USR-Application.md` | No                |
| Module              | Applications are further divided in modules. E.g. Purchase application is divided in Purchase Order, Purchase Invoice, Purchase Return etc.                                                                                                                                                                                                                                            | --                              | No                |
| AppObject / Feature | Objects (referred as AppObject) are independent components within an application. Every AppObject is a complete object in itself. <br /><br />In many cases a Module may also be referred as AppObject (e.g. Purchase Order) and in other cases a module may be further divided in AppObjects (e.g. module Product Master contains several AppObjects like Product, Pack, Branch etc.) | `ERP-MD-CFG-USR-AppObject.md`   | No                |
| Workflow Stage      | An AppObject goes through different stages in its lifecycle. E.g. A Purchase Order may have stages like Draft, Approved, Issued and Closed etc. Different clients may have different Stages for same AppObject based on their business practices.                                                                                                                                      | `ERP-MD-CFG-USR-Stage.md`       | Yes               |
| Action              | Actions that can be performed on an AppObject e.g. Add, Modify, Delete etc. User permission is granted by allowing one or more Actions on an AppObject.                                                                                                                                                                                                                                | --                              | No                |
| Roles               | Normally a Role is related to the Job Profile of users. A Role is a set of AppObjects and permissions given on these AppObjects.                                                                                                                                                                                                                                                       | `ERP-MD-CFG-USR-Role.md`        | Yes               |
| Users               | It is the user of  **HappyERP**. An employee is an internal user of the software and customers, vendors are the external users of the software. A user may be assigned one or more roles and accordingly he/she gets the permissions on different AppObjects.                                                                                                                          | `ERP-MD-CFG-USR-User.md`        | Yes               |

---

## Actions

Permission is given on some fixed actions. These actions together give a comprehensive permission setup. 2 actions are applicable at AppObject level and rest are applicable at Stage level.

### AppObject level Action

Following actions are applicable at AppObject level:  

| Action | Description           |
|:------ |:--------------------- |
| Add    | Can `create` new data |

### Workflow Stage level Actions

> [!IMPORTANT]
> 
> **Scope of Action**
> In organisations people work in teams. Normally teams have access to data of their own teams only. To handle it, **HappyERP** has a provision of defining scope for performing allowed actions. There are 4 possible scopes:
> `Self`, `Team`, `Peer` and `All`. 

Following actions are applicable at Workflow Stage level for any AppObject:

| Action                 | Description                                                                                                              |
|:---------------------- |:------------------------------------------------------------------------------------------------------------------------ |
| View                   | Can  `view`  data at given Stage                                                                                         |
| Modify                 | Can  `modify`  data at given Stage                                                                                       |
| Delete                 | Can  `delete`  data at given Stage                                                                                       |
| Cancel                 | Can `cancel` workflow at given Stage                                                                                     |
| Set Stage              | Allowed to `set` given Stage (from previous Stage)                                                                       |
| Rollback Stage         | Allowed to `rollback` to previous Stage from given Stage<br />*(applicable only when user is allowed to Set this Stage)* |
| Share / Export / Print | Can `share`/`export`/`download`/`print` data at given Stage                                                              |

## System Roles

These roles are global roles for all modules of the ERP.

| Role         | Purpose                                                   |
|:------------ |:--------------------------------------------------------- |
| Auditor      | Read-only audit and compliance access                     |
| Admin        | Full system configuration and control access              |
| System Admin | Full system configuration, control and development access |
