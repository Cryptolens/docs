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

#### A note about Newtonsoft.Json

`Cryptolens.Licensing.CrossPlatform` depends on the `Newtonsoft.Json` library. In earlier versions of Unity, the `Newtonsoft.Json.dll` had to be included alongside the `Cryptolens.Licensing.CrossPlatform.dll` file (it is provided in the same [zip file as our library](https://github.com/Cryptolens/cryptolens-dotnet/releases/tag/v4.0.36)).

In later versions (from our tests, since 2020.3.16f1), Unity added Newtonsoft.Json as an internal package, meaning that it no longer needs to be included in the Asset folder. However, depending on the Newtonsoft.Json version used by Unity, you may have to recompile our library to target that version. In sum, there are three cases:

##### If Unity does not have an internal Newtonsoft.Json package
This is the case for older versions of Unity (from our tests, for versions earlier than 2020.3.16f1). In this case, you need to include both `Cryptolens.Licensing.CrossPlatform.dll` and `Newtonsoft.Json.dll`.

##### If Unity uses Newtonsoft.Json v12
In this case, we recommend using the .NET Framework 4.8 binary (in the net48 folder) without including the `Newtonsoft.Json.dll` file.

##### If Unity uses Newtonsoft.Json v13
In this case, our library needs to be recompiled with a few changes. The steps are as follows:

* Step 1:  [Install the .NET SDK](https://dotnet.microsoft.com/en-us/download/dotnet/6.0).
* Step 2: [Clone the repository](https://github.com/Cryptolens/cryptolens-dotnet) or [download the zip file](https://github.com/Cryptolens/cryptolens-dotnet/archive/refs/heads/master.zip).
* Step 3: In the Cryptolens.Licensing folder, open `Cryptolens.Licensing.CrossPlatform.csproj`. Remove the following lines:

```
...
<SignAssembly>true</SignAssembly>
<DelaySign>false</DelaySign>
<AssemblyOriginatorKeyFile>certifikat.pfx</AssemblyOriginatorKeyFile>
...
```
* Step 4: Change Newtonsoft.Json version from `12.0.3` to `13.0.1` in the section below:

```
<ItemGroup Condition="'$(TargetFramework)' == 'netstandard2.0' or '$(TargetFramework)' == 'net47' or '$(TargetFramework)' == 'net471' or '$(TargetFramework)' == 'net48'">
    <PackageReference Include="Newtonsoft.Json" Version="12.0.3" />
</ItemGroup>
```
* Step 5: Go back to the main folder (with `sln` files) and run the following command: `dotnet build Cryptolens.Licensing.CrossPlatform.sln`.
* Step 6: Navigate to `Cryptolens.Licensing\bin\Debug\net48` (or `Cryptolens.Licensing\bin\Release\net48`) and copy the `Cryptolens.Licensing.CrossPlatform.dll` into the assets folder.

If you have any questions, please reach out to us at <a href="mailto:support@cryptolens.io">support@cryptolens.io</a>.

### License key verification
Once you have the binaries, we can get license key verification up and running quite quickly. We assume that already have [an account](/getting-started/create-account) and [a product](/getting-started/new-product). There are just two things left:

1. Add the Cryptolens.Licensing.dll and in some cases Newtonsoft.Json.dll into the **Assets folder** (please check out the section earlier when Newtonsoft.Json needs to be included).
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
The `GetMachineCode()` and `IsOnRightMachine()` methods require root access on Linux. If you are using Unity with IL2CCP, `Helpers.GetMacAddress()` can be a better alternative, since other methods may throw an error.

Some users who build and then run the unity project may get a null response from `Key.Activate`, which will be treated as if the license key is invalid. We recommend to set **API Compatibility level** to .NET 4.x in Edit > Project Settings > Player > Other Settings > API Compatibility Level. In this case, the the binaries for .NET 4.* need to be used, i.e. not the .NET Standard version.
