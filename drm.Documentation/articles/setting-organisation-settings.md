---
uid: example-organisation-settings
author: Stuart Elcocks
date: 21/04/2023
title: Organization settings
description: Manage your organization settings using DRM templates. Set the Dynamics locale, have audit and advanced filter enabled. There are so many things you can set with this entity.
---

# Organisation settings

Organization settings is a huge entity with lots of properties!

This is just a basic example of what you can do.

The template below sets the following;

- Localeid - 2057 Sets the locale to 'United Kingdon'
- DefaultCountryCode - "+44"
- AuditEnabled - Yes
- Pricing Decimal Precision - 2
- External search index enabled - No
- Advanced filter enabled - Yes

## Template

For each Dynamics environment you are deploying to, you will need to identify the
```organizationid```. In the example below the ```organizationid``` is parameterized.

```json
{
    "targetenvironment": "[variables('dynamicsCredentials')]",
    "type": "drm.crmbaseentity/organizations",
    "apiVersion": "2023-01-09",
    "name": "DEMO_OrgSettings",
    "properties": {
      "data": [
        {
          "organizationid": "[parameters('organizationId')]",
          "localeid": "2057",
          "isdefaultcountrycodecheckenabled": "Yes",
          "defaultcountrycode": "+44",
          "drmmetadata": {
              "comment": "localeid 2057 sets it to United Kingdom"
          },
          "isauditenabled": "Yes",
          "isreadauditenabled": "No",
          "isuseraccessauditenabled": "No",
          "pricingdecimalprecision": 2,
          "isexternalsearchindexenabled": "No",
          "advancedfilteringenabled": "Yes"
        }
      ]
    }
}
```