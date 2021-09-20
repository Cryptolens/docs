---
title: Product configurations
author: Artem Los
description: Describes custom configurations that can be enabled that will alter behaviour of certain operations.
labelID: web_interface
---

# Introduction
It is possible to use data objects on the product level to change the default behaviour of certain methods as well as adjust how licenses are shown. On this page, we have listed the different data objects that can be created and the value that they accept.

## List of configurations

<table border="true">
<tr><td>&nbsp;cryptolens_unblockextendedlicenses&nbsp;</td><td>If the string value is set to <strong>true</strong>, when a blocked license is extended (either through the dashboard or the API), it will automatically be unblocked.</td></tr>
<tr><td>&nbsp;cryptolens_trialactivation&nbsp;</td><td>By default, <a href="/web-interface/trial-activation">start countdown upon activation</a> will extend the time-limit of the license each time a new device is registered. If you want to extend the license only for the first activation, set the string value to <strong>first_activation</strong>.</td></tr>
<tr><td>&nbsp;cryptolens_orderby&nbsp;</td><td>The string value can be used to specify how to order licenses on the product page. For example, setting the string value to <strong>Created (descending)</strong> will order all licenses based on the creation date in descending order.</td></tr>
<tr><td>&nbsp;cryptolens_featuretemplate&nbsp;</td><td>The string value can be used to specify the <a href="/web-interface/feature-templates">feature template</a>.</td></tr>
</table>

