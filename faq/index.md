---
title: General FAQ
author: Artem Los
description: Some common questions related to the Cryptolens platform
labelID: basics
---

# Frequently Asked Questions

> The FAQ is a work in progress and is continuously updated based on questions we receive by mail.

## Table of contents

<ul>
<li><a href="#platform">Platform</a>
    <ul>
        <li><a href="#expiration-date">Expiration date</a>
        <ul>
            <li><a href="#blocking-expired-licenses">Blocking expired licenses</a></li>
            <li><a href="#starting-countdown-upon-activation">Starting countdown upon activation</a></li>
            <li><a href="#plan-ahead">Plan ahead</a></li>
        </ul>
        </li>
        <li><a href="#maximum-number-of-machines">Maximum number of machines</a>
        <ul>
            <li><a href="#setting-to-zero">Setting to zero</a></li>
            <li><a href="#decreasing-the-value">Decreasing the value</a></li>
            <li><a href="#how-access-can-be-restricted">How access can be restricted</a></li>
            <li><a href="#node-locked-vs-floating-licenses">Node-locked vs. floating licenses</a></li>
            <li><a href="#friendly-name">Friendly name</a></li>
            <li><a href="#definition-of-an-end-user--machine-code">Definition of an end user / machine code</a></li>
        </ul>
        </li>
        <li><a href="#protocols">Protocols</a></li>
        <li><a href="#securing-your-account-with-2fa">Securing your account with 2FA</a></li>
        
    </ul>
    </li>
<li><a href="#client-apis--sdks">Client APIs / SDKs</a>
 <ul>
            <li><a href="#machine-code-generation">Machine code generation</a>
             <ul>
            <li><a href="#net">.NET</a></li>
            <li><a href="#java">Java</a></li>
            <li><a href="#python">Python</a></li>
            <li><a href="#c">C++</a></li>
            <li><a href="#plan-ahead-1">Plan ahead</a></li>
        </ul>
            </li>
            <li><a href="#protecting-rsa-public-key-and-access-tokens">Protecting RSA Public Key and Access Tokens</a>
                         <ul>
            <li><a href="#rsa-public-key">RSA Public Key</a></li>
            <li><a href="#access-token">Access Token</a></li>

        </ul>
        <li><a href="#troubleshooting-api-errors">Troubleshooting API errors</a></li>
            </li>
        </ul>

</li>
<li><a href="#billing">Billing</a>
   <ul>
            <li><a href="#how-monthly-pricing-is-computed">How monthly pricing is computed</a></li>
        </ul>
</li>
<li><a href="#legal">Legal</a></li>
<li><a href="#what-is-skgl">What is SKGL?</a></li>
</ul>

## Platform

### Expiration date
When you create a new license, you are asked to set when it should expire. On the product page, every license has a field called `Expires`. The question is, how can we create licenses that never expires?

It turns out that **expiration date will not affect the validity** of a license by default. For example, all code snippets provided in the [key verification tutorial](/examples/key-verification) assume that a license is time-unlimited.

This may feel a bit counter-intuitive and we are continuously working on improving the APIs to make this more trivial.

For now, if you see that a code-example contains a call to the `activate` method (which all examples do at the moment), it means that you need to check the expiration date even if you get a successful result. A way to do this is to call `HasNotExpired` (depending on the library) or simply check the `Expire` field. If you plan to support both time-limited and time-unlimited licenses, you can use one of the feature flags as a way to distinguish this. Please read more [here](/web-interface/keys-that-dont-expire).

#### Blocking expired licenses
In order to ensure that licenses stop working after that they have expired, you can select "Block Expired Licenses" when editing [feature names](/web-interface/feature-definitions). Expired licenses will be blocked within an hour. Note: the following licenses will not be blocked:

