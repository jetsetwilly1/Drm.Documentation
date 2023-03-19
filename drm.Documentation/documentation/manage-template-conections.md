---
uid: manage-template-connections
---

# Manage template connections

This document is a guide on how to optimize a 
template for connections to your Dynamics environment. 

The goal is to minimize the amount of work required for managing multiple 
Dynamics environments, from test to production, by using a maintainable 
approach for creating the connection object. 

The recommended method for achieving this is to set up the connection
object as a variable.

## Application Registrations

The following template demonstrates how to create a variable 
called ```dynamicsCredentials``` using the appropriate parameters. 
By referencing this variable across all resource blocks, it 
eliminates the need to update each resource block individually
if the connection details change. 

Additionally, default values for the client ID and tenant ID have 
been set, as these values typically remain unchanged.

>[!NOTE]
> Please note that this template has been shortened and should
not be copied and pasted.

```json
{ 
  "$schema": "https://drmtemplatesprod.z33.web.core.windows.net/schemas/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0", 
  "parameters": { 
     "dynamicsTenantId": { 
        "type": "string", 
        "defaultValue": "25c26136-dd6c-4119-90ba-bd4fa9025c77"
     },
     "dynamicsUrl": { 
        "type": "string" 
     },
     "drmclientId": { 
        "type": "string", 
        "defaultValue": "20307319-5ea3-49b5-ad6f-b35e3a833231" 
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
        "url": "[parameters('dynamicsUrl')]" 
     }
  },
  ...
  "resources": [
    {
       "targetenvironment": "[variables('dynamicsCredentials')]",
```

## User Credentials

The same approach could be used for client credentials.

In this example we are setting the username directly in the variable but 
this could also be a parameter.

>[!NOTE]
> The template below has been abbreviated, do not copy and paste the template as it is incomplete.

```json
{ 
  "$schema": "https://drmtemplatesprod.z33.web.core.windows.net/schemas/2021-03-01/deploymentTemplate.json#",
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

The dynamicsCredentials variable can be easily referenced in the resource and 
if user credentials change means they only have to change in one place.

## Security Considerations

When working with application or user credentials, it is essential to take
the necessary security precautions. When passing client secrets or passwords
to DRM templates, it is important to ensure that they are stored securely. 
One way to accomplish this is by [referencing keyvault secrets in parameter 
files](xref:reference-keyvault-secrets). 
Another option is to store credentials in variable groups when using templates in ADO.