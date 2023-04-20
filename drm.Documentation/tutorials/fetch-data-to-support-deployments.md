---
uid: fetch-data-support-deployments
---

# Fetch Data using queries when deploying to your target environment

When deploying to your target environment, it is possible to fetch data using queries. This allows
you to update entities based on data that is only known at runtime.

To do this, you can add a "queries" block in your template, give the query a name, and specify
the entity and filter. The filter should be written in OData format.

For example, in the template below, a query named 'adminRoleId' is used to filter the roles
entity by businessunitid_value and name. The result is then used to associate the role in the
'teamroles_association' property.

```json
{ 
...  
  "type": "drm.crmbaseentity/teams", 
  "apiVersion": "2023-01-09", 
  "queries": {
     "adminRoleId": {
        "entity": "roles",
        "filter": "$select=roleid&$filter=_businessunitid_value eq 'a46145646-cd42-ea11-a812-000d3a7ed30d' and name eq 'Admin Team Member'"
     }
  },
  "properties": {
     "data": [
       {
         "teamid": "a5fe711d-9f5f-4d7d-9c8e-434e1c627be8",
         "name": "Test team good",
         "emailaddress": "team@domain.com",
         "businessunitid": "e6145646-cd42-ea11-a812-000d3a7ed30d",
         "teamroles_association": [
            {
               "roleid": "[queries('adminRoleId').value[0].roleid]",
               "drmmetadata": {
                  "comment": "Role Admin Team Member"
               }
            }
          ]
        }
      ]
    }
... 
}
```

## Queries

Lets take a closer look at the queries block

```json
{ 
...  
  "type": "drm.crmbaseentity/teams", 
  "apiVersion": "2023-01-09", 
  "queries": {
     "adminRoleId": {
        "entity": "roles",
        "filter": "$select=roleid&$filter=_businessunitid_value eq 'a46145646-cd42-ea11-a812-000d3a7ed30d' and name eq 'Admin Team Member'"
     }
  },
```

The 'adminRoleId' object above is used to fetch data at runtime by specifying an entity and a filter in OData format.

In this example, a single query called "adminRoleId" is targeting the "roles" entity and applying a 
filter to select the roleid where the businessunitid_value is 'a46145646-cd42-ea11-a812-000d3a7ed30d'
and the name is 'Admin Team Member'.

It is possible to include multiple queries per targeted entity, but it is recommended to keep it under 10 queries
to maintain readability and performance.

Each query is identified by its property name, and the object must include the entity, and the filter you wish
to apply to it. The query returns a specific result in JSON format, as shown in the json below where it returns
the roleid of the role with the specified name and businessunitid_value.

```json
{
   "@odata.context": "https://demo.crm11.dynamics.com/api/data/v9.2/$metadata#roles(name)",
   "value": [
     {
       "@odata.etag": "W/\"104267310\"",
       "name": "Admin Team Member",
       "roleid": "b0b309b5-6efa-41d6-bfb4-15718e2969b9"
     }
   ]
}
```

In order to use the result of a query in your template, you can use the syntax shown in the example provided.

```json
"teams": [
       {
         "teamid": "a5fe711d-9f5f-4d7d-9c8e-434e1c627be8",
         "name": "Test team good",
         "emailaddress": "team@domain.com",
         "businessunitid": "e6145646-cd42-ea11-a812-000d3a7ed30d",
         "teamroles_association": [
            {
               "roleid": "[queries('adminRoleId').value[0].roleid]",
               "drmmetadata": {
                  "comment": "Role Admin Team Member"
               }
            }
          ]
        }
      ]
```

In this case, the goal is to associate the role returned by the "adminRoleId" query with a team. The query result
is accessed using the following notation: ```'[queries('adminRoleId').value[0].roleid]'```.

The "queries" function is used to return the value from the query, and the json path is used to specify the
specific value needed. In this example, we are accessing the roleid of the role returned by the query.

This approach allows you to use data from the target environment during deployment and make your templates more
dynamic and adaptable to different scenarios.

## Queries with multiple results set

Sometimes you might want to use a query that returns multiple results.

Lets stick with roles but the query we have returns the following result.

```json
{ 
  "@odata.context": "https://demo.crm11.dynamics.com/api/data/v9.2/$metadata#roles(name)", 
  "value": [ 
     { 
        "@odata.etag": "W/\"72025834\"", 
        "name": "Role Demo", 
        "roleid": "ac6acdc7-2d80-430c-b50b-0069d1b704e6" 
     }, 
     { 
        "@odata.etag": "W/\"72025835\"", 
        "name": "Role Admin", 
        "roleid": "cd6e0789-9381-4397-8d28-00c8f6c7f5c9" 
     },
     { 
        "@odata.etag": "W/\"72025836\"", 
        "name": "Sales Role", 
        "roleid": "3eabb264-97da-4258-af38-038cb47edf40" 
     }
  ]
}
```

Using JsonPath you can target any of those results.

To return the 'Sales Role' name your json path would be ```'[queries('{nameofquery}').value[2].name]'```

To return the role id cd6e0789-9381-4397-8d28-00c8f6c7f5c9' of the second result the 
json path would be ```'[queries('{nameofquery}').value[1].roleid]'```

## Join queries

It's possible to join queries.

Using the example below we want the business unit id for the business unit 'Demo' then we use the result to fetch roles that equal 
that business unit id.

```json
"queries": { 
   "businessunitdemo": { 
      "entity": "businessunits", 
      "filter": "$filter=name eq 'Demo'" 
   },   
   "myroles": { 
      "entity": "roles", 
      "filter": "[concat('$select=name&$filter=_businessunitid_value eq ', queries('businessunitdemo').value[0].businessunitid)]" 
   }
}
```

The query 'businessunitdemo' will return the business unit where name equals 'Demo'.

The next query 'myroles' uses the businessunitid from the businessunitdemo query.

> [!NOTE]
> Please be aware of circular dependencies when writing joining queries.

## Using concat when building filters

If you need to concat a filter using parameters or variables for example then you will need to 
escape single quotes with another single quote as per the example below.

```json
"queries": { 
   "businessunitdemo": { 
      "entity": "businessunits", 
      "filter": "[concat('$filter=name eq ''', parameters('businessunitname'), '''')]" 
   },   
   "myroles": { 
      "entity": "roles", 
      "filter": "[concat('$select=name&$filter=_businessunitid_value eq ', queries('businessunitid').value[0].businessunitid)]" 
   }
}
```
### Single quotes in concat filters

It's important to remember that when building queries with other functions such as 'concat' you will need to escape 
singles quotes in your query.  So a single quote in your query string will become ''

The example below is fetching the root business unit with name equal to BU Demo and using the result of this query
in the 'roleId' query which will return a role Id.

> [!NOTE]
> The rootBusUnit query does not use concat which means you don't need to escape single quotes in the filter.

```json
"queries": {
    "rootBusUnit": {
        "entity": "businessunits",
        "filter": "$filter=name eq 'BU Demo'"
    },
    "roleId": {
        "entity": "roles",
        "filter": "[concat('$select=name&$filter=_businessunitid_value eq ', queries('rootBusUnit').value[0].businessunitid, ' and name eq ''DRM Role''')]",
        "comment": "Fetching role id for DRM Role"
    }
```

## Next steps

- [Learn more about writing filter queries for the Dynamics Web API](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/webapi/query-data-web-api)
- [Learn how to generate copies of objects using queries](xref:fetch-data-generate-dynamic-lists)