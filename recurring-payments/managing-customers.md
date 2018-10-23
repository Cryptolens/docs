---
title: Managing customers
author: Artem Los
description: Describes how to set up products and plans in Stripe
labelID: payment_forms
---

# Managing customers

## Adding new customers

To allow your customers to login and manage their subscriptions, you need to register them as customers in Cryptolens. You either do this using the Web API or in the dashboard. When creating a new customer, you need to check **enable customer association** and note down the url. This should then be sent to the customer, which will allow them to create a new Cryptolens account or use an existing one. The association url will be similar to the one below:

```
https://app.cryptolens.io/Portal/@cryptolens.io/Associate?id=123&auth=activationcode
```

## Overseeing customers

There are two ways of managing customers: both in Stripe and in Cryptolens. In Stripe you can manage your customers subscriptions and in Cryptolens their licenses. To see all your customers in Stripe, you can go to the `Customers` page and to see active subscriptions, you can go to `Billing>Subscriptions`.

Each **customer** has a metadata parameter `skm_customer_id` that helps you link them back to a Cryptolens customer. You can copy the customer id and paste it in the search box on the [customer](https://app.cryptolens.io/Customer) to find them in Cryptolens.

Each **subscription** has a metadata parameter `skm_key_id` that links a subscription to a license key in Cryptolens. On the product page, you can find this key by entering `Id=<skm_key_id>` in the search box.

If the subscription contains a `skm_temp_var` that says `This parameter will replaced with 'skm_key_id'. If you see this after refreshing the page, it means no key was created for the customer but they were still charged.`, you will need to manually add `skm_key_id` later and send your customer a new license key. You can find the affected customer by visiting the [customer](https://app.cryptolens.io/Customer) and searching for one with `skm_customer_id`.

> Remember, if you create a license key for a customer manually, you need to associate it with the customer object whose id is `skm_customer_id`.
