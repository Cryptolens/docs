---
title: Unity 3D
author: Artem Los
description: Introduction to Unity 3D and Mono software licensing
labelID: getting_started
---

# Unity 3D / Mono

## Idea
In this post we have summarized the necessary steps to add software licensing into a Unity game. Unity uses Mono runtime, which means that a special version of our .NET client library can be used.

> We recommend to read the **Considerations** section in the end of the tutorial that can help when troubleshooting common errors.

## Implementation

### Client library
To implement license key verification a Unity game, we can use a special build of the [client library for .NET](https://github.com/cryptolens/cryptolens-dotnet) that is platform independent. You can download it [here](https://github.com/Cryptolens/cryptolens-dotnet/releases). Make sure to reference the binary with the name `Cryptolens.Licensing.CrossPlatform.dll`. You can also install it through [NuGet](https://www.nuget.org/packages/Cryptolens.Licensing.CrossPlatform/).

### License key verification
Once you have the binaries, we can get license key verification up and running quite quickly. We assume that already have [an account](/getting-started/create-account) and [a product](/getting-started/new-product). There are just two things left:

1. Add the Cryptolens.Licensing.dll and Newtonsoft.Json.dll into the **Assets folder**.
2. Include the [license key verification logic](/examples/key-verification).

> **Note:** we recommend to avoid using [Key.Activate](https://help.cryptolens.io/api/dotnet/api/SKM.V3.Methods.Key.html?#SKM_V3_Methods_Key_Activate_System_String_SKM_V3_Models_ActivateModel_) that takes in an [ActivateModel](https://help.cryptolens.io/api/dotnet/api/SKM.V3.Models.ActivateModel.html) since signature verification will not work on some platforms. Instead, we recommend to use a special version of [Key.Activate](https://help.cryptolens.io/api/dotnet/api/SKM.V3.Methods.Key.html#SKM_V3_Methods_Key_Activate_System_String_System_Int32_System_String_System_String_System_Boolean_System_Int32_System_Int32_) as shown below: 

```cs
// call to activate
var result = Key.Activate(token: auth, productId: 3349, key: "GEBNC-WZZJD-VJIHG-GCMVD", machineCode: "foo");

// obtaining the license key (and verifying the signature automatically).
var license = LicenseKey.FromResponse("RSAPubKey", result);
```

Also, when implementing offline verification, please call `LoadFromFile` and `LoadFromString` with the RSAPubKey. The format of the license file should be set to **Other languages** (see more [here](/faq/index#protocols)) when downloading it in the dashboard or using Activation Forms.

You can download a sample project [here](https://github.com/Cryptolens/Examples/tree/master/unity).

### Other licensing models
Since Unity uses C# as the scripting interface, you can use all of the .NET examples provided in this wiki. As a next step, please take a look at the available [licensing models](/licensing-models/licensetypes).

Please keep in mind if you use the version of [Key.Activate](https://help.cryptolens.io/api/dotnet/api/SKM.V3.Methods.Key.html#SKM_V3_Methods_Key_Activate_System_String_System_Int32_System_String_System_String_System_Boolean_System_Int32_System_Int32_) that was suggested earlier, 
 you would need to create a license key object from the response such as `LicenseKey.FromResponse(result).SaveToFile()` to be able to use the extension methods, eg. `result.LicenseKey.SaveToFile()` will not work.

### Considerations
The `GetMachineCode()` and `IsOnRightMachine()` methods require root access on Linux.

Some users who build and then run the unity project may get a null response from `Key.Activate`, which will be treated as if the license key is invalid. We recommend to set **API Compatibility level** to .NET 4.x in Edit > Project Settings > Player > Other Settings > API Compatibility Level. In this case, the the binaries for .NET 4.* need to be used, i.e. not the .NET Standard version.
