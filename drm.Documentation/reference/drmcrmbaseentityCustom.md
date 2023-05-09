# drm.crmbaseentity custom template reference

API Version: 2023-01-09

## Template format

To update drm.crmbaseentity/custom values, add the following JSON to the resources section of your template.

```json
{
   "targetenvironment": {
     "bearerToken": "string", 
     "userCredentials": { 
         "username": "string", 
         "password": "string", 
         "clientId": "string", 
         "tenantId": "string" 
     }, 
     "applicationCredentials":{ 
         "clientId": "string", 
         "clientSecret": "string", 
         "tenantId": "string" 
     }, 
     "url": "string" 
   },
   "queries": {
     "{queryPropertyName}": {
        "entity": "string",
        "filter": "string"
     }
   },
   "type":"drm.crmbaseentity/custom",
   "apiVersion":"2023-01-09",
   "name":"string",
   "properties":{
      "entityname": "string",
      "data":[
         {
            "uniqueId": "string",
            "column1": "string"
         }
      ]
   }
}

```

## Property values

The following tables describe the values you need to set in the schema.

### drm.crmbaseentity/custom object

| Name       | Type   | Required | Value                                                                                                    |
|-|-|-|-|
| targetenvironment | object | Yes | Object containing Dynamics connection information <br> [TargetEnvironment object](xref:target-dynamics-instance)
| name       | string | Yes      | The name of the resource block being deployed.  Used to easily identify the resource in deployment logs. |
| type       | enum   | Yes      | drm.crmbaseentity/custom                                                                |
| apiVersion | enum   | Yes      | 2023-01-09                                                                                             |
| properties | object | Yes      | The parameters used to patch custom entities <br> [CustomParameters object](#customparameters-object)                  |

### CustomParameters object

| Name   | Type  | Required | Value                                                    |
|-|-|-|-|
| entityname | string | Yes | The name of the entity you are targeting. This should also be the collection name. |
| data | object | Yes | Each object should represent a 'row' of data you are patching into the entity.  Please see the example [here](xref:example-manage-custom-entities) on how to add data. |

### queries object

| Name       | Type   | Required | Value                                                                                                    |
|-|-|-|-|
| \{queryPropertyName\} | object | Yes | The name of the query, this will be used to reference the results in the template. [query filter object](#queries-filter-object) |

### queries filter object

| Name       | Type   | Required | Value                                                                                                    |
|-|-|-|-|
| entity | string | Yes | The name of the entity you are targeting. See this [list](xref:supported-web-entities) for a collection of supported entities |
| filter | string | Yes | The OData based filter string.|

### targetenvironment object

| Name | Type | Required | Value |
|-|-|-|-|
| bearerToken| string | No | A bearer token |
| userCredentials | object | No | User credential details <br> [UserCredentials object](#usercredentials) |
| applicationCredentials | object | No | App registration details <br> [ApplicationCredentials object](#applicationcredentials) |
| url | string | Yes | The url of the Dynamics instance |

### ApplicationCredentials

| Name | Type | Required | Value |
|-|-|-|-|
| clientId | string | Yes | Application client id |
| clientSecret | string | Yes | Application client secret |
| tenantId | string | Yes | Tenant id where registered application resides |

### UserCredentials

| Name | Type | Required | Value |
|-|-|-|-|
| username | string | Yes | Email address of the user account |
| password | string | Yes | Password of the user account |
| clientId | string | Yes | Application client id |
| tenantId | string | Yes | Tenant id where registered application resides |