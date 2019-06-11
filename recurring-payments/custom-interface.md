---
title: Custom interface
author: Artem Los
description: Using custom interface to subscribe and unsubscribe customers while using Cryptolens Stripe webhook endpoint
labelID: payment_forms
---

# Custom interface

## Idea

[Recurring billing module](/recurring-payments/index) consists of two parts: 
* an interface that allows your customers to manage subscriptions.
* a Stripe webhook endpoint that ensures that subscriptions in Stripe stay in sync with the licenses in Cryptolens.

If you want to manage subscriptions through your own interface, you can still take advantage of our webhook endpoint.

## Requirements
The webhook endpoint only requires that each subscription in Stripe has a metadata field `skm_key_globalid` set to the `GlobalId` of a license key.
This makes it possible for Cryptolens to tell which license key should be extended or blocked based on subscription related events.

## Implementation tips

The first time you set up a subscription, we recommend the following sequence of events:

1. Create a new subscription and add `skm_temp_var` as metadata. The variable can contain anything (as long as it is not empty).
2. Call [Create Key](https://app.cryptolens.io/docs/api/v3/CreateKey) and record the `Key`.
3. Call [GetKey](https://app.cryptolens.io/docs/api/v3/GetKey) with the `KeyString` set to `Key`(from step 2) and `ProductId` set to the product that is associated with the subscription. Record the `GlobalId`.
4. Update the subscription by setting `skm_key_globalid` to the `GlobalId` (from step 3) and remove `skm_temp_var` by setting it to empty string.

This should be it! 

