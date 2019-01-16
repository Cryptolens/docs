---
title: Automating customer and license creation
author: Artem Los
description: A tutorial about automatically creating a new customer that we later associate with a new license key.
labelID: payment_forms
---

# Automating customer and license creation

## Introduction
In this tutorial we will go through the steps needed to automatically create new customers and assign them a license key upon a successful transaction.
We will assume that you already have a payment form with at least one payment processor. Please review these articles before you continue reading this tutorial:

* [Overview of Payment Forms](/payment-form/index)
* [Overview of Requests and Custom Field](/payment-form/request)

## Idea

In order to both create a new customer and then assign them a new license key, we will send two requests to the [Web API](https://app.cryptolens.io/docs/api/v3), one to **create a customer** and one to **create license** key associated with that customer. The links to the required methods are provided below.

> Note, we will look more into the details of how to set things up in the next section.

* [Create a customer (Web API docs)](https://app.cryptolens.io/docs/api/v3/Customer)
* [Create a license (Web API docs)](https://app.cryptolens.io/docs/api/v3/CreateKey)

## Implementation

### Creating an access token
In order to ensure that API calls we perform (i.e. the requests) work, we need an access token with **CreateKey** and **AddCustomer** permission, which you can create below:

* [Create an access token](https://app.cryptolens.io/User/AccessToken#/newtoken)

Please tick the right permissions as shown below and save the token that is generated.
![](/images/payment-form-auto-key-create-1.png)

Here is an example of a token you can get:
```
WyIxMjc0IiwieUN2RjYveDZTVktsVm9XZFhVcmtTdzZJSm1yTU1zTnZoTXdKQmNsSSJd
```

### Setting up API calls

#### Creating a customer
A customer can be created using the url below. You can review other parameters of this method [here](https://app.cryptolens.io/docs/api/v3/Customer).

> In the request below we have added name parameter that comes from the custom field.

```
https://app.cryptolens.io/api/customer/AddCustomer?Name=[custom]&token=<use the token we generated in the previous section>
```

#### Creating a new license

Once a customer is created, the API call above will return and `customerId`. When creating a new license, we will use this `customerId` to make sure that the license is added to this customer. In the url below, a license key will be created with `Feature=True` and it will be associated with the customer created earlier.

> Note, you need to specify **ProductId** of the desired product, which you can find on the product page.

```
https://app.cryptolens.io/api/key/CreateKey?CustomerId=[customerId]&Feature=True&ProductId=<enter the product id>&token=<use the token we generated in the previous section>
```

### Adding Requests in the Payment Form
Once we have working API call urls (covered in the previous section), most of the job is done. We now need to add these urls as **request items** in the payment form.

When you scroll down, you will see the requests that will be executed upon a successful transaction:

![](/images/payment-form-auto-key-create-2.png)

For each of the API call urls from [creating a customer](#creating-a-customer) and [creating a new license](#creating-a-new-license) sections, you need to create a request item.

> Note, we should set type to **Data Request** and method to **GET**.

Below is an image summarizing the type of request to create:

![](/images/payment-form-auto-key-create-3.png)

Once you have added the requests, that section in the payment form should look similar to what is shown below:

![](/images/payment-form-auto-key-create-4.png)

> It is important that the top request is to **add a customer** and that the **add license** request is below.

### Customizing success message

If you would like to use the built in success page, we can customize it to show the license key upon a successful payment. We have so far used two API calls, one to [create a customer](https://app.cryptolens.io/docs/api/v3/AddCustomer) and another to [create a license key](https://app.cryptolens.io/docs/api/v3/CreateKey). These methods will return the following variables (see **Results** section for each API call):

* **customerId**  - (from Add Customer) the customer id
* **portalLink** -  (from Add Customer) the link to the sign up form for the portal where customers can manage licenses (requires **EnableCustomerAssociation** to be true when creating the customer)
* **key** - (from Create Key) the key string

> Note: Although each variable is capitalized, the API will format all variables with came case, eg **PortalLink** becomes **portalLink**.

Using these variables, you can create a template for the message, for example:

```
Thank you for your order. Your key is [key] and customer id [customerId].
```

## Finishing up and testing
If everything was set up correctly, you can click on **Preview** of the form, which will show you the live payment form.

> Remember to add `?custom=<name>` in the end of the url to the live payment form.

The link that should be sent to your customer is similar to the one below:

```
https://app.cryptolens.io/Form/P/abcdefg/123?custom=<name>
```