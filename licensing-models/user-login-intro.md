---
title: User Account Authentication (Intro)
author: Artem Los
description: Introduction to how you can allow users to use their account instead of license key to get hold of their licenses.
labelID: licensing_models
---
# User Account Authentication

## Idea

User account authentication allows you to verify what features your customers are entitled to without using their license key. Instead,
they can authenticate using their Cryptolens account. This is more secure since it is easier to protect a user account
(requires a username, a password and optionally two-factor authentication) than a license key.

Another advantage of creating user accounts for your customers is that they will be able to access customer dashboard, where they can review their existing licenses as well as sign up for subscriptions (supports Stripe).

In order to get this to work, you need to send your users a specific link so that their account is associated with a customer object in the 
[dashboard](https://app.cryptolens.io/Customer). Cryptolens will take care of the account creation process, if it is required.

## Implementation

### Creating a customer
The first step is to create a customer in the dashboard. You can either do this in the [dashboard](https://app.cryptolens.io/Customer) or
through the Web API using [Add Customer](https://app.cryptolens.io/docs/api/v3/AddCustomer) method. In both cases, you need to set 
**EnableCustomerAssociation** to true. A link will than be created similar to the one below, which should be sent to your customers.

```
https://app.cryptolens.io/Portal/@cryptolens.io/Associate?id=123&auth=authcode
```
If they do not have an account already, they will be asked to create a new one and if they are already logged in, all the licenses that they
have will show up in their dashboard.

### Code

Let's suppose you want to verify that a certain user has a license key for product **3349** that has not expired and contains Feature 1. 
We can achieve this using the code below:

#### Namespaces

```cs
using System.Linq;
using SKM.V3.Accounts;
using SKM.V3.Methods;
```

#### Verification code

To get this code to work, you need the RSAPubKey, an access token with GetToken permission and the ProductId. 

* The RSAPubKey can be found on [this page](https://app.cryptolens.io/docs/api/v3/QuickStart#api-keys).
* An access token can be created [here](https://app.cryptolens.io/User/AccessToken#/). It needs a **GetToken** permission.
* The product id can be found on the [product page](https://app.cryptolens.io/Product).

```cs
string RSAPubKey = "RSA Pub key";
string token = "access token with GetToken permission";

string existingToken = null; // in case you've already authenticated them once and the token is still valid.

var res = UserAccount.GetLicenseKeys(Helpers.GetMachineCode(), token, "TestApp", 30, RSAPubKey, existingToken);

if(res == null || !string.IsNullOrEmpty(res.Error))
{
    Console.WriteLine("Something went wrong.");
}

// if they have a license with F1=true and which has not yet expired.
if(res.Licenses.Count(x => x.F1 == true && x.Expires >= DateTime.UtcNow && x.ProductId == 3349) > 0) 
{
    Console.WriteLine("Success");
}
else
{
    Console.WriteLine("Failure");
}
```

The code above will open a new browser window where the customer can authorize the app to retrieve the license keys. If this is successful,
you should ideally record the `res.LicenseKeyToken` value so that next time the application starts it is passed to `existingToken`. Only if
`GetLicenseKeys` method is not successful when you pass in the token value should it be called again without the `existingToken` specified.


