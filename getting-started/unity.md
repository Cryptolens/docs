---
title: Unity 3D
author: Artem Los
description: Introduction to Unity 3D software licensing
labelID: getting_started
---

# Unity 3D

## Idea
In this post we have summarized the necessary steps to add software licensing into a Unity game. Unity uses Mono runtime, which means that a special version of our .NET client library can be used.

## Implementation

### Client library
To implement license key verification a Unity game, we can use a special build of the [client library for .NET](https://github.com/cryptolens/cryptolens-dotnet) that is platform independent. You can download it [here](https://github.com/Cryptolens/cryptolens-dotnet/releases). Make sure to select the binaries that are in the "Without System.Management" folder.

### License key verification
Once you have the binaries, we can get license key verification up and running quite quickly. We assume that already have [an account](/getting-started/create-account) and [a product](/getting-started/new-product). There are just two things left:

1. Add the Cryptolens.Licensing.dll and Newtonsoft.Json.dll into the **Assets folder**.
2. Include the [license key verification logic](/examples/key-verification).

You can download a sample project [here](https://github.com/Cryptolens/Examples/tree/master/unity).

### Other licensing models
Since Unity uses C# as the scripting interface, you can use all of the .NET examples provided in this wiki. As a next step, please take a look at the available [licensing models](/licensing-models/licensetypes).

### Considerations
The `GetMachineCode()` and `IsOnRightMachine()` methods require root access on Linux.