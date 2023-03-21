---
uid: connect-to-dynamics-with-bearertoken
---

# Connecting to Dynamics using a Bearer token

It's possible to connect to a Dynamics environment by providing a bearer token. The 
advantages of using a bearer token is that it's quick and easy to setup for 
testing template deployments locally.

Below is how a bearer token might look like in a template.

```json
{ 
  "targetenvironment": {
     "bearerToken":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c...",
     "url":"https://{yourdynamicsinstance}.crm11.dynamics.com" 
  }, 
  "type":"cT.crmbaseentity/queues", 
  "apiVersion":"2023-01-09", 
  ... 
}
```

>[!NOTE]
> Although this option is available it's not recommended to use it in an automated
setup.

## Prerequisites

1. A Dynamics environment with system administrator access, this is the environment
 you will deploy the template too.
2. PowerShell with the DRM module installed. Follow this guide to 
[install the powerhsell module](xref:install-powerhsell-module)

## Generate a bearer token using connect-crmonline

In your PowerShell window, log into your dynamics instance using your user account
and the Dynamics url of the environment we will deploy the template too.

```powershell
connect-crmonline -Username "<useraccount>" -ServerUrl <dynamics url>
```

Now lets copy the bearer token so that we can use it in our template.

Run the command below to get the bearer token and copy it for use in the next step.

```powershell
$conn.CurrentAccessToken
```

## Generate a bearer token using an app reg

Another way of getting a bearer token is by requesting one from Azure.

Before trying this method you need to [setup an App Reg Connection.](xref:setup-app-reg-connection)

Once the app reg is in place you can simply paste the following url into
a browser and you will be prompted to login with your user account.  You could also
use [postman](https://www.postman.com/)

``` string
// Line breaks for legibility only 

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
resource=<your dynamics url> 
client_id=<your app reg client id> 
&response_type=token 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&scope=openid&response_mode=fragment 
&state=12345 
&nonce=678910
```

A successful response will look like something like this.

``` string
https://localhost/#access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyIsImtpZCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyJ9
....
&token_type=Bearer&expires_in=1199&state=&session_state=42d127f7-d04a-4d8f-a699-c5faf74d9adb
```

Copy the bearer token ready for the next step.

## Create your template

Now lets generate a template that will create a simple account.

Create a new file called 'bearertokendrm.json' and copy the json template below. 

```json
{
  "$schema": "https://schemas.drmtemplates.io/2021-03-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "targetenvironment": {
        "bearerToken": "<paste your bearer token here>",
        "url": "<dynamics url>"
      },
      "type": "cT.crmbaseentity/accounts",
      "apiVersion": "2023-01-09",
      "name": "DemoBearerToken_accounts",
      "properties": {
        "data": [
          {
            "accountid": "a10443aa-5b1e-4b28-883e-e13f179d93a9",
            "name": "A Demo DRM account"
          }
        ]
      }
    }
  ]
}

```

## Deploy your template

Replace the ```paste your bearer token here``` text with the bearer token you copied from the 
previous step and save the template.

In your PowerShell window run the following command to deploy your template.

```powershell
New-DrmDeployment -TemplateFile '<absolute path to your template>'
```

Log into your dynamics environment and check the new account has been created.

![Dynamics Account](../images/dynamics-account.png)