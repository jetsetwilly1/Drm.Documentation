---
uid: fetch-data-generate-dynamic-lists
author: Stuart Elcocks
date: 28/03/2023
title: Fetch data and generate dynamic lists within your DRM Template
description: It's possible to create multiple objects inside an array from the results set of a query in your DRM Template. For example by using the queriescopyon property its possible to populate the members of a queue entity.  Read through this document to get a better understanding of generating dynamic lists. 
---

# Fetch data and generate dynamic lists within your DRM Template

Its possible to create multiple objects inside an array from the results set of a query.

The example below shows a private queue being created, the private queue needs all the 
members of contacts team to be added to the queue.

By using the 'queriescopyon' property this template will iterate over the results resturned 
from the 'teamsquery' query and create an object for each member of the team.

```json
{ 
...  
  "type": "drm.crmbaseentity/queues", 
  "apiVersion": "2023-01-09", 
  "queries": {
     "teamsquery":{ 
        "entity": "teams", 
        "filter": "$select=name&$filter=name eq 'contacts'&$expand=teammembership_association($select=fullname,systemuserid)" 
     }
  },
  "properties": {
     "data": [
        { 
           "queueid": "651e3ba4-2fe4-46ae-ac5f-2802b4f38a7a", 
           "name": "Test Queue 3",
           "queuemembership_association": [ 
              { 
                 "systemuserid": "[queriescopyonprop('systemuserid')]", 
                 "metadata": { 
                    "name": "[queriescopyonprop('fullname')]"
                 },
                 "queriescopyon": { 
                    "query": "teamsquery", 
                    "property": "teammembership_association",
                    "ignoreemptyarrays": true
                 }
              }, 
              { 
                 "systemuserid": "784869cd-6b0c-eb11-a812-000d3a7ed330", 
                 "metadata": { 
                    "name": "stuart elcocks" 
                 } 
              } 
           ]
        }
      ]
    }
... 
}
```

The resulting template would then look like below, all the members of the 'contacts' team 
would be added to the queue as members.

```json
{ 
...  
  "type": "drm.crmbaseentity/queues", 
  "apiVersion": "2023-01-09", 
  "queries": {
     "teamsquery":{ 
        "entity": "teams", 
        "filter": "$select=name&$filter=name eq 'contacts'&$expand=teammembership_association($select=fullname,systemuserid)" 
     }
  },
  "properties": {
     "data": [
        { 
           "queueid": "651e3ba4-2fe4-46ae-ac5f-2802b4f38a7a", 
           "name": "Test Queue 3",
           "queuemembership_association": [ 
              { 
                 "systemuserid": "3fffd097-7dd3-4513-8eff-1b49830714b2", 
                 "metadata": { 
                    "name": "fred smith" 
                 } 
              }, 
              { 
                 "systemuserid": "9222608f-2099-4f88-b32f-0a55b106c81d", 
                 "metadata": { 
                    "name": "tina west" 
                 } 
              }, 
              { 
                 "systemuserid": "784869cd-6b0c-eb11-a812-000d3a7ed330", 
                 "metadata": { 
                    "name": "stuart elcocks" 
                 } 
              } 
           ]
        }
      ]
    }
... 
}
```

## How queriescopyon works

Lets take a closer look at the queriescopyon block.

In the queue example below we are setting up a queue but we want members of the contacts team 
to be associated with it.

The 'queuemembership_association' is an array type that accepts objects with 'systemuserid' as 
the required property.

```json
{ 
...  
  "type": "drm.crmbaseentity/teams", 
  "apiVersion": "2023-01-09", 
  "queries": {
     "teamsquery": {
        "entity": "teams", 
        "filter": "$select=name&$filter=name eq 'contacts'&$expand=teammembership_association($select=fullname,systemuserid)" 
     }
  },
  "properties": {
     "data": [
        { 
           ...
           "queuemembership_association": [ 
              { 
                 "systemuserid": "[queriescopyonprop('systemuserid')]", 
                 "metadata": { 
                    "name": "[queriescopyonprop('fullname')]"
                 },
                 "queriescopyon": { 
                    "query": "teamsquery", 
                    "property": "teammembership_association",
                    "ignoreemptyarrays": true
                 }
              }
           ] 
        }
...
```

The 'queriescopyon' object has the following properties set

'query' - Which query to use as the result set

'property' - Which property from the result set to iterate over.

'ignoreemptyarrays' - if the query does not contain any results then ignore and carry on with processing.

Because property is set to 'teammembership_association' then the DRM engine will iterate 
over the results for this property, for each team returned. In this case we will only have one team.

The queriescopyonprop function specifies what property from the result set you want to use as the value.

Below will make sure that systemuserid is set to the value of systemuserid from the result set.

```json
"systemuserid": "[queriescopyonprop('systemuserid')]",  
```

## Usage scenarios

If there is any situation you have where you need to add a collection of items to an entity but the 
list you need to add is dynamic then queriescopyon could be a useful tool.

## Next steps

- [Learn more about writing filter queries for the Dynamics Web API](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/query-data-web-api)
- [Learn how to use single query result sets](xref:fetch-data-support-deployments)
