---
title: Feature definitions
author: Artem Los
description: Describes the idea behind feature definitions.
labelID: web_interface
---

# Feature Definitions

## Idea
Feature definitions is a way to let Cryptolens know which of your features are used to denote trials and time-limited keys. Cryptolens is in most cases feature-agnostic, eg. it does not differentiate between expired licenses. This is up to the client to decide, based on the feature values.

However, in the dashboard, feature definitions can help Cryptolens to correctly render the status of each license. For example, if the license is not time-limited, you will not be able to extend it in the dashboard.

In order to change the type of each feature, you can visit the product page and click on `Edit Feature Names`. There, you can either select which feature should have the **trial** or **time-limited** property, and also mark the entire product as having time-limited licenses. The latter will ensure that no matter the feature values, the box that allows you to extend the time of a license will always be shown.

> If you want to be able to extend the expiration time of all licenses no matter the feature values, please check **Treat all licenses as time-limited**.

## Notes
Recently, [Activate](https://app.cryptolens.io/docs/api/v3/Activate) and [Get Key](https://app.cryptolens.io/docs/api/v3/GetKey) have received a new field called `LicenseStatus`, whose aim is to move more of the license key logic from the client to Cryptolens. To make this possible, Cryptolens needs to know what different features should mean. You can set them up by clicking on `Edit Feature Definitions` on the product page.