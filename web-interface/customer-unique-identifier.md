---
title: Unique customer identifier
author: Artem Los
description: Explains the concept of a unique identifier of a customer.
labelID: web_interface
---

# Unique customer identifier

## Idea

A *Unique Identifier* of a customer can be thought of as a "password" that you can send to your clients so that they can access their licenses without first creating a Cryptolens account.

At the time of writing, it is possible to use [Get Customer Licenses](https://app.cryptolens.io/docs/api/v3/GetCustomerLicenses) and provide the *Unique Identifier* to obtain all the licenses that are associated with that customer. For example, this can be useful if your clients have licenses to multiple products, so that they do not need to remember all of their license keys, just the *Unique Identifier*. You can then extract the right license within your application.