---
title: Keys that don't expire
author: Artem Los
description: Describes how to generate keys that don't expire.
labelID: web_interface
---

# Keys that don't expire

When you are about to create a new license key, you are prompted to set the **Period** (i.e. the number of days the key should be "valid"), which affects the **expiration date**.

However, while Cryptolens requires a period (defaulted to 30) during the creation of a new license, this does not mark the license as "time-limited". Instead, what determines if your license has expired is the code on the client side (i.e. an explicit check for the expiry) or a feature in the dashboard (which has to be enabled).

### How to make a license time-limited
In other words, if you would like the license to be time-limited, you can either:
* **In the code** add additional code that checks the difference between the expiration date and the current time. For example, if you use F1 (Feature 1) to mark a license as time-limited, you can first check that it is set to true and then check if the license has expired.
* **In the dashboard** enable an automatic script that will block expired licenses. You can learn more about it [here](/faq/index#blocking-expired-licenses).

We recommend to enable automatic blocking of expired licenses in the dashboard for most use cases. If you are planning to use Cryptolens offline, we recommend to implement a time check in the code as well.

### How to issue time unlimited licenses
Since the default behaviour is that the license will keep being valid after expiration, there is no need to take any extra steps at this point.


