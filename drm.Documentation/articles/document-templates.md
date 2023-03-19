---
uid: example-transfer-document-templates
---

# Transfer Word Document Templates

Word document templates can be migrated across environments by generating the template from 
a source Dynamics environment.

>[!NOTE]
>We advise generating the template for documents as the content property contains the base64
word document.

To return all the word document types from your source Dynamics environment run the following
command

```powershell
New-DrmTemplate 
    -entityName documenttemplates 
    -filter '$select=organizationid,name,documenttemplateid,languagecode,documenttype,clientdata,content&$filter=documenttype eq 2' 
```

Below is an example of what you will get returned.

The ```organizationid``` will be different across environments so it will
require parametrisation.

```json
{
  "$schema": "https://drmtemplatesprod.z33.web.core.windows.net/schemas/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "targetenvironment": {
        "bearerToken": "<Enter token here>",
        "url": "<Your Dynamics url>"
      },
      "type": "cT.crmbaseentity/documenttemplates",
      "apiVersion": "2023-01-09",
      "name": "GeneratedTemplateFor_documenttemplates",
      "properties": {
        "data": [
          {
            "documenttemplateid": "97e568ad-1033-4138-b682-10c9f69da6ec",
            "organizationid": "ebce9626-e897-48bd-bf2c-76edba0759f2",
            "name": "DRM Demo Document",
            "languagecode": 1033,
            "documenttype": 2,
            "clientdata": "{&quot;Range&quot;:null,&quot;Columns&quot;:null,&quot;DisplayConditions&quot;:&quot;<DisplayConditions><Everyone /></DisplayConditions>&quot;}",
            "content": "<Base64 content>"
          }
        ]
      }
    }
  ]
}
```