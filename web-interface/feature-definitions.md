---
title: Feature definitions
author: Artem Los
description: Describes the idea behind feature definitions.
labelID: web_interface
---

# Feature Definitions
There are many ways to configure feature locking in a product. In some cases, one feature can be used to indicate a trial version whereas another is used to indicate the number of concrete features that a customer has access to.

Cryptolens Web API is in most cases feature-agnostic, i.e. it does not differentiate between expired licenses etc. This is up to the client to decide, based on the feature parameters.

Recently, [Activate](https://app.cryptolens.io/docs/api/v3/Activate) and [Get Key](https://app.cryptolens.io/docs/api/v3/GetKey) have received a new field called `LicenseStatus`, whose aim is to move more of the license key logic from the client to Cryptolens. To make this possible, Cryptolens needs to know what different features should mean. You can set them up by clicking on `Edit Feature Definitions` on the product page.