* if the license key has a data object with the name **cryptolens_stripesubscriptionitemid**, since these licenses are managed by Stripe through the recurring billing module.
* if [start countdown upon activation](https://help.cryptolens.io/web-interface/trial-activation) (aka trial activation) is enabled, provided that the maximum number of activations has not been reached. For example, if you have created a license with `start countdown upon activation=true` and `MaxNoOfMachines=1`, it will not be blocked if it has not been activated. This way, you can pre-generate the licenses and ensure that your customer can still activate them, even when the expiration date has passed. However, once it has been activated, it will be blocked after the new expiration date (assuming that the license is marked as time-limited).

#### Starting countdown upon activation
If you do not know when your customers will activate the license for the first time but you still want them to use it for a set number of days, you can enable [trial activation](/web-interface/trial-activation).

#### Plan ahead
You may noticed that you can [edit feature definitions](/web-interface/feature-definitions) in each product. They can be used as a way to help you to keep track of what each feature flag means and they also help our platform understand how to display a certain license in a meaningful way (eg. if F1 stands for a time-limited license and it's not enabled for a certain license, there won't be an option to prolong it).

Recently, we have added support in our Web API that takes into account the feature definitions and adjusts the response sent to the client. Our plan is to integrate this into all client APIs so that you can have most of the license logic set up in the dashboard.

<!--### Creation, Expiration and Sign date-->

### Maximum number of machines
Maximum number of machines is a way to specify how many unique machine codes can be added to a certain license (using the `Activate` method). When the limit is reached, no more machine codes will be added. There are two special cases that is important to keep in mind:

#### Setting to zero
Setting maximum number of machines to zero turns this feature off, i.e. machine codes will not be added to the license. It means users will be able to run the software on any number of machines.

> Note, `Helpers.IsOnRightMachine()` will return false if no machine code is registered with the license, which will be the case if maximum number of machines is set to 0. As a solution, please check the `MaxNumberOfMachines` field, ensuring it is not 0, before calling `Helpers.IsOnRightMachine()`.

#### Decreasing the value
Let's say you had maximum number of machines set to 10 and one customer has used up the machine code quota, i.e. they have activated on 10 computers. If you decrease this value to something less than 10, eg. 5, all of the activated machines will still work. That's because the platform does not know which machine codes should work and which should not. If you would like to remove some of the machines codes, you can click on the yellow button next to each license and remove the machine codes in from the list.

![](/images/edit-feature-defs.png)

#### How access can be restricted
There are two ways you can restrict the number of machines that can use a license:
1. [Node-locked](/license-models/node-locked): Either you restrict a license to a certain number of machines indefinitely (i.e. until they are deactivated)
2. [Floating](/license-models/floating): or you allow a license to "float" between different machines, as long as at any one point this upper bound of machines is never exceeded

For trial keys, it's better to use the first approach whereas for paid licenses, either approach will work fine, although floating licenses will be more convenient for your customers and remove the hassle of license deactivation.

#### Node-locked vs. floating licenses
There is a difference between how [node-locked](/licensing-models/node-locked) and [floating licenses](/licensing-models/floating) work, and how the maximum number of machines limit is enforced. For node-locked licenses, the device will be registered with the license key and a list of these devices can be found in the `ActivatedMachines` property of a LicenseKey or in the list of _Activated devices_ (in the dashboard). You will need to deactivate unused devices if want to allow new devices to use the license.

In the floating license case, the devices will be registered with the license key temporarily and it will therefore not be possible to list all of them in `ActivatedMachines` property. Only the floating license device that is being activated in the request will be shown.

> **To sum up**: in the node-locked case, activated devices will remain activated (unless deactivated at a later point), whereas for a floating device to remain active, it needs to send periodic **heartbeats**, within a time-window. The length of this time-window is specified using `FloatingTimeInterval` variable. If a device fails to send a request (aka heartbeat) within that interval, it will automatically be deactivated and other devices will be able to activate (i.e. use that seat).

To list all devices, including those registered on a floating license model, you can click on the yellow button next to the license key:

![](/images/edit-feature-defs.png)

The `FloatingTimeInterval` is set to 100 by default and can easily be changed.

> **Note** The number of machines activated using node-locking model will not affect the number of machines in the floating license model. In other words,
if maximum number of machines is set to **5**, then you can have 5 unique devices registered using the node-locked model and another 5 devices can use
the license key concurrently (floating license model).

#### Friendly name
The **friendly name** is a way to assign a name to an activated device so that both you and your customers can keep track of which activation belongs to which user. Normally, the **machine code** contains the fingerprint of a device, for example, `9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08`. If you customer has many end users (i.e. activated devices), navigating between these device fingerprints can become troublesome. To resolve this, you can provide a `FriendlyName` when you call `Key.Activate`. At the time of writing, this is supported in our .NET client.

There are a couple of ways you can compute the friendly name for a particular user. At this point, our recommendation is to set it to [Environment.MachineName](https://docs.microsoft.com/en-us/dotnet/api/system.environment.machinename?view=netstandard-2.1). For example, if your customers use Active Directory, the machine name will show up in the list of devices (for a particular user) in Active Directory Admin dashboard:

<img src="/images/ad-user.png" width="100%" />

When your customers have a large number of employees, we recommend to give them access to the customer portal with activation/deactivation permission. You can access a generic sign up link [here](https://app.cryptolens.io/Customer/SignUpLink). Once they are logged in, they will be able manage their licenses and activations in the [customer portal](https://app.cryptolens.io/Portal/Admin).

#### Definition of an end user / machine code
In most cases, an **end user** is defined as a unique device using a certain license. In our examples, we achieve this by calling the `Key.Activate` method with `MachineCode=Helpers.GetMachineCode()`. The `Helpers.GetMachineCode()` will return a device fingerprint that is unique for each device.

However, there are other ways end users can be defined. The choice will depend on your specific licensing model. We will briefly outline several other ways it can be defined:

##### Per process
If your customers will be able to run multiple instances on the same machine, you can define the end user so that includes the process id in addition to the fingerprint of the device. For example, it can be defined in .NET as follows:

```cs
MachineCode = Helpers.GetMachineCode() + System.Diagnostics.GetCurrentProcess().Id
```

##### Per user
The end user can also be defined as the currently logged in user (for example, Active Directory user), allowing the same user to use their license on multiple machines where they are logged in.

##### Per instance
When the identifier changes frequently (for example, in the case of [docker containers](/licensing-models/containers)) or when there is no reliable way to obtain an identifier (for example, in the case with virtual machines), it is better to generate a random identifier each time the application starts, use it only within one session and the discard it. Since this will lead to a large number of new end users being registered with the license, we recommend to apply the [floating license model](/licensing-models/floating).

In C\#, the GUID can be obtained as follows:

```cs
var machineCode = Guid.NewGuid();
```

In Python, it can be achieved as follows:

```python
import uuid
machine_code = uuid.uuid4().hex
```

##### Per network location
You can also use treat all users within the same network as one end users. For example, this would allow all employees within the same geographical location to be treated as one end user.


### Protocols
Cryptolens uses two different protocols to deliver license key information to the client during activation:

* **.NET compatible** (aka LingSign): if you use C# or VB.NET (unless you use Unity/Mono specific methods, in which case _Other languages_ protocol is used)
* **Other languages** (aka StringSign): if you use C++, Java or Python

Most of the clients have methods that allow to load a license key object from file or from String. For example, [LoadFromFile (.NET)](https://help.cryptolens.io/api/dotnet/api/SKM.V3.ExtensionMethods.html#SKM_V3_ExtensionMethods_LoadFromFile_SKM_V3_LicenseKey_), [LoadFromString (Java)](https://help.cryptolens.io/api/java/io/cryptolens/models/LicenseKey.html#LoadFromString-java.lang.String-java.lang.String-int-) or [load_from_string (Python)](https://help.cryptolens.io/api/python/#licensing.models.LicenseKey.load_from_string).

### Securing your account with 2FA
We recommend to set up two factor authentication to secure your account. At the moment, we support the TOTP protocol. You can install an authenticator app on your phone or use Yubico authenticator in case you have a Yubico security key. For example, if you have Yubikey, you can active two factor authentication as follows:

1. Visit the [two factor set up page](https://app.cryptolens.io/Account/TwoFactorSetUp).
2. Click on "Enable two step authentication".
3. Open [Yubico authenticator](https://www.yubico.com/products/services-software/download/yubico-authenticator/) and click on the plus sign to add a new account. If the QR code is visible, Yubico Authenticator will automatically recognize it. We recommend to require touch to access the credentials.
4. Save the backup code in a secure place.
5. You have now successfully enabled two-factor authentication!

<!--### What is activation-->

## Client APIs / SDKs

### Machine code generation

Machine codes are used to uniquely identify an end user instances, i.e. a machine code is a device fingerprint. The way it is generated depends on the SDK and the platform, which is listed below.

#### .NET
Prior to v4.0.15, machine codes have used [following code](https://github.com/Cryptolens/cryptolens-dotnet/blob/master/Cryptolens.Licensing/SKM.cs#L1233) to gather device specific information and later hash it using either SHA1 or SHA256. The problem with this method is that it is Windows specific and requires System.Management (which is not supported in Mono when integrating with Unity). To solve this, we opted for platform specific methods to retrieve the UUID. You can read more about how to migrate [here](https://help.cryptolens.io/api/dotnet/articles/v4015.html#platform-independent-machine-code-method). Depending on the platform, the following shell calls are made to retrieve the UUID:

**Windows**

Two methods are currently being used. In older versions of the library (and when v=1), the following command is used in platform independent methods (i.e. with PI extension).
```
cmd.exe /C wmic csproduct get uuid
```

In newer versions, the following method is used (which returns the same UUID) as above.

```
cmd /c powershell.exe -Command \"(Get-CimInstance -Class Win32_ComputerSystemProduct).UUID\"\n"
```

The reason for the switch is that WMIC will no longer be supported in future versions of Windows 11.

**Mac**
```
system_profiler SPHardwareDataType | awk '/UUID/ { print $3; }'
```

**Linux**

If we cannot determine the type of hardware, the following method will be used:

```
findmnt --output=UUID --noheadings --target=/boot
```

If we can detect that the application runs on a Raspberry PI (can be determined by calling `cat /proc/device-tree/model`), we will extract the "Serial" by running the following command:

```
cat /proc/cpuinfo
```

Alternative way to identify Linux instances is to run the command below. Note that it requires sudo access.

```
dmidecode -s system-uuid
```

The old method (prior v4.0.15) is implemented in [Helpers.GetMachineCode](https://help.cryptolens.io/api/dotnet/api/SKM.V3.Methods.Helpers.html#SKM_V3_Methods_Helpers_GetMachineCode_System_Boolean_). The new method that uses the UUID from the host OS is implemented in [Helpers.GetMachineCodePI](https://help.cryptolens.io/api/dotnet/api/SKM.V3.Methods.Helpers.html#SKM_V3_Methods_Helpers_GetMachineCodePI). If you call `Helpers.GetMachineCodePI(v:2)` in .NET, you will get the same machine code generated on Windows as in the Python library with similar settings.

#### Java
In Java, the [following method](https://github.com/Cryptolens/cryptolens-java/blob/master/src/main/java/io/cryptolens/methods/Helpers.java#L23) is used by default.

Since v1.23, you can generate the same machine code as in the Python library and .NET libraries by calling `Helpers.GetMachineCode(2)` on Windows. It will use the UUID of the device and works in the `cryptolens-android.jar`, which does not depend on `slf4j`.

#### Python
In Python, similar to .NET, we opted for UUID, which can be provided by the OS. You can see the source code [here](https://github.com/Cryptolens/cryptolens-python/blob/master/licensing/methods.py#L167). Note, the machine code will not be the same as in .NET with default parameters. If you call `Helpers.GetMachineCode(v=2)` in Python and `Helpers.GetMachineCodePI(v: 2)` in .NET, the machine code will be the same on Windows.

#### C++
In C++, we use the same method that was used in .NET prior to v4.0.15. The source code can be found [here](https://github.com/Cryptolens/cryptolens-cpp/blob/master/src/MachineCodeComputer_COM.cpp). We are working on shipping a platform independent version in the next release.

#### Plan ahead
Our plan is to introduce a platform independent method to retrieve the UUIDs, in order to ensure that machine codes are the same for all client libraries. Currently, there is a way to get the same machine code on Windows in .NET and Python clients.

### Protecting RSA Public Key and Access Tokens

If we take the [key verification tutorial](/examples/key-verification) as an example, there are three parameters that are specific to your account: `RSA Public Key`, `Access Token` and `ProductId`. 

#### RSA Public Key
The RSA public key is used to verify a license key object sent by Cryptolens. This is especially useful if you want to implement [offline activation](/examples/offline-verification) since we don't want any of the properties (eg. features and expiration date) to be changed by the user.

> Note: the RSA public key can only be used to verify a license key object, the private key that is used for signing is stored on our server.

#### Access Token
An access token tells Cryptolens who you are (authentication) and what permissions are allowed (authorization). In other words, it can be thought of as username and password combined, but with restricted permission.

It's recommend to restrict access tokens as much as possible, both in terms of what it can do (eg. Activate or Create Key) and what information it returns (read more [here](/legal/DataPolicy#using-feature-lock-for-data-masking)). For example, you should preferably only allow access tokens used in the client application to `Activate` a license key. The permission to `Create Key` should only be accessible in applications that you control, eg. on your server.

So if you have a restricted access token, the chances that an adversary will be able to do any harm is minimal. For example, the worst that an adversary can do is to activate more devices (up to a [certain limit](#maximum-number-of-machines)), which can be fixed quite easily in the dashboard.

### Troubleshooting API errors
When implementing Cryptolens into your software, you may get different kinds of errors depending if you call the API directly or through one of the client libraries. This section 
covers the most common errors, why they occur and how to troubleshoot them.

The majority of errors (99%) are caused by a wrong **product id**, **access token** or **RSA Public key**. In some cases, it can also be because an active subscription is missing.
Our client libraries might not always show the real error message and instead display a generic error. To find out the real error message from the Web API, you can call the method through curl. For example, to check the access token, you can call curl as shown below:

```
curl https://app.cryptolens.io/api/key/Activate? \ 
     -d token={access token} \
     -d ProductId={product id} \
     -d Key={license key string}
```

You can also check this with the browser. The call above would translate to:
```
https://app.cryptolens.io/api/key/Activate?token={access token}&ProductId={product id}&Key={license key string}
```

The most common errors and their cause is summarized below:

| Error message | Description |
|---------------|-------------|
| <span style="white-space:nowrap;">`Unable to authenticate`</span> | The access token is wrong. It is also shown if the subscription has expired. API access requires an active subscription. |
| `Something went wrong. Please contact support@cryptolens.io and we will try to fix it.` | A temporary server error. Try again with a new request. |
|`No active subscription found. Please contact support.`| No active subscription is associated with the account and thus the API call failed. The error can be fixed by updating your billing information and add a valid payment method on the [billing page](https://app.cryptolens.io/billing). |
| `Access denied` | The access token is correct but it does not have sufficient permission. Make sure that if you, for example, want to call GetKey that the access token has "GetKey" permission. |
| `Not enough permission and/or key not found.` | If you have checked that the product contains the license key and the access token has the right permission to call the method, this error can be fixed by setting Key Lock to -1 when creating the access token (assuming that you are working with data object related methods). |
{:.table-bordered}

## Billing

When you sign up for an account, you get 30 days to test the service for free. If you would like to keep the subscription after the trial, it's important to add a valid credit card before 30 days trial has elapsed. After the trial, the account will be downgraded.

### How monthly pricing is computed

To find out how monthly pricing is calculated, please visit the [billing page](https://app.cryptolens.io/Billing) and click on the link **pricing page** that will allow you to estimate your usage.

If your billing page includes the **End users** property, please read the section below to understand how end users are computed.

#### If you are charged for end users

The pricing is entirely based on usage. It's based on two values: the number of **active licenses** (i.e. those that are not blocked) and the number of active end users. 

For example, let's suppose that you have 20 licenses and 40 end users. In that case, since there are more than 10 end users, the service fee (based on end users) will be 50 (in the standard package). Since there are 20 licenses, the total cost for them will be 20 * 0.1 = 2. The sum will be 52 per month. If your usage goes down, the price will go down too.

To compute the number of **active end users**, we count the number of unique (license, machine code) pairs in a certain period of time (for node-locked licenses, it's one year back and for floating licenses it's a month back). For example, if a license key is set to work on at most one machine, then if each machine verifies the license continuously and there are 10 licenses in total, this will result in 10 end users. If you perform license key verifications periodically or only once, this value will be lower. A unblocked license will always count as at least 1 active end user.

Note: for customers inside the EU, we will add an additional VAT on top of the price if no VAT number has been provided. The currency is EUR for customers inside EEA and USD otherwise.

## Legal

### GDPR
General Data Protection Regulation (GDPR) is a new data protection framework that will take effect the 25th of May, 2018. If you plan to do business with European companies or individuals, you need to be compliant with GDPR. Failure to comply can in the worst case lead to fines up to €20 million or 4% of global annual revenue, whichever is higher.

Please take a look at our [security page](/security/#gdpr) for more information.

### Third Party licenses
A list of third party licenses for the C++ client can be found [here](https://github.com/Cryptolens/cryptolens-cpp/blob/master/ThirdPartyLicenses.txt).

## What is SKGL?
Please read [this article](/faq/what-is-skgl).