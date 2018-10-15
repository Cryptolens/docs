---
title: Custom Fields
author: Artem Los
description: Detailed description of custom fields.
labelID: payment_forms
---

# Custom Field
Custom field allows you to customize the requests that are called upon a successful transaction. For example, if you want to allow your customers to upgrade a specific key, you can call the payment form as shown below:
```
https://app.cryptolens.io/form/p/secret/12?custom=ABCDE-FGHIJ-KLMNO-PQRST
```
and add the custom variable to a request by appending [custom], eg.

```
http://app.cryptolens.io/api/key/AddFeature?token=&ProductId=123&Key=[custom]
```

If the transaction is successful, the desired key will be upgraded (in this case, a specific feature will be added). Note, you can also use [custom] can be appended to the message Custom Message.