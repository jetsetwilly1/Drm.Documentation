---
uid: dynamicsautonumber
author: Dave Langan
date: 01/08/2023
title: Manage a Dynamics seed number during a release
description: Manage a Dynamics seed number during a release by using the Get-DynamicsAutoNumber and Set-DynamicsAutoNumber
---

# Manage a Dynamics seed number during a release

Sometimes you might need to reset a tables seed number after solution imports to a Dynamics environment.

By using ```Get-DynamicsAutoNumber``` and ```Set-DynamicsAutoNumber``` cmdlets you can ensure a seed number for a 
table is set to the correct number after a release using ADO pipelines.

## Usage

### Set seed variable in the ADO pipeline.

Lets set the pipeline variables so that we can pick up the values after solutions have been deployed 
to our Dynamics environment.

```powershell
Get-DynamicsAutoNumber -TenantId <00000000-0000-0000-0000-000000000000> `
 -ClientId <00000000-0000-0000-0000-000000000000> `
 -ClientSecret <00000000-0000-0000-0000-000000000000> `
 -DataverseEnvUrl https://drmdemo.crm14.dynamics.com `
 -EntityName contacts `
 -FieldName seedfield `
 -VarName AdoSeedVariable
```

The first four parameters ```TenantId```, ```ClientId```, ```ClientSecret``` and ```DataverseEnvUrl``` 
should be used to connect to your Dynamics environment using an azure application registration.

Please read this for more information on 
[connecting to Dynamics using an app registration.](xref:connect-to-dynamics-with-app-registration)

Please see the table below explaining the remaining parameters required

| Parameter | Description |
|---|
| EntityName | The entity/table name where the seed column is located. |
| FieldName | The name of the field/column. |
| VarName | The name of the ADO variable to create which will hold the seed value. |

When this cmdlet is run in an ADO pipeline it will create a variable with the current seed value of the field 
from the entity specified. 

### Reseting the seed value after solution imports

To reset the seed value to its original value run the following cmdlet in an ADO pipeline, don't forget to reference 
the pipeline variable you created when running the ```Get-DynamicsAutoNumber```.

```powershell
Set-DynamicsAutoNumber -TenantId <00000000-0000-0000-0000-000000000000> `
 -ClientId <00000000-0000-0000-0000-000000000000> `
 -ClientSecret <00000000-0000-0000-0000-000000000000> `
 -DataverseEnvUrl https://drmdemoprod.crm14.dynamics.com `
 -EntityName contacts `
 -FieldName seedfield `
 -SeedValue <reference to ado pipeline variable>
```

| Parameter | Description |
|---|
| EntityName | The entity/table name where the seed column is located. |
| FieldName | The name of the field/column. |
| SeedValue | Provide the Ado variable. |

## Example

Here is a ADO yaml pipeline showing how it might be used when deploying solutions to a dynamics environment.

Ado pipeline variables are used to store the credentials for connecting to the Dynamics environment.

```yaml
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
                displayName: 'Azure PowerShell: GetSeedValue'
                inputs:                  
                  azureSubscription: 'DRM Dev Connection'
                  ScriptType: InlineScript
                  Inline: |
                      Get-DynamicsAutoNumber -TenantId $(varTenantId) -ClientId $(varClientId) -ClientSecret $(varClientSecret) -DataverseEnvUrl $(varEnvUrl) -EntityName contacts -FieldName myseedcol -VarName adoSeedVar
                  azurePowerShellVersion: LatestVersion        
                  
              ...
              # Solution imports executed here
              ...

              - task: AzurePowerShell@5
                displayName: 'Azure PowerShell: SetSeedValue'
                inputs:                  
                  azureSubscription: 'DRM Dev Connection'
                  ScriptType: InlineScript
                  Inline: |
                      Set-DynamicsAutoNumber -TenantId $(varTenantId) -ClientId $(varClientId) -ClientSecret $(varClientSecret) -DataverseEnvUrl $(varEnvUrl) -EntityName contacts -FieldName myseedcol -SeedValue $(adoSeedVar)
                  azurePowerShellVersion: LatestVersion   

```
