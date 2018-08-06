---
title: Overview of SDK licensing
author: Artem Los
description: Summary of SDKs can be protected using Cryptolens Licensing.
labelID: licensing_models
---

# SDK licensing

## Idea

Software Development Kit (SDK) licensing is when you distribute a component, usually a library, that is later going to be integrated as a part of another commercial solution.

In many ways, licensing an SDK or a desktop software is quite similar. In both cases, you will have to send a license key to your customers in order for the software to work. Many of the licensing models for desktop apps can be used in SDKs, although there are some that will be more suitable for SDKs. However, we need to keep in mind that **end users** are almost always different individuals, and the information that is stored in a license key will be shared with all end users (see the [Privacy](#privacy) section) unless configured otherwise.

## Implementation

### License key input
As with desktop apps, we need to perform a [key verification](/examples/key-verification) before the user of the SDK gains access to the methods. A license key field can be a part of the constructor, a method or a config file stored alongside the SDK.

It is recommended not to store this license key in plain text and to either encrypt it or ensure it is compiled (and obfuscated) in the final software.

### Licensing models

#### Feature lock
In Cryptolens, you can set eight features to either true or false. These features can represent your own features in the SDK.

For example, imagine that your SDK has one method to convert raw sound to .mp3, another to .ogg and a third to .flac. All of these methods can be considered as features.

#### Node-locking

A useful way to ensure that you retain control of all end user instances is to enable [node locking](/licensing-models/node-locked).

For example, you can have different pricing tiers for the number of end users your customers can have (eg. 1 end user for trials, 10,000 for standard and 1,000,000 for enterprise),
or you can charge for each new 1000nd activation. In the first case, you can simply enter the upper bound in [maximum number of machines](/web-interface/maximum-number-of-machines) field. In the second case (where you charge for each 1000nd activation), you can set some upper bound (eg. 10,000) as the [maximum number of machines](/web-interface/maximum-number-of-machines), which you later increase as they start to reach this limit. For billing purposes, a script can be used to count the number of new activations since last time checked.

#### Charge per install
Instead of keeping track of each end user instance, we can instead count the number of installs and then charge for them. This can be done quite easily with data objects, which you [increment](https://app.cryptolens.io/docs/api/v3/IncrementIntValue) each time a new install occurs (and recording this on the end user machine to not count the same machine twice).

<!-- #### Usage based-->

## Privacy
When implementing SDK licensing, you have to assume that information in the license key (eg. notes field, activated devices, customer info) will be shared amongst all end users. In most cases, they will not see this, but we always have to assume they can, as this is technically possible.

A [key verification](/examples/key-verification) call translates to a call to [Activate](https://app.cryptolens.io/docs/api/v3/Activate). This method supports data masking using the **feature lock** field, available when creating a new **access token**. If you scroll down to [License Key](https://app.cryptolens.io/docs/api/v3/Activate#LicenseKey), you will see the fields that can be hidden. If possible, please hide all information unless there is field that you really need. Remember,

> **Note**, all fields in a license key can be seen by all end users, unless data masking is used.