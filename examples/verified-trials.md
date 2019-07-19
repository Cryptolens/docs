---
title: Trials keys
author: Artem Los
description: Introduction to verified trial keys
labelID: examples
---

# Trial key

A trial key allows your users to evaluate some or all parts of your software for a limited period of time.
The goal of trial keys is to set it up in such a way that you don't need to manually create them, while still keeping everything secure.

In Cryptolens, all trial keys are bound to the device that requested them, which helps to prevent users from using the trial after reinstalling their device.

You can define which features should count as trial by [editing feature definitions](/web-interface/feature-definitions) on the product page.

> Note: the duration of the trial is 15 days by default. You can change that by creating an access token that has **FeatureLock** set to the number of days the trial should be valid. We recommend to create a separate access token for `Key.CreateTrialKey` since the feature lock can interfere with other methods, such as Activate (where it acts as mask for fields).

## Example

In order to create a trial key, you only need to call `Key.CreateTrialKey`. This method will either return a new trial key or an existing one (if this command was called before).

Once that is done, you can perform [key verification](/examples/key-verification) as usual.

> **Note**, it's very important to check `Helpers.IsOnRightMachine(activate.LicenseKey)` is true, in order to make sure that
the trial belongs to the right machine.

The code below is an example of how trial key creation can be set up together with a key verification afterwards. A VB.NET example is coming soon.

```cs
var newTrialKey = Key.CreateTrialKey("access token", new CreateTrialKeyModel { ProductId= 3941, MachineCode =Helpers.GetMachineCode() });

if(newTrialKey == null || newTrialKey.Result == ResultType.Error)
{
    Assert.Fail("Something went wrong when creating the trial key");
}

var activate = Key.Activate("access token", 
    new ActivateModel {
        ProductId = 3941,
        Sign = true,
        MachineCode = Helpers.GetMachineCode(),
        Key = newTrialKey.Key, Metadata = true
    });

if(activate == null || activate.Result == ResultType.Error)
{
    Assert.Fail("Something went wrong when verifying the trial key");
}

// now we can verify some basic properties
if (Helpers.IsOnRightMachine(activate.LicenseKey) && activate.Metadata.LicenseStatus.IsValid)
{
    // license verification successful.
    return;
}

Assert.Fail();
```
