---
title: Introduction to the basics
author: Artem Los
description: Overview of Cryptolens and explanation of the difference between Cryptolens Platform and Cryptolens Client API.
labelID: basics
---

# Introduction to the basics

<img src="/images/skmoverview3.png" width="80%" />

Before we dig into the specifics, let's look at how Cryptolens works.

## Cryptolens Platform
Cryptolens Platform is accessed through the [cryptolens.io](https://cryptolens.io) website.
This is the central place where you can control all your applications. 
For instance, it enables you to:

* Implement licensing
* Process payments
* Analyse usage data

## Cryptolens Client API
In order to be able to take advantage of all the power of the **Cryptolens Platform**,
you need to include small [library](/web-api/skm-client-api) into your application.
As an example, it enables you to translate your licensing model into simple logic, as shown below:

```cs
if(license.HasFeature(1)
          .HasNotExpired()
          .IsValid())
{
    // do something
}
else
{
    // invalid license.
}
```

## Next

* [Learn about 'products' and 'keys'](/basics/prodkeys)