---
title: Anomaly Detection
author: Artem Los
description: Explains how the anomaly detection module can be used to detect abnormal usage patterns and fraudulent behaviour.
labelID: web_interface
---

# Introduction

> **Note:** This article and some of the features it refers to are still in beta phase. If you find any discrepancies or need help, please
reach out to us at support@cryptolens.io.

The anomaly detection module allows you to detect abnormal usage patterns of your software. For example, a typical anomaly is when your customers experience problems (e.g., with license verification). You can read more about it in the [announcement on our blog](https://cryptolens.io/2021/01/ai-anomaly-detection-in-software-licensing-logs/).

## Getting started
### Enabling anomaly detection

The anomaly detection module can be enabled on [https://anomaly.cryptolens.io/index.html](https://anomaly.cryptolens.io/index.html). When this
is done, it might take some time to train the model and find the anomalies.

### Analysing anomalies with Log Explorer

A summary of all anomalies can be found on the [following page](https://app.cryptolens.io/extensions/SuspectedAnomalies).
When a log entry is classified as anomalous, it means that the group of requests that the log entry is part of was classified as anomalous. Not all requests within a group are anomalous.

For log entries that are anomalous, three additional properties will be shown:
* **Group Id (Anomaly)** -  A unique identifier of the group of anomalous requests that the log entry is part of.
* **Starting At (Anomaly)** - The ID of the first log entry in the group.
* **Ending At (Anomaly)** - The ID of that log entry in the group.

The *Starting At* and *Ending Before* can be used when calling [Get Web API Log](https://app.cryptolens.io/docs/api/v3/GetWebAPILog) method. For example, to retrieve all the requests in the group, you can set the Web API parameter *StartingBefore* to *StartingAt-1* together with *Limit* set to 25 (which is the size of the group).

Another way is to set *EndingBefore* to *EndingAt+1* with the *Limit* set to 25. In this case, you need to set *OrderBy* to *Time descending*.

### Actions to take for suspected anomalies
Typically, groups of requests where there are many unsuccessful requests for a certain license key will be classified as suspected anomalies. This is usually caused when your customers have issues verify their license. If a license key was recorded, you can use it to find which customer it was. In other cases, you can use information such as the *Machine Code* and the *Friendly Name* as a way to identify the customer. 

> **Note:** Some requests within a group of suspected anomalies can still be ok. The anomaly detection module looks at requests as a group and will classify a group of requests as anomalous if the usage pattern deviates from what it has seen previously.

### How anomaly detection works

The anomaly detection module works by learning the historical distribution of your Web API logs. It analyses the logs as a group by examining the following parameters:

* Time
* Successful (inferred from *State*)
* IP (anonymised)
* Key
* Product
* State
* MachineCode
* Country

*Note: IP, Key, Product and MachineCode are anonymised when the model is trained*. 