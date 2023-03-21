---
uid: new-drmtemplate-cmdlet
---

# New-DrmTemplate cmdlet

Module: Drm.Templates.Powershell

Generate a drm template.

``` powershell
New-DrmTemplate
 -Entity <string> <required> 
 -Filter <string> 
 -SetupTemplateForAutomation <switch>
 -OutputToFile <string>
```

## Description

Connect to a Dynamics environment and use the Web API to build a barebones DRM template for use in automation.

## Examples

Here is an example of how you might use the "New-DrmTemplate" function to generate a template 
from the "teams" entity in a Dynamics environment:

```powershell
New-DrmTemplate 
	-Url https://demo.crm11.com 
	-Entity teams
	-OutputToFile 'C:\templates\teams_template.json'
```

This command connects to the Dynamics environment at "https://demo.crm11.com" and targets the "teams" entity.  
The template is saved to a file named "teams_template.json" in the "C:\templates" directory.

To setup for automation include the -SetupTemplateForAutomation switch. This will include in the generated template a
default set of parameters as below.

For example to generate a template from the "queues" entity in a Dynamics environment, 
filter on "name" field and set it up for automation:

```powershell
New-DrmTemplate -Url https://demo.crm11.com 
	-Entity queues 
	-Filter '$select=name' 
	-SetupTemplateForAutomation 
	-OutputToFile 'C:\templates\queues_template.json'

```

This command connects to the Dynamics environment at "https://demo.crm11.com", targets the "queues" entity,
adds a filter to only select the "name" field, sets up the template for automation,
and saves the generated template to a file named "queues_template.json" in the "C:\templates" directory.

```json
{
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#",
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
        "url": "https://demo.crm4.dynamics.com/"
      },
      "type": "drm.crmbaseentity/queues",
      "apiVersion": "2023-01-09",
      "name": "GeneratedTemplateFor_queues",
      "properties": {
        "data": [
          {
            "queueid": "a20b9e0a-1ada-ec11-a7b5-000d3a2efdfa",
            "name": "<# PowerAutomate-MachineProvisioning>"
          }
        ]
      }
    }
  ]
}
```