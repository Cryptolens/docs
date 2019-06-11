---
title: Payment Integration
author: Artem Los
description: Introduction to payments
labelID: payment_forms
---

# Payment Integration

## Introduction

Cryptolens core functionality has always been the software licensing API, which keeps track of what features your customers are entitled to, how long they can use your software, etc. Since Cryptolens is a cloud-based application, you can easily integrate it with other services, including those that process payments.

There are three ways to integrate payments with Cryptolens: [calling our API](/payments/external-providers) manually, using [payment forms](/payment-form/index) or the new [recurring payments](/recurring-payments/index).

![](/images/cryptolens-payments-overview.png)

### Calling the API
If you would like to integrate payments with a platform that does not have native support in Cryptolens, you can easily do that using the Web API. Please check out the [following page](https://app.cryptolens.io/docs/api/v3/key) to see what methods can be called to change a license.

If you sell your application with a one-time fee, you only need to call the [Create Key](https://app.cryptolens.io/docs/api/v3/CreateKey) method. Our integrations with both [SendOwl](https://cryptolens.io/integrations/sendowl/) and [GetDPD](https://cryptolens.io/integrations/dpd-with-software-licensing/) use the same approach.

### Payment Forms
[Payment Forms](/payment-forms/index) are an easy way to get started with payments, particularly useful for selling products on a [perpetual](/licensing-models/perpetual) basis or with manual upgrades on a recurring basis. They are built-in into Cryptolens and support both Stripe and Payment for payment processing.

### Recurring Payments (Stripe)
The aim of the recurring payment feature is to make [subscriptions](/licensing-models/subscription) more seamless by combining Cryptolens license engine with Stripe. Your customers provide a card once and after that payments will occur on a regular basis, without them having to constantly remember to upgrade.