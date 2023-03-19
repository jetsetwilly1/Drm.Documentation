---
uid: connect-to-dynamics-with-usercreds
---

# Connecting to Dynamics using User credentials

In some situations you may need to connect to your dynamics instance using 
user credentials.

User accounts use an application registration to authenticate against.

> [!NOTE]
> The user account must have a Dynamics licence assigned for the connection to work

Once the application is registered and an account has been identified in Dynamics 
for use then the connection block would look something like below.

```json
{ 
  "targetenvironment": { 
     "userCredentials": { 
        "username": "test@test.onmicrosoft.com", 
        "password": "Pas5w0rd", 
        "clientId": "00000000-0000-0000-0000-000000000000", 
        "tenantId": "00000000-0000-0000-0000-000000000000"
     },
     "url": "https://{yourdynamicsinstance}.crm11.dynamics.com" 
  }, 
  "type": "cT.crmbaseentity/workflows", 
  "apiVersion": "2023-01-09", 
  ...
}
```

> [!NOTE]
> Admin consent needs to be granted on the application registration api permissions.
Further information on this can be found [here](xref:setup-app-reg-connection).

> [!WARNING]
> For a successful connection using user credentials, MFA must be turned off on the account.

## Prerequisites

1.  An existing application registration setup in Azure. We will reference this in 
the walkthrough. To [setup an App Reg Connection follow this guide.](xref:setup-app-reg-connection)
2.  A Dynamics environment with system administrator access, this is the environment
 you will deploy the template too.
3. PowerShell with the DRM module installed. Follow this guide to 
[install the powerhsell module](xref:install-powerhsell-module)

## Create a template

Let's create a template that will create a new contact

Create a new file called 'usercredsdrm.json' and copy the json template below. 

```json
{
  "$schema": "https://drmtemplatesprod.z33.web.core.windows.net/schemas/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "targetenvironment": {
        "userCredentials": { 
          "username": "<your user account>", 
          "password": "<your password>", 
          "clientId": "<your app reg client id>", 
          "tenantId": "<your tenant id>"
        },
        "url": "<your dynamics url>"
      },
      "type": "cT.crmbaseentity/contacts",
      "apiVersion": "2023-01-09",
      "name": "DrmDemo_contacts",
      "properties": {
        "data": [
          {
            "contactid": "a209aa4c-90ab-ec11-983f-002248836dab",
            "fullname": "Testing DRM UserCreds"
          }
        ]
      }
    }
  ]
}
```

>[!WARNING]
> We advise not to leave sensitive information like account passwords as plain text.
> If this was being used in an ADO pipeline then the password could be retrieved from
> an ADO variable or keyvault.

## Deploy the template

To deploy the template run this command in your PowerShell session.

```powershell
New-DrmDeployment -TemplateFile '<absolute path to your template>'
```

Log into your Dynamics environment and search for a contact with a fullname of 
```Testing DRM UserCreds```

## Next steps

- Learn how to [reference keyvault secrets in parameter files.](xref:reference-keyvault-secrets)