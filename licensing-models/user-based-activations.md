---
title: User based activations
author: Artem Los
description: Allows yo
labelID: licensing_models
---
# User based activations

## Idea
Normally, node-locking (aka machine code locking) works on a per device basis. When end users switch devices often, node-locked licenses might not be ideal since the old devices would need to be deactivated to allow new ones to activate. One solution to this is to use [floating licensing](/licensing-models/floating), where deactivations occur automatically. Another solution is to activate users instead of their devices, i.e. license on a **per user** rather than **per device** basis.

![](/images/user-based-activations-1.png)

## Implementation

There are two ways you can license on a per user basis:

* **Passwordless (no user input)**: If you can identify a user using a system similar to Azure AD, then there is no need to ask them for a username and password. The identifier from such system can be used directly (or in hashed form) as the machine code. The user email would be stored in the machine code field.

* **Username and password (user input)**: In other cases, especially when there is no reliable way to identify a specific user, it's better to ask users to provide the password since they have control over the username (with Azure AD, they would not, which is why a password is not needed). The idea is the same as case 1, except that machine code would contain a hash of the password, and the friendly name would be the username.

### Passwordless user authentication

One way to identify a user is by using their user principle name, which can be obtained with the command below:

```
whoami.exe /UPN
```

Activation and verification of a user can occur using the same method, e.g. Activate, as shown below:

```cs
var res = Key.Activate(AccessToken.AccessToken.Activate, new ActivateModel 
{ 
    Key = "KMZEW-SBRAE-VWCEK-CDLQE", 
    ProductId = 3349,
    MachineCode = userPrincipalName;
});
```

### Username and password authentication

If there is no way to identify a user automatically, you can ask users to identify themselves. In this case,
it is important that users identify themselves with password that only they know, to prevent other users from using their seat.

The idea is to store their password hash inside the machine code field, allowing your application to verify that a certain password is correct but not be
able to obtain the real password. `Helpers.ComputePasswordHash` uses a special algorithm (more precisely PBKDF2) to compute hash in such a way that it is harder for an adversary to guess. Normal hash functions such as SHA256 are very fast to compute, whereas PBKDF2 makes the computation slow on purpose so that it takes longer time to guess the original password.

To add a new user, the code below can be used:

```cs
var username = "testuser";
var password = Helpers.ComputePasswordHash("testpassword");

// Adding a user
var res = Key.Activate(AccessToken.AccessToken.Activate, new ActivateModel 
{ 
    Key = "KMZEW-SBRAE-VWCEK-CDLQE", 
    ProductId = 3349,
    MachineCode = password,
    FriendlyName = username
});
Assert.IsTrue(Helpers.IsSuccessful(res));

```

In the verification phase, we need to use `Key.GetKey` instead of `Key.Activate`, since we don't want to allow the end user to add new users. If you want to allow users to do that, feel free to change back to using `Key.Activate`.

```cs
//verifying password
var res2 = Key.GetKey(AccessToken.AccessToken.GetKey, new KeyInfoModel
{
    Key = "KMZEW-SBRAE-VWCEK-CDLQE",
    ProductId = 3349
});

Assert.IsTrue(Helpers.VerifyPassword(res2.LicenseKey, "testuser", "testpassword"));
Assert.IsFalse(Helpers.VerifyPassword(res2.LicenseKey, "testuser", "testpassword2"));
```

## FAQ

### How is this approach different from the original user account authentication tutorial?

The method described in the [user account authentication](/licensing-models/user-login-intro) tutorial differs from the method we described in this tutorial. In the user account authentication tutorial, we are assuming that the customer has a Cryptolens customer account linked to the customer object in your dashboard.

![](/images/user-based-activations-2.png)

Currently, a customer object can only have one user associated with it. Since user based activations (described in this tutorial) stores each user as an activation of a license, you can create any number of users. This also means that you have more control over user creation.