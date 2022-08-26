---
title: Key Verification
author: Artem Los
description: A simple implementation of key verification.
labelID: examples
---

# Key Verification

In this example, our aim is to quickly implement the basic licensing functionality into your application.

> Please make sure to add the client API for your language, as described in [this tutorial](/web-api/skm-client-api).

If you would experience any issues, please check [common errors](#common-errors) section.

## Example
It is quite easy to verify a license. This can be done with the code snippet below. In order to make it work, you need to change three parameters:

* `RSAPubKey` - you can find this key on [this page](https://app.cryptolens.io/docs/api/v3/QuickStart#api-keys), by going to the **API Keys** section.
* `Access Token` - you can find this key on [this page](https://app.cryptolens.io/docs/api/v3/QuickStart#api-keys), by going to the **API Keys** section.
* `ProductId` - you can find it on the product page, which you can find more about [here](/getting-started/new-product).

> For production use cases, it is better to create a specific access token as described [here](/getting-started/access-token). 

The code below should be included whenever you want to verify a license key, which normally occurs during app start (eg. Form_Load for desktop apps). In addition, you can invoke it whenever a user updates the license key. In some licensing models, this check needs to be called periodically.

* [In C\#](#in-c)
* [In VB.NET](#in-vbnet)
* [In Python](#in-python)
* [In Java](#in-java)
* [In C++](#in-c-1)
* [In Go](#in-golang)
* [In NodeJs](#in-nodejs)
* [In C (beta)](https://github.com/Cryptolens/cryptolens-c)

### Adding namespaces

#### In C\#
To get the C# code to work, please install **Cryptolens.Licensing** package from NuGet. If your application will run on multiple platforms, please install [Cryptolens.Licensing.CrossPlatform](https://www.nuget.org/packages/Cryptolens.Licensing.CrossPlatform/) instead (see [this tutorial](/getting-started/skm-client-api)).
```csharp
using SKM.V3;
using SKM.V3.Methods;
using SKM.V3.Models;
```

The code to verify a license key is available [here](#in-c-2).

#### In VB.NET
To get the VB.NET code to work, please install **Cryptolens.Licensing** package from NuGet. If your application will run on multiple platforms, please install [Cryptolens.Licensing.CrossPlatform](https://www.nuget.org/packages/Cryptolens.Licensing.CrossPlatform/) instead (see [this tutorial](/getting-started/skm-client-api)).
```vb
Imports SKM.V3
Imports SKM.V3.Methods
Imports SKM.V3.Models
```

The code to verify a license key is available [here](#in-vbnet-1).


#### In Python

##### Python 3
To get the Python code to work, you need to run `pip install licensing` first.

```python
from licensing.models import *
from licensing.methods import Key, Helpers
```

##### Python 2
Python 2 library is currently contained in a single file, [cryptolens_python2.py](https://github.com/Cryptolens/cryptolens-python/blob/master/cryptolens_python2.py).
You need to download it and place in the same folder where you have the rest of your code. It can then be imported as follows:

```python
from cryptolens_python2 import *
```

The code to verify a license key is available [here](#in-python-1).


#### In Java
To get the Java code to work, you need to reference **cryptolens.jar** available [here](https://github.com/cryptolens/cryptolens-java).
```java
import io.cryptolens.methods.*;
import io.cryptolens.models.*;
```

The code to verify a license key is available [here](#in-java-1).


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

The code to verify a license key is available [here](#in-c-3).


#### In Golang

```golang
import "github.com/Cryptolens/cryptolens-golang/cryptolens"
```

The code to verify a license key is available [here](#in-golang-1).

#### In Node.js

```js
const key = require('cryptolens').Key;
const Helpers = require('cryptolens').Helpers;
```

The code to verify a license key is available [here](#in-nodejs-1).


### Key verification

#### In C\#
```csharp
var licenseKey = "GEBNC-WZZJD-VJIHG-GCMVD"; // <--  remember to change this to your license key
var RSAPubKey = "enter the RSA Public key here";

var auth = "access token with permission to access the activate method";
var result = Key.Activate(token: auth, parameters: new ActivateModel()
{
    Key = licenseKey,
    ProductId = 3349,  // <--  remember to change this to your Product Id
    Sign = true,
    MachineCode = Helpers.GetMachineCodePI(v: 2)
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

**Note:** If your application will run in Unity/Mono, Rhino/Grasshopper or on a platform other than Windows, we recommend to use a [different version of Key.Activate](/getting-started/unity#license-key-verification).

#### In VB.NET

```vb
Dim licenseKey = "GEBNC-WZZJD-VJIHG-GCMVD" ' <--  remember to change this to your license key
Dim RSAPubKey = "enter the RSA Public key here"

Dim auth = "access token with permission to access the activate method"

Dim result = Key.Activate(token:=auth, parameters:=New ActivateModel() With {
                          .Key = licenseKey,
                          .ProductId = 3349, ' <--  remember to change this to your Product Id
                          .Sign = True,
                          .MachineCode = Helpers.GetMachineCodePI(v:=2)
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
RSAPubKey = "enter the RSA Public key here"
auth = "access token with permission to access the activate method"

result = Key.activate(token=auth,\
                   rsa_pub_key=RSAPubKey,\
                   product_id=3349, \
                   key="ICVLD-VVSZR-ZTICT-YKGXL",\  
                   machine_code=Helpers.GetMachineCode(v=2))

if result[0] == None or not Helpers.IsOnRightMachine(result[0], v=2):
    # an error occurred or the key is invalid or it cannot be activated
    # (eg. the limit of activated devices was achieved)
    print("The license does not work: {0}".format(result[1]))
else:
    # everything went fine if we are here!
    print("The license is valid!")
```

**Note:** The code above assumes that node-locking is enabled. By default, license keys created with Maximum Number of Machines set to zero, which deactivates node-locking. As a result, machines will not be registered and the call to `Helpers.IsOnRightMachine(result[0])` will return False. You can read more about this behaviour [here](https://help.cryptolens.io/faq/index#maximum-number-of-machines). For testing purposes, please feel free to remove `Helpers.IsOnRightMachine(result[0])` from the if statement.

#### In Java
```java
String RSAPubKey = "enter the RSA Public key here";
String auth = "access token with permission to access the activate method";

LicenseKey license = Key.Activate(auth, RSAPubKey, 
                      new ActivateModel(3349,  // <--  remember to change this to your Product Id
                      "ICVLD-VVSZR-ZTICT-YKGXL", // <--  remember to change this to your license key
                      Helpers.GetMachineCode(2)));

if (license == null || !Helpers.IsOnRightMachine(license, 2)) {
    System.out.println("The license does not work.");
} else {

    System.out.println("The license is valid!");
    System.out.println("It will expire: " + license.Expires);
}
```

**Note:** The code above assumes that node-locking is enabled. By default, license keys created with Maximum Number of Machines set to zero, which deactivates node-locking. As a result, machines will not be registered and the call to `Helpers.IsOnRightMachine(license)` will return False. You can read more about this behaviour [here](https://help.cryptolens.io/faq/index#maximum-number-of-machines). For testing purposes, please feel free to remove `Helpers.IsOnRightMachine(license)` from the if statement.

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

#### In Golang

```golang
token := "access token with permission to access the activate method"
publicKey := "enter the RSA Public key here"

licenseKey, err := cryptolens.KeyActivate(token, cryptolens.KeyActivateArguments{
	ProductId:   3646,
	Key:         "MPDWY-PQAOW-FKSCH-SGAAU",
	MachineCode: "289jf2afs3",
})
if err != nil || !licenseKey.HasValidSignature(publicKey) {
	fmt.Println("License key activation failed!")
	return
}
```

#### In Node.js

```js
var RSAPubKey = "Your RSA Public key, which can be found here: https://app.cryptolens.io/User/Security";
var result = key.Activate(token="access token with permission to access the activate method", RSAPubKey, ProductId=3349, Key="GEBNC-WZZJD-VJIHG-GCMVD", MachineCode=Helpers.GetMachineCode());

result.then(function(license) {

    // success
    
    // Please see https://app.cryptolens.io/docs/api/v3/model/LicenseKey for a complete list of parameters.
    console.log(license.Created);

}).catch(function(error) {
    // in case of an error, an Error object is returned.
    console.log(error.message);
});

```


## Common errors

### .NET Core issues
If you plan to target multiple platforms, we recommend to install [Cryptolens.Licensing.CrossPlatform](https://www.nuget.org/packages/Cryptolens.Licensing.CrossPlatform/).

### Helpers.GetMachineCode issues
In some Windows environments (e.g. when developing Excel addins), it might not be feasible to call Helpers.GetMachineCode on .NET Framework 4.6. The reason for this is the call we make to System.Runtime.InteropServices.RuntimeInformation.IsOSPlatform. To fix this, we have added a boolean flag in Helpers class. Before calling Helpers.GetMachineCode or Helpers.IsOnRightMachine, please set Helpers.WindowsOnly=True.

```cs
Helpers.WindowsOnly = true;
var machineCode = Helpers.GetMachineCode();
```

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
