# DRM template file

## Template file

A template file uses the following format:

```json
{ 
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentParameters.json#", 
  "contentVersion": "", 
  "parameters": { }, 
  "variables": { }, 
  "resources": [ ]
}
```


| Element name | Required | Description
|_|_|
| $schema | Yes | Location of the JavaScript Object Notation (JSON) schema file that describes the version of the template language. The only version available is https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json# |
| contentVersion | Yes | Version of the template (such as 1.0.0.0). You can provide any value for this element. Use this value to document significant changes in your template. When deploying resources using the template, this value can be used to make sure that the right template is being used. |
| parameters | No | Values that are provided when deployment is executed to customize resource deployment. |
| variables | No | Values that are used as JSON fragments in the template to simplify template language expressions. |
| resources | Yes | Resource types that are deployed or updated in a resource group or subscription. |