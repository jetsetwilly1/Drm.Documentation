---
uid: drm-toolbox
author: Stuart Elcocks
date: 16/06/2023
title: DRM Toolbox
description: The Drm toolbox is a collection of handy powershell cmdlets that can help in the automation of deploying Dynamics configuration.
---

# What is DRM Toolbox?

DRM toolbox is a play on XRMToolBox.  The idea is to build a collection of powershell cmdlets with a focus on 
automation of Dynamics environments.

The cmdlets will be added to the Drm.Templates powershell module making all of the automation tools accessible 
from one place.

## Contribute to the module

The DRM Powershell module is available from github meaning everybody can contribute.

If you think you have a cmdlet that somebody else might find useful then follow
the guidelines on the [github repo](https://github.com/jetsetwilly1/Drm.Powershell)
 to get it included in the module!

## Microsoft.Xrm.Data.PowerShell

We are starting to add cmdlets that sometimes might depend on the module developed by Sean McNellis called 
[Microsoft.Xrm.Data.PowerShell](https://github.com/seanmcne/Microsoft.Xrm.Data.PowerShell).  The module
gives you the ability to interact with the Dynamics web api directly using cmdlets such as ```New-CrmRecord```,
```Get-CrmRecord``` and ```Set-CrmRecord```.

Please check out the open source project here https://github.com/seanmcne/Microsoft.Xrm.Data.PowerShell!

