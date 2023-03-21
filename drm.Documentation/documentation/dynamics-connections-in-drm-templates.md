# Making Dynamics connections in DRM templates

In this post we will explore the connection object used in DRM templates. This connection information handles the connection to your dynamics instance.

- There are three types of connections available:
- Application Registration credentials
- User credentials
- Bearer Token

Please read this post about each type of connection [Targeting a Dynamics Instance](xref:target-dynamics-instance).

## Application Registration connections

In most circumstances you would use an application registration setup to connect to your dynamics instances.

To setup this please follow this guide - [Setup an App Reg Connection](xref:setup-app-reg-connection)

There are various ways for creating a connection object.

### Example connections using Application Registrations

The best approach for maintainability is to setup the connection object as a variable.

``` json
{ 
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0", 
  "parameters": { 
     "dynamicsTenantId": { 
        "type": "string", 
        "defaultValue": "25c26126-dd6c-4119-90ba-bd4fa9025c77"
     },
     "dynamicsBaseUrl": { 
        "type": "string" 
     },
     "drmclientId": { 
        "type": "string", 
        "defaultValue": "20307919-5ea3-49b5-ad6f-b35e3a833231" 
     },
     "drmclientSecret": { 
        "type": "string" 
     }, 
     ...
  },
  "variables": { 
     "dynamicsCredentials": { 
        "applicationCredentials": { 
           "clientId": "[parameters('drmclientId')]",
           "clientSecret": "[parameters('drmclientSecret')]",
           "tenantId": "[parameters('dynamicsTenantId')]"
        },
        "url": "[parameters('dynamicsBaseUrl')]" 
     }
  },
  ...
  "resources": [
    {
       "targetenvironment": "[variables('dynamicsCredentials')]",
```

With the dynamicsCredentials variable setup it can be easily referenced in all resources targeted. 
If any of the values change then it only needs to be done in one place rather than each resource.

We have also used default values for the client id and tenant id. 
These values are unlikely to change so setting them in the template makes sense.

Another approach is not to use variables but set the object directly in the resource. 
In this example we have also set the client and tenant id directly in the resource. 
dynamicsBaseUrl and drmclientSecret remain as parameters because they could change per environment.

> [!WARNING]
> Application registration client secrets should remain secure.  
> It's advised that this value is retrieved from a key vault or as a secure variable in ADO for example.

``` json
{ 
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0", 
  "parameters": {
     "dynamicsBaseUrl": { 
        "type": "string" 
     },
     "drmclientSecret": { 
        "type": "string" 
     }, 
     ...
  },
  ...
  "resources": [
    {
       "targetenvironment": { 
          "applicationCredentials": { 
             "clientId": "20307919-5ea3-49b5-ad6f-b35e3a833231",
             "clientSecret": "[parameters('drmclientSecret')]",
             "tenantId": "25c26126-dd6c-4119-90ba-bd4fa9025c77"
          },
          "url": "[parameters('dynamicsBaseUrl')]" 
       }
     },
```

## User Credentials Connection

In some situations you may need to connect to your dynamics instance using user credentials.

User accounts use an application registration to authenticate against. 
You can follow this guide for setting up your app registration [here](../tutorials/setup-app-reg-connection.md).

### User Credential Examples

In this example we are setting the username directly in the variable but this could also be a parameter.

```json
{ 
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0", 
  "parameters": { 
     "dynamicsTenantId": { 
        "type": "string", 
        "defaultValue": "25c26126-dd6c-4119-90ba-bd4fa9025c77"
     },
     "userPassword": {
        "type": "securestring"
     }
     "dynamicsBaseUrl": { 
        "type": "string" 
     },
     "drmclientId": { 
        "type": "string", 
        "defaultValue": "20307919-5ea3-49b5-ad6f-b35e3a833231" 
     }
     ...
  },
  "variables": { 
     "dynamicsCredentials": { 
        "userCredentials": { 
           "username": "svc_test@test.onmicrosoft.com",
           "password": "[parameters('userPassword')]",
           "clientId": "[parameters('drmclientId')]",
           "tenantId": "[parameters('dynamicsTenantId')]"
        },
        "url": "[parameters('dynamicsBaseUrl')]" 
     }
  },
  ...
  "resources": [
    {
       "targetenvironment": "[variables('dynamicsCredentials')]",
```

The dynamicsCredentials variable can be easily referenced in the resource and if properties changed 
means they only have to change in one place.

The 'userPassword' parameter is passed via a parameter. As this is a secure string its advisable to 
reference the value from an Azure keyvault. Please read this guide on [the parameter file](drm-template-file.md).