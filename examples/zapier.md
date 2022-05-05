---
title: Integration with Zapier
author: Artem Los
description: Zapier integration tutorial
labelID: examples
---

# Zapier integration

> Our current Zapier integration is private. To access it, you can use the following invite link: [https://zapier.com/developer/public-invite/128412/23e8cc7f6b2ca1a3a79dcadf2bfb0560/](https://zapier.com/developer/public-invite/128412/23e8cc7f6b2ca1a3a79dcadf2bfb0560/).

## Authentication
To authenticate, you will need an [access token](https://app.cryptolens.io/User/AccessToken#/) with the following permissions:

* `Get Web API Log`
* `Create Key`

## Features
### New License Key trigger
This trigger will listen for all license key creation events and return the following object for each newly created license:

```
{
  "id": 47720990,
  "productId": 3941,
  "key": "KWJRH-IWVJH-OGMTQ-ISOOZ",
  "ip": "85.230.98.0",
  "time": 1614705487,
  "state": 3010,
  "machineCode": null,
  "friendlyName": "",
  "floatingExpires": 0,
  "doIntValue": 0,
  "doId": 0
}
```

The most useful fields from this response are `productId` (tells you the ID of the product), `key` (the license key string that you can send to your customers) and `time` (when the event occurred). The other fields can be disregarded.

### Create Key action
This action will call the [Create Key](https://app.cryptolens.io/docs/api/v3/CreateKey/) method and return a new license key. Below is an example response:

```
{"key":"ABCDE-ABCDE-ABCDE-ABCDE","result":0,"message":""}
```