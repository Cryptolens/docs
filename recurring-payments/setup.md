---
title: Setting up Stripe
author: Artem Los
description: Describes how to set up products and plans in Stripe
labelID: payment_forms
---

# Setting things up

Before we can use recurring payments feature in Cryptolens, we need to set up a few things in Stripe and in Cryptolens. In this article, we detail all the necessary steps.

## Stripe setup
Stripe will contain most of the information regarding products, plans, subscriptions and customers. Additional information that is required for Cryptolens is stored in the metadata parameters. We have listed the specific metadata parameters for each object in Stripe below:

### Products
The first step is to create a Stripe product, which can be done in `Billing>Products`. Each Stripe product will be linked to a product in Cryptolens. The way to link them is by creating a metadata parameter `skm_product_id`, and assign it the product id of a Cryptolens product. You can specify the metadata parameter after that the product has been created. The end result should be similar as shown in the picture below (3349 should be changed to your product id).

<img src="/images/product-metadata-stripe.png" style="width:100%;" />

### Plans

A product in Stripe can have multiple plans (assuming you are using the newest version of the API). A plan allows you to set the billing interval (eg. monthly or early) and optionally the number of trial days (i.e. the number of days a user can use the software for free before being charged). Cryptolens will detect all of these settings. The only piece of additional information Cryptolens needs to know is the type of license key to generate (eg. what features, etc). This can also be specified using the metadata field `skm` in JSON format.

> **Note:** Under the hood, all the parameters passed in the metadata field will be sent to [CreateKey](https://app.cryptolens.io/docs/api/v3/CreateKey) method.

For example, if you want the license key to have Feature 3 enabled, you need to create a metadata parameter `skm` with the value `{"F3":true}`. If you want the same key to be [node-locked](/licensing-models/node-locked) to one device and have Feature 3 enabled, you can set `skm` parameter to `{"F3": true, "MaxNoOfMachines": 1}`. You can optionally add `ProductId` in the `skm` parameter, in case you want the license key to belong to a different product.

### Webhooks
In order to ensure that Cryptolens stays sync with Stripe about the status of each subscription, we need to add a receiving webhook endpoint. You can do this by visiting `Developers>Webhooks`and creating an endpoint with the following configuration:

* **URL to be called**: `https://app.cryptolens.io/api/subscription/StripeUrl?id=<your user id>` (**note**, this is the Stripe Webhook url )
* **Filter event**: select `select types to send` and check `invoice.payment_succeeded` and `customer.subscription.deleted`.

Once the webhook is created, please note down its **Signing secret**, which looks similar to this `whsec_CTQa9jRJY3LlnE28Xpv4gLejONQKjHv4`. We will need this value in the **Cryptolens setup** section.

## Cryptolens setup

All the stripe configuration will occur on [Company Profile](https://app.cryptolens.io/User/Profile) page (show below). We assume you have already created a Cryptolens product.

<img src="/images/company-profile-stripe.png/" style="width:100%;" />

* **Name** and **Description**: This information be shown on your profile page, which can be accessed at `https://app.cryptolens.io/Portal/@<domain>/`.
* **Url**: This is required for recurring payments and the profile page. Please enter your domain name only, eg. `example.com`.
* **Payment Processor**: Please select a Stripe payment processor, where you will be able to enter your test keys or connect to Stripe for live keys.
* **Signing Secret**: This is the webhook secret we obtained in the previous section, which is similar to `whsec_CTQa9jRJY3LlnE28Xpv4gLejONQKjHv4`.
* **Stripe Webhook Url**: This is the endpoint that should be registered with Stripe.

