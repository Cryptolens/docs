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

There are two ways of protecting SDKs. Either we keep track of each new device that uses the SDK (which requires internet access) or we allow developers to have unlimited number of end users, as long as it is their own application that uses the SDK (does not require access to the internet).

The choice depends on both the platform and the licensing model you would like to support:

* **Track all devices** - this approach is better suited for SDKs where you would like to charge your customers for usage, eg. per install or unique device. This will work on all platforms. Each instance using the SDK will need to contact Cryptolens at least once.

* **Offline mode** - if you do not want to charge customers per device/install, this approach would suit better. There is no need to contact Cryptolens at any point. This approach works if there is a way to identify the application that calls the SDK. In this tutorial, we will cover how it can be done if your SDKs are targeting .NET (including Mono/Unity) or if it the SDK will work on mobile devices.

### Track all devices (all platforms)
If your SDK will have access to the internet, you can protect it in two ways: by registering every end user of the SDK or by counting each time it is installed.

As with desktop apps, we need to perform a [key verification](/examples/key-verification) before the user of the SDK gains access to the methods. A license key field can be a part of the constructor, a method or a config file stored alongside the SDK. If you only want to contact Cryptolens only once, you can save the response from `Key.Activate` and use it instead of contacting Cryptolens. Please read more [here](/examples/offline-verification).

#### Charge per end user

A useful way to ensure that you retain control of all end user instances is to enable [node locking](/licensing-models/node-locked).

For example, you can have different pricing tiers for the number of end users your customers can have (eg. 1 end user for trials, 10,000 for standard and 1,000,000 for enterprise), or you can charge for each new 1000nd activation. In the first case, you can simply enter the upper bound in [maximum number of machines](/web-interface/maximum-number-of-machines) field. In the second case (where you charge for each 1000nd activation), you can set some upper bound (eg. 10,000) as the [maximum number of machines](/web-interface/maximum-number-of-machines), which you later increase as they start to reach this limit. For billing purposes, a script can be used to count the number of new activations since last time checked.

