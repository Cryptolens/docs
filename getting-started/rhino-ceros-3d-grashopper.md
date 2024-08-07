---
title: Rhino 3D / Grashopper
author: Artem Los
description: Introduction to Rhino ceros and Grashopper plugin software licensing
labelID: getting_started
---

# Rhino 3D / Grashopper

## Idea
In this article we will cover steps necessary to set up key verifications in your Rhinoceros or Grashopper plugin using Cryptolens.

## Implementation

### Client library
By adding the Cryptolens client library, you get access to methods such as `Key.Activate` that make it easier to verify licenses, and perform other licensing related operations.

The easiest way is to install `Cryptolens.Licensing` using NuGet. However, based on our tests, Rhino 8 (and potentially other versions of Rhino) do not seem to support this approach. As a workaround, we suggest the steps below that have been tested in Rhino 8.

1. Download `Cryptolens.Licensing.dll` <a href="https://github.com/Cryptolens/cryptolens-dotnet/releases" target="_blank">from this link</a>.
2. Extract the folder.
3. In Solution Explorer in Visual Studio, right click on **Dependencies** under the name of your project.
4. Using **Browse**, select the path to Cryptolens.Licensing.dll located in the **netstandard2.0**.
5. Using NuGet, install Newtonsoft.Json vs 13.

### Key verification code

Adding key verification code works the same way as described in the [key verification tutorial](/examples/key-verification). 

If you would encounter any issues or have any other questions, please reach out to our support.