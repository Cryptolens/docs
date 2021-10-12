---
title: Floating licenses
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
* a company has 10 devices that have the software installed, but only 2 of them will use it concurrently. Sometimes, all of the 10 devices have to be used simultaneously and you want to permit that (aka overdraft license).
* your application will run in a container that will created and removed frequently, and you want to ensure that your clients can only run 10 containers simultaneously.

> **Note:** This tutorial focuses on how floating licenses can be implemented when there is a connection to the internet. If some of your clients will be offline, they can install our license server instead. More information is available [here](https://github.com/Cryptolens/license-server#floating-licenses-offline).

## Implementation

### In the dashboard
As with [node-locked](/licensing-models/node-locked) licenses, floating licenses are enforced by setting **maximum number of machines** to a value greater than 0.

### In your application

We can reuse almost the entire code snippet from the [key verification](/examples/key-verification) tutorial, with the only difference in the last parameter, which we have to add. The `IsOnRightMachine` check is similar to the one in the [node-locking](/licensing-models/node-locked) example, only differing in `isFloatingLicense`, which is set to true. 

#### Floating without overdraft

The code bellow allows at most the **maximum number of machines** to use the software concurrently.

**In C#**
```cs
var auth = "Access token with permission to access the activate method";
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
Dim auth = "Access token with permission to access the activate method"
Dim result = Key.Activate(token:=auth, parameters:=New ActivateModel() With {
                          .Key = licenseKey,
                          .ProductId = 3349,
                          .Sign = True,
                          .MachineCode = Helpers.GetMachineCode(),
                          .FloatingTimeInterval = 100      ' <- we have added this parameter.
                          })

' from node-locking example
If Helpers.IsOnRightMachine(result.LicenseKey, isFloatingLicense:= true) Then
    ' everything is ok
Else
    ' an error occurred
End If
```

**In Java**

```java
String RSAPubKey = "{Your RSA Public key}";
String auth = "Access token with permission to access the activate method";

// note that the new parameter set to 100.
LicenseKey license = Key.Activate(auth, RSAPubKey, new ActivateModel(3349, "MTMPW-VZERP-JZVNZ-SCPZM", Helpers.GetMachineCode(), 100));

// from node-locking example
if (license == null || !Helpers.IsOnRightMachine(license, true)) {
    System.out.println("The license does not work.");
} else {
    System.out.println("The license is valid!");
    System.out.println("It will expire: " + license.Expires);
}
```

**In Python**

```python
RSAPubKey = "{enter the RSA Public key here}"
auth = "{access token with permission to access the activate method}"

result = Key.activate(token=auth,\
                   rsa_pub_key=RSAPubKey,\
                   product_id=3349, \
                   key="ICVLD-VVSZR-ZTICT-YKGXL",\
                   machine_code=Helpers.GetMachineCode(),\
                   floating_time_interval=100)

if result[0] == None or not Helpers.IsOnRightMachine(result[0], is_floating_license= True):
    # an error occurred or the key is invalid or it cannot be activated
    # (eg. the limit of activated devices was achieved)
    print("The license does not work: {0}".format(result[1]))
else:
    # everything went fine if we are here!
    print("The license is valid!")
```

#### Floating with overdraft
The code bellow allows at most the **maximum number of machines** + 1 to use the software concurrently.

**In C#**

```cs
var auth = "Access token with permission to access the activate method";
var result = Key.Activate(token: auth, parameters: new ActivateModel()
{
    Key = licenseKey,
    ProductId = 3349,
    Sign = true,
    MachineCode = Helpers.GetMachineCode(),
    FloatingTimeInterval = 100,     // <- we have added this parameter.
    MaxOverdraft = 1                // <- we can exceed the max number of machines by one.
});

// from node-locking example
if(Helpers.IsOnRightMachine(result.LicenseKey, isFloatingLicense: true, allowOverdraft: true))) 
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
Dim auth = "Access token with permission to access the activate method"
Dim result = Key.Activate(token:=auth, parameters:=New ActivateModel() With {
                          .Key = licenseKey,
                          .ProductId = 3349,
                          .Sign = True,
                          .MachineCode = Helpers.GetMachineCode()
                          .FloatingTimeInterval = 100,     ' <- we have added this parameter.
                          .MaxOverdraft = 1                ' <- we can exceed the max number of machines by one.
                          })

' from node-locking example
If Helpers.IsOnRightMachine(result.LicenseKey, isFloatingLicense:= true, allowOverdraft:= true) Then
    ' everything is ok
Else
    ' an error occurred
End If
```

**In Java**
```java
String RSAPubKey = "{Your RSA Public key}";
String auth = "Access token with permission to access the activate method";

// note the two new parameter, one for floating time interval (i.e. 100) and one telling how much we can exceed the max number of machines (i.e. by 1).
LicenseKey license = Key.Activate(auth, RSAPubKey, new ActivateModel(3349, "MTMPW-VZERP-JZVNZ-SCPZM", Helpers.GetMachineCode(), 100, 1));

// from node-locking example (NB: we need to provide an additional true flag in Helpers.IsOnRightMachine to support overdraft)
if (license == null || !Helpers.IsOnRightMachine(license, true, true)) {
    System.out.println("The license does not work.");
} else {
    System.out.println("The license is valid!");
    System.out.println("It will expire: " + license.Expires);
}
```

**In Python**

```python
RSAPubKey = "{enter the RSA Public key here}"
auth = "{access token with permission to access the activate method}"

result = Key.activate(token=auth,\
                   rsa_pub_key=RSAPubKey,\
                   product_id=3349, \
                   key="ICVLD-VVSZR-ZTICT-YKGXL",\
                   machine_code=Helpers.GetMachineCode(),\
                   floating_time_interval=100,\ 
                   max_overdraft = 1)

if result[0] == None or not Helpers.IsOnRightMachine(result[0], is_floating_license= True, allow_overdraft=True):
    # an error occurred or the key is invalid or it cannot be activated
    # (eg. the limit of activated devices was achieved)
    print("The license does not work: {0}".format(result[1]))
else:
    # everything went fine if we are here!
    print("The license is valid!")
```

#### Notes about the code

> **FloatingTimeInterval** specifies how long back in history of activations we should go, in order to decide whether a
machine is still using the software or not (specified in seconds). **MaxOverdraft** specifies how many more devices than
specified in **maximum number of machines** can use the software concurrently.

**Note**, in comparison to node-locking, where key verification for most applications can occur once during startup, floating licensing requires continuous key verifications. The smaller FloatingTimeInterval, the more key verifications have to occur.

> As a rule of thumb, if you plan to verify a license every 10 minutes, the **FloatingTimeInterval** should be no less than 600 (600s = 10min) if you want to ensure that your customers never exceed the **maximum number of machines**. However, if you can allow the customers to exceed this boundary in eg. 5 minutes, you could reduce **FloatingTimeInterval**  to 300 (300s = 5min).

### Tips

#### Releasing a floating license
Normally, floating licenses will automatically be released in a certain period of time (specified by `FloatingTimeInterval`). However, you can manually release a floating license by using [Key.Deactivate](https://help.cryptolens.io/api/dotnet/api/SKM.V3.Methods.Key.html?#SKM_V3_Methods_Key_Deactivate_System_String_SKM_V3_Models_DeactivateModel_) with `Floating=True`, as shown below:

**In C\#**

```cs
var auth = "Access token with permission to access the deactivate method";
Key.Deactivate(activateToken, new DeactivateModel { 
    Key = "GEBNC-WZZJD-VJIHG-GCMVD", 
    ProductId = 3349, 
    MachineCode = Helpers.GetMachineCode(),
    Floating = true // <- add this
});
```

```python
result = Key.deactivate(token="Access token with permission to access the deactivate method",\
                        product_id=3349,\
                        key="ICVLD-VVSZR-ZTICT-YKGXL",\
                        machine_code = Helpers.GetMachineCode(),\
                        floating=True)
```

#### Number of used and free licenses

To get the number of floating licenses left (among other things), you can simply add `Metadata=true`.
A helper method, [GetFloatingLicenseInformation](https://help.cryptolens.io/api/dotnet/api/SKM.V3.Methods.Helpers.html#SKM_V3_Methods_Helpers_GetFloatingLicenseInformation_SKM_V3_Models_ActivateModel_SKM_V3_Models_KeyInfoResult_) can be used to extract this information.

```cs

// we have just separated the initialization of the input parameters for activate
// into a separate variable, since we will need it later on.
var activateModel = new ActivateModel()
{
    Key = licenseKey,
    ProductId = 3349,
    Sign = true,
    MachineCode = Helpers.GetMachineCode(),
    FloatingTimeInterval = 100,      // <- we have added this parameter.
    MaxOverdraft = 1,                // <- we can exceed the max number of machines by one.
    Metadata = true
}

var auth = "Access token with permission to access the activate method";
var result = Key.Activate(token: auth, parameters: activateModel);

// some code here

if (result != null && result.Result == ResultType.Success)
{
    var info = Helpers.GetFloatingLicenseInformation(activateModel, result);

    Console.WriteLine(info.AvailableDevices);
    Console.WriteLine(info.UsedDevices);
    Console.WriteLine(info.OverdraftDevices);
}
```