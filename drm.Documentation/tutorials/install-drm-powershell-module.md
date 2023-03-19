---
uid: install-powerhsell-module
---

# The Drm.Templates.Powershell Module

The powershell module currently offers the following cmdlets

| Cmdlet | Description |
|-|-|
| [Connect-CrmOnline](xref:connect-crmonline-cmdlet) | Connect to your Dynamics environments, primarily used in conjunction with generating templates. |
| [New-DrmTemplate](xref:new-drmtemplate-cmdlet) | Generate templates from existing Dynamics environments. |
| [New-DrmDeployment](xref:new-drmdeployment-cmdlet) | Deploy your templates to multiple Dynamics environments. |

It's available from the PowerShell Gallery at [https://www.powershellgallery.com/packages/Drm.Templates.Powershell](https://www.powershellgallery.com/packages/Drm.Templates.Powershell)

## Install the PowerShell module

To install the PowerShell module run this command

```powershell
Install-Module -Name Drm.Templates.Powershell -AllowClobber
```

To check its installed run this to retrieve the drm commands.

```powershell
Get-Command *drm*
```


## PowerShell compatibility

Currently the Module has dependencies on the 
[Microsoft.Xrm.Tooling.Connector](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/xrm-tooling/use-powershell-cmdlets-xrm-tooling-connect) provided by Microsoft.  
Unfortunately support for this does not include PowerShell Core.

The plan at this stage is to move away from this dependency and make Drm.Templates.Powershell PowerShell core compatible.

