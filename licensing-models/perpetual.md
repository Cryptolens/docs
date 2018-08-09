---
title: Perpetual licensing model
author: Artem Los
description: Explains how the simple one-time purchase model (perpetual) can be implemented in Cryptolens.
labelID: licensing_models
---

# Perpetual licensing (try/buy)

## Idea

This model enables you to securely distribute trial versions of your product, which your customer can easily upgrade.

A trial license can either be time-limited (eg. 30 days) or not. When it's not time-limited, it can have a reduced set features (eg. lite version). Once the time limit is reached, it can either disable the entire application or just certain features.

The user can later upgrade the same license key to be able to get a full-featured version of the application.

## Implementation

To implement this model, there are just two concepts that are required: key generation (for trial- and full-featured licenses) and key verification (inside your application).

### Key generation

#### Trial keys
In order to allow your customers to try your software, trial keys can be used. Trial keys can be generated inside your application using [Key.CreateTrialKey](https://help.cryptolens.io/api/dotnet/api/SKM.V3.Methods.Key.html?q=create%20trial#SKM_V3_Methods_Key_CreateTrialKey_System_String_SKM_V3_Models_CreateTrialKeyModel_) method.

These keys will be locked to the machine that requested them, which ensures that the trial cannot be requested again. Once the trial has elapsed, users can keep the same key (they only need to upgrade it). We describe this below.

#### Upgrading trial

If your users liked the product, the next step is to **upgrade the license*** key with the features they need (or extending the duration of the key if you have a SaaS model). You can **create an entirely new license** key also. We cover both of the options below:

##### Upgrading existing key
The simplest way of doing this is to take their existing license key and upgrade it. In this case, changes will reach the software instantly. 

There are two ways to upgrade a license key. The fastest way is to use [Payment Forms](/payment-form/index) built-in into Cryptolens. You can also use any other third party service or perform this operation manually using our dashboard. If you use Payment Forms, we can take advantage of the [custom field](/payment-form/request), when passing a license key to the form. In this way, you can create a link inside your app which allows your users to upgrade a license.

When upgrading a license key, [key related methods](https://app.cryptolens.io/docs/api/v3/Key) in the Web API will be of great help, for example, `Add Feature` or `Extend License`.

##### Creating a new key

If you do not use the built-in trial keys, there is an option to create an entierely new license key (upon a successful payment). As we described in the previous sections, you can either use [Payment Forms](/payment-form/index) built-in into Cryptolens or any other third party service. 

[Create Key](https://app.cryptolens.io/docs/api/v3/CreateKey) method can be used to create new license keys using the Web API. Please take a look at the tutorial below:

* [Key generation tutorial](/examples/key-generation)

> Note, to generate a trial key, a similar procedure can be performed (if you don't want to create trial keys inside your application).

### Key validation
Assuming your customer has received a license key (either by requesting a trial inside the app or by any other means), we now want to verify it in the application. This is described in the tutorial below:

* [Key verification tutorial](/examples/key-verification)

The only change that has to be added is the distinction between trial- and full-featured licenses. In the `else` statement, you can add something similar to:

> Note, we have assumed that **feature 1** stands means it's a trial license. This is quite flexible and you can choose any configuration of features to your specific needs.

In C#:

```csharp
// ...
// everything went fine if we are here!

if (result.license.HasFeature(1).HasNotExpired().IsValid()) 
{
    // feature 1 is trial, so we check if it's enabled, we check
    // the expiration date
} 
else if (result.license.HasNotFeature(1).IsValid())
{
    // if feature 1 is not enabled, it is a full featured license.
}
else 
{
    // the license has expired.
}

```

In VB.NET

```vb
' ...
' everything went fine if we are here!

If result.license.HasFeature(1).HasNotExpired().IsValid() Then
    ' feature 1 is trial, so we check if it's enabled, we check
    ' the expiration date
Else If result.license.HasNotFeature(1).IsValid() Then
    ' if feature 1 is not enabled, it is a full featured license.
Else 
    ' the license has expired.
End If

```
