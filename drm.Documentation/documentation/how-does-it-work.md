---
uid: how-does-it-work
---

# What is DRM Templates?

Dynamic Resource Management (Drm) is a template system based on ARM. DRM however
allows developers and engineers to compose templates of configuration that
go towards building Dynamics environments.

It can be used on small to large scale projects and provides engineers a tool that
can help support configuration of multiple environments from development all the way 
to production.

## How does it work?

Engineers interact with the DRM engine by running PowerShell commands like
```New-DrmTemplate``` and ```New-DrmDeployment```

The DRM engine during deployments processes template parameters and variables and
validates data using schemas and Dynamics metadata information for every targeted
entity.

When generating templates, it uses odata queries to return the data from a Dynamics 
environment all wrapped up in a template ready for deployment.

The DRM engine deploys the templates direct from your powershell console.

The engine has been built from the ground up, written in .net core. It doesn't use any
SDK's from Microsoft when connecting to your target Dynamics environments.

## How secure are the connections to Dynamics environments?

Connections made to Dynamics environments are achieved via OAuth access tokens.  

[Please read the official Microsoft documentation on Azure access tokens](https://learn.microsoft.com/en-us/azure/active-directory/develop/access-tokens#access-token-lifetime)

No persistent connections are made to Dynamics environments.

## How are keyvault secrets retrieved from Azure?

If keyvault references are found in a parameter file, the PowerShell command ```New-DrmDeployment```
will attempt to use an existing azure context.

For example if testing deployments locally and you have keyvault references in a parameter file
then you must be connected to azure by using 
```Connect-AzAccount -TenantId 00000000-0000-0000-0000-000000```. This will make available an 
Azure context in your PowerShell session meaning ```New-DrmDeployment``` can attempt to request an 
access token to the relevant keyvault.

The process is the same in pipelines, using the AzurePowershell@5 Task and an Azure subscription.