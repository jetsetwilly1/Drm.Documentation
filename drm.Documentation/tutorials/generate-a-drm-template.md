---
uid: generate-drm-template
---

# Generate a template

You can generate a template from an existing Dynamics environment using odata based queries and the Powershell 
module.

Instead of hand crafting a template, just use the ```New-DrmTemplate``` command to generate it for you.

## Prerequisites

1.  A Dynamics environment with system administrator access, this is the environment
 you will use to generate a template from.
2.  Powershell 5+ to generate the template.

## Install the Powershell module

You can install the PowerShell module from the PowerShell gallery.

Currently we only support PowerShell Desktop v5 upwards.

```powershell
Install-Module -Name Drm.Templates.Powershell -AllowClobber
```

## Connect to your Dynamics environment

Before generating templates you need to connect to the Dynamics environment.

Run the command below

```powershell
Connect-Crmonline -Username "<useraccount>" -ServerUrl <dynamics url>
```

An object is returned containing information on the connection you have to Dynamics.

## Generate a template

Now we have a connection to Dynamics you can generate templates.

>[!NOTE]
> By default templates are saved at the working directory location.  To change the location of the
saved template use the ```-OutputToFile``` parameter.

### Filtering data

You can filter the data returned in your template by using the ```$filter``` option in your query.

For example the query below will return all teams that have a name equal to 'Drm Team'.

```powershell
New-DrmTemplate -entityName teams -filter '$select=name&$filter=name eq ''Drm Team''' 
```

>[!NOTE]
>If you need to filter by a string and need to quote it.  Please prefix the single quote with another.
As per the example above.

Another example which returns all document templates that equal 'Drm Document'.

```powershell
New-DrmTemplate -entityName documenttemplates -filter '$filter=name eq ''Drm Document'''
```

### Examples

#### Teams

To generate a template that returns all teams in your dynamics environment.

```powershell
New-DrmTemplate -entityName teams
```

The template returned will look something like below.

```json
{
  "$schema": "https://drmtemplatesprod.z33.web.core.windows.net/schemas/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    
  },
  "resources": [
    {
      "targetenvironment": {
        "bearerToken": "<Enter token here>",
        "url": "<Dynamics environment url>"
      },
      "type": "cT.principal/teams",
      "apiVersion": "2023-01-09",
      "name": "GeneratedTemplateFor_teams",
      "properties": {
        "data": [
          {
            "teamid": "702254fd-01a7-ec11-9840-000d3a2fa64c",
            "name": "orgafd36c6e",
            "description": "Default team for the parent business unit. The name and membership for default team are inherited from their parent business unit.",
            "teamtype": 0,
            "membershiptype": 0,
            "ownerid": "702254fd-01a7-ec11-9840-000d3a2fa64c",
            "queueid": "712254fd-01a7-ec11-9840-000d3a2fa64c",
            "businessunitid": "6f2254fd-01a7-ec11-9840-000d3a2fa64c",
            "administratorid": "612954fd-01a7-ec11-9840-000d3a2fa64c"
          }
        ]
      }
    }
  ]
}
```

To return only the team name in your template run

```powershell
New-DrmTemplate -entityName teams -filter '$select=name'
```

The resulting template would look something like.

>[!NOTE]
> The owner id property is returned in this case as its the team entities primary key.
> See [https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/reference/team?view=dataverse-latest](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/reference/team?view=dataverse-latest)

```json 
{
  "$schema": "https://drmtemplatesprod.z33.web.core.windows.net/schemas/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    
  },
  "resources": [
    {
      "targetenvironment": {
        "bearerToken": "<Enter token here>",
        "url": "<Dynamics environment url>"
      },
      "type": "cT.principal/teams",
      "apiVersion": "2023-01-09",
      "name": "GeneratedTemplateFor_teams",
      "properties": {
        "data": [
          {
            "teamid": "702254fd-01a7-ec11-9840-000d3a2fa64c",
            "name": "orgafd36c6e",
            "ownerid": "702254fd-01a7-ec11-9840-000d3a2fa64c"
          }
        ]
      }
    }
  ]
}
```

If you wanted to expand team members for every team you can run this

```powershell
New-DrmTemplate -entityName teams -filter '$select=name&$expand=teammembership_association($select=fullname,systemuserid)'
```

This will output a template like this 

```json
{
  "$schema": "https://drmtemplatesprod.z33.web.core.windows.net/schemas/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    
  },
  "resources": [
    {
      "targetenvironment": {
        "bearerToken": "<Enter token here>",
        "url": "https://orgafd36c6e.crm4.dynamics.com/"
      },
      "type": "cT.principal/teams",
      "apiVersion": "2023-01-09",
      "name": "GeneratedTemplateFor_teams",
      "properties": {
        "data": [
          {
            "teamid": "702254fd-01a7-ec11-9840-000d3a2fa64c",
            "name": "orgsfd36c6e",
            "ownerid": "702254fd-01a7-ec11-9840-000d3a2fa64c",
            "teammembership_association": [
              {
                "fullname": "Drm Dave",
                "systemuserid": "d30c9e0a-1ada-ec11-a7b5-000d3a2efdfa",
                "ownerid": "d30c9e0a-1ada-ec11-a7b5-000d3a2efdfa"
              },
              {
                "fullname": "Stuart Elcocks",
                "systemuserid": "612954fd-01a7-ec11-9840-000d3a2fa64c",
                "ownerid": "612954fd-01a7-ec11-9840-000d3a2fa64c"
              }
            ]
          }
        ]
      }
    }
  ]
}
```

#### Environment variables

To return all environment variable values from a Dynamics environment but only including the 
schemaname and value properties.

```powershell
New-DrmTemplate -entityName environmentvariablevalues -filter '$select=schemaname,value'
```
#### Business units

Or to return every business unit.

```powershell
New-DrmTemplate -entityName businessunits
```

>[!NOTE]
> The command ```New-DrmTemplate``` will not return 'null' values.

## How can this be used in an automated setup?

Being able to generate templates makes the process of setting up your automation pipelines easier.

With [Dynamics ALM](https://learn.microsoft.com/en-us/power-platform/alm/overview-alm) in mind, you 
might have a master development environment in place that is the current 'source of truth' for the dynamics 
applications being developed.  This could include configuration of

- Organisation settings
- Connection references
- Workflows (Cloud flows)
- Environment variables
- Business units
- Roles
- Teams
- Queues

This list is just an example but by extracting basic configuration of the entities above means you have an
immediate starting point for building up templates for test and production environments.

## Next steps
- [Walk through the quick start and quickly deploy a template](xref:drm-quickstart)
- [Learn how to deploy a template with parameters](xref:deploy-drm-template-including-parameters)
- [Learn how DRM connects to dynamics environments](xref:target-dynamics-instance)
- [Connect to Dynamics using an application registration](xref:connect-to-dynamics-with-app-registration)
- [Learn how to write OData queries following this Microsoft documentation](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/query-data-web-api)