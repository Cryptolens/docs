---
title: Enabling notes field access
author: Artem Los
description: Describes how to enable notes field access.
labelID: web_interface
---
<p>&nbsp;</p>
> **This article is kept as a reference only**. When you use our recent examples and Web API 3, this field has no effect.

# Enabling notes field access

This article describes the way you can access the notes field through the API.

## Enabling access to notes field
Notes field is not accessible by default for security reasons, however, if you would like to be able to access this field through your application, you will have to:

1. Go to [https://app.cryptolens.io/User/Security](https://app.cryptolens.io/User/Security) (i.e. press on your name in the right top corner and then Security Settings)
2. Check `Enable Access to Notes Field`.

## To keep in mind
* By allowing access to notes field, this will apply to all products that are set to be public. Please make sure to upgrade all your client applications to use the newest version of SKGL Extension, from 1.0.8.1 or later.

* If you use Offline Key Validation, the notes field will be signed also. That means that it cannot be changed by the client user.
