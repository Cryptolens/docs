---
title: Ordering licenses based on a property
author: Artem Los
description: Explains how ordering of licenses can be configured.
labelID: web_interface
---

## Customize which licenses are shown first

By default, the product page will show the recently created licenses first. If you prefer to change the ordering property, this can be accomplished in two ways:

### Temporary

The last state of the **Order By** will be saved in a locally and will persist for that session.

### Permanently

If you would like to persist the value of **Order By** for all users, this can be accomplished by adding data object (aka. metadata field) for a specific product.

1. On the product page, click on **Data Objects**
<img src="/images/data-object-product-page.png" width="100%" />
2. Create a new data object with the name `cryptolens_orderby` and set the string value to one of the supported values. _Note, this value is case sensitive._


### List of supported values

You can use one of the following values to change the how licenses are ordered:
<table border="true">
<tr><td>ID (ascending)</td></tr>
<tr><td>ID (descending)</td></tr>
<tr><td>Created (ascending)</td></tr>
<tr><td>Created (descending)</td></tr>
<tr><td>Expires (ascending)</td></tr>
<tr><td>Expires (descending)</td></tr>
<tr><td>Customer (ascending)</td></tr>
<tr><td>Customer (descending)</td></tr>
<tr><td>F1 (ascending)</td></tr>
<tr><td>F1 (descending)</td></tr>
<tr><td>F2 (ascending)</td></tr>
<tr><td>F2 (descending)</td></tr>
<tr><td>F3 (ascending)</td></tr>
<tr><td>F3 (descending)</td></tr>
<tr><td>F4 (ascending)</td></tr>
<tr><td>F4 (descending)</td></tr>
<tr><td>F5 (ascending)</td></tr>
<tr><td>F5 (descending)</td></tr>
<tr><td>F6 (ascending)</td></tr>
<tr><td>F6 (descending)</td></tr>
<tr><td>F7 (ascending)</td></tr>
<tr><td>F7 (descending)</td></tr>
<tr><td>F8 (ascending)</td></tr>
<tr><td>F8 (descending)</td></tr>
<tr><td>Block (ascending)</td></tr>
<tr><td>Block (descending)</td></tr>
<tr><td>Modified (ascending)</td></tr>
<tr><td>Modified (descending)</td></tr>
</table>
