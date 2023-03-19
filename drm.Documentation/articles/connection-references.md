---
uid: example-connection-references
---

# Connection References

This document focuses on connection references.  It is
important to note that if the connection itself is not present in the Dynamics environment where
the connection reference is being deployed, the reference will fail.

In this example, you will achieve the following

- Create a Microsoft Dataverse connection in your target Dynamics environment.
- Generate a template to update a connection reference with a specific name.
- Deploy the template to complete the connection reference.

## Prerequisites

- To deploy the template, you will need a Dynamics environment with system administrator access.
- [An application registration used for connecting to your Dynamics environment.](xref:connect-to-dynamics-with-app-registration)
- PowerShell with the DRM module installed. Follow this guide to 
[install the powerhsell module](xref:install-powerhsell-module)

## Create the connection

As a one time step you will need to create the connection in your target Dynamics environment.

In this example we are going to create a 'Microsoft Dataverse' connection.

![Create new connection](/images/connref-create-new.png)

Once the connection is created we need to share it with the application registration used
to deploy the template.

![Share the connection](/images/connref-create-share.png)

Share the connection with the application registration and select the permission as
'Can use'

![Share the connection](/images/connref-create-share-appreg.png)

Finally lets get the connection id, by clicking on the connection and making a note of the
second guid in the url.  This is the connection id.

![Get the connection id](/images/connref-connid.png)

## Generate the template

In this example we are going to run the ```New-DrmTemplate``` command against the Dynamics
environment we will eventually deploy too.  In the real world you would probably generate the 
template against a master environment, but the outcome would be the same.

We want to update a specific connection reference with the name of 'Drm Connection Reference'.

Run this command against your environment.

```powershell
New-DrmTemplate 
    -entityName connectionreferences 
    -filter '$select=connectionreferencedisplayname&$filter=connectionreferencedisplayname eq ''Drm Connection Reference''' 
    -SetupTemplateForAutomation
```

The template returned would be as below.

>[!NOTE]
>This example is for updating one connection reference, adjust the filter to 
suit your requirements.

```json
{
  "$schema": "https://drmtemplatesprod.z33.web.core.windows.net/schemas/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "drmclientId": {
      "type": "string",
      "defaultValue": "00000000-0000-0000-0000-000000000000"
    },
    "drmclientSecret": {
      "type": "string",
      "defaultValue": "00000000-0000-0000-0000-000000000000"
    },
    "dynamicsTenantId": {
      "type": "string",
      "defaultValue": "00000000-0000-0000-0000-000000000000"
    }
  },
  "resources": [
    {
      "targetenvironment": {
        "applicationCredentials": {
          "clientId": "[parameters('drmclientId')]",
          "clientSecret": "[parameters('drmclientSecret')]",
          "tenantId": "[parameters('dynamicsTenantId')]"
        },
        "url": "<Your dynamics url>"
      },
      "type": "cT.crmbaseentity/connectionreferences",
      "apiVersion": "2023-01-09",
      "name": "GeneratedTemplateFor_connectionreferences",
      "properties": {
        "data": [
          {
            "connectionreferenceid": "a14ada61-aa5b-ab12-acda-a5a9f14e7f94",
            "connectionreferencedisplayname": "Drm Connection Reference"
          }
        ]
      }
    }
  ]
}
```

For this exercise populate the ```defaultValue``` for ```drmclientId```,
```drmclientSecret``` and ```dynamicsTenantId```.  Also make sure the ```url```
property is correct.


## Deploying the template

Before deploying the template we need to add a property to our generated template
that will update the connectionreference to our 'Dataverse' connection you 
created earlier.

Following our example add the property ```connectionid``` to the template
data object using the connection id we noted down when creating the connection.

>[!NOTE]
> To update a connection reference with a connection id, the only properties
> you need to include are ```connectionreferenceid``` and ```connectionid```.

```json
"data": [
    {
      "connectionreferenceid": "a14ada61-aa5b-ab12-acda-a5a9f14e7f94",
      "connectionreferencedisplayname": "Drm Connection Reference",
      "connectionid": "be244052c3e14d4cab828d2bfe98fd18"
    }
]
```

To deploy the template run the following command

```powershell
New-DrmDeployment -TemplateFile '<absolute path to the template file>'
```


## Next steps

[Learn how to manage workflows](xref:example-manage-workflows) for example turning them
on and off to update cached environment variables or connection references.