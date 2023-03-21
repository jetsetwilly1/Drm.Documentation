---
uid: example-manage-business-unit
---


# Manage Business Units

In this example we are going to rename the root business unit and manage a child business unit.

When managing the root business unit it is more likely to have a different unique id per 
environment you are managing. Root business units are created at the same time a new 
environment is initialised. Because of this we will need to get the root business unit id 
and parameterise it in our templates.

## Prerequisites

Before running through this demo it is assumed you have setup your connection details. 
Check [this document out on how to target a dynamics instance](xref:target-dynamics-instance).

## Root business unit

When creating a new dynamics instance a root business unit will automatically be created and look 
something like this

![Root business unit](/images/bu-rootbusinessunit.png "Root business unit")

For the DRM template we are interested in the guid for the root business unit as we know this 
will be different across all environments we want to manage.

Note down the guid for this environment.

## Template

In this example, the ```rootBUId``` parameter references the unique identifier of 
the root business unit in a target environment. It is used to set the 
```businessunitid``` property of the first record in the template below,
the name is set to "BU DEMO".

The child business unit then sets its ```parentbusinessunitid``` property to the 
```rootBUId``` parameter, making "BU DEMO" its parent business unit.

```json
{ 
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0", 
  "parameters": { 
     "rootBUId": {
        "type": "string" 
     } 
  }, 
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
       "type": "drm.crmbaseentity/businessunits", 
       "apiVersion": "2023-01-09", 
       "name": "DEMO_BusinessUnits", 
       "properties": { 
          "data": [ 
             { 
                "businessunitid": "[parameters('rootBUId')]",
                "name": "BU DEMO" 
             },
             { 
                "businessunitid": "e6145646-cd42-ea11-a812-000d3a7ed30d", 
                "name": "Child Business Unit", 
                "parentbusinessunitid": "[parameters('rootBUId')]" 
             }
          ]
       }
     }
  ] 
}
```

## Parameters

The parameters file will look as below

```json
{ 
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentParameters.json#", 
  "contentVersion": "1.0.0.0", 
  "parameters": { 
     "rootBUId": { 
        "value": "3233847C-9796-EC11-B400-000D3ADCE87B" 
     }
  }
}
```

## Next steps

[Create a pipeline in ADO and automate your deployment](xref:deploy-drm-ado-pipelines).