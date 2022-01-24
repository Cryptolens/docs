---
title: IPs used by the API
author: Artem Los
description: The list of IP addresses used by Cryptolens Web API.
labelID: security_advice
---

# IPs used by the API

In some cases, your clients might have a heavily restricted firewall, and this would require that they whitelist the IPs used by Cryptolens to perform license key activations.

We recommend to whitelist the following IPs on port 443, which are used by Cryptolens:

<table border="true">
<tr><th>IP</th><th>Notes</th></tr>
<tr><td>23.102.21.212</td><td>app.cryptolens.io, used at the moment  </td></tr>
<tr><td>40.113.70.59</td><td>api.cryptolens.io, will be used in newer updates</td></tr>
<tr><td>20.82.170.150</td><td>api.cryptolens, reserved and may be used in the future</td></tr>
</table>
