---
title: Restricted accounts
author: Artem Los
description: Explains how to create restricted accounts (aka operator view).
labelID: web_interface
---

# Restricted accounts

> **Note:** The restricted accounts requires a separate subscription. Please contact support@cryptolens.io for more information.

## Idea

Operator accounts allow you to grant your employees restricted access to your dashboard. This is useful if you want to prevent them to accidentally remove product or an access token, while still allow them to perform necessary changes to licenses, etc.

All operations performed by the user will be logged in the [Object log](https://app.cryptolens.io/docs/api/v3/GetObjectLog).

## Getting started

### Adding an operator
To invite an employee to sign up for an operator account, you can share the link on the [operators page](https://app.cryptolens.io/Operator). When they have signed up, they will not have any access by default, so you will need to add this afterwards.

### Modifying permissions
Once an employee has signed up, you will be able to grant them permission by clicking on **Modify** on [operators page](https://app.cryptolens.io/Operator).

In most cases, we would recommend to give them two permissions: **edit** permission for **products** and **owner** permission for **customers**. **Resource Id** should be set to 0 in most cases. You only need to set it if you want to restrict access to a specific product or customer only.

## Permissions

In this section we describe what your employees can do with different permission levels. 

### Access Type
There are three different access types that you can assign to a user. Note, extra permission is given to users depending on the resource they have permission to, which is covered in _Resources_.

#### View
The user can only view the object.

#### Edit 
The user can edit the object but not remove it.

#### Owner
The user can add new and remove existing objects.

### Resources
In this section we cover extra permission given to users depending on the resource.

#### Product
In addition to the permission granted based on the access types described earlier, users will get extra permission to other objects depending on the access type.

##### View
Users will get read-only access to see all license keys, machine code and data objects (associated with the product, a license key or machine code).

##### Edit & Owner
Users will be able to add and modifying the aforementioned objects. Moreover, they will also be able to create new customers when creating a new license key (although they won't see existing customers if they don't have a separate permission to view customers).

#### Customer
No special permission is granted beyond what is defined under _Access Type_.

### Resource Id
When this is set to zero, the user will have access to all products of that type. If you set it to be an id of an object (eg. product id), the user will only be granted the specified permission for that object.