---
title: Web API Basics
author: Artem Los
description: Basics of Web API
labelID: basics
---

# Web API

<img src="/images/web-api-illustration.png" width="80%"/>
## Talking to Web API
Web API can be thought of as a channel that can be used to talk to Cryptolens Platform from
your application and third party services. It contains a wide range of methods, which allow you to activate keys, create new ones,
analyse usage data, and so much more.

### Cryptolens Client APIs
This is a set of client libraries that you can add into your project to faciliate license key verification.
You can find out more [here](/web-api/skm-client-api).

### Direct Communication 
If you are targeting a language for which there is no client library, you can still take advantage of the functionality of Cryptolens's Web API.
For example, in order to retrieve key information in JSON, you can call
```
https://app.cryptolens.io/api/key/GetKey?token={accesstoken}&ProductId=1234&Key=MUYVD-LSEBY-CXHRQ-XFAGY&Sign=True
```
You can find more [here](https://app.cryptolens.io/docs/api/v3/).

## Next steps
You've now learned about the basic concepts of Cryptolens. Here's what's next:

1. [Get started with a pratical implementation](/getting-started/index)
2. [Learn more about possible licensing models](/licensing-models/licensetypes)
3. [Understand GDPR and its implications](/legal/GDPR)