#### Charge per install
Instead of keeping track of each end user instance, we can instead count the number of installs and then charge for them. This can be done quite easily with data objects, which you [increment](https://app.cryptolens.io/docs/api/v3/IncrementIntValue) each time a new install occurs (and recording this on the end user machine to not count the same machine twice).

### Offline mode (track applications using the SDK)
If your SDK won't have access to the internet, we can protect it by restricting which applications can call its methods, ensuring that only the applications that were developed by your customers can use the SDK. This is accomplished by storing the identifier of the application inside the license file, either inside the "Machine code" field or as a data object.

In this model, you can charge **per developer** or **per application**.

#### In .NET/Mono/Unity
When you use the offline mode approach to protect your SDK, end users are not required to contact Cryptolens. However, to get a signed certificate, developers using the SDK need to contact Cryptolens. The signing utility will send the machine code of the device that requests a certificate, which allows you to enforce [node-locking](/licensing-models/node-locked). For example, you can limit how many devices can be used for signing that use the same license key.

* [Signing utility and docs](https://github.com/Cryptolens/sdk-licensing).

#### Other environments (mobile SDKs)
Depending on the platform, there will be different ways to retrieve the identifier of the application. If you have developed a mobile SDK, you can use the application/bundle id as a way of identifying the application. The distribution process will be similar to the one below:

1. To allow an application to use the SDK, you need to obtain the application id and add it as an "activated device" inside the license (for example, by calling Key.Activate or in the dashboard).
2. Next, send the license file to the customer. They need to place it so that your SDK will be able to read it upon initialization.
3. Whenever the SDK is called, it will verify that the license contains the application id of the calling application.


<!-- #### Usage based-->

## Privacy
When implementing SDK licensing, you have to assume that information in the license key (eg. notes field, activated devices, customer info) will be shared amongst all end users. In most cases, they will not see this, but we always have to assume they can, as this is technically possible.

A [key verification](/examples/key-verification) call translates to a call to [Activate](https://app.cryptolens.io/docs/api/v3/Activate). This method supports data masking using the **feature lock** field, available when creating a new **access token**. If you scroll down to [License Key](https://app.cryptolens.io/docs/api/v3/Activate#LicenseKey), you will see the fields that can be hidden. If possible, please hide all information unless there is field that you really need.

> **Note**, all fields in a license key can be seen by all end users, unless data masking is used.

## Code example

We will illustrate how SDKs can be protected on a class that contains helper methods for various mathematical operations, `MathHelpers`. This class contains three methods that require different features to be true:

* `Abs` - requires F5 to be set to true
* `Factorial` - requires F6 to be set to true
* `Fibonacci` - requires F7 to be set to true

> With this setup, you can easily customize the permitted methods in the dashboard for each license key.

### Initialization
From the customer perspective, SDK usage will depend on if we use the **track all devices** approach or **offline mode**.

#### Track all devices approach
The customer (developer using the SDK in their approach) will need to supply a license key to unlock functionality.

```cs
var math = new MathMethods("FULXY-NADQW-ZAMPX-PQHUT");

Console.WriteLine(math.Abs(5));
Console.WriteLine(math.Fibonacci(5));
```

#### Offline mode approach
Since the customer has already provided the license key when signing the assembly, it will not be required when initializing the class.

```cs
var math = new MathMethods();

Console.WriteLine(math.Abs(5));
Console.WriteLine(math.Fibonacci(5));
```

Depending on the license key, some methods may or may not work.

### Under the hood
As can be seen in the previous section, the license check occurs in the constructor when the customer provides us with the license key. In the **track all devices** approach with end user tracking, the code for this is essentially the same as the one in the [key verification](/examples/key-verification) tutorial. The only difference is that we have added some code to take into account the expiration date and the features.

The `permittedMethods` variable is a `Dictionary<string, bool>` and we use it to track what methods the customer has access to.

```cs
// ...
// in case the license key verification was successful.

// this is a time-limited license, so check expiration date.
if (result.LicenseKey.F1 && !result.LicenseKey.HasNotExpired().IsValid())
    return false;

// everything went fine if we are here!
permittedMethods = new Dictionary<string, bool>();
if (result.LicenseKey.F5)
{
    permittedMethods.Add("Abs", true);
}

if (result.LicenseKey.F6)
{
    permittedMethods.Add("Factorial", true);
}

if (result.LicenseKey.F7)
{
    permittedMethods.Add("Fibonacci", true);
}

return true;
```

If the license key verification was successful, we just need to add an if-statement that checks this before the method is executed.

```cs
/// <summary>
/// Compute the absolute value of a number
/// </summary>
public int Abs(int num)
{
    if (!permittedMethods.ContainsKey("Abs"))
        throw new ArgumentException("The use of this method is not permitted.");

    if (num > 0)
        return num;
    return -num;
}
```

### Source code

#### Track all changes approach

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;


using SKM.V3;
using SKM.V3.Methods;
using SKM.V3.Models;

namespace SDKExample
{
    public class MathMethods
    {
        private Dictionary<string, bool> permittedMethods = new Dictionary<string, bool>();

        public MathMethods(string licenseKey)
        {
            if (!licenseCheck(licenseKey))
            {
                // error occurred when verifying the license.
                throw new ArgumentException("License verification failed.");
            }

        }

        private bool licenseCheck(string licenseKey)
        {
            // From: https://help.cryptolens.io/examples/key-verification

            var RSAPubKey = "{Your RSA Public key}";

            var auth = "{access token with permission to access the activate method}";
            var result = Key.Activate(token: auth, parameters: new ActivateModel()
            {
                Key = licenseKey,
                ProductId = 3941,
                Sign = true,
                MachineCode = Helpers.GetMachineCode()
            });

            if (result == null || result.Result == ResultType.Error ||
                !result.LicenseKey.HasValidSignature(RSAPubKey).IsValid())
            {
                // an error occurred or the key is invalid or it cannot be activated
                // (eg. the limit of activated devices was achieved)
                return false;
            }
            else
            {
                // this is a time-limited license, so check expiration date.
                if (result.LicenseKey.F1 && !result.LicenseKey.HasNotExpired().IsValid())
                    return false;

                // everything went fine if we are here!
                permittedMethods = new Dictionary<string, bool>();

                if (result.LicenseKey.F5)
                {
                    permittedMethods.Add("Abs", true);
                }
                if (result.LicenseKey.F6)
                {
                    permittedMethods.Add("Factorial", true);
                }
                if (result.LicenseKey.F7)
                {
                    permittedMethods.Add("Fibonacci", true);
                }

                return true;
            }

        }

        /// <summary>
        /// Compute the absolute value of a number
        /// </summary>
        public int Abs(int num)
        {
            if (!permittedMethods.ContainsKey("Abs"))
                throw new ArgumentException("The use of this method is not permitted.");

            if (num > 0)
                return num;
            return -num;
        }

        /// <summary>
        /// Compute the factorial, eg. num!.
        /// </summary>
        public int Factorial(int num)
        {
            if (!permittedMethods.ContainsKey("Factorial"))
                throw new ArgumentException("The use of this method is not permitted.");

            if (num == 0)
                return 1;
            return num * Factorial(num - 1);
        }

        /// <summary>
        /// Compute the nth fibonacci number.
        /// </summary>
        public int Fibonacci(int num)
        {
            if (!permittedMethods.ContainsKey("Fibonacci"))
                throw new ArgumentException("The use of this method is not permitted.");

            return fibonacciHelper(num);
        }

        // we use a helper methods to avoid checking the permission on each iteration.
        private int fibonacciHelper(int num)
        {
            if (num == 0 || num == 1)
                return 1;
            return fibonacciHelper(num - 1) + fibonacciHelper(num - 2);
        }
    }
}
```
#### Offline mode

```cs
using System;
using System.Collections.Generic;
using System.Reflection;

namespace SDKExample
{
    public class MathMethods
    {
        private Dictionary<string, bool> permittedMethods = new Dictionary<string, bool>();

        public MathMethods()
        {
            if (!licenseCheck())
            {
                // error occurred when verifying the license.
                throw new ArgumentException("License verification failed.");
            }
        }

        private bool licenseCheck()
        {
            string RSA = "{enter the RSA Public key here}";
            var license = SKM.V3.Methods.Helpers.VerifySDKLicenseCertificate(RSA);

            if(license == null)
            {
                return false;
            }

            // everything went fine if we are here!
            permittedMethods = new Dictionary<string, bool>();

            if (license.F5)
            {
                permittedMethods.Add("Abs", true);
            }
            if (license.F6)
            {
                permittedMethods.Add("Factorial", true);
            }
            if (license.F7)
            {
                permittedMethods.Add("Fibonacci", true);
            }

            return true;

        }

        /// <summary>
        /// Compute the absolute value of a number
        /// </summary>
        public int Abs(int num)
        {
            if (!permittedMethods.ContainsKey("Abs"))
                throw new ArgumentException("The use of this method is not permitted.");

            if (num > 0)
                return num;
            return -num;
        }

        /// <summary>
        /// Compute the factorial, eg. num!.
        /// </summary>
        public int Factorial(int num)
        {
            if (!permittedMethods.ContainsKey("Factorial"))
                throw new ArgumentException("The use of this method is not permitted.");

            if (num == 0)
                return 1;
            return num * Factorial(num - 1);
        }

        /// <summary>
        /// Compute the nth fibonacci number.
        /// </summary>
        public int Fibonacci(int num)
        {
            if (!permittedMethods.ContainsKey("Fibonacci"))
                throw new ArgumentException("The use of this method is not permitted.");

            return fibonacciHelper(num);
        }

        // we use a helper methods to avoid checking the permission on each iteration.
        private int fibonacciHelper(int num)
        {
            if (num == 0 || num == 1)
                return 1;
            return fibonacciHelper(num - 1) + fibonacciHelper(num - 2);
        }
    }
}
```