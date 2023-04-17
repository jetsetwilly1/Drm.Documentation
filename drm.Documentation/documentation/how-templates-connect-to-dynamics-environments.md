---
uid: target-dynamics-instance
---

# How templates connect to Dynamics environments

Before deploying a DRM Template, it is important to consider the method of connecting to the Dynamics 
environments that you plan to deploy to.

In most cases you should [setup an application registration in Azure](xref:setup-app-reg-connection)
and use it to
[connect to your Dynamics environments](xref:connect-to-dynamics-with-app-registration).

In some situations however you may need to impersonate as a user to run, for example workflow updates.
Follow this link for [connecting to Dynamics using User credentials](xref:connect-to-dynamics-with-usercreds)

This article explains the different connection types available in DRM templates.

## targetenvironment object

To connect to a dynamics instance you will need to provide an object as 
below for each resource being deployed.

See the full reference for a DRM template [here](xref:default-crmbaseentity-reference)

```json
{ 
  ...
  "targetenvironment": { 
     "bearerToken": "string", 
     "userCredentials": { 
         "username": "string", 
         "password": "string", 
         "clientId": "string", 
         "tenantId": "string" 
     }, 
     "applicationCredentials":{ 
         "clientId": "string", 
         "clientSecret": "string", 
         "tenantId": "string" 
     }, 
     "url": "string" 
  }, 
  ...
}
```

## Available connection types

There are three ways to connect into a Dynamics instance.

- Bearer Token
- Application Registration
- User Credentials

## Token expiry

Each connection type results in a Bearer token that is passed to the Dynamics environment
during a deployment.

The bearer token that is issued by Azure Active Directory (AAD) to represent the 
application or user identity has a limited lifespan.

The exact lifespan of the bearer token will depend on the specific settings
of the Azure Active Directory tenant and can be defined by the administrator.

Typically, the bearer token issued for an application registration lasts for 
1 hour (3600 seconds) by default. This means that the token is valid for 1 hour 
after it is issued and can be used to authenticate requests to the Dynamics 365 
web API during that time.

>[!NOTE]
> It's unlikely that you will hit the token expiry time during a template deployment.

## Connection types explained

### Bearer token connection

The bearer token connection only requires two properties, the ```bearerToken``` and ```url```.

```json
{ 
  ...
  "targetenvironment": { 
     "bearerToken": "string",
     "url": "string" 
  }, 
  ...
}
```

We advise using this connection type for local testing while templates are being generated/created.

Typically you can return a bearer token using the ```Connect-CrmOnline``` PowerShell cmd.

Please see [Connect-CrmOnline](xref:connect-crmonline-cmdlet) for more information.

### Application Registration connection

A targetenvironment object for an app registration connection should contain the information below.

```json
{ 
  ...
  "targetenvironment": {
     "applicationCredentials":{ 
         "clientId": "string", 
         "clientSecret": "string", 
         "tenantId": "string" 
     }, 
     "url": "string" 
  }, 
  ...
}
```

Using this connection type is the recommended approach when deploying templates in an automated process.

A Dynamics 365 application registration does not require a specific license.  It's important 
to note that to call the Dynamics 365 Web API, you will also need to ensure that 
application has the appropriate permissions to access the Dynamics 365 data.

Rate limits could be in force but will depend on the specific Dynamics 365 instance and can 
vary depending on the type of subscription. Using DRM to configure your environments however shouldn't come 
any where near these rate limits.

### User Credential connection

To connect to a Dynamics environment, your connection object will require the following
values.

```json
{ 
  ...
  "targetenvironment": {
     "userCredentials": { 
         "username": "string", 
         "password": "string", 
         "clientId": "string", 
         "tenantId": "string" 
     },
     "url": "string" 
  }, 
  ...
}
```

When connecting to Dynamics 365 web API, there are several restrictions to keep in mind:

- Rate limiting: Dynamics 365 enforces rate limits on the number of requests that 
can be made per minute.

- Data access: The web API can only be used to access data that the authenticated
user has permissions to access. Attempts to access restricted data will result 
in a 403 Forbidden response.

### User licencing

The most common licenses that include access to the Dynamics 365 web API are:

1. Dynamics 365 Sales Enterprise: This license includes access to the Sales, 
Marketing, and Customer Service modules, as well as the Dynamics 365 web API.

2. Dynamics 365 Customer Engagement Plan: This license includes access 
to the Dynamics 365 web API, as well as the Sales, Marketing, and Customer Service modules.

3. Dynamics 365 Enterprise Plan 1: This license includes access to the 
Dynamics 365 web API, as well as the Financials, Project Service Automation, and Field Service modules.

4. Dynamics 365 Enterprise Plan 2: This license includes access to the 
Dynamics 365 web API, as well as the Financials, Project Service Automation,
Field Service, and Customer Service Intelligence modules.

## Next steps

- Learn how to [connect to Dynamics with an Azure App Registration.](xref:connect-to-dynamics-with-app-registration)
- Learn how to [connect to Dynamics using a Bearer token.](xref:connect-to-dynamics-with-bearertoken)
- Learn how to [connect to Dynamics using User credentials](xref:connect-to-dynamics-with-usercreds)
