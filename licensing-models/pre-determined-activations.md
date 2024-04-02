---
title:  
author: Artem Los
description: Explains how to allow a pre determined list of machines to use a license key.
labelID: licensing_models
---

# Pre-determined activations

## Idea

A common use case when licensing a software to a larger organization is to require pre-approval of the machines that will use a license key. This tutorial shows how to limit activations to a pre-determined list of machines.

## Implementation

The first step is to ask customers to send the list of all machines they want to activate to you. We provide CMD and PowerShell scripts that they can run using e.g. Microsoft SCCM: [https://github.com/Cryptolens/admin-tools/tree/master/manual-activation#powershell-script](https://github.com/Cryptolens/admin-tools/tree/master/manual-activation#powershell-script).

When you have received the list, you can use the following tool to activate them at the same time: [https://app.cryptolens.io/extensions/ActivateDevices](https://app.cryptolens.io/extensions/ActivateDevices).

To verify a license key, the idea is to use `Key.GetKey` instead of `Key.Activate`. When you call, `Key.GetKey`, you will receive the license key object that contains the list of all permitted machines. You can then use this list to see if the machine the client uses is included in that list and return an error message if it's not in the list.
