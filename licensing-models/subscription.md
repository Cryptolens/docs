---
title: SaaS licensing model
author: Artem Los
description: Explains how you can set up recurring payments for your application, aka SaaS model. This can also be used to keep track of potential support/maintenance subscriptions.
labelID: licensing_models
---

# Subscription licensing (SaaS)

## Idea

Instead of selling your software as a [one-time purchase](/licensing-models/perpetual), you can instead collect monthly or early payments from your customers.

There are at least three ways to set up a licensing model that supports some form of recurring payments. You can require an active subscription to: 

* Use the product (or some of its features) 
* Get updates, but allow access to older versions of the product after the expiration date (similar to [one-time purchase model](/licensing-models/perpetual)) 
* Receive support, but still allow access to the product. You can combine this with the "updates constraint" above.

Both of these models depend on the `expiration date` property that each license key has. We will cover the details of how this is implemented in the next section.


## Implementation

### In the dashboard

The expiration date of a license key can be controlled by clicking on the desired license on the product page. A box will popup, similar to the one below.

<img src="/images/extend-license-key.png">

By adding a certain amount of days to a license key, Cryptolens will automatically determine the expiration date, in order to ensure that the license key can be used for at least the number of days that was specified or longer (if the user has some days left before expiration).

> **Note**, if the textbox in the picture above does not show up, a quick solution is to click on **Edit Feature Names** on the product page and tick **Treat all licenses as time-limited**. You can read more about feature definitions [here](/web-interface/feature-definitions).

### In your application

In the introduction, we mentioned three ideas to subscription based licensing model can be used: by constraining access to the entire product, newer versions of it and/or support. 

#### Require subscription to access the product
This is quite simple to restrict access to entire product if the license key has expired. In the code below, we assume that you have read the [key verification tutorial](/examples/key-verification). The only change is to add `HasNotExpired()` in the if-statement below:

##### In C#
```cs
// notice that we have added ".HasNotExpired()".

if (result == null || result.Result == ResultType.Error ||
    !result.LicenseKey.HasValidSignature(RSAPubKey)
           .HasNotExpired().IsValid())
{
    // an error occurred or the key is invalid or it cannot be activated
    // (eg. the limit of activated devices was achieved)
    Console.WriteLine("The license does not work.");
}
else
{
    // everything went fine if we are here!
    Console.WriteLine("The license is valid!");
}
```

##### In VB.NET
```vb
' notice that we have added ".HasNotExpired()".

If result Is Nothing OrElse result.Result = ResultType.[Error] OrElse
    Not result.LicenseKey.HasValidSignature(RSAPubKey).HasNotExpired().IsValid Then
    ' an error occurred or the key is invalid or it cannot be activated
    ' (eg. the limit of activated devices was achieved)
    Console.WriteLine("The license does not work.")

Else
    ' everything went fine if we are here!
    Console.WriteLine("The license is valid!")
End If
```

If you use a specific feature to denote time-limited licenses, you can similarly add `HasFeature(<feature number>)` in the if-statement.

#### Require subscription to access updates
In this scenario, instead of restricting access to all versions of the product, we allow users to keep using the latest major version before they stopped paying for updates.

<img src="/images/ProductUpdatesSaaS.png" width="100%" alt="Illustrates a timeline where major releases are 3 times a year."/>

In the image above, we assume major releases (eg. v1.0 and v2.0) are released three times a year. Minor releases (eg. v1.1 and v1.2) are released more frequently. Let us further assume that users pay on a monthly basis.

The idea is as follows: if the user stops extending their subscription before the end of May, they will only be able to use **v1.\***. If they stop in June, they will be able to use **v2.\***, and so for.

We can express this in the code below. It assumes you use the code-snippet from the [key verification tutorial](/examples/key-verification). The code below is put inside the else-statement.

```cs

// ..
// everything went fine if we are here!
Console.WriteLine("The license is valid!");

// we need to add this extra code inside the else-statement

string currentApplicationVersion = "1";

if (license.HasNotExpired().IsValid())
{
     // the SLA is still valid so we proceed as usual
}
else if (license.HasExpired().IsValid())
{
    // if the SLA has expired, we want to check the notes field 
    // to see which version they bought the last time
    if (license.notes >= currentApplicationVersion) 
    {
        // proceed as usual
    }
    else 
    {
        // close the application
    }
}
```

> Remember, in addition to extending the duration of the license key, we need update the notes field with the last major release number. 

#### Require subscription to receive support
If you plan to only use subscriptions to keep track of who is eligible for support, no additional code is requires besides what is presented in the [key verification tutorial](/examples/key-verification).

### Integration with other services
The key to get subscriptions up and running is the [ExtendLicense](https://app.cryptolens.io/docs/api/v3/ExtendLicense) method in the Web API. It allows you to extend the expiration date of a license key by a certain number of days. For example, let's assume the users pays for the subscription on a monthly basis. Once a payment is successful, we can extend the duration of the license by sending a GET request to as shown below:

```
https://app.cryptolens.io/api/key/ExtendLicense?ProductId=123&Key=FRQHQ-FSOSD-BWOPU-KJOWF&Days=30
```

> Although we extending the duration of the license key by **30** days (for a monthly subscription), it is better to add a few extra days in case there are issues with the payment, in order to avoid disruptions.

#### Tips when integrating with Stripe
If you use subscriptions in Stripe, there are two useful events that you can use to monitor subscriptions (when using Webhooks): 

* `invoice.payment_succeeded` - when this event is received, [ExtendLicense](https://app.cryptolens.io/docs/api/v3/ExtendLicense) method should be called for the license that the subscription belongs to.
* `customer.subscription.deleted` - when this event is received, [BlockKey](https://app.cryptolens.io/docs/api/v3/BlockKey) should be called to make sure the license key can no longer be used.

> An implementation that supports recurring payments will be released in the coming weeks. In meantime, please reach out to us should you have any questions!

