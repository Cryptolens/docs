---
title: Site license
author: Artem Los
description: Explains how to allow your clients to use a license on any number of machines within a physical location
labelID: licensing_models
---

# Site license

## Idea

A site license is a way to license your application that does not involve tracking individual devices that are using a license key. Instead, it limits usage to a physical location or a domain, allowing any number of users to use the license within that location or domain.

This can be an appealing option for customers that do not want to manage individual devices and is particularly suited for larger companies.

A site license reduces the administrative overhead of managing employees who should have access and deactivation of old devices when employees leave.

## Implementation

If you have already implemented [node-locking](/licensing-models/node-locked) or [floating licenses](https://help.cryptolens.io/licensing-models/floating) using our defaults, the license will be limited to a specific number of machines. You can verify this by looking at the value of `MachineCode`. If it's set to Helpers.GetMachineCode() or similar, it means that the license will track machines, allowing you to enforce a limit on concurrent use.

To change to a site license, we need to change the value of `MachineCode` so that it identifies the entire location or domain where the customer will use your application.

If we can assume that all users are domain authenticated, the domain name could be passed on as the `MachineCode` during activation, as an example.

```cs
string upn = System.Security.Principal.WindowsIdentity.GetCurrent().Name;
string domain = upn.Split('@')[1];”
``` 

