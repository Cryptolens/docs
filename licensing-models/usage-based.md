---
title: Usage-based licensing / Pay-per-use
author: Artem Los
description: Explains how you can set up implement usage-based licensing / pay per use / pay per click
labelID: licensing_models
---

# Usage-based licensing (pay per use)

## Idea

Usage-based licensing is when you charge for usage of specific features. For example, if you have an accounting software, you can charge per
created yearly report. If you have a movie editing software, you can charge per created movie (or for each conversion to a different movie format).

The point is to allow a larger group of people to be able to use your software. For example, let's return to the movie editing software. There can be two groups of users: those that will use the software a lot (eg. professional use), in which case they will prefer a subscription or a perpetual license. Another group can have movie editing as a hobby, in which case they may create very few movies, so a subscription may be too expensive.

> By supporting usage-based licenses, we can monetize a group of users that would otherwise not have purchased the product (eg. because it is too expensive).

Example project can be found [here](https://github.com/Cryptolens/Examples/tree/master/usage-based/java).

> **Note:** If some of your clients will be offline, the usage information can still be tracked using our license server, as described [here](https://github.com/Cryptolens/license-server#usage-based-licensing-offline).

## Implementation

We can implement usage based licensing using [data objects (aka custom variables)](https://app.cryptolens.io/docs/api/v3/Data). An advantage of using them is that they allow us to increment and decrement them atomically, which means that the counter will always reflect the actual usage of a specific feature.

We can implement this in two ways; the choice of which depends on if we want to bill users upfront (in advance) or in the end of the billing cycle (based on the actual usage).

* **Paying upfront**: If you want to bill users upfront, one strategy is to treat the value of a data object as a credit, i.e. users can buy a certain amount of credits, eg. 1000, which will allow them to use a specific feature 1000 times before they have to refill. That is, we will decrement this value each time the feature is used.
* **Paying based on actual usage**: When charging based on actual usage, one way is to set the data object to zero and keep incrementing it as a feature is being used. In the end of the month, we record the usage and either reset it or keep the value (if we keep it, we need to know it next month so that we don't charge customers twice).

### Creating a data object

In order to associate a data object with a license key, you can use the code below (in C#):

```cs
// note, if we ran Key Verification earlier, we can can set Key=result.LicenseKey.KeyString

var parameters = new AddDataObjectToKeyModel() 
{
    Name="usagecount" ProductId = 3941, 
    Key = "FRQHQ-FSOSD-BWOPU-KJOWF", 
    IntValue=0 
};

var result = Data.AddDataObject("access token with AddDataObject permission (and keylock set to '-1' for improved security)", parameters);
```

In Java, it's quite similar:

```java
String auth = "access token with AddDataObject permission (and keylock set to '-1' for improved security)";
BasicResult addResult = Data.AddDataObject(auth,
        new AddDataObjectToKeyModel(
                3941,
                "FRQHQ-FSOSD-BWOPU-KJOWF",
                "usagecount",
                0,
                ""));

if (!Helpers.IsSuccessful(addResult)) {
    System.out.println("Could not add a new data object. Maybe the limit was reached?");
}
```

In curl, this can be accomplished as shown below:
```
curl https://app.cryptolens.io/api/data/AddDataObjectToKey? \
    -d token=access token with AddDataObject permission \
    -d Name=usagecount \
    -d ProductId=3941 \
    -d Key=FRQHQ-FSOSD-BWOPU-KJOWF \
    -d IntValue=0
```

If you are using [Payment Forms](/payment-form/index), you can create a data object by sending a GET request our Web API (shown below). More information about the parameters can be found [here](https://app.cryptolens.io/docs/api/v3/AddDataObject).

```
https://app.cryptolens.io/api/data/AddDataObjectToKey?token=accesstoken&name=usagecount&ProductId=3941&Key=FRQHQ-FSOSD-BWOPU-KJOWF&IntValue=0
```

#### IntValue
If you plan to charge people upfront, you can set the `IntValue` to eg. 1000 and if you charge in the end of the billing cycle based on actual usage, you can set it to 0.

### Updating the usage counter

There are three ways to update the counter stored in `IntValue`: using `IncrementIntValue`, `DecrementIntValue` or `SetIntValue`. In your software, you should either use `IncrementIntValue` or `DecrementIntValue` (remember that you need to create a specific access token that permits use of one of the methods). In other words, you only want the client software to update the counter in one direction only. `SetIntValue` allows you to assign an arbitrary value, so it should only be used on the server side (where you have control).

Note that `IncrementIntValue` or `DecrementIntValue` methods allow you to specify an upper or lower bound, which is especially useful if you have a **pay upfront** model. For example, if you give your users a certain credit, eg. 1000, you can then specify a lower bound to be 0, so that once it is reached, they won't be able to use that particular feature.

#### Paying based on actual usage
If we simply want to keep track of the number of times a feature was used (and bill our customers in the end of the month), we can use the following code:

```cs
var auth = "Access token with AddDataObject, ListDataObject and IncrementIntValue permission. Please also set KeyLock value to '-1'";
var licenseKey = "LZKZU-MPJEW-TARNP-UHDBQ";

var result = Data.ListDataObjects(auth, new ListDataObjectsToKeyModel 
{
    Contains = "usagecount",
    Key = licenseKey,
    ProductId = 3349 
});

var obj = result.DataObjects.Get("usagecount");

if (obj == null)
{
    // make sure to create it in case it does not exist.
    Data.AddDataObject(auth, new AddDataObjectToKeyModel { Key = licenseKey, ProductId = 3349, Name = "usagecount", IntValue = 1 });

    if(res == null || res.Result == ResultType.Error)
    {
        Console.WriteLine("Could not create new data object. Terminate." + res.Message);
    }
}
else
{
    var res = obj.IncrementIntValue(auth, 1, licenseKey: new LicenseKey { Key = licenseKey, ProductId = 3349 });

    if (res == false) 
    {
        Console.WriteLine("We could not update the data object. Terminate.");
    }
}
```

```java
// note, if we ran Key Verification earlier, we can can set Key=license.KeyString
String auth = "Access token with AddDataObject, ListDataObject and IncrementIntValue permission. Please also set KeyLock value to '-1'";

ListOfDataObjectsResult listResult = Data.ListDataObjects(auth, new ListDataObjectsToKeyModel(3941, "FRQHQ-FSOSD-BWOPU-KJOWF", "usagecount"));

if (!Helpers.IsSuccessful(listResult)) {
    System.out.println("Could not list the data objects.");
}

if (listResult.DataObjects.size() == 0) {
    BasicResult addResult = Data.AddDataObject(auth,
            new AddDataObjectToKeyModel(
                    3941,
                    "FRQHQ-FSOSD-BWOPU-KJOWF",
                    "usagecount",
                    0,
                    ""));
    if (!Helpers.IsSuccessful(addResult)) {
        System.out.println("Could not add a new data object. Maybe the limit was reached?");
    }
} else {
    // if you set enableBound=true and bound=50 (as an example)
    // it won't be possible to increase to a value greater than 50.
    BasicResult incrementResult = Data.IncrementIntValue(auth,
            new IncrementIntValueToKeyModel(
                    3941,
                    "FRQHQ-FSOSD-BWOPU-KJOWF",
                    listResult.DataObjects.get(0).Id,
                    1,
                    false,
                    0));
    if(!Helpers.IsSuccessful(incrementResult)) {
        System.out.println("Could not increment the data object");
    }
}
```

The idea is to either create a new data object (if such does not exist) or increment and existing one.

#### Paying upfront

If you instead want your customers to pay upfront, eg. buy the "usage credits" in advance, we can use the code below. We assume that a data object with name 
**usagecount** already exists.

If the value gets below zero, we will not permit the customers to continue using that feature.

```cs

var auth = "Access token with AddDataObject, ListDataObject and IncrementIntValue permission. Please also set KeyLock value to '-1'";
var licenseKey = "LZKZU-MPJEW-TARNP-UHDBQ";

var result = Data.ListDataObjects(auth, new ListDataObjectsToKeyModel { Contains = "usagecount", Key = licenseKey, ProductId = 3349 });
var obj = result.DataObjects.Get("usagecount");

var res = obj.DecrementIntValue(auth, decrementValue: 1, enableBound:true, lowerBound: 0, licenseKey: new LicenseKey { Key = licenseKey, ProductId = 3349 });

if (!res)
{
    Console.WriteLine("Could not decrement the data object. The limit was reached.");
}

```

```java
String auth = "Access token with AddDataObject, ListDataObject and IncrementIntValue permission. Please also set KeyLock value to '-1'";

ListOfDataObjectsResult listResult = Data.ListDataObjects(auth, new ListDataObjectsToKeyModel(3941, "FRQHQ-FSOSD-BWOPU-KJOWF", "usagecount"));

if (!Helpers.IsSuccessful(listResult)) {
    System.out.println("Could not list the data objects.");
}

BasicResult decrementResult = Data.DecrementIntValue(auth,
        new DecrementIntValueToKeyModel(
                3941,
                "FRQHQ-FSOSD-BWOPU-KJOWF",
                listResult.DataObjects.get(0).Id,
                1,
                true,
                0));

if(!Helpers.IsSuccessful(decrementResult)) {
    System.out.println("Could not decrement the data object");
}
```

### Security note

You may have noticed that we recommend to set **KeyLock to '-1'** when creating access tokens that will be used to work with data objects.
The reason for that is to ensure that a license key and a product id are required to perform operations on data objects. If we don't set the key lock value,
it is possible to use the same access token to modify data objects of other customers.
