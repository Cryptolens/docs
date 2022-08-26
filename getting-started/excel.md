---
title: Excel Addins
author: Artem Los
description: Information on how to add software licensing for an Excel add in.
labelID: getting_started
---

# Excel Addins

## Idea
In this post we have summarized the necessary steps to add software licensing into an Excel add in.

> We recommend to read the **Considerations** section in the end of the tutorial that can help when troubleshooting common errors.

## Implementation

There are two steps to get add key verification into an Excel add in.

### Installing the SDK
Cryptolens offers a .NET SDK that we recommend installing into your projects, which makes it easier to perform license key verification and implement other licensing models (such as offline verifications).

The easiest way is to install [Cryptolens.Licensing through NuGet](https://www.nuget.org/packages/Cryptolens.Licensing). Pre-compiled binaries and the source code are also available on [our GitHub page](https://github.com/Cryptolens/cryptolens-dotnet/).

### Adding the key verification code

If your add in uses VB.NET or C\#, you can find the complete code snippet on [this page](https://help.cryptolens.io/examples/key-verification).

## Considerations

When computing the machine code, we recommend using the latest version of the method, which is used in the key verification tutorial. However, in some versions of Excel, it might not be feasible to call Helpers.GetMachineCode on .NET Framework 4.6. The reason for this is the call we make to `System.Runtime.InteropServices.RuntimeInformation.IsOSPlatform`. To fix this, we have added a boolean flag in `Helpers` class. Before calling `Helpers.GetMachineCode` or `Helpers.IsOnRightMachine`, please set `Helpers.WindowsOnly=True`.

```cs
Helpers.WindowsOnly = true;
var machineCode = Helpers.GetMachineCode();
```

If the approach above does not work, please try the following call instead:

```cs
var machineCode = SKGL.SKM.getMachineCode(SKGL.SKM.getSHA1);
```

