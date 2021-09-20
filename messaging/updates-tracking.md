---
title:  Updates tracking
author: Artem Los
description: Explains how to use messaging api to notify users about updates.
labelID: messaging_api
---

# Updates tracking

## Idea

It is usually desired that users always have the latest version of the software. Messages can be used to notify only those users that run older versions of the application, providing details where updates can be obtained and what's new.

You can also automate the process of updates by providing a link to the new version in the message, and perform the download and update automatically in the application. In this tutorial, we will focus on simply letting users know an update is available and where to get it.

It is also quite common to offers users the possibility to choose what type of updates they want. Some may prefer to only get the stable ones, whereas others want the newest features first and accept that there may be some bugs. We can solve this by grouping our updates notifications into **stable** and **experimental** channels.

## Implementation

### Code
Let's suppose the user is listening on the stable branch and we want to show them the recent message if such exists. Then, we can use the code below. **Remember** that this method requires a separate [access token](/getting-started/access-token) with 'GetMessages' permission, which can be created [here](https://app.cryptolens.io/User/AccessToken#/).

**In C\#**

```cs
var currentVersion = 1538610060;
var result = Message.GetMessages("access token with GetMessages permission", new GetMessagesModel { Channel = "stable", Time = currentVersion } );

if (!Helpers.IsSuccessful(messages))
{
    // we could not check for updates
    Console.WriteLine("Sorry, we could not check for updates.");
}
else if (result.Messages.Count > 0)
{
    // there are some new messages, we pick the newest one 
    // (they are sorted in descending order)
    Console.WriteLine(result.Messages[0].Content);
}
else
{
    // No messages, so they have the latest version.
    Console.WriteLine("You have the latest version.");
}

Console.Read();
```

**In VB.NET**
```vb
Dim currentVersion = 1538610060
Dim result = Message.GetMessages("access token with GetMessages permission", New GetMessagesModel() With {
                                   .Channel = "stable",
                                   .Time = currentVersion
                                 })

If Not Helpers.IsSuccessful(result) Then
    ' we could not check for updates
    Console.WriteLine("Sorry, we could not check for updates.")
ElseIf result.Messages.Count > 0 Then
    ' there are some new messages, we pick the newest one
    ' (they are sorted in descending order)
    Console.WriteLine(result.Messages(0).Content)
Else
    ' No messages, so they have the latest version.
    Console.WriteLine("You have the latest version.")
End If
```

**In Python**

```python
currentVersion = 1538610060
res = Message.get_messages("access token with GetMessages permission","stable", currentVersion)

if res[0] == None:
    # we could not check for updates
    print("Sorry, we could not check for updates.")
elif len(res[0]) > 0:
    # there are some new messages, we pick the newest one 
    # (they are sorted in descending order)
    print(res[0][0]["content"])
else:
    #No messages, so they have the latest version.
    print("You have the latest version.")
```

**In Java**
```java
long currentVersion = 1538610060;
GetMessagesResult result = Message.GetMessages("access token with GetMessages permission", new GetMessagesModel("stable", currentVersion));

if(!Helpers.IsSuccessful(result)) {
    // we could not check for updates
    System.out.println("Sorry, we could not check for updates.");
} else if(result.Messages.size() > 0) {
    // there are some new messages, we pick the newest one
    System.out.println(result.Messages.get(0).Content);
} else{
    // No messages, so they have the latest version.
    System.out.println("You have the latest version.");
}
```

The variable `currentVersion` is the date when you published this release (in unix timestamp format). Each time you update the application, you need to remember to update this value.

The easiest way to obtain the unix timestamp is by visiting [this link](https://duckduckgo.com/?q=unix+timestamp) or, if you are on Linux or macOS, type the following in the terminal:

```
$ date +%s
```

In .NET, this can be done using `DateTimeOffset` as follows:

```cs
var currentTime = DateTimeOffset.Now.ToUnixTimeSeconds()
```

In Python, this can be done with `datetime`:

```python
import datetime
datetime.datetime.utcnow().timestamp()
```

### Broadcast a message
New messages can be sent on [this page](https://app.cryptolens.io/Message). You can also use our [API endpoint](https://app.cryptolens.io/docs/api/v3/CreateMessage).