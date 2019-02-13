---
title: Maximum number of machines
author: Artem Los
description: Describes the idea behind the is maximum number of machines parameter, when creating new products.
labelID: web_interface
---

# Maximum number of machines

> Please check out the [FAQ article](/faq/index#maximum-number-of-machines) as well.

The maximum number of machines allows you to lock a license key to specific number of devices (aka hardware lock / node-lock). This value is an upper bound. If it is set to zero, this feature will be deactivated, which means that unlimited number of activations can be performed. This will only affect [key verification](https://help.cryptolens.io/examples/key-verification), i.e. you will still be able to retrieve key information using [GetKey method](https://app.cryptolens.io/docs/api/v3/GetKey).

* 0 – hardware lock disabled, i.e. unlimited number of activation.
* 1,2, ... – sets the upper bound for the number of devices that can activate the license key.

## Ways of restricting access
There are two ways you can restrict the number of machines that can use a license:
1. [Node-locked](/license-models/node-locked): Either you restrict a license to a certain number of machines indefinitely (i.e. until they are deactivated)
2. [Floating](/license-models/floating): or you allow a license to "float" between different machines, as long as at any one point this upper bound of machines is never exceeded

For trial keys, it's better to use the first approach whereas for paid licenses, either approach will work fine, although floating licenses will be more convenient for your customers and remove the hassle of license deactivation.

## Example
Suppose you want to allow a team of 5 people to use a license key that you’ve created. In that case, you simply set `Machine Number of Machines` to 5.