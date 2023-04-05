# Dynamics Resource Management (DRM) Templates

Powershell Module: https://www.powershellgallery.com/packages/Drm.Templates.Powershell

## Documentation

Documentation is hosted at https://docs.drmtemplates.io

## Contribute

Please feel free to contribute to the documentation.

The documentation is markdown based and compiled using DocFx.

Create a branch, make your changes, once the PR is approved an action will be triggered and the documentation updated.

Start creating templates quickly. https://docs.drmtemplates.io/tutorials/quickstart.html

Generate templates using a simple powershell cmd: https://docs.drmtemplates.io/tutorials/generate-a-drm-template.html 

Here's some of the how-to's already written

- [Manage application users](https://docs.drmtemplates.io/articles/manage-application-user-accounts.html)
- [Manage Business Units](https://docs.drmtemplates.io/articles/manage-business-units.html)
- [Connection References](https://docs.drmtemplates.io/articles/connection-references.html)
- [Setting Environment Variables](https://docs.drmtemplates.io/articles/setting-environment-variables.html)
- [Manage Organisation Settings](https://docs.drmtemplates.io/articles/setting-organisation-settings.html)
- [Transfer Word Document Templates](https://docs.drmtemplates.io/articles/document-templates.html)
- [Manage workflows](https://docs.drmtemplates.io/articles/manage-workflows.html)
- [Manage Custom Entities](https://docs.drmtemplates.io/articles/manage-custom-entities.html)

## About DRM Templates

Dynamics Resource Management Templates (DRM) was built with Devops 
Engineers and Dynamics developers in mind. Based on ARM Templates, 
they are constructed in the same way and offer many of the same 
functionality.

Managing multiple Dynamics environments can be challenging but by 
using this tool, environments can be easily maintained and controlled
all through your automation pipelines.

DRM Templates allow you to 'PATCH' entities with your configuration.
This includes custom entities. A common scenario for a Dynamics 
environment is to manage environment variables or build Queues and Teams. 
Not only can you manage basic properties of these entities but
you can also apply members to your queues and teams.

Generate templates using the `New-DrmTemplate` cmd

Deploy templates using the `New-DrmDeployment` cmd

Below is an example template for an environmentvariablevalue.

```json
{
      "targetenvironment": "[variables('dynamicsCredentials')]",
      "type": "drm.crmbaseentity/environmentvariablevalues",
      "apiVersion": "2023-01-09",
      "name": "DEMO_Environment_Variables",
      "properties": {
        "data": [
          {
            "environmentvariablevalueid": "634b1a5e-0775-4de0-8cdd-9a5fdb4ac21f",
            "schemaname": "new_TestVariable",
            "value": "Hello Drm Current Value"
          }
        ]
      }
    }
```