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
string domain = upn.Split('@')[1];
``` 

### Tracking machines for analytics purposes

With site licenses, a limit on individual machines/devices is no longer enforced and instead any number of machines within a network/site can use a license key. If you would still like to keep track of the machines that use the license, you could use the the `FriendlyName` field, which is a part of the `Key.Activate` call. This will allow you to monitor usage on a machine level from the API Logs.

### Limiting maximum number of machines

If you would still like to enforce a limit on the maximum number of concurrent machines (either using [node-locking](/licensing-models/node-locked) or [floating licensing](/licensing-models/floating)) as well as limit usage to a specific location/site, we can store the domain (e.g. Active directory or a similar way of identifying a site) in a data object associated with the license key. This way, only machines that run in a domain that was added to a license may proceed with activation. 

License key verification can proceed as follows:

1. Call `Key.GetKey` to obtain the license key object.
2. Look up the data object that you have defined that contains the approved domain/site identifier. Compare it to the one that is computed on the client machine.
3. If the client machine uses an approved domain, proceed with activation by calling Key.Activate.

> **Tip** To add or edit a data object associated with a license key, click on the license key on the product page and scroll down to *data objects* section. You will then see a link to *add/edit data objects*.