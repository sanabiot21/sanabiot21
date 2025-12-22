| TABLE NAME      | ATTRIBUTE NAME  | CONTENTS                                    | TYPE         | PK OR FK | FK REFERENCED TABLE |
| :-------------- | :-------------- | :------------------------------------------ | :----------- | :------- | :------------------ |
| **USER**        | `userId`        | Unique identifier for the user              | INT          | PK       |                     |
|                 | `email`         | User's email address                        | VARCHAR(100) |          |                     |
|                 | `hashPassword`  | Encrypted password string                   | VARCHAR(255) |          |                     |
|                 | `role`          | Role type (Farmer, Farm Admin, Super Admin) | VARCHAR(20)  |          |                     |
|                 | `isVerified`    | Account verification status                 | BOOLEAN      |          |                     |
| **FARMER**      | `farmerId`      | Unique identifier for the farmer profile    | INT          | PK       |                     |
|                 | `userId`        | Reference to the base User account          | INT          | FK       | USER                |
|                 | `farmId`        | Reference to the farm they work on          | INT          | FK       | FARM                |
|                 | `fullname`      | Farmer's full name                          | VARCHAR(100) |          |                     |
|                 | `address`       | Farmer's residential address                | VARCHAR(255) |          |                     |
| **FARM_ADMIN**  | `farmAdminId`   | Unique identifier for the admin profile     | INT          | PK       |                     |
|                 | `userId`        | Reference to the base User account          | INT          | FK       | USER                |
|                 | `fullname`      | Admin's full name                           | VARCHAR(100) |          |                     |
|                 | `contactNumber` | Admin's mobile number                       | VARCHAR(15)  |          |                     |
| **SUPER_ADMIN** | `superAdminId`  | Unique identifier for the super admin       | INT          | PK       |                     |
|                 | `userId`        | Reference to the base User account          | INT          | FK       | USER                |
|                 | `fullname`      | Super Admin's full name                     | VARCHAR(100) |          |                     |

---

| TABLE NAME | ATTRIBUTE NAME   | CONTENTS                                       | TYPE         | PK OR FK | FK REFERENCED TABLE |
| :--------- | :--------------- | :--------------------------------------------- | :----------- | :------- | :------------------ |
| **FARM**   | `farmId`         | Unique identifier for the farm                 | INT          | PK       |                     |
|            | `farmAdminId`    | Reference to the manager of the farm           | INT          | FK       | FARM_ADMIN          |
|            | `farmName`       | Name of the dragon fruit farm                  | VARCHAR(100) |          |                     |
|            | `location`       | Physical location/Address                      | VARCHAR(255) |          |                     |
|            | `gpsCoordinates` | Geo-coordinates for mapping                    | VARCHAR(50)  |          |                     |
| **BLOCK**  | `blockId`        | Unique identifier for a farm section           | INT          | PK       |                     |
|            | `farmId`         | Reference to the parent farm                   | INT          | FK       | FARM                |
|            | `blockName`      | Name or code of the block (e.g., Block A)      | VARCHAR(50)  |          |                     |
| **POST**   | `postId`         | Unique identifier for a specific planting post | INT          | PK       |                     |
|            | `blockId`        | Reference to the parent block                  | INT          | FK       | BLOCK               |
|            | `postName`       | Label of the post (e.g., Post 001)             | VARCHAR(50)  |          |                     |

---

| TABLE NAME    | ATTRIBUTE NAME    | CONTENTS                                  | TYPE         | PK OR FK | FK REFERENCED TABLE |
| :------------ | :---------------- | :---------------------------------------- | :----------- | :------- | :------------------ |
| **CASE**      | `caseId`          | Unique identifier for the reported issue  | INT          | PK       |                     |
|               | `farmerId`        | Reference to the farmer who reported it   | INT          | FK       | FARMER              |
|               | `postId`          | Reference to the specific affected post   | INT          | FK       | POST                |
|               | `status`          | Current status (Open, Treating, Resolved) | VARCHAR(20)  |          |                     |
|               | `dateReported`    | Timestamp of the initial report           | DATETIME     |          |                     |
|               | `imageURL`        | Link to the initial scanned image         | VARCHAR(255) |          |                     |
| **DIAGNOSIS** | `diagnosisId`     | Unique identifier for AI analysis result  | INT          | PK       |                     |
|               | `caseId`          | Reference to the parent case              | INT          | FK       | CASE                |
|               | `ailmentId`       | Reference to the detected disease/pest    | INT          | FK       | CROP_AILMENT        |
|               | `confidenceScore` | AI confidence level (0.0 to 1.0)          | DECIMAL(5,2) |          |                     |
|               | `severity`        | Severity level (Low, Medium, High)        | VARCHAR(20)  |          |                     |
| **PROGRESS**  | `progressId`      | Unique identifier for a progress update   | INT          | PK       |                     |
|               | `caseId`          | Reference to the parent case              | INT          | FK       | CASE                |
|               | `updateDate`      | Timestamp of the update                   | DATETIME     |          |                     |
|               | `statusChange`    | Description of change (e.g., "Healing")   | VARCHAR(50)  |          |                     |
|               | `newImageURL`     | Link to the new image for comparison      | VARCHAR(255) |          |                     |
| **CASE_NOTE** | `noteId`          | Unique identifier for a text note         | INT          | PK       |                     |
|               | `caseId`          | Reference to the parent case              | INT          | FK       | CASE                |
|               | `message`         | Content of the note                       | TEXT         |          |                     |
|               | `timestamp`       | Time the note was added                   | DATETIME     |          |                     |

