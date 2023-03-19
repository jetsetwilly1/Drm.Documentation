# Template functions

DRM templates offer the following functions that can be used in your templates.

## concat function

The basic syntax for using the "concat" function is:

```json
"[concat(string1, string2, string3, ...)]"
```

For example, if you have two variables var1 and var2 and you want to join them together, you would use the following:

```json
"[concat(variables('var1'), variables('var2'))]"
```

## add function

The basic syntax for using the "add" function is:

```json
"[add(number1, number2, number3, ...)]"
```

For example, if you have two variables var1 and var2 and you want to add them together, you would use the following:

```json
"[add(variables('var1'), variables('var2'))]"
```

## toupper function

The toupper() function in DRM templates is used to convert a string to uppercase. The basic syntax for using the toupper() function is:

```json
"[toupper(string)]"
```

For example, if you have a variable var1 with the value "Hello World" and 
you want to convert it to uppercase, you would use the following:

```json
"[toupper(variables('var1'))]"
```

This will return "HELLO WORLD".

## tolower function

The tolower() function in DRM templates is used to 
convert a string to lowercase. The basic syntax for using the tolower() function is:

```json
"[tolower(string)]"
```

For example, if you have a variable var1 with the value "Hello World" and you 
want to convert it to lowercase, you would use the following:

```json
"[tolower(variables('var1'))]"
```

This will return "hello world".

You can use the tolower() function to convert a string to lowercase in order 
to make it consistent with other strings you're working with, or to make it 
easier to compare strings that may have mixed case. For example, you can 
use tolower() function to compare two string variables case insensitively.

## queries function

Use the queries function to target a specific query and use the data returned
in your template.

[Check this document out on creating queries](xref:fetch-data-support-deployments)

```json
"[queries(string)]"
```

Here is a quick example, we have a query called ```roleIds``` in the queries
object.

Under the ```teamroles_association``` array a role is being associated with a
team by executing the function ```[queries('roleIds').value[0].roleid]```.

The ```queries``` function uses the data returned from the roleIds query and returns
the first roleid.

```json
{ 
...  
  "type": "cT.crmbaseentity/teams", 
  "apiVersion": "2023-01-09", 
  "queries": {
     "roleIds": {
        "entity": "roles",
        "filter": "$select=roleid"
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
               "roleid": "[queries('roleIds').value[0].roleid]"
            }
          ]
        }
      ]
    }
... 
}
```

## queriescopyonprop function

This function is used in conjunction with 'queriescopyon' dynamic list process.

[Click here to see how to generate dynamic lists during template deployments.](xref:fetch-data-generate-dynamic-lists)

```json
"[queriescopyonprop(string)]"
```