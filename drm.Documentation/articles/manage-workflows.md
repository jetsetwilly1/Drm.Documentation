---
uid: example-manage-workflows
---

# Managing workflows

The Workflows entity in Dynamics covers the following types or category of workflow.

- Workflow
- Dialog
- Business Rule
- Action
- Business Process Flow
- Modern Flow (Cloudflows)
- Desktop Flow

As more examples become available for interactions with these flow, this document will be updated.

>[!NOTE]
>When interacting with workflows you may need to connect to your Dynamics instance with a user account.

## Cloudflows (Modern Flow)

An example template for interacting with cloudflows is below.  It simply turns off a cloud flow by changing
its state to 'draft'

```json
{
  "targetenvironment": "[variables('dynamicsCredentials')]",
  "type": "drm.crmbaseentity/workflows",
  "apiVersion": "2022-04-03",
  "name": "CVS_Workflows_Turn_Off",
  "properties": {
    "workflows": [
      {
        "workflowid": "a0096b6f-198f-ec11-b400-102248425a72",
        "statecode": 0,
        "drmmetadata": {
          "name": "Drm Demo - hello world",
          "description": "turning off"
        }
      }
    ]
  }
}
```

To turn the flow back on, change the statecode property to 1 or 'activated'.

>[!NOTE]
> Use the 'drmmetadata' property to add comments, describing what will happen when the template is deployed.