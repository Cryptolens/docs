---
title: General FAQ
author: Artem Los
description: Some common questions related to the Cryptolens platform
labelID: basics
---

# Frequently Asked Questions

> The FAQ is a work in progress and is continuously updated based on questions we receive by mail.

## Platform

### Expiration date
When you create a new license, you are asked to set when it should expire. On the product page, every license has a field called `Expires`. The question is, how can we create licenses that never expires?

It turns out that **expiration date will not affect the validity** of a license by default. For example, all code snippets provided in the [key verification tutorial](/examples/key-verification) assume that a license is time-unlimited.

This may feel a bit counter-intuitive and we are continuously working on improving the APIs to make this more trivial.

For now, if you see that a code-example contains a call to the `activate` method (which all examples do at the moment), it means that you need to check the expiration date even if you get a successful result. A way to do this is to call `HasNotExpired` (depending on the library) or simply check the `Expire` field. If you plan to support both time-limited and time-unlimited licenses, you can use one of the feature flags as a way to distinguish this. Please read more [here](/web-interface/keys-that-dont-expire).

#### Plan ahead
You may noticed that you can [edit feature definitions](/web-interface/feature-definitions) in each product. They can be used as a way to help you to keep track of what each feature flag means and they also help our platform understand how to display a certain license in a meaningful way (eg. if F1 stands for a time-limited license and it's not enabled for a certain license, there won't be an option to prolong it).

Recently, we have added support in our Web API that takes into account the feature definitions and adjusts the response sent to the client. Our plan is to integrate this into all client APIs so that you can have most of the license logic set up in the dashboard.

<!--### Creation, Expiration and Sign date-->

### Maximum number of machines
Maximum number of machines is a way to specify how many unique machine codes can be added to a certain license (using the `Activate` method). When the limit is reached, no more machine codes will be added. There are two special cases that is important to keep in mind:

#### Setting to zero
Setting maximum number of machines turns this feature off, i.e. machine codes will not be added to the license. It means users will be able to run the software on any number of machines.

#### Decreasing the value
Let's say you had maximum number of machines set to 10 and one customer has used up the machine code quota, i.e. they have activated on 10 computers. If you decrease this value to something less than 10, eg. 5, all of the activated machines will still work. That's because the platform does not know which machine codes should work and which should not. If you would like to remove some of the machines codes, you can click on the yellow button next to each license and remove the machine codes in from the list.

![](/images/edit-feature-defs.png)

#### How access can be restricted
There are two ways you can restrict the number of machines that can use a license:
1. [Node-locked](/license-models/node-locked): Either you restrict a license to a certain number of machines indefinitely (i.e. until they are deactivated)
2. [Floating](/license-models/floating): or you allow a license to "float" between different machines, as long as at any one point this upper bound of machines is never exceeded

For trial keys, it's better to use the first approach whereas for paid licenses, either approach will work fine, although floating licenses will be more convenient for your customers and remove the hassle of license deactivation.

### Protocols
Cryptolens uses two different protocols to deliver license key information to the client during activation:

* **.NET compatible** (aka LingSign): if you use C# or VB.NET
* **Other languages** (aka StringSign): if you use C++, Java or Python

Most of the clients have methods that allow to load a license key object from file or from String. For example, [LoadFromFile (.NET)](https://help.cryptolens.io/api/dotnet/api/SKM.V3.ExtensionMethods.html#SKM_V3_ExtensionMethods_LoadFromFile_SKM_V3_LicenseKey_), [LoadFromString (Java)](https://help.cryptolens.io/api/java/io/cryptolens/models/LicenseKey.html#LoadFromString-java.lang.String-java.lang.String-int-) or [load_from_string (Python)](https://help.cryptolens.io/api/python/#licensing.models.LicenseKey.load_from_string).

## Legal

### GDPR
General Data Protection Regulation (GDPR) is a new data protection framework that will take effect the 25th of May, 2018. If you plan to do business with European companies or individuals, you need to be compliant with GDPR. Failure to comply can in the worst case lead to fines up to €20 million or 4% of global annual revenue, whichever is higher.

Please take a look at our [security page](/security/#gdpr) for more information.

### Third Party licenses
A list of third party licenses for the C++ client can be found [here](https://github.com/Cryptolens/cryptolens-cpp/blob/master/ThirdPartyLicenses.txt).

## What is SKGL?
Please read [this article](/faq/what-is-skgl).