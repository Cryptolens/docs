---
title: Floating icenses
author: Artem Los
description: An overview of how floating licenses can be implemented in Cryptolens
labelID: licensing_models
---

# Floating licenses

## Idea
Using [node-locking](/licensing-models/node-locked), we saw a way to "lock" a license to a limited number of machines. A problem with this set up is if your customers plans to change computers often, in which case it can turn out to be inconvenient to keep activate and deactivate machines.

Instead, we can use the notion of floating licenses, where we ensure that a license can be used on a certain number of machines at a time. 

Here are some examples:

* you have developed a desktop app that your customers can install unlimited number of times, with the condition that they use it on 1 device at a time.
* a company has 100 employees, but you only want to ensure that 20 of those can use your software simultaneously. 

## Implementation

### In the dashboard
As with [node-locked](/licensing-models/node-locked) licenses, floating licenses are enforced by setting **maximum number of machines** to a value greater than 0.

### In your application

We can reuse almost the entire code snippet from the [key verification](/examples/key-verification) tutorial, with the only difference in the last parameter, which we have to add. The `IsOnRightMachine` check is similar to the one in the [node-locking](/licensing-models/node-locking) example, only differing in `isFloatingLicense`, which is set to true. 

**In C#**
```cs
var result = Key.Activate(token: auth, parameters: new ActivateModel()
{
    Key = licenseKey,
    ProductId = 3349,
    Sign = true,
    MachineCode = Helpers.GetMachineCode(),
    FloatingTimeInterval = 100      // <- we have added this parameter.
});

// from node-locking example
if(Helpers.IsOnRightMachine(result.LicenseKey, isFloatingLicense: true))) 
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
Dim result = Key.Activate(token:=auth, parameters:=New ActivateModel() With {
                          .Key = licenseKey,
                          .ProductId = 3349,
                          .Sign = True,
                          .MachineCode = Helpers.GetMachineCode()
                          .FloatingTimeInterval = 100      ' <- we have added this parameter.
                          })

' from node-locking example
If Helpers.IsOnRightMachine(result.LicenseKey, isFloatingLicense:= true) Then
    ' everything is ok
Else
    ' an error occurred
End If
```

> **FloatingTimeInterval** specifies how long back in history of activations we should go, in order to decide whether a
machine is still using the software or not (specified in seconds).

**Note**, in comparison to node-locking, where key verification for most applications can occur once during startup, floating licensing requires continuous key verifications. The smaller FloatingTimeInterval, the more key verifications have to occur.

> As a rule of thumb, if you plan to verify a license every 10 minutes, the **FloatingTimeInterval** should be no less than 600 (600s = 10min) if you want to ensure that your customers never exceed the **maximum number of machines**. However, if you can allow the customers to exceed this boundary in eg. 5 minutes, you could reduce **FloatingTimeInterval**  to 300 (300s = 5min).