---

| TABLE NAME         | ATTRIBUTE NAME | CONTENTS                                 | TYPE         | PK OR FK | FK REFERENCED TABLE |
| :----------------- | :------------- | :--------------------------------------- | :----------- | :------- | :------------------ |
| **CROP_AILMENT**   | `ailmentId`    | Unique identifier for the pest/disease   | INT          | PK       |                     |
|                    | `name`         | Common name (e.g., Anthracnose)          | VARCHAR(100) |          |                     |
|                    | `type`         | Classification (Fungal, Pest, bacterial) | VARCHAR(50)  |          |                     |
| **TREATMENT\_**    | `treatmentId`  | Unique identifier for the recommendation | INT          | PK       |                     |
| **RECOMMENDATION** | `ailmentId`    | Reference to the specific ailment        | INT          | FK       | CROP_AILMENT        |
|                    | `action`       | Recommended action (e.g., "Prune")       | TEXT         |          |                     |
|                    | `frequency`    | How often to apply the treatment         | VARCHAR(100) |          |                     |

---

| TABLE NAME     | ATTRIBUTE NAME     | CONTENTS                                       | TYPE         | PK OR FK | FK REFERENCED TABLE |
| :------------- | :----------------- | :--------------------------------------------- | :----------- | :------- | :------------------ |
| **ADVISORY\_** | `templateId`       | Unique identifier for the message template     | INT          | PK       |                     |
| **TEMPLATE**   | `ailmentId`        | Reference to related ailment (if any)          | INT          | FK       | CROP_AILMENT        |
|                | `messageBody`      | The core text of the advisory                  | TEXT         |          |                     |
|                | `triggerCondition` | Logic rule (e.g., "Humidity > 90%")            | VARCHAR(255) |          |                     |
| **WEATHER\_**  | `metricId`         | Unique identifier for raw API data             | INT          | PK       |                     |
| **METRIC**     | `temperature`      | Recorded temperature in Celsius                | DECIMAL(5,2) |          |                     |
|                | `humidity`         | Recorded humidity percentage                   | DECIMAL(5,2) |          |                     |
|                | `rainfall`         | Recorded rainfall in mm                        | DECIMAL(5,2) |          |                     |
|                | `timestamp`        | Time the data was fetched                      | DATETIME     |          |                     |
| **WEATHER\_**  | `alertId`          | Unique identifier for a generated alert        | INT          | PK       |                     |
| **ALERTS**     | `metricId`         | Reference to the raw data used                 | INT          | FK       | WEATHER_METRIC      |
|                | `alertLevel`       | Level (Advisory, Watch, Warning)               | VARCHAR(20)  |          |                     |
|                | `description`      | Description of the threat                      | VARCHAR(255) |          |                     |
| **SENT\_**     | `advisoryId`       | Unique identifier for the transaction          | INT          | PK       |                     |
| **ADVISORY**   | `farmerId`         | Reference to the recipient                     | INT          | FK       | FARMER              |
|                | `farmAdminId`      | Reference to the sender (system/admin)         | INT          | FK       | FARM_ADMIN          |
|                | `templateId`       | Reference to the message template used         | INT          | FK       | ADVISORY_TEMPLATE   |
|                | `alertId`          | Reference to the weather alert (if applicable) | INT          | FK       | WEATHER_ALERTS      |
|                | `sentDate`         | Timestamp when the advisory was sent           | DATETIME     |          |                     |
|                | `status`           | Delivery status (Sent, Failed, Read)           | VARCHAR(20)  |          |                     |

---

| TABLE NAME   | ATTRIBUTE NAME  | CONTENTS                                    | TYPE        | PK OR FK | FK REFERENCED TABLE |
| :----------- | :-------------- | :------------------------------------------ | :---------- | :------- | :------------------ |
| **TICKET**   | `ticketId`      | Unique identifier for the support request   | INT         | PK       |                     |
|              | `farmerId`      | Reference to the farmer creating the ticket | INT         | FK       | FARMER              |
|              | `farmAdminId`   | Reference to the admin assigned             | INT         | FK       | FARM_ADMIN          |
|              | `issueCategory` | Type of issue (Account, App Bug, Crop)      | VARCHAR(50) |          |                     |
|              | `status`        | Ticket status (Open, In Progress, Closed)   | VARCHAR(20) |          |                     |
|              | `priority`      | Urgency level (Low, High, Critical)         | VARCHAR(20) |          |                     |
| **TICKET\_** | `messageId`     | Unique identifier for a chat message        | INT         | PK       |                     |
| **MESSAGE**  | `ticketId`      | Reference to the parent ticket              | INT         | FK       | TICKET              |
|              | `senderRole`    | Who sent it (Farmer or Admin)               | VARCHAR(20) |          |                     |
|              | `messageBody`   | Content of the message                      | TEXT        |          |                     |
|              | `timestamp`     | Time the message was sent                   | DATETIME    |          |                     |
