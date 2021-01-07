---
title: Data analysis and reports
author: Artem Los
description: Example of how to analyse various data sources in Cryptolens
labelID: examples
---

# Data analysis and reports

## Introduction

There are several data sources you can use to analyse the usage of your application and generate audit reports. We will describe them in more detail below and provide several examples of how they can be analysed.

## Data sources

* [Web API Log](https://app.cryptolens.io/docs/api/v3/model/WebAPILog) - contains a list events that are created each time a license changes (eg. activation or change of expiration date). Data object changes are also logged in this log. Note, this log contains minimal information about the event. 
* [Events Log](https://app.cryptolens.io/docs/api/v3/model/EventObject) - contains events that were registered using [Register Event](https://app.cryptolens.io/docs/api/v3/RegisterEvent) as well several other internal events.
* [Object Log](https://app.cryptolens.io/docs/api/v3/model/ObjectLog) - contains a list of events that are created when certain object types are added or modified to allow you to track all changes made by you and your team members.

## Analysis

We suggest to review the [following repository](https://github.com/Cryptolens/reporting-tools) to check if the report that you need to create is already available. As always, please let us know should you have any questions at [support@cryptolens.io](mailto:support@cryptolens.io).

The easiest way to analyse the logs is using our [Python SDK](https://github.com/Cryptolens/cryptolens-python) in combination with [Anaconda](https://www.anaconda.com/) to manage the packages. Let's look at how we can get some basics stats on the ratio of successful vs. unsuccessful requests. First, we need to load the relevant data:

```python
from datetime import datetime, timedelta, date

from licensing.models import *
from licensing.methods import Key, Helpers

import pandas as pd
import matplotlib.pyplot as plt

from tools import *

month_back = int(datetime.datetime.timestamp(datetime.datetime.today() - datetime.timedelta(days=30)))

logs = []

ending_before=0

"""
Loading the data
"""
while True:
    res = Key.get_web_api_log(token=get_api_token(), order_by="Id descending", limit = 1000, ending_before=ending_before)
    
    if res[0] == None:
        break
    
    logs = logs + res[0]
    
    if res[0][-1]["time"] < month_back:
        break;
        
    ending_before = res[0][-1]["id"] 
    
logs = pd.DataFrame(logs)
logs = logs[logs["time"]>month_back]
```

Once we have the data in a pandas DataFrame, let's create the plot:

```python
def success(state):
    return state % 100 // 10 - 1

logs["Success"] = success(logs["state"])
logs[logs["Success"]== 0] = "Success"
logs[logs["Success"]== 1] = "Failure"

logs["Success"].value_counts(normalize=True).plot(kind="bar", title='Successful vs. unsuccessful API requests')
```

This will create the following plot:

![](/images/data-analysis-successfulratio.png)