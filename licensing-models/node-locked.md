---
title: Node-locked licenses / Machine code locking
author: Artem Los
description: An overview of node-locked licenses
labelID: licensing_models
---

# Node-locked licenses

## Idea
In order to ensure that a license key is not used on more machines than the user is entitled to, **machine code locking** (aka node-locked licenses) can be used.

Here are some example cases:

* your customer bought 1 license that should only work on two computers, their work and home computer
* all employees of your customer should have access to your software, as long as they are working on-site.

Once the machines are registered, the license will be locked to them. The customer will have to de-activate machines to be able to use them on a new device (if this is permitted). If you instead want your customers to seamlessly switch machines (as long as the quota is not reached), floating licenses may be better suited for this case.

## Implementation

### In the dashboard

Node-locking is enforced by setting [maximum number of machines](/web-interface/maximum-number-of-machines) to a value greater than 0. This can be done when creating a new or by modifying an existing license. Once set, each time a [key verification](/examples/key-verification) request is performed, Cryptolens will ensure that the number of machines that have activated the license does not exceed the value that you have specified.

### Code
In order to get node-locking to work, you can use the same code that was shown in the [key verification](/examples/key-verification) tutorial.
For additional security, we should add a check as shown below:

**In C#**

```cs
if(Helpers.IsOnRightMachine(result.LicenseKey)) 
{
    // everything is ok
}
else
{
    // an error occurred
} 

```

**In VB.NET**

```vb
If Helpers.IsOnRightMachine(result.LicenseKey) Then
    ' everything is ok
Else
    ' an error occurred
End If
```

This code will work for the first example (remember to set **maximum number of machines** to **2** in your dashboard).

For the second example, we will need to modify the key verification code slightly. Instead of using `MachineCode = Helpers.GetMachineCode()`, we need another way of identifying unique machine codes. In this case, we need to ensure every device in the on-site generates the same machine code. This could, for example, be the network name, IP address, etc.