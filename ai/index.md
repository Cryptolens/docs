---
title: Price Optimization
author: Artem Los
description: Getting started with price optimization
labelID: ai_module
---

# Price Optimization

> Price optimization currently has an active beta programme that you can join by emailing [support@cryptolens.io](mailto:support@cryptolens.io).

## Idea

It is often challenging to price a product optimally.

A good example that demonstrates this is when you set the price for accounting software. Imagine you have two groups of customers: those that use the software regularly in their profession (eg. accountants helping other companies) and those that only use it once a month (eg. small businesses). It makes sense to have different pricing models for these two groups: professionals can be charged monthly (and are very likely to pay more) and the smaller business can pay per usage (eg. per generated monthly report).

As a result, software providers are able to increase their revenues and capture both customer groups by taking into account the true value the software has for each group, and adjusting the pricing model to meet the needs for each group. Please read more in the [announcement](https://cryptolens.io/2018/08/new-ai-feature-helps-optimize-software-pricing/) on our blog.

### How Cryptolens Pricing Engine works

Cryptolens new AI pricing optimization feature works by combining usage data of each feature in your app with the amount payed by each customer. This will then be used to find an optimal usage-based pricing model.

## Integration

### In the app
To register an event, you can call the `AI.RegisterEvent` method. Most of the parameters are optional, but it's useful to at least supply a `FeatureName` and either `Key` or `MachineCode`. If you just have one product, `ProductId` is not necessary, but it's useful if you have multiple products.

Cryptolens always needs to be able to link usage data to a unique user to give better results. The code snippet below can be used to register event, in our case that the user started `YearReportGenerator` module.

```cs
AI.RegisterEvent("access token with RegisterEvent permission", 
    new RegisterEventModel { EventName = "start", FeatureName = "YearReportGenerator", Key= "AAAA-BBBB-CCCC-DDDD", MachineCode = Helpers.GetMachineCode(), ProductId = 3 });
```

### Integration with payment providers
The second key part of the AI pricing optimization feature is to link successful transactions to usage-data. This can either be accomplished by calling the Web API directly or `AI.RegisterEvent` method (in case your backend is in .NET).

Let's assume they have paid 30 usd for the product. In that case, you can call the following url:

```
https://app.cryptolens.io/api/ai/RegisterEvent?token=<access token with RegisterEvent permission>&Value=30&Currency=USD&ProductId=3&MachineCode=<machine code>&
```
