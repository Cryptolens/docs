---
title: Troubleshooting guide
author: Artem Los
description: Troubleshooting guide of the most common problems with the Stripe integration (recurring billing).
labelID: payment_forms
---

# Troubleshooting guide

## Introduction

In this guide, we have listed the most common errors and how they can be solved. If the error would persist, please reach out to us at
[support@cryptolens.io](mailto:support@cryptolens.io).

## Common problems

### Products are not showing up

If the products are not showing up, it means the product in Stripe is missing `skm_product_id` metadata parameter, which should be set to the product id of your product in Cryptolens. More documentation about this is available [here](https://help.cryptolens.io/recurring-payments/stripe-product-example#creating-a-product).

### Products show up but not the plans

If the product names are showing up but not the plans, it means the *Pricing tiers* in Stripe are missing `skm` parameter in the metadata with information about the type of license to create. Please check out [this article](https://help.cryptolens.io/recurring-payments/stripe-product-example#updating-pricing-plan)


### The Stripe webhook fails with 500 error
If the Stripe webhook keeps failing with an **500** error, the issue is one the following:

* **The signing secret is incorrect** - typically, this error occurs if you first used Stripe's *test* environment and then did not update the signing secret when moving to the *live* environment. We recommend to re-create the webhook and update the signing secret as described [here](https://help.cryptolens.io/recurring-payments/setup#webhooks).
* **The API version of the webhook is incorrect** - Our Stripe webhook listener uses the webhook version **2018-09-06**. At the time of writing, such webhook can only be created using Stripe's API. The solution is to re-create the webhook as described [here](https://help.cryptolens.io/recurring-payments/setup#webhooks). 
