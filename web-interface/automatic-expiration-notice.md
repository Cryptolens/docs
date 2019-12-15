---
title: Automatic Expiration Notice
author: Artem Los
description: Describes the way you can send automatic email expiration notices to your customers.
labelID: web_interface
---

# Automatic expiration notice

## Idea

Automatic expiration notice allows you to send out email notifications to your customers when a license is about to expire. You can either set this up so that an expiration notice is
sent a couple of days in advance and/or when the license has been blocked. You can configure when a license key should be treated as time-limited by clicking on _Edit Feature Names_ on the product page, as described [here](/web-interface/feature-definitions).

In order to be able to send these notifications, a license needs to be associated with a customer. If the customer has signed up for a Cryptolens account, we will use their accounts email address. Otherwise, the email address in the customer object will be used. It's important that you have the updated customer email in case we are not able to send an email to their account.

## Implementation

To enable automatic expiration notifications, you can click on _Edit Feature Names_ on the product page and then select `Automatic expiration notification`. The default behaviour is to send an email notification 7 and 3 in advance and another email when the license key gets blocked (assuming automatic license key blocking is enabled).

If you want to edit how many days in advance an email is sent and whether an email should be sent when the license key gets blocked, you can do so by modifying a data object associated with the product. It's called `cryptolens_expirationnotice`. By default, its value is `0,3,7`. The string value `0,3,7` means that an expiration notice should be sent 7 and 3 days in advance and the 0 means that when the license gets blocked, a notification will be sent too. For example, if you only want to send expiration notice in advance, you can change this string to `3,7` and if you only want to send confirmations when a license gets blocked, you can can set it to `0`.

