---
uid: example-team-creation
author: Stuart Elcocks
date: 06/11/2023
title: Manage your Dynamics Teams
description: Creating templates to manage team creation is a common task when you have many teams to setup. In this post we will discover how easy it is to complete this task.
---

# Manage your Dynamics Teams

Creating templates to manage team creation is a common task when you have many teams to setup.

In this post we will discover how easy it is to complete this task.

## A basic team template

You can [generate your team template](xref:generate-drm-template) from a master environment or hand craft it yourself.  Below is what a basic team configuration template might look like.

```json
{
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "targetenvironment": "[variables('dynamicsCredentials')]",
      "type": "drm.principal/teams",
      "apiVersion": "2023-01-09",
      "name": "DEMO_team",
      "properties": {
        "data": [
          {
            "teamid": "ec0e7243-7454-ea11-a813-000d3a7f1f18",
            "name": "DRM Dream Team",
            "description": "The one and only dream team."
          }
        ]
      }
    }
  ]
}
```

After the template above is deployed you will have a team called 'DRM Dream Team' in your Dynamics environment. As always all of the properties that make up a team 
can be updated.

## Associate roles to a team

One of the nice features of the teams entity is that you can associate roles to your teams.  

Using the same example, lets associate the basic user role to this team.

```json
{
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "targetenvironment": "[variables('dynamicsCredentials')]",
      "type": "drm.principal/teams",
      "apiVersion": "2023-01-09",
      "name": "DEMO_team",
      "properties": {
        "data": [
          {
            "teamid": "ec0e7243-7454-ea11-a813-000d3a7f1f18",
            "name": "DRM Dream Team",
            "description": "The one and only dream team.",
            "teamroles_association": [
              {
                "roleid": "f16da2b3-591d-ee11-9cbe-000d3a53df53",
                "metadata": { "value": "the basic user role" }
              }
            ]
          }
        ]
      }
    }
  ]
}
```

By adding the ```teamroles_association``` property we can add all of the roles we want to associate with this team.

In your Dynamics environments you may not know the role id's until the template deployment is run.  If this is the case then 
put together a query to retrieve role id's.

```json
{
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "targetenvironment": "[variables('dynamicsCredentials')]",
      "type": "drm.principal/teams",
      "apiVersion": "2023-01-09",
      "name": "DEMO_team",
      "queries": {
        "rootBusinessUnit": {
          "entity": "businessunits",
          "filter": "$filter=name eq 'BU DRM Root'"
        },
        "basicUserRoleId": {
          "entity": "roles",
          "filter": "[concat('$select=name&$filter=_businessunitid_value eq ', queries('rootBusinessUnit').value[0].businessunitid, ' and name eq ''Basic User''')]",
          "comment": "Fetching role id for Basic User"
        },
      },
      "properties": {
        "data": [
          {
            "teamid": "ec0e7243-7454-ea11-a813-000d3a7f1f18",
            "name": "DRM Dream Team",
            "description": "The one and only dream team.",
            "teamroles_association": [
              {
                "roleid": "[queries('basicUserRoleId').value[0].roleid]",
                "metadata": { "value": "Basic user role" }
              }
            ]
          }
        ]
      }
    }
  ]
}
```

In this example we have included a ```queries``` object where we have two queries which do the following:

 1.  Fetch the root business unit id.
 2.  Fetch the basic user role id for the root business unit.

These queries will be run at deployment time and make the returned information available while the resource is deployed.

In the ```teamroles_association``` array we can now reference the basic user role id by using the queries function ```[queries('basicUserRoleId').value[0].roleid]```.

For more information on [how to use queries in your templates check this out](xref:fetch-data-support-deployments).

> [!NOTE]
> Creating queries is one way of providing the template with information during deployment, you could also use parameters 
> and or variables in your template.

## Associate members to your teams

You can also associate members to your teams. So for example if the dream team only consisted of one known member then make it part of the deployment.

```json
{
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "targetenvironment": "[variables('dynamicsCredentials')]",
      "type": "drm.principal/teams",
      "apiVersion": "2023-01-09",
      "name": "DEMO_team",
      "properties": {
        "data": [
          {
            "teamid": "ec0e7243-7454-ea11-a813-000d3a7f1f18",
            "name": "DRM Dream Team",
            "description": "The one and only dream team.",
            "teammembership_association": [
              {
                "systemuserid": "784869cd-6b0c-eb11-a812-000d3a7ed330",
                "metadata": { "value": "Stuart Elcocks" }
              }
            ]
          }
        ]
      }
    }
  ]
}
```

By using the property ```teammembership_association``` we can add members to the team.  You could also
use queries to return specific team members if you don't know the user id's upfront.

> [!NOTE]
> Creating queries is one way of providing the template with information during deployment, you could also use parameters 
> and or variables in your template.

## Next steps

[Create a pipeline in ADO and automate your deployment](xref:deploy-drm-ado-pipelines).