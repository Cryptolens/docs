---
title: Feature templates
author: Artem Los
description: Describes the idea behind feature templates so that you can support more than 8 features.
labelID: web_interface
---

# Feature Templates

> Originally published on [our blog](https://cryptolens.io/2019/05/support-for-any-number-of-features/).

## Idea
A common request we have received from our customers is to support more than 8 features. Until now, the recommended approach has been to use the notes field or data object fields to store any additional feature information. With this update, the dashboard and several of our clients have built in support for additional features.

In addition to being able to define any number of features, we have also made it possible to define feature hierarchies. For example, we can define the following feature hierarchy:

![](https://i0.wp.com/cryptolens.io/wp-content/uploads/2019/05/image-2.png?zoom=1.25&ssl=1)

Now, suppose the user opens ModuleB. With the above setup, we can either check if they have permission to use ModuleB or we can be more specific and require Submodule 1 to be present.

We will go through in more detail how you can get started later in the article. The feature template used for our above example is shown below:

```
["ModuleA", ["ModuleB", ["Submodule 1", "Submodule 2"]], "ModuleC", ["ModuleD", [["ModuleD1", ["Submodule D1", "Submodule D2"]]]]]
```

## Set up
### Defining features
Let's suppose we want to define the following feature hierarchy:

![](https://i2.wp.com/cryptolens.io/wp-content/uploads/2019/05/image-3.png?zoom=1.25&ssl=1)

To define it, we can use a JSON array structure shown below:

```
["ModuleA", "ModuleB", "ModuleC"]
```

Suppose now that we want to add sub features to ModuleB. For example, Submodule 1 and Submodule 2. To do that, we introduce a new array instead of the string "Module B", which has the following structure:

```
["Module B", ["Submodule 1", "Submodule 2"]]
```

The first element defines name of the module, and the second element should ways be a list of submodules.

We can keep adding submodules to submodules in a similar fashion. For example, to express the following features:

<img src="/images/2022-04-20-feature-template.png" style="width:300px;" />

the template below can be used:

```
["Module C", ["ModuleD", [["ModuleD1", ["Submodule D2", "Submodule D4"]]]]]
```

To add your feature template to a product, you can click on Edit Feature Names on the product page and then scroll down until you see Feature Template.

In the example below, we would get the following feature hierarchy

![](https://i2.wp.com/cryptolens.io/wp-content/uploads/2019/05/image-4.png?zoom=1.25&ssl=1)

It's defined with the following feature template

```
["ModuleA", ["ModuleB", ["Submodule 1", "Submodule 2"]], "ModuleC"]
```

Please check out _Examples_ section at the end of this page for more templates.

### Assigning features
Once you have defined the feature template, the page to create a new license key and to edit existing one will have a box that allows you to define them, as shown below. The state will be stored in a data object with the name cryptolens_features, which we will cover in the next step.

![](https://i0.wp.com/cryptolens.io/wp-content/uploads/2019/05/image.png?zoom=1.25&ssl=1)

### Verifying features
You can verify that a certain feature exists using `Helpers.HasFeature`. For example, to check if Submodule1 is present, the following code can be used:

```
Helpers.HasFeature(license, "ModuleB.Submodule 1")
```

If you only want to check if ModuleB is present, without being specific, you can instead use:

```
Helpers.HasFeature(license, "ModuleB")
```

### Examples

The modules below:
<img src="/images/2022-04-20-feature-template-2.png" style="width:300px;" />

can be expressed as:

```
["ModuleA", "ModuleC", ["ModuleD", [["ModuleD1", ["ModuleD1.1", "ModuleD1.2"]], ["ModuleD2", ["ModuleD2.1", "ModuleD2.2"] ], "ModuleD3", "ModuleD4" ]], "ModuleE"]
```