---
uid: example-set-env-variables
author: Stuart Elcocks
date: 21/04/2023
title: Setting environment variables
description: Lets take a look at how DRM templates can be used to manage your Environment variables.  This article covers two approaches which are by configuring either the 'environmentvariablevalues' entity or 'environmentvariabledefinitions'.
---

# Setting environment variables

Environment variables can be set in two different ways.

1. Target the ```environmentvariablevalues``` entity.  If you target this entity then
a value must already be set for the environment definition.

2. Target the ```environmentvariabledefinitions``` entity. In this entity, if a custom
value hasn't been set, then you can update the default value.

## Target the environmentvariablevalues entity

As an example lets say we have an environment variable called ```new_TestVariable```
which **doesn't** have a current value set.

You can set the value of ```new_TestVariable``` with the following template.

```json
{
      "targetenvironment": "[variables('dynamicsCredentials')]",
      "type": "drm.crmbaseentity/environmentvariablevalues",
      "apiVersion": "2023-01-09",
      "name": "DEMO_Environment_Variables",
      "properties": {
        "data": [
          {
            "environmentvariablevalueid": "634b1a5e-0775-4de0-8cdd-9a5fdb4ac21f",
            "schemaname": "new_TestVariable",
            "value": "Hello Drm Current Value"
          }
        ]
      }
    }
```

In this example, the ```environmentvariablevalueid``` is a unique, randomly generated id. 
This id is used across all environments where this template is deployed and is utilized 
after the initial deployment. During the initial deployment, the ```schemaname``` value is used
to identify the appropriate environment variable.

>[!WARNING]
>If the developers have already assigned a value to the environment variable 
```new_TestVariable```, then the "environmentvariablevalueid" must be identified in 
order to override it.

## Target the environmentvariabledefinitions entity

Instead of creating/updating a 'current value' for an environment variable, you can
update the default value.

This is what your template would look like. To update a default value for an environment variable
the resource type needs to be ```drm.crmbaseentity/environmentvariabledefinitions```.

```json
{
      "targetenvironment": "[variables('dynamicsCredentials')]",
      "type": "drm.crmbaseentity/environmentvariabledefinitions",
      "apiVersion": "2023-01-09",
      "name": "DEMO_Environment_Variables",
      "properties": {
        "data": [
          {
            "environmentvariabledefinitionid": "8301c397-42e0-46d3-9dd5-db5a4833499d",
            "schemaname": "new_TestVariable",
            "defaultvalue": "Hello Drm Default Value"
          }
        ]
      }
    }
```

You will need to identify the environmentvariabledefinitionid for the env variable
```new_TestVariable``` to ensure the default value is updated correctly.