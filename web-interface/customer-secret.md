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

## Configuration

### Changing the format
By default, the customer secret will be in the form of a GUID, for example: `6dffda95-05cb-4754-bb49-c4a0be6ea647`.

If you prefer to have shorter customer secrets that resemble a license key, i.e., `WPIGP-UPHHT-INUYJ-MSDTJ`, you can change that as follows:

1. Visit the global data object page [here](https://app.cryptolens.io/Data?refType=0).
2. Create a new data object with the **Name** `cryptolens_customer_secret` and **IntValue** set to `2`.

From now on, all customer secrets will resemble the license key format.

### Rare exception that may be thrown
Please note that we do not check if other customers have the same customer secret, since the likelihood of a collision is practically zero. However, if it this would occur, you would receive `A customer with the same secret exists.` when calling [Get Customer Licenses By Secret](https://app.cryptolens.io/docs/api/v3/GetCustomerLicenses), we would suggest to do the following:

1. Find the second customer with the same customer secret by searching for on the [customer page](https://app.cryptolens.io/Customer).
2. Update customer secret for both of the customers.
3. Inform both clients that it has changed.



