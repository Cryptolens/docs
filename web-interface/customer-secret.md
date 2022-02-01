---
title: Customer secret
author: Artem Los
description: Explains the concept of a customer secret.
labelID: web_interface
---

# Customer secret

## Idea

A *Customer Secret* can be thought of as a "password" that you can send to your clients so that they can access their licenses without first creating a Cryptolens account.

At the time of writing, it is possible to use [Get Customer Licenses](https://app.cryptolens.io/docs/api/v3/GetCustomerLicenses) and provide the *Customer Secret* to obtain all the licenses that are associated with that customer. For example, this can be useful if your clients have licenses to multiple products, so that they do not need to remember all of their license keys, just the *Customer Secret*. You can then extract the right license within your application.