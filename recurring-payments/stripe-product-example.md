---
title: Creating a Stripe product and pricing plan
author: Artem Los
description: This tutorial explains in more detail what needs to be in Stripe to create a product with a pricing plan.
labelID: payment_forms
---

# Stripe products and pricing plans

## Introduction

In this tutorial, we will focus on how to create a product and a pricing plan step by step. This is complementary material to [this tutorial](/recurring-payments/setup). The goal is to make sure that your metadata for the product and the pricing plan are set up correctly.

## Creating a product
To create a product, click on Product in the left menu:

![](/images/stripe-example-product.png)

Then click on **Add Product**:

<img src="/images/stripe-example-product-2.png" width="100%" />

You will now see a page similar to the one below. When you click **Save Product**, Stripe will combine product and pricing plan creation into one step. We will still need to change a couple of properties afterwards.

![](/images/stripe-example-product-3.png)

Once the product is saved, please add the ID of the product in Cryptolens and save it as `skm_product_id`. This is the only metadata field we need to add to a product. Now, you just need to update the price plan, which can be accomplished by clicking on it under **Pricing**.
<img src="/images/stripe-example-product-4.png" width="100%" />

## Updating pricing plan
The only thing left is to update the metadata of the pricing plan and its description. First, let's add the metadata field.

<img src="/images/stripe-example-product-5.png" width="100%" />

The value of `skm` is a JSON object of parameters that will be sent to the [CreateKey](https://app.cryptolens.io/docs/api/v3/CreateKey) method. More information about this can be found [here](/recurring-payments/setup#plans).

The last step is to add a **Price description** to the pricing plan, so that it can be displayed in the customer portal. You need to click **Advanced options** to see this field.

<img src="/images/stripe-example-product-6.png" width="100%" />

Now, everything should be set up correctly. If you still encounter any issues, please get in touch with us!
