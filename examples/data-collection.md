---
title: Data collection
author: Artem Los
description: Explains how data is being collected and how additional data can be supplied for better insights
labelID: examples
---

# Data collection

## Idea
By default, Cryptolens [logs](https://app.cryptolens.io/docs/api/v2/WebAPILog) many of the requests made to the Web API. This includes all [data object methods](https://app.cryptolens.io/docs/api/v3/Data) and all [key methods](https://app.cryptolens.io/docs/api/v3/Key) except for [Get Key](https://app.cryptolens.io/docs/api/v3/GetKey). However, these logs contain limited information and may not capture all events that could be useful from an analytics standpoint.

When you deploy your product, it can for instance be useful to know the OS it's running on or what features customers use the most, etc. Although these are useful on their own, when you link them together, you can get even better insights that can aid you in business critical decision making. For example, if you link transaction data with the OS data, you can see which OS brings you most of the revenue. If you see that a certain OS version does not bring in significant revenues, you can use this as basis to stop supporting it.

In Cryptolens, additional data is referred to as **events** and can be gathered using [analytics methods](https://app.cryptolens.io/docs/api/v3/AI) in the Web API. We will cover these in the implementation section.

## Implementation

### In the app
To register an event, you can call the [AI.RegisterEvent](https://help.cryptolens.io/api/dotnet/api/SKM.V3.Methods.AI.html#SKM_V3_Methods_AI_RegisterEvent_System_String_SKM_V3_Models_RegisterEventModel_) method. Most of the parameters are optional, but it's useful to at least supply a `FeatureName` and either `Key` or `MachineCode`. If you just have one product, `ProductId` is not necessary, but it's useful if you have multiple products.

The code snippet below can be used to register event, in our case that the user started `YearReportGenerator` module.

```cs
AI.RegisterEvent("access token with RegisterEvent permission", 
    new RegisterEventModel { EventName = "start", FeatureName = "YearReportGenerator", Key= "AAAA-BBBB-CCCC-DDDD", MachineCode = Helpers.GetMachineCode(), ProductId = 3,
    Metadata = Helpers.GetOSStats() });
```

If you are on a platform that does not support this method, you can always send in data using by calling the URL below:

```
https://app.cryptolens.io/api/ai/RegisterEvent?token=<access token with RegisterEvent permission>&EventName=start&FeatureName=YearReportGenerator&Key=AAAA-BBBB-CCCC-DDDD&ProductId=3&MachineCode=<machine code>&
```


### Integration with payment providers
In order to track transactions, we can use the same [AI.RegisterEvent](https://help.cryptolens.io/api/dotnet/api/SKM.V3.Methods.AI.html#SKM_V3_Methods_AI_RegisterEvent_System_String_SKM_V3_Models_RegisterEventModel_) method, but supply different set of parameters.

To record the value, we can use the parameters `Value` and `Currency`. This can either be accomplished by calling the Web API directly or `AI.RegisterEvent` method (in case your backend is in .NET).

Let's assume your customer has have paid 30 usd for the product. In that case, you can call the following url:

```
https://app.cryptolens.io/api/ai/RegisterEvent?token=<access token with RegisterEvent permission>&Value=30&Currency=USD&ProductId=3&MachineCode=<machine code>&
```
