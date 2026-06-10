# Introduction

**HappyERP** is an ERP solution available on Web and Mobile. It is developed as a generic solution which caters to various customers across different industries. Though every industry has its own requirements, I feel such individual requirements can be fulfilled by providing necessary configurations in the software.

# Technical Details

It will be developed on Google Cloud Platform. Following technologies will be used:

* **Web Frontend**: Next.js (React)

* **Mobile App**: React Native (shares logic with web)

* **Backend/API**: Next.js API Routes

* **Database**: Firebase Firestore
  
  * MySQL database on CloudSQL (at a later stage)

* **Authentication**: Firebase Auth (Phone/OTP)

* **Hosting**: GCP Cloud Run or Firebase Hosting

* **Storage**: Firebase Storage (for files/images)

## Domain and Subdomains

We already have 2 domains registered for this purpose:

1. [www.HappyERP.in](http://www.happyerp.in): This domain will host a website of this product. Giving its introduction, features, pricing, option to buy/renew and more. This website will be put on SEO. Anybody searching for an ERP solution would be directed to this page.  
2. [www.HappyGoLive.in](http://www.happygolive.in). This is the main application. It may be accessed in following ways:  
   1. Client’s domain name: our client may register their own domain name that would point to it  
   2. Using subdomain e.g. [abc.heppygolive.in](http://abc.heppygolive.in). This method will be used when client does not have their own domain for this application.  
   3. Using folder e.g. [www.happygolive.in/abc](http://www.happygolive.in/abc). It is similar to the above option.

## Client Setup

A separate environment will be created for each client. Separate GCP, separate Application and separate database.

# Applications

**HappyERP** contains several applications. Normally every application performs tasks related to one particular department of an organisation. These applications are further divided in modules. Main application and modules are given hereunder.

| S.No. | Application     | Short Name | Modules                                                                                                                                               | Description                                                                            |
| ----- | --------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| 1     | Masters         | MAS        | `PRD`: Product Master<br />`PARTY`: Party Master<br />`EMP`: Employee Master                                                                          | Maintains all masters of the software                                                  |
| 2     | Sales           | SAL        | `SO` : Sales Order<br />`SINV`: Sales Invoice<br />`SRET`: Sales Return<br />`SIREV`: Sales Invoice Revision                                          | Maintains Sales related operations                                                     |
| 3     | Purchase        | PUR        | `PO` : Purchase Order<br />`PINV`: Purchase Invoice<br />`PRET`: Purchase Return<br />`PIREV`: Purchase Invoice Revision                              | Maintains Purchase related operations                                                  |
| 4     | Inventory       | INV        | `GRN`: Material Receive<br />`DISP`: Dispatch Module<br />`TRF`: Stock Transfer<br />`MIMO`: Material In, Material Out                                | Maintains Inventory related operations                                                 |
| 5     | Accounts        | ACCT       | `RPAY`: Receive Payment<br />`MPAY`: Make Payment<br />`PADJ`: Payment Adjust<br />`DNOTE`: Debit Note<br />`CNOTE`: Credit Note<br />`VCH`: Vouchers |                                                                                        |
| 6     | Production      | PROD       |                                                                                                                                                       |                                                                                        |
| 7     | Sub Contracting | SUB        |                                                                                                                                                       |                                                                                        |
| 8     | Budgeting       | BUDG       |                                                                                                                                                       |                                                                                        |
| 9     | Dashboard/MIS   | MIS        |                                                                                                                                                       |                                                                                        |
| 10    | Configuration   | CFG        | `ORG`: Business<br />`USER`: User Management<br />`SET`: Configuration                                                                                | Maintains Business Settings:<br /> - Organisations <br />- Users<br />- Configurations |

Although the software will be developed using generic approach but it would serve following industries in the beginning:

* Garments  
* Dairy  
* Mandi

# Layouts

A consistent, professional and user friendly interface is the prime requirement of this software. Most common layouts of the software would be as follows:

* Login Screen
* Common Application Layout
* Main Dashboard  
  * It would contain multiple widgets.  
  * There will be several pre-programmed widgets on the software.  
  * These widgets would be arranged in a grid manner. Individual widget may cover single or multiple cells in the grid.  
  * The widgets will be shown only on the basis of the user permissions.  
  * Users may select which widgets he/she wants to see on the dashboard along with the size and sequence given to each widget.   
* Application Dashboard  
  * Application Dashboard would also contain several widgets.  
  * These widgets would be specific to current application.  
  * Selection, sequence and size of the widgets will be user customisable.  
* List/Table
* Data entry page
* Data entry popup
* Data view page

# Standard Validations

Following is the list of validations that will be applicable on all pages in the software.

| Validation | Characters | Length | Values | Remarks |
|:---------- |:---------- |:------ |:------ |:------- |
| Name       | All        | 3-100  | –      |         |
| Short Name | All        | 1-25   | –      |         |

# General Guidelines

## Common Features

* Following information will be maintained with all features (`AppObject`)
  
  * Provision of stage both at header and detail level
  
  * Provision of multiple Media files
  
  * Provision of multiple Notes

* Delete will soft delete the data. No data is deleted permanently form the database.

## Design Documents

Complete design details of the software will be maintained in the `.md` files. The main reason is that AI understands, reads and writes the `.md` files very easily. **HappyERP** is big project so its design document will also be very big. To maintain the information properly, it is suggested to keep complete details in different files. To maintain all information in correct files, it is important to define how the information will be kept in different files, how the files will be named and grouped. This section provides such information.

> [!NOTE]
> 
> - Separate files at module level
> - Separate files for design type e.g. `UI`, `DB` etc.

### Filename

All `.md` files will be named as : `ERP-<Type>-<App>-<Mod>-<Topic>.md`, where

- ERP stands for **HappyERP**
- ***Type*** indicates the Type of document:
  - `Info`: This document contains **information** about that module or some general purpose
  - `DES`: This document contains **design** related information of the module and its features
  - `UI`: This document contains **UI** details of the module and its features
  - `DB`: This document contains **DB** details like schema, indexes, triggers etc.
  - `MD`: This document contains **meta data** that is shipped in-built with the software.
- ***App*** is the short name of the application e.g. `PUR` for Purchase. If any file is related to multiple applications or all application, use the code `GEN`.
- ***Mod** (optional)* is the short name of the module under any application. If the file contains information for more than one modules of an Application then this part may be omitted from the filename.
- ***Topic** (optional)* indicates additional information about the content of this file.
