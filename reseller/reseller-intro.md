---
title: Reseller portal for resellers
author: Artem Los
description: Introduction to reseller portal for resellers
labelID: reseller
---

# Reseller portal for resellers

> The documentation is still under development and some information may be missing.
Please let us know if something is unclear at support@cryptolens.io.

## Introduction

## Concepts

### License issuance right
A license issuance right gives you the right to issue a specific amount of licenses on behalf of the vendor (who issued the right to you).

## Common actions

### Creating a customer
You can create a customer on [this page](https://app.cryptolens.io/Customer/Create). It is important to select a vendor which this customer should be associated with. If
you have an issuance right from that vendor, you will be able to issue licenses to that customer directly.

* **Enable Customer Association** - Whether it should be possible to customers to sign up for a customer portal account. This can be enabled later.
* **Is Public** - If this is set to true, other resellers that are associated with the vendor will see this customer. They will only be able to issue new licenses to the customer. The remaining fields will be read only.

### Issuing a license

There are two ways to issue a license. Either, the license will not be linked to any customer (i.e. just a license key will be created) or it can be linked to a customer.

#### Customer-linked
To issue a license key that will automatically be associated with a customer, you can select a customer on the [customer page](https://app.cryptolens.io/Customer) and then choose which license issuance right you want to use. If a valid license issuance is selected, you can click on 'Issue' and a new license key will be issued and associated with the specified customer.

<img src="/images/reseller-issue-license-customer.png" style="width:100%">

#### Not-linked to a customer

To simply create a license key without associating it with a license key, you can use the link on 'Issue New License' from the [main page](https://app.cryptolens.io/ResellerPortal) of the reseller portal:

<img src="/images/reseller-issue-license-non-customer.png" style="width:100%">