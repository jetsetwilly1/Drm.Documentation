---
uid: deploy-drm-ado-pipelines-with-params
---

# ADO - Deploy DRM Templates using parameter keyvault secrets

In this post we will show you how to execute template deployments
using the PowerShell module.  This example will deploy templates with parameters that 
also reference keyvault secrets.

[Learn how to reference keyvault secrets in parameter files here.](xref:reference-keyvault-secrets)

To accomplish deploying parameters that reference keyvault secrets we will use the Azure PowerShell task
and an Azure subscription to connect to the keyvault.

```yaml
- task: AzurePowerShell@5
  displayName: 'Azure PowerShell: DRM Template - Business Units'
  inputs:                  
    azureSubscription: 'DRM Dev Connection'
    ScriptType: InlineScript
    Inline: |
        New-DrmDeployment -TemplateFile '$(System.DefaultWorkingDirectory)\platform-drm-demo\templates\demo_businessunits.json' -TemplateParameterFile '$(System.DefaultWorkingDirectory)\platform-drm-demo\parameters-stg\demo_businessunits.params.json'
    azurePowerShellVersion: LatestVersion         
```

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

## Yaml Pipeline

To deploy DRM templates we will use the PowerShell module ```Drm.Templates.Powershell```.
The pipeline will install the module from PowerShell gallery then call ```New-DrmDeployment``` to deploy
the template.

>[!NOTE]
>To deploy DRM templates you will need to use a vmImage of 'windows-latest'.  The powershell
module only runs on PowerShell Desktop version 5.1 upwards. It doesn't support Powershell Core yet.

The pipeline below will do the following things

1. Check out the repository to the build agent, this makes available the templates and parameters.
2. Install and Import the PowerShell module ```Drm.Templates.Powershell```.
3. Execute the template using the Cmdlet New-DrmDeployment.

In the example below we are only deploying the business unit template, to deploy the others 
you would repeat the ```powershell``` task and update the relevant properties with template and
parameter locations.

Because we are referencing keyvault secrets in our parameter files, we are using the ```AzurePowerShell@5```
task to deploy the template.

``` yaml
trigger: none

stages:
- stage: drm_tst_deploy
  jobs:
    - deployment: tst_deploy
      pool:
        vmImage: 'windows-latest'
      environment: 'Drm-Test'
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
              
              - task: AzurePowerShell@5
                displayName: 'Azure PowerShell: DRM Template - Business Units'
                inputs:                  
                  azureSubscription: 'DRM Dev Connection'
                  ScriptType: InlineScript
                  Inline: |
                      New-DrmDeployment -TemplateFile '$(System.DefaultWorkingDirectory)\platform-drm-demo\templates\demo_businessunits.json' -TemplateParameterFile '$(System.DefaultWorkingDirectory)\platform-drm-demo\parameters-stg\demo_businessunits.params.json'
                  azurePowerShellVersion: LatestVersion         
```

### Using the AzurePowershell task

Let's take a closer look at the AzurePowershell@5 task

```yaml
- task: AzurePowerShell@5
  displayName: 'Azure PowerShell: DRM Template - Business Units'
  inputs:                  
    azureSubscription: 'DRM Dev Connection'
    ScriptType: InlineScript
    Inline: |
        New-DrmDeployment -TemplateFile '$(System.DefaultWorkingDirectory)\platform-drm-demo\templates\demo_businessunits.json' -TemplateParameterFile '$(System.DefaultWorkingDirectory)\platform-drm-demo\parameters-stg\demo_businessunits.params.json'
    azurePowerShellVersion: LatestVersion    
```

The inline script simply calls ```New-DrmDeployment``` passing in the template and parameter files.

In this example it will use the ```azureSubscription``` 'DRM Dev Connection' to connect with any keyvault references in the 
parameter file.

[Follow this walkthrough on how to setup and use keyvault secrets in parameter files.](xref:reference-keyvault-secrets)

## Pipeline output

Running the pipeline above would generate output similar to the following.

![Pipeline output](/images/ado-pipeline_output-keyvault.png "Pipeline output")

You can see that its detected keyvault references in the parameter file and will attempt to
retrieve the secrets using the azureSubscription connection in the PowerShell task.

## Next Steps

- [Learn how to reference keyvault secrets in parameter files](xref:reference-keyvault-secrets)
- [Learn how to connect to a Dynamics environment using an application registration.](xref:connect-to-dynamics-with-app-registration)