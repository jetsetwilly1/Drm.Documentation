---
uid: example-manage-app-user-accounts
---

# Manage Application Users

Using the SystemUsers entity its possible to manage any application user accounts you may require 
in your Dynamics instance.

In this post we will gather the required information from your dynamics environment and put 
together the DRM template.

> [!NOTE]
>  If the application user doesn't exist in the target environment, it will be created.

## Prerequisites

Before running through this demo it is assumed you have setup your connection details. 
Check [this document out on how to target a dynamics instance](xref:target-dynamics-instance).

You will also need to know what Business Unit the application user will sit under and 
any role id's that will be assigned to the user.

## Retrieve Business Unit and Role Id's

To retrieve business unit information for a dynamics instance just run this query.

```
GET [Organization URI]/api/data/v9.2/businessunits?$select=businessunitid,name
Accept: application/json 
Content-Type: application/json; charset=utf-8 
OData-MaxVersion: 4.0 OData-Version: 4.0
```

Which will return a list of id's and the name of the business unit.

```json
{ 
  "@odata.context": "[Organization URI]/api/data/v9.2/$metadata#businessunits(businessunitid,name)", 
  "value": [ 
    { 
      "@odata.etag": "W/\"4270038\"",
      "businessunitid": "e6145646-cd42-ea11-a812-000d3a7ed30d",
      "name": "Demo" 
    },
    ...
}
```

From the returned list, select which business unit the application user should belong too.

To retrieve the security roles and id's run this query

```
GET [Organization URI]/api/data/v9.2/roles?$select=roleid,name
Accept: application/json 
Content-Type: application/json; charset=utf-8 
OData-MaxVersion: 4.0 OData-Version: 4.0
```

Returned are the roleid's and name.

```json
{ 
  "@odata.context": "[Organization URI]/api/data/v9.2/$metadata#roles(roleid,name)", 
  "value": [ 
    { 
      "@odata.etag": "W/\"36334771\"",
      "roleid": "00bb53d6-da92-e111-9d8c-000c2959f9b8", 
      "name": "Field Service - Administrator" 
    }, 
    {
      "@odata.etag": "W/\"36334800\"", 
      "roleid": "d008bfde-da92-e111-9d8c-000c2959f9b8",
      "name": "Field Service - Dispatcher" 
    },
    ...
}
```

We now have the right information to build our template.

## Application Registration Client Id

You will need to get the client id of the application registration from Azure.

Log into the Azure portal and copy the client id from the app reg.

![app reg details](/images/app_reg_copy.png "app reg details")

This is the application client id we want to register with Dynamics.

## Template

Our final template will create/update the system user account with the id of 'e2a85abb-8854-4963-8337-93cb41f95d44'.

We have no parameters in this particular example just a variable block which defines the connection.

drm metadata objects are used to describe what the resource block is maintaining.

```json
{ 
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#", 
  "contentVersion": "1.0.0.0",
  "variables": { 
    "dynamicsCredentials": { 
      "applicationCredentials": { 
        "clientId": "45db5702-3d6e-405e-b74b-fae6344b2051", 
        "clientSecret": "secret", 
        "tenantId": "fc9f83b9-9f06-4def-a2b2-cba7589d763e"
      },
      "url": "https://{yourdynamicsinstance}.crm11.dynamics.com"
    }
  },
  "resources": [ 
    {
      "targetenvironment": "[variables('dynamicsCredentials')]", 
      "type": "cT.crmbaseentity/systemusers", 
      "apiVersion": "2023-01-09", 
      "name": "DEMO_ApplicationUser", 
      "properties": { 
        "data": [ 
          { 
            "systemuserid": "e2a85abb-8854-4963-8337-93cb41f95d44", 
            "applicationid": "834030fb-c9d8-46e8-a740-c281a8eea6b1",
            "businessunitid": "e6145646-cd42-ea11-a812-000d3a7ed30d",
            "systemuserroles_association":[
               {
                 "roleid": "00bb53d6-da92-e111-9d8c-000c2959f9b8",
                 "drmmetadata": {
                    "comment": "Field Service - Administrator"
                 }
               }
            ],
            "drmmetadata": {
               "comment": "Demo application user"
            }
          } 
        ]
    }
  ]
}
```

## Next steps

[Create a pipeline in ADO and automate your deployment](xref:deploy-drm-ado-pipelines).