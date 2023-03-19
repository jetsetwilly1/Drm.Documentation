---
uid: example-manage-custom-entities
---

# Manage Custom Entities

The custom entities api allows you to patch custom entities within your Dynamics 
CRM environment.

A custom entity is a table/entity that has been created to meet a projects 
requirements and therefore cannot be targeted using any standard dynamics odata 
web api call.

> [!NOTE]
>  The custom api will NOT create the entity/table if it does not exist. The deployment will 
fail in this scenario.

## Examples

Lets use an example where you might have two entities called cfg_group and cfg_item. 
You now want to ensure a group and its items are always available in your targeted
Dynamics instance.

cfg_group is a parent container for cfg_item. The relationship is 1-n. 
A cfg_group can have many cfg_item.

In this example we can see straight away that we have a dependency on cfg_group 
being available first before adding items to it.

To patch the cfg_group the template will look like this

```json
{ 
  "targetenvironment": { ... }, 
  "type": "cT.crmbaseentity/custom", 
  "apiVersion": "2023-01-09", 
  "name": "configurationgroup", 
  "properties": { 
    "entityname": "cfg_group",
    "data":[ 
      { 
        "cfg_group_id": "173753c7-f315-4d3f-994e-d2fd6a2502c8", 
        "cfg_group_name" : "Configuration Group 1" 
      }
    ] 
  }
}
```

And to add two cfg_items we would do this.

```json
{ 
  "targetenvironment": { ... }, 
  "type": "cT.crmbaseentity/custom",
  "apiVersion": "2023-01-09", 
  "name": "configurationitems", 
  "properties": { 
    "entityname": "cfg_item", 
    "data": [
      { 
        "cfg_item_id": "cd0ae30f-5cb8-4808-afa0-651afca1e952", 
        "cfg_item_name" : "Item1", 
        "cfg_item_value" : "Value1", 
        "cfg_item_configurationgroupid" : "173753c7-f315-4d3f-994e-d2fd6a2502c8"
      },
      { 
        "cfg_item_id": "d04822ac-6fb0-4d8a-9aea-57a8e1f8f350", 
        "cfg_item_name" : "Item2", 
        "cfg_item_value" : "Value2", 
        "cfg_item_configurationgroupid" : "173753c7-f315-4d3f-994e-d2fd6a2502c8"
      },
    ]
  },
  "dependson":[ "cT.crmbaseentity/custom/configurationgroup" ]
}
```

The 'dependson' parameter ensures that the configuration group data is added before the items.

## Data patching

Data within a template is handled individually. What this means is that given a set of data if 
one or more rows fail to patch correctly then the whole template deployment will fail. 
You will be given the unique id's of the objects that have failed so that you can correct 
them and rerun the deployment.

All data types are handled, if you come across a custom entity patch which is not working 
as expected then please contact an administrator

> [!NOTE]
> Datatypes are all handled by the DRM engine. If your unsure what a column
datatype in a custom entity is, either contact the developer who created the table or
run a web api query against the table to retrieve its metadata.

## Importance of providing values for Unique Identifiers

You must supply a unique id for each piece of data you are patching. The reason
for this is to keep the templates idempotent meaning the templates can be deployed
over and over and give the same result each time. Without providing unique
identifier values during a deployment the system would not be able to track
data patches.

## Lookup columns

In the example above a lookup column would be cfg_item_configurationgroupid. 
The DRM engine will attempt to check the value provided actually exists. 
If the value cannot be found the data patch will fail.

## Choice columns

Choice columns are handled by the DRM engine. Given a choice column the DRM engine
will select the relative picklist and attempt to map the value.

For example if a Choice column is set up like this

```json
{ 
  "columnName" : "cfg_item_type", 
  "items" : { 
    "TestType1" : 0, 
    "TestType2" : 1,
    "TestType3" : 2, 
  }
}
```

The value provided for 'cfg_item_type' could be the string key name or value.
