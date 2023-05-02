---
uid: drm-home
author: Stuart Elcocks
date: 20/04/2023
title: DRM Templates
description: Dynamics Resource Management Templates (DRM) allow you to manage your Dynamics 365 environment configuration using templates. 
---

# Dynamics Resource Management (DRM) Templates

Automate your Dynamics 365 deployments using templates.

Get started now! https://docs.drmtemplates.io/tutorials/quickstart.html 

## About DRM Templates

Dynamics Resource Management Templates (DRM) was built with Devops Engineers and Dynamics developers in mind. Based on 
ARM Templates, they are constructed in the same way and offer much of the same functionality.

Managing multiple Dynamics 365 environments can be challenging but by using this tool, 
environments can be easily configured and maintained all through your automation pipelines.

DRM Templates allow you to 'PATCH' entities with your configuration. This includes custom entities. 
A common scenario for a Dynamics environment is to build Queues and Teams for example. 
Not only can you manage basic properties of these entities but you can also apply members to your queues and teams.

Check out which entities are DRM Templates support [here](xref:supported-web-entities)

## Simplify the management of your Dynamics 365 Environments.

Build your templates as the project develops and handle everything from queues, 
teams, environment variables and organisation settings.

You can run your templates inside pipelines to fully automate the deployment process. 
This can save time and reduce the risk of deployment issues.

Here's an example of how a template might look.

```json

  ...
    "drmclientSecret": {
      "type": "securestring",
      "metadata": {
        "comment": "In the pipelines this will be provided from the yml file."
      }
    }
  },
  "variables": {
    "dynamicsCredentials": {
      "applicationCredentials": {
        "clientId": "[parameters('drmclientId')]",
        "clientSecret": "[parameters('drmclientSecret')]",
        "tenantId": "[parameters('tenantId')]"
      },
      "url": "[parameters('dynamicsBaseUrl')]"
    }
  },
  "resources": [
    {
      "targetenvironment": "[variables('dynamicsCredentials')]",
      "type": "drm.crmbaseentity/organizations",
      "apiVersion": "2023-01-09",
      "name": "DRM_OrgSettings",
      "properties": {
        "data": [
          {
            "organizationid": "[parameters('organizationId')]",
            "localeid": "2057",
            "isdefaultcountrycodecheckenabled": "Yes",
            "defaultcountrycode": "+44",
            "drmmetadata": {
              "comment": "localeid 2057 sets it to United Kingdom"
            },
    ...
```

Using a template format that engineers are already familiar with will make it easier for 
them to learn and work with DRM templates.

If you're ready to deploy your first DRM template, a quickstart guide is available [here](xref:drm-quickstart). 
This guide will walk you through the process of setting up and deploying your first template, 
and provide a good starting point for working with DRM templates.