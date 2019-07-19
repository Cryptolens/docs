---
title: License Templates
author: Artem Los
description: Introduction to license templates
labelID: web_interface
---

# License Templates

## Idea

When you create many licenses of the same type (eg. expiration date, set of features), it can be handy to be able to save these configurations and re-use them next time you create a new license.

## Usage

### Creating a new license template
The easiest way to create a license template is by clicking on **Save as Template** when creating a new license key. This will redirect you to a new page where you can specify the name of the template. The **parameters** field is the internal representation of the template, which we will cover a bit later. 

### Using an existing license template
When you are on the page to create a new license, you can select the license template from the dropdown list and click on the create button next to it. This will ensure that the configuration from the license template is used.

## Advanced

### Format of the 'parameters' field
The **parameters** field is a JSON representation of what type of license to generate. There are two formats that are used. The first one (which is used in most cases) is a JSON dictionary of the paremters accepted by [Create Key](https://app.cryptolens.io/docs/api/v3/CreateKey) method, for example:

```
{"ProductId":3645,"Period":30,"F1":true,"F2":true,"F3":false,"F4":false,"F5":false,"F6":false,"F7":false,"F8":false,"Notes":null,"Block":false,"CustomerId":0,"TrialActivation":false,"MaxNoOfMachines":0,"AllowedMachines":null}
```

The second format will only be used if you create a license template with the custom defined features (more info [here](https://cryptolens.io/2019/05/support-for-any-number-of-features/)). The format allows you to specify what methods are to be called and in what order. It is also possible to re-use the result from the previous method in the next method, for example, `[key]` in [AddDataObjectToKey](https://app.cryptolens.io/docs/api/v3/AddDataObject) refers to the key returned by [Create Key](https://app.cryptolens.io/docs/api/v3/CreateKey). An example is shown below:

```
[{"APIMethodName":"CreateKey","Parameters":{"ProductId":3645,"Period":30,"F1":false,"F2":false,"F3":false,"F4":false,"F5":false,"F6":false,"F7":false,"F8":false,"Notes":null,"Block":false,"CustomerId":0,"TrialActivation":false,"MaxNoOfMachines":0,"AllowedMachines":null}},{"APIMethodName":"AddDataObjectToKey","Parameters":{"Name":"cryptolens_features","StringValue":"[\"ModuleA\",[\"ModuleB\",[\"Submodule 1\",\"Submodule 2\"]]]","IntValue":0,"CheckForDuplicates":false,"ProductId":3645,"Key":"[key]"}}]
```

Note, for the time being, only [Create Key](https://app.cryptolens.io/docs/api/v3/CreateKey) and [AddDataObjectToKey](https://app.cryptolens.io/docs/api/v3/AddDataObject) are supported.