Following collections will be maintained for Party Master

* m\_party\_category: contains data of   
* m\_party: contains data of the parties \- customers and vendors

### Collection: m\_party\_category

| Name                        | Type    | Optional | Default Value | Key     | Reference             | Remarks     |
|:--------------------------- |:------- |:-------- |:------------- |:------- |:--------------------- |:----------- |
| id                          | String  | –        | –             | Primary | –                     | Document Id |
| name                        | String  | –        | –             | –       | –                     |             |
| parent\_party\_category\_id | String  | Yes      | –             | Foreign | m\_party\_category.id |             |
| maintain\_account\_head     | Boolean | –        | False         | –       | –                     |             |
| is\_customer                | Boolean | –        | False         | –       | –                     |             |
| is\_vendor                  | Boolean | –        | False         | –       | –                     |             |

### Collection: m\_party

| Name                                     | Type      | Optional | Default Value | Key     | Reference                | Remarks     |
|:---------------------------------------- |:--------- |:-------- |:------------- |:------- |:------------------------ |:----------- |
| id                                       | String    | –        | –             | Primary | –                        | Document Id |
| name                                     | String    | –        | –             | –       | –                        |             |
| short\_name                              | String    | –        | –             | –       | –                        |             |
| identifier                               | String    | Yes      | –             | –       | –                        |             |
| display\_name                            | String    | –        | –             | –       | –                        |             |
| mobile                                   | String    | –        | –             | –       | –                        |             |
| mobile\_whatsapp                         | Boolean   | –        | False         | –       | –                        |             |
| mobile\_2                                | String    | Yes      | –             | –       | –                        |             |
| mobile\_2\_whatsapp                      | Boolean   | –        | False         | –       | –                        |             |
| email                                    | String    | Yes      | –             | –       | –                        |             |
| email\_personal                          | String    | Yes      | –             | –       | –                        |             |
| is\_customer                             | Boolean   | –        | False         | –       | –                        |             |
| is\_vendor                               | Boolean   | –        | False         | –       | –                        |             |
| main\_address                            | map       | Yes      | –             | –       | –                        |             |
| main\_address.street                     | String    | Yes      | –             | –       | –                        |             |
| main\_address.city\_id                   | String    | Yes      | –             | Foreign | m\_city.id               |             |
| main\_address.city                       | String    | Yes      | –             | –       | –                        |             |
| main\_address.pin                        | String    | Yes      | –             | –       | –                        |             |
| main\_address.state\_id                  | String    | Yes      | –             | Foreign | m\_state.id              |             |
| main\_address.state                      | String    | Yes      | –             | –       | –                        |             |
| main\_address.country\_id                | String    | Yes      | –             | Foreign | m\_country.id            |             |
| main\_address.country                    | String    | Yes      | –             | –       | –                        |             |
| main\_address.location                   | Geopoint  | Yes      | –             | –       | –                        |             |
| main\_address.tagged\_by                 | String    | Yes      | –             | Foreign | m\_party.id              |             |
| main\_address.tagged\_on                 | Timestamp | Yes      | –             | –       | –                        |             |
| media                                    | Array Map | Yes      | –             | –       | –                        |             |
| media\[\].name                           | String    | Yes      | –             | –       | –                        |             |
| media\[\].description                    | String    | Yes      | –             | –       | –                        |             |
| media\[\].url                            | String    | No       | –             | –       | –                        |             |
| media\[\].is\_parimary                   | Boolean   | –        | False         | –       | –                        |             |
| media\[\].sequence                       | Int64     | No       | 0             | –       | –                        |             |
| media\[\].start\_date                    | Timestamp | Yes      | –             | –       | –                        |             |
| media\[\].end\_date                      | Timestamp | Yes      | –             | –       | –                        |             |
| regulatory\_id                           | Array Map | Yes      | –             | –       | –                        |             |
| regulatory\_id\[\].reg\_class            | String    | No       | –             | –       | –                        |             |
| regulatory\_id\[\].number                | String    | No       | –             | –       | –                        |             |
| regulatory\_id\[\].issue\_date           | Timestamp | Yes      | –             | –       | –                        |             |
| regulatory\_id\[\].valid\_from           | Timestamp | Yes      | –             | –       | –                        |             |
| regulatory\_id\[\].valid\_upto           | Timestamp | Yes      | –             | –       | –                        |             |
| party\_data                              | Map       | Yes      | –             | –       | –                        |             |
| party\_data.customer\_category           | String    | Yes      | –             | Foreign | m\_party\_category.id    |             |
| party\_data.vendor\_category             | String    | Yes      | –             | Foreign | m\_party\_category.id    |             |
| party\_data.contact\_person              | String    | Yes      | –             | –       |                          |             |
| party\_data.parent\_party\_id            | String    | Yes      | –             | Foreign | m\_party.id              |             |
| party\_data.credit\_limit                | Int64     | Yes      | –             | –       | –                        |             |
| party\_data.organisation                 | Array Map | Yes      | –             | –       | –                        |             |
| party\_data.organisation\[\].id          | String    | –        | –             | Foreign | m\_organisation.id       |             |
| party\_data.organisation\[\].start\_date | Timestamp | –        | –             | –       | –                        |             |
| party\_data.organisation\[\].end\_date   | Timestamp | Yes      | –             | –       | –                        |             |
| party\_data.organisation\[\].reason\_end | String    | Yes      | –             | –       | –                        |             |
| remarks                                  | String    | Yes      | –             | –       | –                        |             |
| stage                                    | Map       | –        | –             | –       | –                        |             |
| stage.id                                 | String    | –        | –             | Foreign | m\_app\_object\_stage.id |             |
| stage.name                               | String    | –        | –             | –       | –                        |             |
| stage.badge\_variant                     | String    | –        | –             | –       | –                        |             |
| stage.remarks                            | String    | Yes      | –             | –       | –                        |             |
| stage.set\_by                            | String    | Yes      | –             | Foreign | m\_party.id              |             |
| stage.set\_on                            | Timestamp | Yes      | –             | –       | –                        |             |
