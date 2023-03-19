# Dynamics Resource Management (DRM) Templates

## About DRM Templates

Dynamics Resource Management Templates (DRM) was built with Devops Engineers and Dynamics developers in mind. Based on 
ARM Templates, they are constructed in the same way and offer many of the same functionality.

Managing multiple Dynamics environments can be challenging but by using this tool, 
environments can be easily maintained and controlled all through your automation pipelines.

DRM Templates allow you to 'PATCH' entities with your configuration. This includes custom entities. 
A common scenario for a Dynamics environment is to build Queues and Teams for example. 
Not only can you manage basic properties of these entities but you can also apply members to your queues and teams.

All entities are documented by Microsoft [here](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/reference/entitytypes?view=dataverse-latest) enabling you to get to grips with any 
entity you might need to configure.

## Manage your Dynamics Environments with Ease

![Templates](/images/home_templates.png "Template Schemas")

Build your templates as the project develops and handle everything from queues, 
teams and your environment variables.

You can run your templates inside pipelines to fully automate the deployment process. 
This can save time and reduce the risk of errors.

Using a template format that engineers are already familiar with will make it easier for 
them to learn and work with DRM templates.

If you're ready to deploy your first DRM template, a quickstart guide is available [here](xref:drm-quickstart). 
This guide will walk you through the process of setting up and deploying your first template, 
and provide a good starting point for working with DRM templates.