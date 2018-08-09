---
title: Usage-based licensing / Pay-per-use
author: Artem Los
description: Explains how you can set up implement usage-based licensing / pay per use / pay per click
labelID: licensing_models
---

# Usage-based licensing (pay per use)

## Idea

Usage-based licensing is when you charge for usage of specific features. For example, if you have an accounting software, you can charge per
created yearly report. If you have a movie editing software, you can charge per created movie (or for each conversion to a different movie format).

The point is to allow a larger group of people to be able to use your software. For example, let's return to the movie editing software. There can be two groups of users: those that will use the software a lot (eg. professional use), in which case they will prefer a subscription or a perpetual license. Another group can have movie editing as a hobby, in which case they may create very few movies, so a subscription may be to expensive.

> By supporting usage-based licenses, we can monetize a group of users that would otherwise not have purchased the product (eg. because it is too expensive).

## Implementation

We can implement usage based licensing using [data objects (aka custom variables)](https://app.cryptolens.io/docs/api/v3/Data). An advantage of using them is that they allow us to increment and decrement them atomically, which means that the counter will always reflect the actual usage of a specific feature.

We can implement this in two ways; the choice of which depends on if we want to bill users upfront (in advance) or in the end of the billing cycle (based on the actual usage).

We are updating the code examples. Please check back soon or contact us for example code.