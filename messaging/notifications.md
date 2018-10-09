---
title: Notifications
author: Artem Los
description: Broadcasting notifications to end users
labelID: messaging_api
---

# Notifications

## Idea

Keeping an active relationship with your customers is good, especially when you sell subscriptions rather than a product. 
One way of doing it is by regularly updating your customers about new features, tips and other useful information.

It can also be useful to send specific messages to various customer groups, for example, you might have some customers running
a beta version, in which case notifications can help to keep them updated. This is accomplished by grouping messages into **channels**.


## Implementation
Broadcasting messages to end users is implemented in a similar way as in the [Updates tracking](/updates-tracking) tutorial, except that we need to record the date of the last message
i.e. `result.Messages[0].Created` so that we can use it as a reference when searching for new notifications. If it is the first time they started it, we can use store `DateTimeOffset.Now.ToUnixTimeSeconds()` and use it to retrieve messages.

The channel is set to **news**, but this can be changed depending on the user preferences, etc.

```cs
// this should the last messages that you have read. 
// if it does not exist, use DateTimeOffset.Now.ToUnixTimeSeconds(), i.e. current unix time stamp.
// var lastSeenTime = result.Messages[0].Created

var result = (GetMessagesResult)Message.GetMessages("token with GetMessages permission", new GetMessagesModel { Channel = "news", Time = lastSeenTime } );

if(result == null || result.Result == ResultType.Error)
{
    // we could not check for new message
    Console.WriteLine("Sorry, we could not access new messages");
}
else if (result.Messages.Count > 0)
{
    // return all new unseen messages
    for(int i = 0; i < result.Messages.Count; i++)
    {
        Console.WriteLine(result.Messages[i].Content);
    }

    // save this date and use it when calling 
    //result.Messages[0].Created
    
}
else
{
    // No new messages
    Console.WriteLine("No new messages available");
}

Console.Read();
```