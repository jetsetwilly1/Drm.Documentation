---
uid: property-metadata
---

# How entity properties are handled

The DRM engine is responsible for validating the templates passed to it, ensuring properties and
there values are what it's expecting.

In some cases it's possible to send the numeric value or a string value for some properties where the
type is of Edm.Int32 or Edm.Boolean.

## The DRM engine deployment process

The DRM engine will run through two processes for each template it receives.

1.  Validate the template against it's schema
2.  Load metadata information from the target dynamics environment.

As mentioned before step 1 is handled with schema validation whereby it will accept numeric values for some
properties where ```labels``` exist.

A good example of this is on the [queues](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/reference/queue?view=dataverse-latest) entity.

| Name | Type | Details |
|-|-|-|
| statecode | Edm.Int32 | Status of the queue.<br>Display Name: Status<br>Default Options<br><table><tbody><tr><th>Value</th><th>Label</th></tr><tr><td>0</td><td>Active</td></tr><tr><td>1</td><td>Inactive</td></tr></tbody></table>|
| statuscode | Edm.Int32 | Status of the queue.<br>Display Name: Status<br>Default Options<br><table><tbody><tr><th>Value</th><th>Label</th></tr><tr><td>2</td><td>Inactive</td></tr><tr><td>1</td><td>Active</td></tr></tbody></table>|

You can see that both ```statecode``` and ```statuscode``` are of type Edm.Int32 but also accept labels as a value.

The DRM engine will accept either of the values above because the schema will allow it.

When it loads the metadata for the entity type queue it will determine which labels could be accepted
as values and allow/disallow the deployment to continue.

>[!NOTE]
> When generating templates using the ```New-DrmTemplate``` command, it will generate templates using 
the numeric values for types of Edm.Int32 or Edm.Boolean.

## Example template for a queue

Lets look at an example template for managing queues.

```json
{
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "targetenvironment": ...,
      "type": "cT.crmbaseentity/queues",
      "apiVersion": "2023-01-09",
      "name": "Demo_Queues",
      "properties": {
        "data": [
          {
            "queueid": "a9b1b5d7-70b5-4540-ab19-4d65922704f7",
            "name": "Test queue 1",
            "queueviewtype": "public",
            "statecode": "active"
          },
          {
            "queueid": "a166668e-eb4a-ed11-bba2-0022483fdc52",
            "name": "Test queue 2",
            "statecode": 0
          }
        ]
      }
    }
  ]
}
```

In both queue definitions ```statecode``` is set using the label and numeric value.  In both 
cases the template would be accepted by the DRM engine.