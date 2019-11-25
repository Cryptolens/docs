---
title: Managing customers
author: Artem Los
description: Describes how to set up products and plans in Stripe
labelID: payment_forms
---

# Managing customers

## Adding new customers

To allow your customers to login and manage their subscriptions, they need to have login details to access the customer dashboard. There are two ways this can be done:

* **Provide a generic sign up link** (automatic) - anyone who has access to the link will be able to sign up for an account to access the customer dashboard. You can find this link [here](https://app.cryptolens.io/Customer/SignUpLink). We recommend this option if you want to have an **automated** way for your prospects to sign up for the customer portal.
* **Register customers yourself** (manual) - if want to control who can sign up for the customer portal, you can create the customers yourself. When you create a new customer on the [customer page](https://app.cryptolens.io/Customer), please select **enable customer association**. This will generate a sign up link similar to the one below, which you can send to a specific customer.
```
https://app.cryptolens.io/Portal/@cryptolens.io/Associate?id=123&auth=activationcode
```

## Overseeing customers

There are two ways of managing customers: both in Stripe and in Cryptolens. In Stripe you can manage your customers subscriptions and in Cryptolens their licenses. To see all your customers in Stripe, you can go to the `Customers` page and to see active subscriptions, you can go to `Billing>Subscriptions`.

Each **customer** has a metadata parameter `skm_customer_id` that helps you link them back to a Cryptolens customer. You can copy the customer id and paste it in the search box on the [customer](https://app.cryptolens.io/Customer) to find them in Cryptolens.

Each **subscription** has a metadata parameter `skm_key_id` that links a subscription to a license key in Cryptolens. On the product page, you can find this key by entering `Id=<skm_key_id>` in the search box.

If the subscription contains a `skm_temp_var` that says `This parameter will replaced with 'skm_key_id'. If you see this after refreshing the page, it means no key was created for the customer but they were still charged.`, you will need to manually add `skm_key_id` later and send your customer a new license key. You can find the affected customer by visiting the [customer](https://app.cryptolens.io/Customer) and searching for one with `skm_customer_id`.

> Remember, if you create a license key for a customer manually, you need to associate it with the customer object whose id is `skm_customer_id`.
