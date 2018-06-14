---
title: Offline Verification
author: Artem Los
description: An example of how to verify licenses without access to the internet.
labelID: examples
---

# Offline Verification

Although Cryptolens is a web-based service, we can still use it to protect applications that have no direct access to the internet (eg. air-gapped devices).
This is especially useful if you plan to sell your software to larger enterprises.

## Idea

To verify licenses without internet access, Cryptolens uses public-key cryptography to sign each response that is sent to your application. You can think of each of these responses as a
**certificate** or **license file**.

> The reason why we had to include your RSA public key in the [Key Verification](/examples/key-verification) tutorial was to verify the signature provided by Cryptolens.

So, to be able to verify licenses offline, all you have to do is to provide your customers with this response/certificate. There are many different ways of setting this up, which we have outlined below:

* **Key verification once or periodically** - the easiest way of delivering this **certificate** is to require your users to perform [verification](/examples/key-verification) once (and then keep using a cached version of the certificate) or to [verify](/examples/key-verification) the license key periodically (eg. you try to verify the license each time the application starts and if it fails you use the cached certificate, provided it has not expired).

* **Using USB stick** - if your customers want to use your application on an [air-gapped](https://en.wikipedia.org/wiki/Air_gap_(networking)) device, you can deliver the certificate on a USB stick or by other means. Once your software sees this file, it will know that it is a valid license.

## Implementation
We have outlined two examples with detailed code examples:

* [Periodic key verification](#periodic-key-verification)
* [USB stick (air-gapped)](#usb-stick-air-gapped)

### Periodic Key Verification
If you have already used the code in the [Key Verification](/examples/key-verification) tutorial, you only need to add few lines of code to get it to work.
The additional code that has to be added is shown below (we will explain how it works later in the tutorial):

#### In C\#
```cs

// ...

if (result == null || result.Result == ResultType.Error ||
    !result.LicenseKey.HasValidSignature(RSAPubKey).IsValid())
{
    // an error occurred or the key is invalid or it cannot be activated
    // (eg. the limit of activated devices was achieved)

    // -------------------new code starts -------------------
    // we will try to check for a cached response/certificate

    var licensefile = new LicenseKey();

    if (licensefile.LoadFromFile("licensefile")
                  .HasValidSignature(RSAPubKey, 3)
                  .IsValid())
    {
        Console.WriteLine("The license is valid!");
    }
    else
    {
        Console.WriteLine("The license does not work.");
    }
    // -------------------new code ends ---------------------
}
else
{
    // everything went fine if we are here!
    Console.WriteLine("The license is valid!");

    // -------------------new code starts -------------------
    // saving a copy of the response/certificate
    result.LicenseKey.SaveToFile("licensefile");
    // -------------------new code ends ---------------------
}
```

#### In VB.NET
```vb

' ...

If result Is Nothing OrElse result.Result = ResultType.[Error] OrElse
    Not result.LicenseKey.HasValidSignature(RSAPubKey).IsValid Then
    ' an error occurred or the key is invalid or it cannot be activated
    ' (eg. the limit of activated devices was achieved)

    ' -------------------New code starts -------------------
    ' we will try to check for a cached response/certificate

    Dim licensefile As New LicenseKey()

    If licensefile.LoadFromFile("licensefile").HasValidSignature(RSAPubKey, 3).IsValid() Then
        Console.WriteLine("The license is valid!")
    Else
        Console.WriteLine("The license does not work.")
    End If
    ' -------------------new code ends ---------------------
Else
    ' everything went fine if we are here!
    Console.WriteLine("The license is valid!")

    ' -------------------New code starts -------------------
    ' saving a copy of the response/certificate
    result.LicenseKey.SaveToFile("licensefile")
    ' -------------------New code ends ---------------------
End If
```
#### How it works
The code examples above will always try to access Cryptolens to check a license key. If a request to Cryptolens would fail, we check for an **cached** version of the response/certificate and that it has ***not expired** (eg. the last successful access was at most 3 days ago).

#### Remarks
The examples use methods `SaveToFile`and `LoadFromFile`, which are useful abstractions so that you do need to worry about serialization and storage. In some cases, it can be useful to store the `LicenseKey` in some other way. In that case, you just to need serialize the `LicenseKey` object. You can also use [settings variables](/web-api/dotnet/v401#storing-a-license-key-in-a-file). 

> Note, you can perform the verification once with Cryptolens and then keep using the cached license file. We don't recommend this though. It's better to have a longer expiration date on the cached certificates instead.

### USB stick (air-gapped)

In order to check a license key on an [air-gapped device](https://en.wikipedia.org/wiki/Air_gap_(networking)), we need to first load the certificate and then verify it.

> Note, on an air-gapped device, it's important to check that the certificate is locked to to that device (aka machine code locking), which is why we have included `IsOnRightMachine()` call.

For the code below, we need to ensure that:
* **machine code locking** is enabled. You can change this for existing keys by selecting the key and providing the value in the box below (remember to save):
![](/images/machine-code-locking.png)

* **activation forms** or another way of obtaining the certificate is present. Activation Forms are easy to set up and can be done [here](https://app.cryptolens.io/ActivationForms). All your customer has to do is to enter the license key and machine code in the form and then move the file into the same directory as your application. 

> Note, the name of the files returned by the activation form can differ, so it's better to ask the customer about the name of it or ask them to rename the file obtained from the activation  with the one hardcoded in the code below:

#### In C\#
```cs
var RSAPubKey = "{enter the RSA Public key here}";

var licensefile = new LicenseKey().LoadFromFile("ActivationFile20180606.skm");

if(licensefile.HasValidSignature(RSAPubKey, 365).IsOnRightMachine(SKGL.SKM.getSHA256).IsValid())
{
    // if you have multiple products, make sure the license file has correct product id.
    
    //if(licensefile.ProductId != 123)
    //{
    //    Console.WriteLine("This license file is not for this product.");
    //    return;
    //}

    Console.WriteLine("License verification successful.");
}
else
{
    Console.WriteLine("The license file is not valid or has expired.");
    Console.WriteLine("Please obtain a new one here: https://app.cryptolens.io/Form/A/onp4cDAc/222");
    Console.WriteLine("Your machine code is: " + Helpers.GetMachineCode());
}
```

#### In VB.NET
```vb
Dim RSAPubKey = "{enter the RSA Public key here}"

Dim licensefile = New LicenseKey().LoadFromFile("ActivationFile20180606.skm")

If (licensefile.HasValidSignature(RSAPubKey, 365).IsOnRightMachine(AddressOf SKGL.SKM.getSHA256).IsValid()) Then

    ' if you have multiple products, make sure the license file has correct product id.
    'If (licensefile.ProductId <> 123) Then
    '    Console.WriteLine("This license file is not for this product.")
    '    Return
    'End If

    Console.WriteLine("License verification successful.")
Else
    Console.WriteLine("The license file is not valid or has expired.")
    Console.WriteLine("Please obtain a new one here: https://app.cryptolens.io/Form/A/onp4cDAc/222")
    Console.WriteLine("Your machine code is: " + Helpers.GetMachineCode())
End If
```