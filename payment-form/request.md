---
title: Request Items
author: Artem Los
description: Detailed description of request items.
labelID: payment_forms
---
# Request Items (aka Webhooks)

Request items are used to call the Web API, for example. However, they may also be used to notify your website (as a <em>webhook</em>)

<ul>
 	<li><strong>Url</strong> - The URL that SKM Platform will call upon successful transaction.</li>
 	<li><strong>Type</strong> - The type of request.
<ul>
 	<li><strong>DataRequest</strong> - This assumes the website (eg. Web API result) returns a JSON object (a hashmap, to be precise). SKM will take these values and insert them into the thank you page, instead of the <strong>[name]</strong>. So, if JSON object is <strong>{"key":"ABCD"}</strong>, and our message contains <strong>Thank you. Your key is [key].</strong>, the customer will see <strong>Thank you. Your key is ABCD</strong>.</li>
 	<li><strong>VoidRequest</strong> - Simply sends a request without taking into account what it returns.</li>
</ul>
</li>
 	<li><strong>Method</strong>  - This can either be GET or POST. Web API 3 requires this to be GET.</li>
</ul>

## Passing variables from one request
If the first request returns a JSON, those values can be used in the URL of succeeding requests further down. For example, if the first request returns a `key`, then we can add this value to the next request using `[key]`. These values can also be used in the custom message.

## Pre-set variables
In addition to the variables returned from each request and the [custom field](/payment-form/custom-field), there are also several variables provided by Cryptolens. They all start with "cl_" prefix.

Currently, the following variables are available:

* `cl_transaction_id` - the transaction id returned by either Stripe or PayPal. Useful if you would need to issue a refund, etc.
* `cl_email` - the email provided in the payment form.