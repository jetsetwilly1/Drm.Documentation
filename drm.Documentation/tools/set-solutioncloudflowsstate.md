---
uid: set-solutioncloudflowsstate
author: Danny Styles
date: 27/07/2023
title: Turn on/off cloudflows belonging to a solution.
description: Connect to Dataverse and bulk turn off/on cloudflows associated with a specific solution.
---

# Turn on/off cloudflows belonging to a particular solution

If you ever need to turn on/off a bunch of cloudflows belonging to a particular solution then this command is for you.

## Usage

To use this cmdlet you need to install the module [Microsoft.Xrm.Data.PowerShell](https://github.com/seanmcne/Microsoft.Xrm.Data.PowerShell).

If you haven't already installed the module run

```powershell
Install-Module -Name Microsoft.Xrm.Data.PowerShell
```

Then to import the module into your session run

```powershell
Import-Module -Name Microsoft.Xrm.Data.PowerShell
```

You will also need to connect to your Dynamics environment using Connect-CrmOnline

For example

```powershell
Connect-CrmOnline -ServerUrl https://drmdemo.crm.dynamics.com/ -Username d.styles@demo.com
```

Now you can turn workflows on by running

``` powershell
Set-SolutionCloudflowsState -RequiredState On -SolutionName drmCloudflowsSolution
```

or turn them off

``` powershell
Set-SolutionCloudflowsState -RequiredState Off -SolutionName drmCloudflowsSolution
```

> [!NOTE]
> Ensure the solution name you provide the cmdlet is the unique solution name.
