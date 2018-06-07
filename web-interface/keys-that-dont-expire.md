---
title: Keys that don't expire
author: Artem Los
description: Describes how to generate keys that don't expire.
labelID: web_interface
---

<p>&nbsp;</p>

> Note, **this article contains outdated code examples**. It will be updated soon.

# Keys that don't expire

When you are about to create a new key, you might have noticed that you are required to specify an expiration date (i.e. the number of day the key should be “valid”). However, expiration date does not affect the validity of a key. Here’s how:

When you verify a license key, Cryptolens will check whether the key exists in the database and that it is not blocked. If these conditions are fulfilled, an object with key information will be returned. You will see fields such as creation date, set time, time left, features, etc, stored in the object. Even if the key has “expired”, you will be able to get a key information object.

## Time Limited Keys
So, if keys don’t become invalid when they have “expired”, how would we go about to enable time restriction? This is quite easy, if we use the 8 boolean fields (true or false) called features.  Say we use feature1 as an indicator that the key is time-restricted. Then, we could write some code that first checks whether feature1 is true, and if this is the case, we could check the timeleft field (note, if you check periodically, please read the section Periodic Validation as it is done in a slightly different way). Below, there is some code in C#. The pid, uid, and hsum can be found here.


```cs
public void TimeLimitedKeysFeatureLocking()
{
    var machineCode = SKGL.SKM.getMachineCode(SKGL.SKM.getSHA1);
 
    var activationResult = SKGL.SKM.KeyActivation("pid", "uid", "hsum", "serial key to validate", machineCode);
 
    if (activationResult.IsValid())
    {
        // the key is valid (it might have expired, but at least it's not blocked).
 
        if(activationResult.HasFeature(1).IsValid()) // this basically says that the key is time limited.
        {
            if(activationResult.HasNotExpired().IsValid()) // the time check.
            {
                // everything is ok.
            }
            else
            {
                // tell the user that the key has expired.
            }
        }
        else
        {
            // If this feature was false, maybe this is a different key, so we could have different logic here.
            // otherwise, you could also treat this as invalid.
        }
    }
    else
    {
        //invalid key
        Assert.Fail();
    }
}
```