---
uid: connect-crmonline-cmdlet
---

# Connect-CrmOnline cmdlet

Module: Drm.Templates.Powershell

Connect to a dynamics environment.

## Description

The function enables you to sign in to a Dynamics environment interactively or by passing your username and server url.

## Examples

If you want to use the common login control to provide your credentials to connect to Dataverse, use the following command. 
The connection information is stored in the $conn variable so that you can use it later.

```powershell
Connect-CrmOnline -InteractiveMode
```

Provide your username and dynamics environment server url to connect via OAuth

```powershell
Connect-Crmonline -Username <username> -ServerUrl <dynamics url>
```