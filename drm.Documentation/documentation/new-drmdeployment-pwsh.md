---
uid: new-drmdeployment-cmdlet
---

# New-DrmDeployment cmdlet

Module: Drm.Templates.Powershell

Deploy a drm template.

``` powershell
New-DrmDeployment 
 -TemplateFile <string> <required> 
 -TemplateParameterFile <string> 
 -TemplateParameterObject <hashtable> 
 -SkipAzureLogin <switch>
```

## Description
Use this cmdlet to deploy DRM templates to your dynamics environments.

Provide the template file location and an optional parameters file.

## Examples
If you provide only the template file, it will use the supplied default values for each parameter.

``` powershell
New-DrmDeployment 
  -TemplateFile '<Absolute path to template file>' 
```

Using the sample templates located in the samples folder. Supply both the template and parameters file.

``` powershell
New-DrmDeployment 
  -TemplateFile '<Absolute path to template file>' 
  -TemplateParameterFile '<Absolute path to parameters file>' 
```

It's also possible to pass a hashtable of parameters as values for a template.

``` powershell
New-DrmDeployment 
  -TemplateFile '<Absolute path to template file>' 
  -TemplateParameterObject @{ "param1" = "value1" } 
```

If you want to override parameters given by a parameter file then just supply the TemplateParameterObject with the overridden values

``` powershell
New-DrmDeployment 
  -TemplateFile '<Absolute path to template file>' 
  -TemplateParameterFile '<Absolute path to parameters file>'
  -TemplateParameterObject @{ "param1" = "value1" } 
```

In some cases you may need to skip the login to azure attempt made by this command, just add the ```-SkipAzureLogin``` switch as below.

``` powershell
New-DrmDeployment 
  -TemplateFile '<Absolute path to template file>' 
  -SkipAzureLogin
```