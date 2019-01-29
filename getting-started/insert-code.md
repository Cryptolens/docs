---
title: Adding the code snippet
author: Artem Los
description: The code that should be inserted into your application.
labelID: getting_started
---

# Adding Code
We almost have all the pieces in place to get a working licensing solution.
We will cover the code in this section and look at how to retrieve **access tokens** and **public key (RSA)** later.

Our application will have two places where we will need to insert the code:
* During license key registration - the first time the user inserts a license key
* During application start - to check for existing license on launch

You can download the source code <a href="https://github.com/Cryptolens/Examples/tree/master/Digital%20Tools%20Collection" target="_blank">here</a>.

> Note, you can find more detailed examples of .NET [here](/examples/) and other environments [here](/web-api/skm-client-api). Please don't hesitate to contact us should you have any questions.

## License Key Registration

```vb
' The code below is based on https://help.cryptolens.io/examples/key-verification.

Dim token = "WyIyNjAxIiwiL0ZYYndDTm1jTlJNdGRPeDFNS29iNnlaQSs1NTVkZHcyREVWZXFDdyJd"
Dim RSAPubKey = "<RSAKeyValue><Modulus>sGbvxwdlDbqFXOMlVUnAF5ew0t0WpPW7rFpI5jHQOFkht/326dvh7t74RYeMpjy357NljouhpTLA3a6idnn4j6c3jmPWBkjZndGsPL4Bqm+fwE48nKpGPjkj4q/yzT4tHXBTyvaBjA8bVoCTnu+LiC4XEaLZRThGzIn5KQXKCigg6tQRy0GXE13XYFVz/x1mjFbT9/7dS8p85n8BuwlY5JvuBIQkKhuCNFfrUxBWyu87CFnXWjIupCD2VO/GbxaCvzrRjLZjAngLCMtZbYBALksqGPgTUN7ZM24XbPWyLtKPaXF2i4XRR9u6eTj5BfnLbKAU5PIVfjIS+vNYYogteQ==</Modulus><Exponent>AQAB</Exponent></RSAKeyValue>"
Dim keyStr = TextBox1.Text.Replace(" ", "")

Dim result = Key.Activate(token:=token, parameters:=New ActivateModel() With {
                  .Key = keyStr,
                  .ProductId = 3349,
                  .Sign = True,
                  .MachineCode = Helpers.GetMachineCode()
                  })

If result Is Nothing OrElse result.Result = ResultType.[Error] OrElse
    Not result.LicenseKey.HasValidSignature(RSAPubKey).IsValid Then
    ' an error occurred or the key is invalid or it cannot be activated
    ' (eg. the limit of activated devices was achieved)
    MsgBox("Unable to access the license server or the key is wrong.")

Else
    ' everything went fine if we are here!

    Dim license = result.LicenseKey

    Form1.Button1.Enabled = license.HasFeature(1).IsValid() ' either we have feature1 or not.
    Form1.Button2.Enabled = license.HasFeature(2).IsValid() ' either we have feature2 or not.
    Form1.Button4.Enabled = license.HasFeature(3).IsValid() ' either we have feature3 or not.

    Form1.Text = "Digital Tools"

    If license.HasFeature(4).HasNotExpired().IsValid() Then
        ' feature 1 is a time limited, so we check that it has not expired.
        Form1.Text = "Digital Tools - " + license.DaysLeft().ToString() + " day(s) left"
    ElseIf license.HasNotFeature(4).IsValid() Then

    Else
        MsgBox("Your license has expired and cannot be used.")
        nolicense()

    End If

    license.SaveToFile()

End If
```

This code will retrieve all the license data (using `Refresh`) and enable buttons depending on the features in the license.
Features 3-5 indicate a certain functionality whereas Feature 1 is used to indicate a time-limit (in case it's a subscription).
Once we have a valid license, we will save it locally (it will be signed by SKM, because **sign** is set to true in `Refresh`).
 
## Application Start
 ```vb
 NoLicense()

' The code below is based on https://help.cryptolens.io/examples/key-verification.

Dim license = New LicenseKey().LoadFromFile()
Dim publicKey = "<RSAKeyValue><Modulus>sGbvxwdlDbqFXOMlVUnAF5ew0t0WpPW7rFpI5jHQOFkht/326dvh7t74RYeMpjy357NljouhpTLA3a6idnn4j6c3jmPWBkjZndGsPL4Bqm+fwE48nKpGPjkj4q/yzT4tHXBTyvaBjA8bVoCTnu+LiC4XEaLZRThGzIn5KQXKCigg6tQRy0GXE13XYFVz/x1mjFbT9/7dS8p85n8BuwlY5JvuBIQkKhuCNFfrUxBWyu87CFnXWjIupCD2VO/GbxaCvzrRjLZjAngLCMtZbYBALksqGPgTUN7ZM24XbPWyLtKPaXF2i4XRR9u6eTj5BfnLbKAU5PIVfjIS+vNYYogteQ==</Modulus><Exponent>AQAB</Exponent></RSAKeyValue>"
Dim token = "WyIyNjAxIiwiL0ZYYndDTm1jTlJNdGRPeDFNS29iNnlaQSs1NTVkZHcyREVWZXFDdyJd"

If license IsNot Nothing Then

    Dim result = Key.Activate(token:=token, parameters:=New ActivateModel() With {
                  .Key = license.Key,
                  .ProductId = 3349,
                  .Sign = True,
                  .MachineCode = Helpers.GetMachineCode()
                  })

    ' either we get a fresh copy of the license or we use the existing one (given it is no more than 90 days old)
    If (result.Result <> ResultType.Error And result.LicenseKey.HasValidSignature(publicKey).IsValid) Or license.HasValidSignature(publicKey, 90).IsValid() Then

        Button1.Enabled = license.HasFeature(1).IsValid() ' either we have feature1 or not.
        Button2.Enabled = license.HasFeature(2).IsValid() ' either we have feature2 or not.
        Button4.Enabled = license.HasFeature(3).IsValid() ' either we have feature3 or not.

        Text = "Digital Tools"

        If license.HasFeature(4).HasNotExpired().IsValid() Then
            ' feature 1 is a time limited, so we check that it has not expired.
            Text = "Digital Tools - " + license.DaysLeft().ToString() + " day(s) left"
        ElseIf license.HasNotFeature(4).IsValid() Then
            ' not time limited.

        Else
            MsgBox("Your license has expired and cannot be used.")
            NoLicense()

        End If

        license.SaveToFile()

    Else
        MsgBox("Your license has expired and cannot be used.")
    End If

Else
    ' no license found. you could tell the user to provide a license key.
End If
 ```
When your application launches, we will try to get a new version of the license key, in case it has been modified.
If we don't have internet access, we will use the local copy as long as 90 days have not passed since it was retrieved.
Everything else is the same as when the user registers the license for the first time.

## Next

* [Create a new access token](/getting-started/access-token)