---
title: Data protection policy for the Cryptolens platform
author: Artem Los
description: Details of your rights as data controller, how to be GDPR compliant and safety measures in place to safeguard your data.
labelID: security_advice
---

# Data Protection Policy

## Summary

1. [Introduction](#introduction)
2. [How to be compliant as a software vendor](#how-to-be-compliant-as-a-software-vendor)
3. [What data we collect](#what-data-we-collect)
4. [Third party services](#third-party-services)
4. [Safeguarding measures](#safeguarding-measures)
5. [How to increase privacy for your customers](#how-to-increase-privacy-for-your-customers)

## Introduction
This document summarizes the data being stored by Cryptolens AB ("Cryptolens") to provide the service Cryptolens (https://app.cryptolens.io) and the protection measures we have in place to safeguard against accidental loss and data breach.

It should be used for informational purposes only. If you are unsure, please consult a lawyer.

## Your rights and the rights of your customers
You have the right to [[1]](https://ec.europa.eu/info/law/law-topic/data-protection/reform/rights-citizens/my-rights/what-are-my-rights_en):

* information about the processing of your personal data;
* obtain access to the personal data held about you;
* ask for incorrect, inaccurate or incomplete personal data to be corrected;
* request that personal data be erased when it’s no longer needed or if processing it is unlawful;
* object to the processing of your personal data for marketing purposes or on grounds relating to your particular situation;
* request the restriction of the processing of your personal data in specific cases;
receive your personal data in a machine-readable format and send it to another controller (‘data portability’);
* request that decisions based on automated processing concerning you or significantly affecting you and based on your personal data are made by natural persons, not only by computers. You also have the right in this case to express your point of view and to contest the decision.

To exercise your rights, please contact us at support@cryptolens.io.

## How to be compliant as a software vendor

Once you start distributing your software, you will most likely have  users using it. Depending on if you sell to end users directly (eg. for desktop apps) or indirectly (eg. for SDKs), different steps have to be taken to ensure compliance.

These different cases are summarized below: on the left is the when the end users are your direct customers and on the right is when the end users are customers of your customers.

<img src="/images/DesktopAndSDK.png" alt="Drawing" style="width: 500px;"/>

### Compliance by default (update)
If you use Cryptolens as it is intended to be used, there is no need to include a consent in your agreement. The only personal data that Cryptolens needs is the IP address and Machine Code of the end user device. Both of these are needed to provide your customers the service.

* **IP address** is needed to be able to spot unauthorized use of your software.
* **Machine code** is needed to be able to enforce your licensing model (eg. restrict usage of a license to a specific number of machines).

> If you want to use this information for any other purpose, you need to obtain a consent from your customers.

## What data we collect
In short, we collect two types of data: data about you and data about your customers. In cases where your customers integrate your software (eg. a library/SDK) as a part of theirs, the end user is also their customers.

To make this as general as possible, we will go through the features in Cryptolens and describe who this data belongs to.

### Products
Products is a way of grouping licenses. Many of the parameters in a product such as `name`, `description` and `licenses` belong to you directly. However, if you collect information in the `notes` field or `data objects` that can be linked, directly or indirectly to a person, this then becomes personally identifiable information.

> Note, be very careful with the way you define personal data, since even IP address are considered personal identifiable information. Even primary keys in a table can be considered as such, if they can be used to link the data, directly or indirectly, to a person.

Always ask the question whether the data you put in these fields is necessary to provide the service. If you need additional data, the user has to give you an explicit consent.

### Activated machines
In order to to be able to tell the difference between the computers/devices (we refer to them as _machines_) that have activated their license, we store an additional set of fields for each of those machines. This includes:

* `Machine Code` - a device identifier
* `IP` - the IP of the client device that performed the activation.
* `IP Proxy` - the IP of the proxy (Cryptolens backed) used to perform the activation.
* `Time` - the time of the first activation.

The `machine code` and the `IP address` constitute personal identifiable information. We also have a record that links this to a certain key, but this is never returned through the API.

### Logging
**Activated machines** are can be thought of as up-to-date records of who is entitled to use a license. Once they stop using the service (eg. by deactivating), this record will be removed. However, for statistical purposes, it can be useful to have a history of all licensing activity related activity:

* `Pid` - the product id
* `Key` - the key id of the key	always returned
* `IP` - the IP address that called this method. this is the client user's IP.
* `Time` - the date and time when the activation was performed (the first time).
* `Machine Code` - a device identifier
* `State` - a number with a certain pattern that describes the request and its results.

Similar to **activated machines**, the `machine code` and the `IP address` constitute personal identifiable information. In addition, we store your user id in order to ensure that we only return your records.

### Customer objects
To facilitate grouping of licenses that belong to the same customer, we use the notion of **customers**. A customer object contains the following:

* `Name` - the name of the customer.
* `Email` - the email of the customer.
* `CompanyName` - the company name of the company the customer belongs to.

All of the fields above are personal identifiable information.

## Third party services
The list below shows the list of sub processors that we are using to deliver you the service:

* [Microsoft Azure](https://azure.microsoft.com) - for application and data storage (hosted in North Europe).
* [Hetzner](https://www.hetzner.de/) - stores the analytics and cdn modules (hosted in Finland).
* [AWS](https://aws.amazon.com/) - to send transactional mail (hosted in Sweden).
* [Intercom](https://www.intercom.com/) - to make sure you can chat with us.
* [Google Analytics](https://analytics.google.com) - for website analytics.


## Safeguarding measures
When developing Cryptolens, we apply an **assume breach policy**. This means that we develop components in such a way as to ensure that if a breach occurs, we can minimize its damanage.

### When data is no longer needed
Cryptolens strives to only store data that is needed to provide the service. Once such data is no longer needed, it will be removed or anonymized. To be more precise:

* The IP and address in both the **Activatated Machines** and **Logging** will be erased after 24 months.
* Inactive accounts will be erased after 1 year of inactivity.

### Protection of the database
#### Encryption of data at rest
We use the built in feature of Azure SQL Server for encryption of data at rest.

#### Firewall & restricted permissions
Our database firewall is restricted so that only authorized services can access it. Moreover, we restrict the permission each service has and mask sensitive fields.

#### Backup & protection against accidental loss
Automatic backups occur on a daily basis.

## How to increase privacy for your customers
In order to safeguard customer data and ensure compliance, both you as a software vendor and us as the data controller have to cooperate. In this section, we have outlined several tips of how to increase safety when using Cryptolens.

### Access tokens
#### Restrict the scope
It is important to create `access tokens` that have very constrained scopes of permissions. For example, be specific by:

* limiting the set of methods that the access token has access to
* selecting the product the access token will work with
* using feature lock for data masking, where applicable

#### Using feature lock for data masking
Methods such as [Activate](https://app.cryptolens.io/docs/api/v3/Activate) and [GetKey](https://app.cryptolens.io/docs/api/v3/GetKey) allow you to mask certain fields to boost privacy. Masking is especially important if you are developing an SDK. We recommend that you mask:

* `Notes`, `Data objects` - if you are storing data related to your customer, since it will be visible by all the end users.
* `Activated Machines` - should always be masked since it reveals personal identifiable information about the customers.

### Securing your account

#### Strong password
Always use a strong password and preferably one that you cannot remember, relying on a password manager.

#### Setting up two-factor auth
Two-factor auth provides an additional layer of protection on top of the password. Please 

#### Making sure we have your correct email
On the [security settings](https://app.cryptolens.io/User/Security) page, please makes sure we have your correct email address.

#### Disable Web API 2
Please do not use Web API 2. It can be blocked on the [security settings](https://app.cryptolens.io/User/Security) page.

#### Add object locks
You can add [Object locks](https://app.cryptolens.io/security/ObjectLocks) to prevent accidental deletion of objects, for example, products and access tokens.

## History
* 2020.12.29 Update the provider for transactional mail + add new security features.
* 2020.04.05 Update the list of third party services. Update the name of the service from SKM to Cryptolens.
* 2018.05.24 Update the consent requirement (i.e. you no longer need a consent from your customers to be compliant).
* 2018.03.16 First version.

## References
1. https://ec.europa.eu/info/law/law-topic/data-protection/reform/rights-citizens/my-rights/what-are-my-rights_en, Last accessed 2018-02-28.
2. https://app.cryptolens.io/docs/api/v3/model/ActivationData, Last accessed 2018-03-06.
3. https://app.cryptolens.io/docs/api/v2/WebAPILog, Last accessed 2018-03-06.
4. https://app.cryptolens.io/docs/api/v3/AddCustomer, Last accessed 2018-03-06.
5. https://app.cryptolens.io/docs/api/v3/Activate, Last accessed 2018-03-12
6. https://app.cryptolens.io/docs/api/v3/GetKey, Last accessed 2018-03-12