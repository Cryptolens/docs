---
title: Metered usage
author: Artem Los
description: Describes how you can record usage for subscriptions that support metered usage (in Stripe).
labelID: payment_forms
---

# Metered usage

## Idea

So far we have focused on how to create subscriptions where a constant amount is charged on a recurring basis (eg. $9/month).
Often, however, it is quite useful to be able to charge customers for their actual usage (eg. how many times they have used a certain feature).
This is where metered usage feature can be helpful.

## Implementation

If you have used Cryptolens without the recurring payment module, it was most likely implemented [using data objects](/licensing-models/usage-based).
In the recurring payment module, all usage tracking is taken care of by Stripe, so a different method needs to be used.

### Creating a metered subscription (Stripe)
> Before continuing reading, please go through all the steps in the [setup](/recurring-payments/setup) tutorial. It is assumed that you already have a product in Stripe and that you are familiar with how plans work.

When you create a plan, you can choose between `recurring quantity` or `metered usage`. For this tutorial, the `metered usage` option needs to be selected.

<img src="/images/stripe-metered-setup1.png" width="100%" />

Most of the settings don't need to be changed; we will cover the important ones below:

* **Usage aggregation mode** - the choice depends on how you plan to charge for usage. If you want to charge your customers each time a feature is used (eg. in an accounting software, this could be each time they generate a new tax report), please set it to "sum up usage during period".
* **Currency** - it's important to keep this the same for all customers and plans. Otherwise, Stripe will give an error.
* **Does this pricing plan have multiple price tiers based on quantity** - this option is quite useful if you want to apply a **flat fee** on top of the usage counter. For example, you
can have a recurring plan where you charge $10/month and then add any additional usage to it.

### Record usage
To record usage, we can use [Record Usage](https://app.cryptolens.io/docs/api/v3/RecordUsage) method. It will call [Stripe's metered usage API](https://stripe.com/docs/api/usage_records) and increment the current value with the provided amount. All you need is to create an access token with `Subscription` permission and provide a license key with a product id.

#### In .NET

The [Record Usage method](https://help.cryptolens.io/api/dotnet/api/SKM.V3.Methods.Subscription.html#SKM_V3_Methods_Subscription_RecordUsage_System_String_SKM_V3_Models_RecordUsageModel_) in .NET can be used as shown below:

```cs
var auth = AccessToken.AccessToken.SubscriptionMethods;
var res = Subscription.RecordUsage(auth, new RecordUsageModel { Amount = 1, ProductId = 3349, Key = "CMXKC-GUQRW-EJUGS-RRPUR" });

Assert.IsTrue(res != null && res.Result == ResultType.Success);
```

#### In Java
The [Record Usage method](https://help.cryptolens.io/api/java/io/cryptolens/methods/Subscription.html#RecordUsage-java.lang.String-io.cryptolens.models.RecordUsageModel-) in Java is very similar to .NET:

```java
BasicResult res = Subscription.RecordUsage(APIKey.get("subscriptionmethods"),
        new RecordUsageModel(3349, "CMXKC-GUQRW-EJUGS-RRPUR", 1));

if(!Helpers.IsSuccessful(res)) {
    fail("Could not register usage");
}
```
