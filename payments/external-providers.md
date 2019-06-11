---
title: Integrating with external payment providers
author: Artem Los
description: Explains how you can use other payment systems to process payments
labelID: payment_forms
---

# Integrating with external payment providers

## Introduction
If you would like to process payments with a payment gateway that Cryptolens does not support out of the box, you can
use several Web API methods to your help. In most cases, you only need to use [license key related](https://app.cryptolens.io/docs/api/v3/Key) methods
of our Web API, in particular:

* [Create Key](https://app.cryptolens.io/docs/api/v3/CreateKey) - to issue a new license key
* [Extend License](https://app.cryptolens.io/docs/api/v3/ExtendLicense) - to prolong an existing license (eg. when a subscription is renewed)
* [Block Key](https://app.cryptolens.io/docs/api/v3/BlockKey) - block/delete a license (eg. if an invoice was not paid) 
* [Add Feature](https://app.cryptolens.io/docs/api/v3/AddFeature) - add more features to a license (eg. upgrade to new pricing tier)
* [Machine Lock Limit](https://app.cryptolens.io/docs/api/v3/MachineLockLimit) - change how many devices can use the license (eg. to allow activating on additional devices)

Typically, payment gateways will allow you to call an external URL upon a successful transaction. For example, if you use [SendOwl](https://cryptolens.io/integrations/sendowl/) or [DPD](https://cryptolens.io/integrations/dpd-with-software-licensing/), there is a way to provide a **license url** which is called when a payment is successful and the license key is then displayed to the customer. If you build the system yourself, you can either use our client APIs to the methods above or call the URL yourself.

## Implementation

### Pre-paid payments
Pre-paid payments are the easiest to implement. You can both sell your software on a perpetual basis (i.e. one-time payment) or as a subscription (e.g. customers need to obtain a new license every year).

#### One-time payments
If you sell licenses that should be valid in perpetuity, you only need to call the [Create Key](https://app.cryptolens.io/docs/api/v3/CreateKey) method when a transaction succeeds.

#### Subscriptions
If you sell a subscription, you can setting it up in to ways:

1. Issue a new license with [Create Key](https://app.cryptolens.io/docs/api/v3/CreateKey) each time the customer prolongs their subscription
2. Call [Create Key](https://app.cryptolens.io/docs/api/v3/CreateKey) the first time and later [Extend License](https://app.cryptolens.io/docs/api/v3/ExtendLicense).

### Recurring payments

#### Idea
If you either integrate with a platform that supports recurring payments (i.e. charges occur automatically every month) or build it yourself, we can use a similar approach as for subscriptions described earlier. When a subscription is set up, you can call [Create Key](https://app.cryptolens.io/docs/api/v3/CreateKey) and then [Extend License](https://app.cryptolens.io/docs/api/v3/ExtendLicense) each time the charge is successful. It is a good practice to add several days, i.e. if the billing period is 30 days, it better to extend the license for eg. 35 days to account for payment issues. When a subscription should be cancelled, you can call [Block Key](https://app.cryptolens.io/docs/api/v3/BlockKey) method.

#### Cryptolens backend with Stripe
[Recurring billing module](/recurring-payments/index) already offers a backend that communicates with Stripe to ensure that licenses are prolonged if a payment is received and blocked if a subscription is cancelled. If you do not want to use Cryptolens GUI, you can still take an advantage of the backend. More information can be found [here](/recurring-payments/custom-interface).
