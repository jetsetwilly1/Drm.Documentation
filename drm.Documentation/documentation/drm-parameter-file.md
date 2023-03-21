---
uid: drm-parameter-file
---

# DRM parameter file

Rather than passing parameters as inline values in your template, you can use a JSON file that contains the parameter values. 
This article shows how to create a parameter file that you use with a JSON template.

## Parameter file

A parameter file uses the following format:

```json
{ 
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentParameters.json#", 
  "contentVersion": "1.0.0.0", 
  "parameters": { 
     "<first-parameter-name>": { 
        "value": "<first-value>" 
     },
     "<second-parameter-name>": {
        "value": "<second-value>" 
     }
  }
}
```

Notice that the parameter file stores parameter values as plain text. This approach works for values that aren't sensitive, 
such as a GUID or lookup option. Plain text doesn't work for sensitive values, such as passwords. 
If you need to pass a parameter that contains a sensitive value, store the value in a key vault. 
Then reference the key vault in your parameter file. The sensitive value is securely retrieved during deployment.

The following parameter file includes a plain text value and a sensitive value that's stored in a key vault.

``` json
{ 
  "$schema": "https://drmtemplatestest.z33.web.core.windows.net/Schemas/2021-03-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0", 
  "parameters": { 
     "<first-parameter-name>": { 
        "value": "<first-value>" 
     }, 
     "<second-parameter-name>": { 
        "reference": { 
           "keyVault": { 
              "name": "<name-of-keyvault>"
           },
           "secretName": "<secret-name>" 
        }
     }
  }
}
```

In ARM templates you reference a key vault by resource id but for DRM templates you reference the key vault by name. 
This is the only difference between both.

## Next steps
Please see the documentation for ARM parameter files 
[here](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/parameter-files?tabs=json#parameter-file) 
as it's applicable to DRM templates.

Documentation for using key vaults with parameter files can be found [here](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/key-vault-parameter?tabs=azure-cli%2Cjson)