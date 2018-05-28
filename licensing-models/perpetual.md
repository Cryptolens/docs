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
In order to deliver a license key to your customer, you can either use [Payment Forms](/payment-form/index) built-in into Cryptolens or any other third party service.
In either way, you need to call the key generation method to obtain a license key eg. upon a successful transaction. Below is an example of how this can be done in Web API 2.

```
https://app.cryptolens.io/Ext/GenerateKey?UserId=3903&PrivateKey=<private key>&ProductId=3843&SetTime=30&Feature1=True&Feature2=False&Feature3=False&Feature4=False&Feature5=False&Feature6=False&Feature7=False&Feature8=False&AutomaticActivation=True&UpdateTrialOnActivation=False&MaxNoOfMachines=0&OptionalFieldA=0&CustomerInformationId=0&json=true
```

Please read the step-by-step tutorial available below:

* [Key generation tutorial](/examples/key-generation)

> Note, to generate a trial key, a similar procedure can be performed.

### Key validation
Assuming your customer has received a license key, we now want to verify it in the application. This is described in the tutorial below:

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
