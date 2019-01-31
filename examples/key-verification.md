---
title: Key Verification
author: Artem Los
description: A simple implementation of key verification.
labelID: examples
---

# Key Verification

In this example, our aim is to quickly implement the basic licensing functionality into your application.

> Please make sure to add the client API for your language, as described in [this tutorial](/getting-started/skm-client-api).

If you would experience any issues, please check [common errors](#common-errors) section.

## Example
It is quite easy to verify a license. This can be done with the code snippet below. In order to make it work, you need to change three parameters:

* `RSAPubKey` - you can find this key on [this page](https://app.cryptolens.io/docs/api/v3/QuickStart#api-keys), by going to the **API Keys** section.
* `Access Token` - you can find this key on [this page](https://app.cryptolens.io/docs/api/v3/QuickStart#api-keys), by going to the **API Keys** section.
* `ProductId` - you can find it on the product page, which you can find more about [here](/getting-started/new-product).

> For production use cases, it is better to create a specific access token as described [here](/getting-started/access-token). 

The code below should be included whenever you want to verify a license key, which intuitively happens during app start (eg. Form_Load for desktop apps). In addition, you can invoke it whenever a user updates the license key.

### Adding namespaces

#### In C\#
To get the C# code to work, please install **Cryptolens.Licensing** package from NuGet. (see [this tutorial](/getting-started/skm-client-api)).
```csharp
using SKM.V3;
using SKM.V3.Methods;
using SKM.V3.Models;
```

#### In VB.NET
To get the VB.NET code to work, please install **Cryptolens.Licensing** package from NuGet. (see [this tutorial](/getting-started/skm-client-api)).
```vb
Imports SKM.V3
Imports SKM.V3.Methods
Imports SKM.V3.Models
```

#### In Python
To get the Python code to work, you need to run `pip install licensing` first.

```python
from licensing.models import *
from licensing.methods import Key, Helpers
```

#### In Java
To get the Java code to work, you need to reference **cryptolens.jar** available [here](https://github.com/cryptolens/cryptolens-java).
```java
import io.cryptolens.methods.*;
import io.cryptolens.models.*;
```

#### In C++
Please read through the [following instructions](https://github.com/cryptolens/cryptolens-cpp) on how to obtain the client library.

```cpp
#include <iostream>

#include <curl/curl.h>

#include <cryptolens/core.hpp>
#include <cryptolens/Error.hpp>
#include <cryptolens/RequestHandler_curl.hpp>
#include <cryptolens/SignatureVerifier_OpenSSL.hpp>

namespace cryptolens = ::cryptolens_io::v20180502;
using Cryptolens = cryptolens::basic_SKM<cryptolens::RequestHandler_curl,cryptolens::SignatureVerifier_OpenSSL>;
```

### Key verification

#### In C\#
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

#### In VB.NET

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

#### In Python
```python
RSAPubKey = "{enter the RSA Public key here}"
auth = "{access token with permission to access the activate method}"

result = Key.activate(token=auth,\
                   rsa_pub_key=RSAPubKey,\
                   product_id=3349, \
                   key="ICVLD-VVSZR-ZTICT-YKGXL",\
                   machine_code=Helpers.GetMachineCode())

if result[0] == None or not Helpers.IsOnRightMachine(res[0]):
    # an error occurred or the key is invalid or it cannot be activated
    # (eg. the limit of activated devices was achieved)
    print("The license does not work: {0}".format(result[1]))
else:
    # everything went fine if we are here!
    print("The license is valid!")
```

#### In Java
```java
String RSAPubKey = "{enter the RSA Public key here}";
String auth = "{access token with permission to access the activate method}";

LicenseKey license = Key.Activate(auth, RSAPubKey, 
                      new ActivateModel(3349, 
                      "ICVLD-VVSZR-ZTICT-YKGXL", 
                      Helpers.GetMachineCode()));

if (license == null || !Helpers.IsOnRightMachine(license)) {
    System.out.println("The license does not work.");
} else {

    System.out.println("The license is valid!");
    System.out.println("It will expire: " + license.Expires);
}
```

#### In C++
```cpp
/*
 * This example uses the basic_SKM class to make a request to the WebAPI
 * and then checks some properties of the license keys.
 */

int main()
{
  curl_global_init(CURL_GLOBAL_SSL);

  Cryptolens cryptolens_handle;
  cryptolens::Error e;
  // Setting up the signature verifier with credentials from "Security Settings"
  // on cryptolens.io
  cryptolens_handle.signature_verifier.set_modulus_base64(e, "khbyu3/vAEBHi339fTuo2nUaQgSTBj0jvpt5xnLTTF35FLkGI+5Z3wiKfnvQiCLf+5s4r8JB/Uic/i6/iNjPMILlFeE0N6XZ+2pkgwRkfMOcx6eoewypTPUoPpzuAINJxJRpHym3V6ZJZ1UfYvzRcQBD/lBeAYrvhpCwukQMkGushKsOS6U+d+2C9ZNeP+U+uwuv/xu8YBCBAgGb8YdNojcGzM4SbCtwvJ0fuOfmCWZvUoiumfE4x7rAhp1pa9OEbUe0a5HL+1v7+JLBgkNZ7Z2biiHaM6za7GjHCXU8rojatEQER+MpgDuQV3ZPx8RKRdiJgPnz9ApBHFYDHLDzDw==");
  cryptolens_handle.signature_verifier.set_exponent_base64(e, "AQAB");

  cryptolens::optional<cryptolens::LicenseKey> license_key =
    cryptolens_handle.activate
      ( // Object used for reporting if an error occured
        e
      , // Cryptolens Access Token
        "WyI0NjUiLCJBWTBGTlQwZm9WV0FyVnZzMEV1Mm9LOHJmRDZ1SjF0Vk52WTU0VzB2Il0="
      , // Product id
        "3646"
      , // License Key
        "MPDWY-PQAOW-FKSCH-SGAAU"
      , // Machine Code
        "289jf2afs3"
      );

  if (e) {
    // Error occured trying to activate the license key
    using namespace cryptolens::errors;

    if (e.get_subsystem() == Subsystem::Main) {
      // Handle errors from the Cryptolens API
      std::cout << "Cryptolens error: " << e.get_reason() << std::endl;
    } else if (e.get_subsystem() == Subsystem::RequestHandler && e.get_reason() == RequestHandler_curl::PERFORM) {
      int curlcode = e.get_extra();
      std::cout << "Error connecting to the server: curlcode: " << curlcode << std::endl;
    } else {
      std::cout << "Unhandled error: " << e.get_subsystem() << " " << e.get_reason() << " " << e.get_extra() << std::endl;
    }
    return 1;
  }

  std::cout << "License key for product with id: " << license_key->get_product_id() << std::endl;

  // Use LicenseKeyChecker to check properties of the license key
  if (license_key->check().has_expired(1234567)) {
    std::cout << "Your subscription has expired." << std::endl;
    return 1;
  }

  if (license_key->check().has_feature(1)) { std::cout << "Welcome! Pro version enabled!" << std::endl; }
  else                                     { std::cout << "Welcome!" << std::endl; }

  curl_global_cleanup();
}
```


## Common errors

### .NET Core issues
The `Helpers.GetMachineCode()` method is currently not supported on the .NET Core. **MachineCode** can be any value that allows you to uniquely identify an end user (eg. their computer).

### Namespaces missing
This means that [Cryptolens.Licensing](/web-api/skm-client-api) library was not included into the project. It can be easily added using **NuGet packager manager**, which you can find by right clicking on the project:

<img src="/images/nuget-vs2017-example.png" style="width:100%" />

> Note, `Cryptolens.Licensing` has nothing in common with `SKGL`. There is no need to include `SKGL`.

### Result is null
In most cases, this is because some of the required parameters are missing. These are:

* `RSAPubKey`
* `auth`
* `ProductId`

It can also be that the `licenseKey` is missing. Please check [the beginning of the tutorial](#examples) on how to find them.

> Note, if you have **blocked** a license key, it will also return a `null` result.