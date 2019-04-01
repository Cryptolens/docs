---
title: Start countdown upon activation (aka Trial activation)
author: Artem Los
description: Describes the trial activation setting for a license key.
labelID: web_interface
---

# Start countdown upon activation (aka trial activation)

## Idea

Trial activation allows you to pre-generate time-limited licenses so that the time-limit starts from the first day of activation. This is especially useful when you don't know in advance when the license key will be used for the first time.

## Getting started
By default, a newly generated license key does not have this option enabled. In order to enable it,

1. Go to https://app.cryptolens.io/Product
2. Select the product you want to use.
3. Click Create new key
4. Tick **start trial upon activation**
6. Set the Maximum number of machines to 1. (note, this value can be anything greater than zero for this feature to work)
7. Click Create.

If your set time was set to 30 (as an example), then if the user activates the license key in 2 months from now, they will still be able to use the license for 30 days.

## Remarks

Please read these through before you start using feature in production.

### Maximum number of machines greater than 1
Trial activation feature ensures that each new machine code can use the license for a set number of days (specified in set time / period). However, since the "expiration date" is global for
the license (shared among all machines), setting maximum number of machines to a value greater than 1 will allow the old machines to use the license until the last machine expires.

For example, let's say you set maximum number of machines to 2 and set time to 30 (this value is referred to as "period" on the product page). When the first machine activates the license, it will extend the expiration date so that the license can be used in 30 days. After that 30 days have expired, the second machine activates the license, which will extend the expiration date for another 30 days. The old machine will still be able to use the license.

### SKGL key generation algorithm
We recommend to always use SKM15, which is the default for all newly created products. Using SKGL will update the license key string, which is described [here](https://app.cryptolens.io/docs/api/v3/Activate).

### Hard coding licenses
Never hard code a trial key into your application. Although the trial will not be extended if only one user is using it, once we allow multiple users to activate the trial (for the same key), they will update the global trial count. If you need trial key functionality, please take a look at [verified trials](/examples/verified-trials).