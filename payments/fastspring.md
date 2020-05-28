---
title: Integration with FastSpring
author: Artem Los
description: Explains how FastSpring can be integrated with Cryptolens
labelID: payment_forms
---

# Integration with FastSpring

## Introduction

FastSpring allows you to easily setup an online store where you can sell your products and subscriptions.
By combining it with Cryptolens, you can use FastSpring as the payment processor and Cryptolens as the software licensing backend.
Currently, the integration supports one-time payments.

<img src="/images/fastspring-overview.png" style="width:100%;" />

## Implementation

Visit your [FastSpring dashboard](https://dashboard.fastspring.com/) and create or select an existing product:

<img src="/images/fastspring-1.png" style="width:100%;" />

Once you are on the product page, click on **Add New Fulfillment**.
<img src="/images/fastspring-2.png" style="width:100%;" />

Next, select **Generate a License** and then in the dropdown, select **Remote Server Request**.
<img src="/images/fastspring-3.png" style="width:100%;" />

On the next page, we need to change the **URL** to
```
https://app.cryptolens.io/api/key/createkey?format=plaintext
```
and make sure that **Output Format** is set to **Single-Line License (Quantity Based)**.
<img src="/images/fastspring-4.png" style="width:100%;" />

When you click create, please click on **Parameters** in the menu bar as shown below:
<img src="/images/fastspring-5.png" style="width:100%;" />

On that page, you can provide additional parameters that should be sent to the [CreateKey](https://app.cryptolens.io/docs/api/v3/CreateKey/) method. You always need to provide a **token**
and a **ProductId**. To customize the license key, you can use other parameters that [CreateKey](https://app.cryptolens.io/docs/api/v3/CreateKey/) method accepts. For example,
if you want to create a license key that has `F1` enabled, the setup in FastSpring would look like the one below:

<img src="/images/fastspring-6.png" style="width:100%;" />

> The **token** needs to have **CreateKey** permission and can be created [here](https://app.cryptolens.io/User/AccessToken#/).

Upon a successful purchase, FastSpring will also send additional information related to the transaction, such as customer email, product id etc. You can capture these parameters so that
this information is stored together with the license key. One such parameter that is particularly important to setup is **Quantity**. There are at least two ways it can be interpreted from a licensing context. Either, you can use it to create a certain amount of unique licenses. Another way is to create one license and set **MaxNoOfMachines** to the **Quantity**.

For example, if you want create the same number of licenses as the **Quantity**, you can change the variable name as shown below.

<img src="/images/fastspring-7.png" style="width:100%;" />

If you instead want to create one license but restrict it to the same number of machines as the quantity, you can rename it to **MaxNoOfMachines**.


