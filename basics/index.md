---
title: Introduction to the basics
author: Artem Los
description: Overview of Cryptolens and explanation of the difference between Cryptolens Platform and Cryptolens Client API.
labelID: basics
---

# Basics of Cryptolens

<img src="/images/cryptolens-payments-overview.png" width="50%" />

Cryptolens is a software licensing platform that does the following:

* **Licensing**: keeping track of all instances of your software. For example, you can restrict which features a customer should have access to, on how many devices they should
be able to run the application, keep track usage of individual features as well as ensure that a license stops working after the expiration date.
* **Payment processing**: automating selling of your software, either through Cryptolens (supports Stripe & PayPal) or by integrating it with external payment
providers (e.g. FastSpring or your own system).
* **Analytics of usage data**: Analyse the usage of your software globally or on user level.


## Cryptolens Platform
Cryptolens platform is accessed through the [app.cryptolens.io](https://app.cryptolens.io).
This is the central place where you can control all your applications. It allows you to:

* Implement licensing
* Process payments
* Analyse usage

## Cryptolens Client API
To make sure that your applications can communicate with our server, you can use one of our [client libraries]((/web-api/skm-client-api)).
A client library provides an implementation of all the methods of the Web API as well as several other methods that
are useful. A client library is not necessary to be able to verify a license, but it makes it easier.

By adding a small code snippet (read more [here](/examples/key-verification)), it enables you to translate your licensing model into simple logic, as shown below:

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