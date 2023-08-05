---
uid: set-cloudflowsowner
author: Danny Styles
date: 03/08/2023
title: Update the owner of all cloudflows in a Dynamics environment.
description: Update your cloudflows to a specific owner id in your dynamics environment by simply providing there full name.
---

# Update the owner of all cloudflows in a Dynamics environment.

After updating your Dynamics environment with cloudlfows you might need to update the owner of those flows to a specific user.

Use this cmdlet to do just that. This cmdlet will iterate through all cloudflows in an environment and update the owner.

## Usage

Make sure your connected to a Dynamics environment using the ```Connect-CrmOnline``` cmdlet.

Then pass in the full name of the new flow owner.

```powershell
Set-CloudflowsOwner -NewFlowOwner "Danny Styles"
```


