---
uid: drm-quickstart
---

# Quickstart

Deploy your very first Dynamics Resource Management (DRM) template.

## Why DRM Templates

This template engine provides engineers with the following capabilities:

1. **Exportable Templates:** Generate templates from Dynamics environments easily using one PowerShell cmd.
2. **Declarative syntax:** DRM templates allow you to create/update Dynamics entities declaratively. For example, you can manage teams, queues, 
business units, environment variables and many more.
3. **CI/CD integration:** Enables cooperation between Dynamics and DevOps engineers to create configuration templates and 
automate Dynamics deployments.

Using DRM templates simplifies the automation process for new or existing Dynamics projects.

## Tools for creating templates

Use VSCode or Visual Studio for example and take advantage of schema help while developing your templates.

![VSCode schema help](/images/schemahelp.png "Schema Help")

Generate templates from a pre configured instance of Dynamics 
and deploy templates all from our PowerShell module.

[https://www.powershellgallery.com/packages/Drm.Templates.Powershell/](https://www.powershellgallery.com/packages/Drm.Templates.Powershell/)

## Prerequisites

To run through this quick start you will need;

1. Access to a Dynamics environment with system administrator privileges, which will be the target
 environment for deploying the template.
2. We will use VSCode to modify our template
3. PowerShell 5+ to deploy the template.

## Install the PowerShell module

You can install the PowerShell module from the [PowerShell gallery](https://www.powershellgallery.com/packages/Drm.Templates.Powershell).

Currently we only support PowerShell Desktop v5 upwards.

```powershell
Install-Module -Name Drm.Templates.Powershell -AllowClobber
```

## Generate a bearer token

The simplest approach for connecting to the Dynamics environment we want to deploy too is to generate
a bearer token by connecting to your Dynamics environment using the command below.

In your PowerShell window, log into your dynamics instance using your user account.

```powershell
connect-crmonline -Username "<useraccount>" -ServerUrl <dynamics url>
```

Now lets copy the bearer token so that we can use it in our template.

Run the command below to get the bearer token and copy it for use in the next step.

```powershell
$conn.CurrentAccessToken
```

>[!NOTE]
> Normally you would connect to Dynamics [using user or application credentials](xref:target-dynamics-instance) but for the purpose of this 
 tutorial we are using the bearer token.

## Create your template

Now lets generate a template that will create a new team.

Create a new file called 'quickstartdrm.json' and copy the json template below. 

```json
{ 
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [ 
     {
        "name": "Basic Team",
        "type": "drm.principal/teams",
        "apiVersion": "2023-01-09",
        "targetenvironment": {
            "bearerToken": "<paste your bearer token here>",
            "url": "<dynamics url>"
        },
        "properties": {
            "data": [{
                "teamid": "110443aa-5b1e-4b28-883e-e13f179d93a9",
                "name": "Basic DRM team",
                "description": "A basic team description",
                "drmmetadata": {
                    "description": "A basic team, nothing much else to say."
                }
            }]
        }
    }
  ] 
}
```

Replace the ```<paste your bearer token here>``` text with the bearer token you copied from the 
previous step and save the template.

Replace the ```<dynamics url>``` value with the dynamics url e.g. https://drmdemo.crm4.dynamics.com

## Deploy your template

In your PowerShell window run the following command to deploy your template.

```powershell
New-DrmDeployment -TemplateFile '<absolute path to your template>'
```

Log into your dynamics environment and check for a new team called 'Basic DRM Team'

![Quickstart Drm Basic Team](/images/quickstart-drm-team.png "Quickstart Drm Team")

Change the name of the team and redeploy the template to update the team name!

## Next steps

- Learn [how templates connect to Dynamics environments](xref:target-dynamics-instance)
- Learn how to [generate a template](xref:generate-drm-template)
- Learn how to [deploy a template with parameters](xref:deploy-drm-template-including-parameters)