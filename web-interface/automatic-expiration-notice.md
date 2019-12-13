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

The easiest way to setup an automatic notifications is by creating a data object (associated with the product) with the name `cryptolens_expirationnotice` and the StringValue `0,3,7`. To manage product data objects, you can click on "Data Objects" link on the product page.

The string value `0,3,7` means that an expiration notice should be sent 7 and 3 days in advance and the 0 means that when the license gets blocked, a notification will be sent too. For example, if you only want to send expiration notice in advance, you can change this string to `3,7` and if you only want to send confirmations when a license gets blocked, you can can set it to `0`.