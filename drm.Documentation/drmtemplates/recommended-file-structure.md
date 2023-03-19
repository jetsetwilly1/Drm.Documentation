---
uid: recommended-file-structure
---

# Recommended file structure

>[!NOTE]
> This is a recommended approach, you could follow any pattern you like 
as long as the outcome is the same which is, templates and parameters are
easily identifiable.

You may want to house the DRM templates in a single, isolated repository or 
add them to an existing one.  The repository you choose to use isn't really
important, the structure of the files is.

Ideally you should split your templates and parameters as below.

This example shows the simple split between templates and parameters.
Under the parameters-prod folder you can see the parameter files for each 
template.

```
|   +-- parameters-qa
|   +-- parameters-uat
|   +-- parameters-prod
|       +-- demo_businessunits.params.json
|       +-- demo_environment_values.params.json
|       +-- demo_queues.params.json
|       +-- demo_teams.params.json
+-- templates
|   +-- demo_businessunits.json
|   +-- demo_environment_values.json
|   +-- demo_queues.json
|   +-- demo_teams.json

```

You don't have to split the templates by entity as we have above, you could 
combine deployments into one template.

Below we have a template that is responsible for business units and queues.

>[!NOTE]
> There maybe a reason for combining templates because one depends on the other.
>
> See this for [using the dependsOn property in templates](xref:using-dependson-property)

```
|   +-- parameters-qa
|   +-- parameters-uat
|   +-- parameters-prod
|       +-- demo_businessunits_queues.params.json
|       +-- demo_environment_values.params.json
|       +-- demo_teams.params.json
+-- templates
|   +-- demo_businessunits_queues.json
|   +-- demo_environment_values.json
|   +-- demo_teams.json
```

You could deploy all the configuration in one template.

This could be a good idea if the configuration is relatively small.

```
|   +-- parameters-qa
|   +-- parameters-uat
|   +-- parameters-prod
|       +-- demo_configuration.params.json
+-- templates
|   +-- demo_configuration.json
```