---
title: Basics of Access Tokens
author: Artem Los
description: Introduction to access tokens and guide of creating them.
labelID: getting_started
---

# Access Tokens
In all of the code examples, you will see variables such as `token` or `auth`.
They refer to an <a href="https://app.cryptolens.io/docs/api/v3/auth" target="_blank">access token</a>.

The idea behind access tokens is to allow you to:
 * identify yourself with Cryptolens (authentication) - let Cryptolens know that you are you
 * make sure only desired permission is given to something (authorization) - eg. method scope, product, etc.

Using an access token, you can specify the methods you want to be able to call,
the product you want to use, the license key, and optionally feature you want to change.
You can read more about them <a href="https://app.cryptolens.io/docs/api/v3/auth" target="_blank">here</a>.

## Creating a new Access Token

In order to create an access token with the permission required by the previous example:

1. Go to 'Your account name' (right corner) > 'Access Token'.
2. Click on 'Create new Access Token'.
You will now be on <a href="https://app.cryptolens.io/User/AccessToken#/newtoken" target="_blank">this page</a>.
3. Enter a name, such as "ActivateToken".
4. Check the 'Activate' box (under LicenseKey)
5. Select the 'Product Lock' to be the name of your application.
6. Press 'Create an Access Token'.
7. Copy the access token and replace our token with yours.

## Example
<img src="/images/skm-access-token.png" style="width:100%; max-width:615px;" />

## Next

* [Finding your public key](/getting-started/pubkey)





