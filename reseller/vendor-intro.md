---
title: Reseller portal for vendors
author: Artem Los
description: Introduction to reseller portal for vendors
labelID: reseller
---

# Reseller portal for vendors

> The documentation is still under development and some information may be missing.
Please let us know if something is unclear at support@cryptolens.io.

## Introduction

## Getting started

### Prior setup

#### Company profile
It is important to set up company profile with a company page and domain on [this page](https://app.cryptolens.io/User/Profile).

#### License templates
When issuing a **license issuance right**, Cryptolens needs to know the settings to use for the new licenses, eg. features, when it will expire, etc. These can be defined in a [license template](/web-interface/license-template).

The easiest way to create a license template is by visiting the key creation page and click on **Save as template**.


### Considerations

When you start using the reseller features, please keep in mind that once a **license issuance right** is issued to a reseller, it will <u>not</u> be possible to:

* remove the license issuance right (although it can be blocked, which will render it unusable).
* remove the reseller from the list of registered resellers.
* remove the **license template** associated with the license issuance right.

### Adding new resellers
In order to grant a user a license issuance right, you need to send them an invite. Once they have accepted it, they will show up in the dashboard and you will be able to manage their license issuance rights.

### Granting issuance right
An issuance right gives the reseller the ability to create new licenses keys on your behalf subject to your conditions. You can specify how long the issuance right will be valid and how many licenses the reseller can issue.

* **Friendly Name** - This is how the reseller will be able to distinguish between issuance rights. A reseller is not able to see the product id or the properties of the license key.
* **License Template** - This contains information about how the license key should be created and which product it should belong to. You can read more about license templates [here](/web-interface/license-template).
* **Amount** - This specifies how many licenses they will be able to issue. You can always grant a reseller a new issuance right if they use up an existing one.
* **Expires In** - This specifies how long the issuance right will be valid. If you do not want to impose a time constraint, please set this to a large number.
