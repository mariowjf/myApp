# Dictator API
Dictator API is a *lightweight cloud resource management* service similar to its standalone library presence in the current ecosystem.

## Contents

* [Image APIs](#1-image-apis)
  * [List all Images](#11-list-all-images)
  * [Create an Image](#12-create-an-image)
  * [Retrieve an Image](#13-retrieve-an-image)
  * [Update an Image](#14-update-an-image)
  * [Remove an Image](#15-remove-an-image)
  * [Show all Images status](#16-show-all-images-status)
  * [Show Images count](#17-show-images-count)
* [Instance APIs](#2-instance-apis)
  * [List all Instances](#21-list-all-instances)
  * [Create Instances](#22-create-instances)
  * [Retrieve an Instance](#23-retrieve-an-instance)
  * [Update an Instance](#24-update-an-instance)
  * [Remove an Instance](#25-remove-an-instance)
  * [Start an Instance](#26-start-an-instance)
  * [Stop an Instance](#27-stop-an-instance)
  * [Lock an Instance](#28-lock-an-instance)
  * [Unlock and Instance](#29-unlock-an-instance)
  * [Show all Instances status](#210-show-all-instances-status)
  * [Show Instances count](#211-show-instances-count)
* [Instance Snapshot APIs](#3-instance-snapshot-apis)
  * [List all Instance Snapshots](#31-list-all-instance-snapshots)
  * [Create an Instance Snapshot](#32-create-an-instance-snapshot)
  * [Retrieve an Instance Snapshot](#33-retrieve-an-instance-snapshot)
  * [Update an Instance Snapshot](#34-update-an-instance-snapshot)
  * [Remove an Instance Snapshot](#35-remove-an-instance-snapshot)
  * [Revert an Instance Snapshot](#36-revert-an-instance-snapshot)
* [Instance Guest Operation APIs](#4-instance-guest-operation-apis)
  * [Check file existence on instance](#41-check-file-existence-on-instance)
  * [Copy file to instance](#42-copy-file-to-instance)
  * [Run program on instance](#43-run-program-on-instance)

## 1 Image APIs

### 1.1 List all Images

URL|Method
-----|-----
/images|GET
/images?region={region name}|GET

+ Response 200 (application/json)

        [
          {
            "id": 12345,
            "name": "m20_71_win7ent_64",
            "owner": "itools",
            "applications": ["inv"],
            "tags": [],
            "platform": "win7_64",
            "label": "m.20.0.7100.9000",
            "type": "official",
            "ispublic": true,
            "status": "ready",
            "description": "",
            "createdate": "2014-12-08T18:25:43.511Z",
            "expiredate": "2015-12-08T18:25:43.511Z",
            "parentid": 1,
            "region": "ECS-USWEST"
          }, 
          {
            "id": 23456,
            "name": "m20_71_win7ent_64",
            "owner": "itools",
            "applications": ["lt", "oem", "datasrv", "grasrv"],
            "tags": [],
            "platform": "win7_64",
            "label": "m.20.0.7100.9000",
            "type": "official",
            "ispublic": true,
            "status": "publishing",
            "description": "",
            "createdate": "2014-12-08T18:25:43.511Z",
            "expiredate": "2015-12-08T18:25:43.511Z",
            "parentid": 2,
            "region": "ECS-USWEST-DEV"
          }
        ]
        
Attribute|Value Type|Description
-----|-----|-----
id|number|image repository id
name|string|image name
owner|string|image owner who created the image
applications|string array|pre-installed applications in the image
tags|string array|custom tags of the image
platform|string|OS platform on which the image was built
label|string|label to show image version, here is source Perforce label
type|enum: template, official, custom, codecoverage|image type
ispublic|boolean|determines an image's visibility
status|enum: publishing, ready, error|indicates an image's status
description|string|custom image description
createdate|datetime|indicates when an image was created
expiredate|datetime|indicates when an image is to expire
parentid|number|parent image repository id
region|string|name of the availability region where the instance resides

### 1.2 Create an Image
URL|Method
-----|-----
/images|POST

+ Request (application/json)

        {
            "instanceid": 12345,
            "name": "m20_71_win7ent_64",
            "owner": "itools",
            "applications": ["inv"],
            "tags": [],
            "label": "m.20.0.7100.9000",
            "type": "official",
            "description": ""
          }

Attribute|Value Type|Required|Description
-----|-----|-----|-----
instanceid|number|yes|instance repository id from which to build an image
name|string|yes|image name
owner|string|no|image owner who created the image, default to 'itools'
applications|string array|no|pre-installed applications in the image
tags|string array|no|custom tags of the image
label|string|no|label to show image version, here is source Perforce label
type|enum: template, official, custom, codecoverage|no|image type
description|string|no|custom image description

+ Response

  - On success: 202 (application/json)

          {
            "id": 12345
          }

  - On failure: 400 (application/json)

          {
            "error": "Error message"
          }
 
    or an array of error messages if there are any problem saving the db.
 
          {
            "errors": ["Error message1", ...]
          }


Attribute|Value Type|Description
-----|-----|-----
id|number|image repository id

### 1.3 Retrieve an Image
URL|Parameters|Method
-----|-----|-----
/images/{id}|id (required, number, 12345)|GET

+ Response

  - On success: 200 (application/json)

          {
            "id": 12345,
            "name": "m20_71_win7ent_64",
            "owner": "itools",
            "applications": ["inv"],
            "tags": [],
            "platform": "win7_64",
            "label": "m.20.0.7100.9000",
            "type": "official",
            "ispublic": true,
            "status": "ready",
            "description": "",
            "createdate": "2014-12-08T18:25:43.511Z",
            "expiredate": "2015-12-08T18:25:43.511Z",
            "parentid": 1,
            "region": "ECS-USWEST-DEV"
          }
        
  - On failure: 400 (application/json)

          {
            "error": "Image not exist"
          }


### 1.4 Update an Image
URL|Parameters|Method
-----|-----|-----
/images/{id}|id (required, number, 12345)|PUT

+ Request (application/json)

        {
            "name": "m20_71_win7ent_64",
            "owner": "itools",
            "applications": ["inv"],
            "tags": [],
            "label": "m.20.0.7100.9000",
            "type": "official",
            "description": ""
        }
        
Attribute|Value Type|Required|Description
-----|-----|-----|-----
name|string|no|image name
owner|string|no|image owner who created the image
applications|string array|no|pre-installed applications in the image
tags|string array|no|custom tags of the image
label|string|no|label to show image version, here is source Perforce label
type|enum: template, official, custom, codecoverage|no|image type
description|string|no|custom image description
          
+ Response

  - On success: 202 (application/json)

          {
          }

  - On failure: 400 (application/json)

          {
            "error": "Error message"
          }
 
    or an array of error messages if there are any problem updating the db.
 
          {
            "errors": ["Error message1", ...]
          }


### 1.5 Remove an Image
URL|Parameters|Method
-----|-----|-----
/images/{id}|id (required, number, 12345)|DELETE

+ Response

  - On success: 202 (application/json)

          {
          }
          
  - On failure: 400 (application/json)

          {
            "error": "Image not exist"
          }

### 1.6 Show all Images status

URL|Method
-----|-----
/images/status|GET
/images/status?region={region name}|GET

+ Response 200 (application/json)

        [
          {
            "id": 12345,
            "status": "ready"
          },
          {
            "id": 23456,
            "status": "publishing"
          }
        ]

Attribute|Value Type|Description
-----|-----|-----
id|number|image repository id
status|enum: publishing, ready, error|indicates an image's status

### 1.7 Show Images count

URL|Method
-----|-----
/images/count|GET

+ Response 200 (application/json)

        {
          "count": 123
        }

Attribute|Value Type|Description
-----|-----|-----
count|number|number of images exist on MMS

## 2 Instance APIs

### 2.1 List all Instances
URL|Method
-----|-----
/instances|GET
/instances?region={region name}|GET

+ Response 200 (application/json)

        [
          {
            "id": 12345
            "name": "dtf-ecsdtfsrv-prd-0012",
            "owner": "itools",
            "ipaddress": "10.43.82.123",
            "tags": [],
            "islocked": false,
            "status": "running",
            "description": "",
            "createdate": "2014-12-08T18:25:43.511Z",
            "platform": "windows 7 (64-bit)",
            "applications": ["inv"],
            "label": "m.20.0.7100.9000",
            "type": "official",
            "region": "ECS-USWEST",
            "terminateafter: "2014-12-10T18:25:43.511Z",
            "stopafter: "2014-12-09T18:25:43.511Z"
          }, 
          {
            "id": 23456,
            "name": "dtf-ecsdtfsrv-prd-0014",
            "owner": "itools",
            "ipaddress": "10.43.82.111",
            "tags": [],
            "islocked": false,
            "status": "stopped",
            "description": "",
            "createdate": "2014-12-08T18:25:43.511Z",
            "platform": "windows 8 (64-bit)",
            "applications": ["inv"],
            "label": "m.20.0.7100.9000",
            "type": "custom",
            "region": "ECS-USWEST-DEV",
            "terminateafter: "2014-12-10T18:25:43.511Z",
            "stopafter: "2014-12-09T18:25:43.511Z"
          }
        ]
        
Attribute|Value Type|Description
-----|-----|-----
id|number|instance repository id
name|string|instance name
owner|string|instance owner who created the instance
ipaddress|string|instance ip address
tags|string array|custom tags of the instance
islocked|boolean|determines if an instance is locked or not
status|enum: starting, stopping, running, stopped, timeout, error|indicates an instance's status
description|string|custom instance description
createdate|datetime|indicates when an instance was created
platform|string|OS platform running on the instance
applications|string array|pre-installed applications in the image
label|string|label to show image version, here is source Perforce label
type|enum: template, official, custom, codecoverage|image type
region|string|name of the availability region where the instance resides
terminateafter|datetime|indicates when the instance will be terminated
stopafter|datetime|indicates when the instance will be stopped

### 2.2 Create Instances
URL|Method
-----|-----
/instances|POST

+ Request (application/json)

        {
            "imageid": 12345,
            "name": "dtf-ecsdtfsrv-prd-0010",
            "owner": "itools",
            "instancecount": 1,
            "tags": [],
            "description": "",
            "serviceoffering": "c1.large",
            "terminateafter": "2014-12-10T18:25:43.511Z",
            "stopafter": "2014-12-09T18:25:43.511Z"
        }

Attribute|Value Type|Required|Description
-----|-----|-----|-----
imageid|number|image repository id
name|string|yes|instance name
owner|string|no|instance owner who created the instance, default to 'itools'
instancecount|number|no|the number of instances to create, default to 1
tags|string array|no|custom tags of the instance
description|string|no|custom instance description
serviceoffering|string|no|service offering for the instance, default to 'c1.large'
terminateafter|datetime|no|indicates when the instance will be terminated
stopafter|datetime|no|indicates when the instance will be stopped

+ Response

  - On success: 202 (application/json)

          {
            "ids": [12345]
          }

  - On failure: 400 (application/json)

          {
            "error": "Error message"
          }
 
    or an array of error messages if there are any problem saving the db.
 
          {
            "errors": ["Error message1", ...]
          }


Attribute|Value Type|Description
-----|-----|-----
id|number|instance repository id

### 2.3 Retrieve an Instance
URL|Parameters|Method
-----|-----|-----
/instances/{id}|id (required, number, 12345)|GET

+ Response

  - On success: 200 (application/json)

          {
            "id": 12345,
            "name": "dtf-ecsdtfsrv-prd-0014",
            "owner": "itools",
            "ipaddress": "10.43.82.111",
            "tags": [],
            "status": "stopped",
            "description": "",
            "createdate": "2014-12-08T18:25:43.511Z",
            "platform": "window 7 (64-bit)",
            "region": "ECS-USWEST",
            "terminateafter: "2014-12-10T18:25:43.511Z",
            "stopafter: "2014-12-09T18:25:43.511Z"
          }

  - On failure: 400 (application/json)

          {
            "error": "Instance not exist"
          }

### 2.4 Update an Instance
URL|Parameters|Method
-----|-----|-----
/instances/{id}|id (required, number, 12345)|PUT

+ Request (application/json)

        {
            "name": "dtf-ecsdtfsrv-prd-0014",
            "owner": "itools",
            "tags": [],
            "description": ""
        }
        
Attribute|Value Type|Required|Description
-----|-----|-----|-----
name|string|no|instance name
owner|string|no|instance owner who created the instance
tags|string array|no|custom tags of the instance
description|string|no|custom instance description
          
+ Response

  - On success: 202 (application/json)

          {
          }

  - On failure: 400 (application/json)

          {
            "error": "Error message"
          }
 
    or an array of error messages if there are any problem saving the db.
 
          {
            "errors": ["Error message1", ...]
          }

### 2.5 Remove an Instance
URL|Parameters|Method
-----|-----|-----
/instances/{id}|id (required, number, 12345)|DELETE

+ Response

  - On success: 202 (application/json)

          {
          }
          
  - On failure: 400 (application/json)

          {
            "error": "Instance not exist"
          }

### 2.6 Start an Instance
URL|Parameters|Method
-----|-----|-----
/instances/{id}/start|id (required, number, 12345)|POST

+ Response

  - On success: 202 (application/json)

          {
          }
          
  - On failure: 400 (application/json)

          {
            "error": "Instance not exist"
          }

### 2.7 Stop an Instance
URL|Parameters|Method
-----|-----|-----
/instances/{id}/stop|id (required, number, 12345)|POST

+ Response

  - On success: 202 (application/json)

          {
          }
          
  - On failure: 400 (application/json)

          {
            "error": "Instance not exist"
          }

### 2.8 Lock an Instance
URL|Parameters|Method
-----|-----|-----
/instances/{id}/lock|id (required, number, 12345)|POST

+ Response

  - On success: 202 (application/json)

          {
          }
          
  - On failure: 400 (application/json)

          {
            "error": "Instance not exist"
          }

### 2.9 Unlock an Instance
URL|Parameters|Method
-----|-----|-----
/instances/{id}/unlock|id (required, number, 12345)|POST

+ Response

  - On success: 202 (application/json)

          {
          }
          
  - On failure: 400 (application/json)

          {
            "error": "Instance not exist"
          }

### 2.10 Show all Instances status
URL|Method
-----|-----
/instances/status|GET
/instances/status?region={region name}|GET

+ Response 200 (application/json)

        [
          {
            "id": 12345
            "status": "running"
          },
          {
            "id": 23456,
            "status": "stopped"
          }
        ]

Attribute|Value Type|Description
-----|-----|-----
id|number|instance repository id
status|enum: starting, stopping, running, stopped, timeout, error|indicates an instance's status

### 2.11 Show Instances count

URL|Method
-----|-----
/instances/count|GET

+ Response 200 (application/json)

        {
          "count": 123
        }

Attribute|Value Type|Description
-----|-----|-----
count|number|number of instances exist on MMS

## 3 Instance Snapshot APIs

### 3.1 List all Instance Snapshots
URL|Parameters|Method
-----|-----|-----
/instances/{instanceid}/snapshots|instanceid (required, number, 12345)|GET

+ Response 200 (application/json)

        [
          {
            "id": 23456,
            "name": "cleanos",
            "status": "ready",
            "description": "",
            "createdate": "2014-12-08T18:25:43.511Z"
          }, 
          {
            "id": 34567,
            "name": "cleanos",
            "status": "error",
            "description": "",
            "createdate": "2014-12-08T18:25:43.511Z"
          }
        ]

Attribute|Value Type|Description
-----|-----|-----
id|number|instance snapshot repository id
name|string|instance snapshot name
status|enum: creating, reverting, ready, error|indicates an instance snapshot's status
description|string|custom instance snapshot description
createdate|datetime|indicates when an instance snapshot was created

### 3.2 Create an Instance Snapshot
URL|Parameters|Method
-----|-----|-----
/instances/{instanceid}/snapshots|instanceid (required, number, 12345)|POST

+ Request (application/json)

        {
            "name": "cleanos",
            "description": ""
        }

Attribute|Value Type|Required|Description
-----|-----|-----|-----
name|string|yes|instance snapshot name
description|string|no|custom instance snapshot description

  - On success: 202 (application/json)

          {
            "id": 23456
          }

  - On failure: 400 (application/json)

          {
            "error": "Error message"
          }
 
    or an array of error messages if there are any problem saving the db.
 
          {
            "errors": ["Error message1", ...]
          }

Attribute|Value Type|Description
-----|-----|-----
id|number|instance snapshot repository id

### 3.3 Retrieve an Instance Snapshot
URL|Parameters|Method
-----|-----|-----
/instances/{instanceid}/snapshots/{snapshotid}|instanceid (required, number, 12345); snapshotid (required, number, 23456)|GET

+ Response

  - On success: 200 (application/json)

          {
            "id": 23456,
            "name": "cleanos",
            "status": "ready",
            "description": "",
            "createdate": "2014-12-08T18:25:43.511Z"
          }

  - On failure: 400 (application/json)

          {
            "error": "Instance snapshot not exist"
          }

### 3.4 Update an Instance Snapshot
URL|Parameters|Method
-----|-----|-----
/instances/{instanceid}/snapshots/{snapshotid}|instanceid (required, number, 12345); snapshotid (required, number, 23456)|PUT

+ Request (application/json)

        {
            "name": "cleanos",
            "description": ""
        }
        
Attribute|Value Type|Required|Description
-----|-----|-----|-----
name|string|no|instance snapshot name
description|string|no|custom instance snapshot description
          
+ Response

  - On success: 202 (application/json)

          {
          }

  - On failure: 400 (application/json)

          {
            "error": "Error message"
          }
 
    or an array of error messages if there are any problem updating the db.
 
          {
            "errors": ["Error message1", ...]
          }


### 3.5 Remove an Instance Snapshot
URL|Parameters|Method
-----|-----|-----
/instances/{instanceid}/snapshots/{snapshotid}|instanceid (required, number, 12345); snapshotid (required, number, 23456)|DELETE

+ Response

  - On success: 202 (application/json)

          {
          }
          
  - On failure: 400 (application/json)

          {
            "error": "Image not exist"
          }

### 3.6 Revert an Instance Snapshot
URL|Parameters|Method
-----|-----|-----
/instances/{instanceid}/snapshots/{snapshotid}/revert|instanceid (required, number, 12345); snapshotid (required, number, 23456)|POST

+ Response

  - On success: 202 (application/json)

          {
          }

  - On failure: 400 (application/json)

          {
            "error": "Error message"
          }
 
    or an array of error messages if there are any problem updating the db.
 
          {
            "errors": ["Error message1", ...]
          }




## 4 Instance Guest Operation APIs

### 4.1 Check file existence on instance
URL|Parameters|Method
-----|-----|-----
/instances/{instanceid}/file_exist|instanceid (required, number, 12345)|POST

+ Request (application/json)

        {
            "file": "c:\\1.txt",
            "user": "ads\\somebody",
            "pw": "abc123"
        }
        
    Attribute|Value Type|Required|Description
    -----|-----|-----|-----
    file|string|yes|a file path to check existence on the instance
    user|string|yes|user account to access the instance
    pw|string|yes|password to access the instance

+ Response

  - On success: 202 (application/json)

          {
            "fileExist": true|false
          }

  - On failure: 400 (application/json)
  
    When the instance id is invalid.
  
        {
          "error": "Instance not exist"
        }
  
    or when a parameter (file, user, pw) is nil or empty string.
 
        {
          "error": "Invalid parameters"
        }
  
    or when the Vapor API returns error.

        {
          "error": "Vapor error message"
        }

### 4.2 Copy file to instance
URL|Parameters|Method
-----|-----|-----
/instances/instanceid/tasks/copy_file|instanceid (required, number, 12345)|POST

+ Request (application/json)

        {
            "src": "\\\\server\share\\1.txt",
            "dest": "c:\\1.txt",
            "user": "ads\\somebody",
            "pw": "abc123"
        }
        
    Attribute|Value Type|Required|Description
    -----|-----|-----|-----
    src|string|yes|file path to copy from, it should be in a network folder shared to everyone
    dest|string|yes|file path to copy to, it's on the target instance
    user|string|yes|user account to access the instance
    pw|string|yes|password to access the instance

+ Response

  - On success: 202 (application/json)

          {
          }

  - On failure: 400 (application/json)
  
    When the instance id is invalid.
  
        {
          "error": "Instance not exist"
        }
  
    or when a parameter (src, dest, user, pw) is nil or empty string.
 
        {
          "error": "Invalid parameters"
        }
  
    or when the Vapor API returns error.

        {
          "error": "Vapor error message"
        }
  
    or when the MMS API fails to save data to DB.

        {
          "error": "DB error message"
        }

### 4.3 Run program on instance
URL|Parameters|Method
-----|-----|-----
/instances/instanceid/tasks/run_program|instanceid (required, number, 12345)|POST

+ Request (application/json)

        {
            "app": "C:\\app.exe",
            "args": "some arguments",
            "user": "ads\\somebody",
            "pw": "abc123"
        }
        
    Attribute|Value Type|Required|Description
    -----|-----|-----|-----
    app|string|yes|program executable path on instance
    args|string|yes|program arguments, optional
    user|string|yes|user account to access the instance
    pw|string|yes|password to access the instance

+ Response

  - On success: 202 (application/json)

          {
          }

  - On failure: 400 (application/json)
  
    When the instance id is invalid.
  
        {
          "error": "Instance not exist"
        }
  
    or when a parameter (src, dest, user, pw) is nil or empty string.
 
        {
          "error": "Invalid parameters"
        }
  
    or when the Vapor API returns error.

        {
          "error": "Vapor error message"
        }
  
    or when the MMS API fails to save data to DB.

        {
          "error": "DB error message"
        }
