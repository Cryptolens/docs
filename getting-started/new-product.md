---
title: Creating a new product
author: Artem Los
description: Create a new product step by step.
labelID: getting_started
---

# Create a Product

Each application you deploy can be thought of as a <a href="#prodkeys" target="_blank">product</a>.
A product is a collection of license keys.

In order to create a new product:
1. Click on 'create new product' on the <a href="https://app.cryptolens.io" target="_blank">main page</a>   
2. On the '<a href="https://app.cryptolens.io/Product/Create" target="_blank">create new product page</a>', 
enter the name of your application and press 'Create'.
id as well.

## Product Id
When you start implementing Cryptolens Client API into your code, you will need to provide
a `ProductId`, which is simply a number used to identify your product.

One of the ways to get the product id is to:

1. Go to the <a href="https://app.cryptolens.io/Product" target="_blank">product page</a>
2. Select the product in the list.
3. The `product id` is located under the description of the product.

## Examples
### The main page

<img src="/images/skm-new-product.png" style="width:100%; max-width:566px;" />

### The 'create new product' page

<img src="/images/skm-new-product-2.png" style="width:100%; max-width:491px;" />

## Next

* [Add code to your application](/getting-started/insert-code)