---
title: Key Generation using External Services
author: Artem Los
description: Example of how to create new keys using our API.
labelID: examples
---

# Key Generation

Being able to create new license keys through external applications is important if you want to integrate Cryptolens with an external providers (eg. for payments, distribution etc).

For example, if you already have a web store in place and want to keep using it, you can still use Cryptolens for eg. software licensing.

This article describes the way you can generate keys by a simple web request through another website or an application.

> The goal is to automate software distribution by allowing your customers to receive valid license keys upon successful payments.

## Enabling Key Generation

1. First, log in to your account so that you can access you security page: https://app.cryptolens.io/User/Security.
2. Press Allow external key generation.
3. (later) Copy the Private Key.

Once this feature is activated (step 2), any request that is in the correct format with the private key (step 3) will be able to generate one key in a given product, if the  product is set to Is Public.

## Generating a Key

Once you have enabled key generation, you are able to generate a new key. The simplest way is to assign all parameters (can be found here) to get a link that will return the specific key you want. A quick way to get that link without actually reading the Web API documentation is as following:

1. Select the product: https://app.cryptolens.io/Product
2. Press Create New Key
3. Fill in all the parameters (such as set time, features, etc.)
4. Press on the </> button.
5. Copy the link in the textbox.

Done! If you try to visit the link, you will notice that a new key will be returned. If you prefer to get a json object instead, please add  &json=true .