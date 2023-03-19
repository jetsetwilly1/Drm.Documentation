# cT.principal default template reference

API Version: 2022-04-03

cT.principal type entities wraps the following principle entities

- [teams](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/reference/team?view=dataverse-latest)
- [systemusers](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/reference/systemuser?view=dataverse-latest)



## Template format

To target a ct.principal/\{collection\}, add the following JSON to your template.

\{collection\} can be 'teams' or 'systemusers'

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
   "type":"cT.principal/{collection}",
   "apiVersion":"2023-01-09",
   "name":"string",
   "properties":{
      "data":[
         {
            ...
         }
      ]
   }
}

```

## Property values

The following tables describe the values you need to set in the schema.

### cT.crmbaseentity/\{collection\} object

| Name       | Type   | Required | Value                                                                                                    |
|-|-|-|-|
| targetenvironment | object | Yes | Object containing Dynamics connection information.
| queries | object | No | Object containing queries to run against your target Dynamics environment. [queries object](#queries-object) |
| name       | string | Yes      | The name of the resource block being deployed.  Used to easily identify the resource in deployment logs. |
| type       | enum   | Yes      | cT.crmbaseentity/\{collection\} Where collection is one of the known collection names.                                                               |
| apiVersion | enum   | Yes      | 2023-01-09                                                                                            |
| properties | object | Yes      | The parameters used to patch a collection <br> [\{collection\} object](#collection-parameters) |

### queries object

| Name       | Type   | Required | Value                                                                                                    |
|-|-|-|-|
| \{queryPropertyName\} | object | Yes | The name of the query, this will be used to reference the results in the template. [query filter object](#queries-filter-object) |

### queries filter object

| Name       | Type   | Required | Value                                                                                                    |
|-|-|-|-|
| entity | string | Yes | The name of the entity you are targeting. See this [list](xref:supported-web-entities) for a collection of supported entities |
| filter | string | Yes | The Odata based filter string.|

### collection parameters

| Name | Type | Required | Value |
|-|-|-|-|
|\{collection\}|array|Yes| An array of the entity rows to update. See this [teams](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/reference/team?view=dataverse-latest) or [systemusers](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/reference/systemuser?view=dataverse-latest)  |


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