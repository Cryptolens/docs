---
title: Key Verification
author: Artem Los
description: A simple implementation of key verification.
labelID: examples
---

# Key Verification

In this example, our aim is to quickly implement the basic licensing functionality into your application.

> We assume you have created an project using Visual Studio 2017 and added SKGL Extension (see [this tutorial](/getting-started/skm-client-api)).

If you would experience any issues, please check [common errors](#common-errors) section.

## Example
It is quite easy to verify a license. This can be done with the code snippet below. In order to make it work, you need to change three parameters:

* `RSAPubKey` - you can find this key on [this page](https://app.cryptolens.io/docs/api/v3/QuickStart#api-keys), by going to the **API Keys** section.
* `Access Token` - you can find this key on [this page](https://app.cryptolens.io/docs/api/v3/QuickStart#api-keys), by going to the **API Keys** section.
* `ProductId` - you can find it on the product page, which you can find more about [here](/getting-started/new-product).

> For production use cases, it is better to create a specific access token as described [here](/getting-started/access-token). 

The code below should be included whenever you want to verify a license key, which intuitively happens during app start (eg. Form_Load for desktop apps). In addition, you can invoke it whenever a user updates the license key.

### Adding namespaces in C\#

```csharp
using SKM.V3;
using SKM.V3.Methods;
using SKM.V3.Models;
```

### Adding namespaces in VB.NET

```vb
Imports SKM.V3
Imports SKM.V3.Methods
Imports SKM.V3.Models
```

### Key verification in C\#

```csharp
var licenseKey = "GEBNC-WZZJD-VJIHG-GCMVD";
var RSAPubKey = "{enter the RSA Public key here}";

var auth = "{access token with permission to access the activate method}";
var result = Key.Activate(token: auth, parameters: new ActivateModel()
{
    Key = licenseKey,
    ProductId = 3349,
    Sign = true,
    MachineCode = Helpers.GetMachineCode()
});

if (result == null || result.Result == ResultType.Error ||
    !result.LicenseKey.HasValidSignature(RSAPubKey).IsValid())
{
    // an error occurred or the key is invalid or it cannot be activated
    // (eg. the limit of activated devices was achieved)
    Console.WriteLine("The license does not work.");
}
else
{
    // everything went fine if we are here!
    Console.WriteLine("The license is valid!");
}

Console.ReadLine();
```

### Key verification in VB.NET

```vb
Dim licenseKey = "GEBNC-WZZJD-VJIHG-GCMVD"
Dim RSAPubKey = "{enter the RSA Public key here}"

Dim auth = "{access token with permission to access the activate method}"

Dim result = Key.Activate(token:=auth, parameters:=New ActivateModel() With {
                          .Key = licenseKey,
                          .ProductId = 3349,
                          .Sign = True,
                          .MachineCode = Helpers.GetMachineCode()
                          })

If result Is Nothing OrElse result.Result = ResultType.[Error] OrElse
    Not result.LicenseKey.HasValidSignature(RSAPubKey).IsValid Then
    ' an error occurred or the key is invalid or it cannot be activated
    ' (eg. the limit of activated devices was achieved)
    Console.WriteLine("The license does not work.")

Else
    ' everything went fine if we are here!
    Console.WriteLine("The license is valid!")
End If
```

## Common errors

### .NET Core issues
The `Helpers.GetMachineCode()` method is currently not supported on the .NET Core. **MachineCode** can be any value that allows you to uniquely identify an end user (eg. their computer).

### Namespaces missing
This means that [SKGLExtesion](/web-api/skm-client-api) library (aka SKM Client API) was not included into the project. It can be easily added using **NuGet packager manager**, which you can find by right clicking on the project:

<img src="/images/nuget-vs2017-example.png" style="width:100%" />

> Note, `SKGLExtension` has nothing in common with `SKGL`. There is no need to include `SKGL`.

### Result is null
In most cases, this is because some of the required parameters are missing. These are:

* `RSAPubKey`
* `auth`
* `ProductId`

It can also be that the `licenseKey` is missing. Please check [the beginning of the tutorial](#examples) on how to find them.

> Note, if you have **blocked** a license key, it will also return a `null` result.