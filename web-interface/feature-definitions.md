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

## FAQ

In the following FAQ, we have compiled the questions we have received related to defining trial keys, extending licenses, etc.

### Why is it not possible to change the expiration date?
When you try edit license properties on the product, you might not always get the option to change the expiration date as shown below: 
<img src="/images/extend-license-key.png" style="width:100%;" />
Instead, you might see something like this:
<img src="/images/extend-license-key-none.png" style="width:100%;" />

The reason you might not always see the license key extension box is because the license key is not marked as a time-limited license. Although you will still be able to access the expiration date through the API (as well extend the license duration), we made a design choice earlier to hide it for licenses that are not time-limited in the dashboard.

There are two ways this can be solved:

1. When you are on the product page, you can click on **Edit Feature Names** and then check **Treat all licenses as time-limited**. This will mark all licenses as time-limited and the license extension box will show up when editing all licenses.
2. Enable the feature that is defined as a trial feature for that license. On the **Edit Feature Names** page, you can select which feature is used to denote a time-limited license. By default it's F1. In other words, if you set `F1=true` for a specific license, it will be marked as time-limited and you will be able to change the expiration date.

> **Note:** a license that is marked as time-limited (either when the time-limited feature is set to true or when **Treat all licenses as time-limited** is enabled) will **not** automatically expire. To achieve this, you need to configure that on the **Edit Feature Names** page. In other words, using one of the two options we have suggested will not block the license key in any way or make it invalid, unless you have changed the defaults.

If you have any questions, feel free to reach out to us at support@cryptolens.io.