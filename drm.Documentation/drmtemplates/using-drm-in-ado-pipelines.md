---
uid: deploy-drm-ado-pipelines
---

# ADO - Deploy DRM Templates

In this post we will show you how to execute template deployments
using the PowerShell module.  This example will deploy templates with parameters and we will
also use an ADO variable group which that will contain the application registration client secret.

To deploy DRM templates in yaml pipelines you only need the PowerShell tasks below.

```yaml
- powershell: |
    Install-Module -Name Drm.Templates.Powershell -force -verbose
    Import-Module -Name Drm.Templates.Powershell -verbose
  displayName: 'PowerShell Install ''DRMTemplates'' Module'

- powershell: |
    New-DrmDeployment -TemplateFile '$(System.DefaultWorkingDirectory)\platform-drm-demo\templates\demo_businessunits.json' -TemplateParameterFile '$(System.DefaultWorkingDirectory)\platform-drm-demo\parameters-stg\demo_businessunits.params.json' -TemplateParameterObject @{ 'drmclientSecret' = '$(drmclientSecret)' }
displayName: 'PowerShell: DRM BusinessUnits'
```

Follow the document for a complete pipeline that is executable.

## Prerequisites

Before setting up your pipeline you will need to create a repo to hold
the following items

1. An ADO instance.
2. DRM Templates - [Click here for recommendations on file structure](xref:recommended-file-structure)

[To create a repo in ADO follow this guide from Microsoft](https://learn.microsoft.com/en-us/azure/devops/repos/git/create-new-repo?view=azure-devops)

## Repository setup

As per the prerequisites you should have in place the repository.

The file layout is up to you but in this walkthrough you can see the structure we
have below.

![Repository layout](/images/ado-repo_layoutv2.png "Repository layout")

## Drm Templates

For our example we have one environment we are deploying to which is staging (stg).

The parameters and templates are all held under the parent folder 'platform-drm-demo'

![Repository file structure](/images/ado-repo_drmlayout.png "Repository file structure")

The connection to the Dynamic environment will done by an application registration.
[Click here to see how to connect to a Dynamics environment using an application registration.](xref:connect-to-dynamics-with-app-registration)

>[!NOTE]
>If you wanted to introduce a new Dynamics environment to configure, just add a new parameters-\<env\> folder.

## Variable group

For this walkthrough we will also be using a variable group which contains one value ```drmclientSecret```.

This variable group will be used by the pipeline and the ```drmclientSecret``` value passed to the template
as a parameter override.

![Variable group](/images/ado-variablegroup.png "Variable group")

## Yaml Pipeline

To deploy DRM templates we will use the PowerShell module ```Drm.Templates.Powershell```.
The pipeline will install the module from PowerShell gallery then call ```New-DrmDeployment``` to deploy
the template.

>[!NOTE]
>To deploy DRM templates you will need to use a vmImage of 'windows-latest'.  The PowerShell
module only runs on PowerShell Desktop version 5.1 upwards. It doesn't support PowerShell Core yet.

The pipeline below will do the following things

1. Check out the repository to the build agent, this makes available the templates and parameters.
2. Install and Import the PowerShell module ```Drm.Templates.Powershell```.
3. Execute the template using the Cmdlet New-DrmDeployment.

In the example below we are only deploying the business unit template, to deploy the others 
you would repeat the ```powershell``` task and update the relevant properties with template and
parameter locations.

``` yaml
trigger: none

stages:
- stage: drm_tst_deploy
  jobs:
    - deployment: tst_deploy
      pool:
        vmImage: 'windows-latest'
      environment: 'Drm-Test'
      variables:
      - group: 'devops-drmtemplates-tst'
      strategy:
        runOnce:
          deploy:
            steps:
              - checkout: self
                clean: false

              - powershell: |
                  Install-Module -Name Drm.Templates.Powershell -force -verbose
                  Import-Module -Name Drm.Templates.Powershell -verbose
                displayName: 'PowerShell Install ''DRMTemplates'' Module'

              - powershell: |
                  New-DrmDeployment -TemplateFile '$(System.DefaultWorkingDirectory)\platform-drm-demo\templates\demo_businessunits.json' -TemplateParameterFile '$(System.DefaultWorkingDirectory)\platform-drm-demo\parameters-stg\demo_businessunits.params.json' -TemplateParameterObject @{ 'drmclientSecret' = '$(drmclientSecret)' } 
                displayName: 'PowerShell: DRM BusinessUnits'
```

### Overriding parameters using ADO variables

Let's breakdown the ```New-DrmDeployment``` command.

```powershell
New-DrmDeployment 
    -TemplateFile '$(System.DefaultWorkingDirectory)\platform-drm-demo\templates\demo_businessunits.json' 
    -TemplateParameterFile '$(System.DefaultWorkingDirectory)\platform-drm-demo\parameters-stg\demo_businessunits.params.json' 
    -TemplateParameterObject @{ 'drmclientSecret' = '$(drmclientSecret)' }
```

We reference the ```-TemplateFile``` and ```-TemplateParameterFile``` as normal.

By adding ```-TemplateParameterObject``` we are overriding the 'drmclientSecret' drm parameter with the variable group value  
```'$(drmclientSecret)'```.

## Pipeline output

Running the pipeline above would generate output similar to the following.

![Pipeline output](/images/ado-pipeline_output.png "Pipeline output")

If a deployment is successful it will output each patch or post request made to the 
odata api and also return the total requests made.

## Next Steps

- [Learn how to deploy templates that use keyvault references in parameters in ADO](xref:deploy-drm-ado-pipelines-with-params)
- [Learn how to reference keyvault secrets in parameter files](xref:reference-keyvault-secrets)
- [Learn how to connect to a Dynamics environment using an application registration.](xref:connect-to-dynamics-with-app-registration)