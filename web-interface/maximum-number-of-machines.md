---
title: Maximum number of machines
author: Artem Los
description: Describes the idea behind the is maximum number of machines parameter, when creating new products.
labelID: web_interface
---

# Maximum number of machines

The maximum number of machines allows you to lock a license key to specific number of devices (aka hardware lock). This value is an upper bound. If it is set to zero, this feature will be deactivated, which means that unlimited number of activations can be performed. This will only affect [key verification](https://help.cryptolens.io/examples/key-verification), i.e. you will still be able to retrieve key information using [GetKey method](https://app.cryptolens.io/docs/api/v3/GetKey).

* 0 – hardware lock disabled, i.e. unlimited number of activation.
* 1,2, ... – sets the upper bound for the number of devices that can activate the license key.

## Example
Suppose you want to allow a team of 5 people to use a license key that you’ve created. In that case, you simply set `Machine Number of Machines` to 5.