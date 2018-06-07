---
title: Trial activation
author: Artem Los
description: Describes the trial activation setting for a license key.
labelID: web_interface
---

# Trial Activation

> Note: Since the introduction of [SKM15 (Key Algorithm)](/web-interface/skgl-vs-skm15), the key string will no longer have to be updated (if that algorithm is used). This means that, instead of being limited to one device only, you can have as many device as you like using the same key. But keep in mind, every new device will update the global trial "count" for every user.

## Introduction
Say you have generated 1000 trial keys for a full-featured version of your software that you ship along with another product in order to encourage customers to try it out. Or, you have a product that is subscription based (for example 365 days). In both examples, you want the trial to start from the moment a user starts your application.

In offline based validation system as SKGL, this is not possible unless you set up your own system that is going to track if the user has used the trial or not.

However, thanks to a server based validation system, in this case serialkeymanager.com, you can let the server to take care of that. The only thing that is required is to execute several lines of code.

> NOTE: A trial can only be registered on one machine. If you have specified the Maximum number of machines to be greater than 1, the last user that has activated the software has to save the updated key in order to be able to activate the software on a different machine.

## Getting started
By default, a newly generated key does not have this option enabled. In order to enable it,

1. Go to https://app.cryptolens.io/Product
2. Select the product you want to use.
3. Press Create new key
4. Tick **start trial upon activation**
6. Set the Maximum number of machines to 1. (note, this value can be anything greater than zero for this feature to work)
7. Press Create.

All keys you generate using this procedure will be trial keys that can be updated a specific number of times. In this case, only once.

You can also change individual keys’ settings by clicking on a key and ticking **trial activation** box. Remember to set **maximum number of machines** to a value greater than 0.

## General Advice
Never hard code a trial key into your application. Although the trial will not be extended if only one user is using it, once we allow multiple users to activate the trial (for the same key), they will update the global trial count.