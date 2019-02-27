---
title: Android and iOS
author: Artem Los
description: Introduction to iOS and Android software licensing
labelID: getting_started
---

# Android and iOS

## Idea
The way software licensing can be added to an application for a mobile OS such as Android or iOS depends on what framework your application is written in. There are multiple possibilities:

* **Default for that platform**: the application is written in the default language for that platform i.e. Java for Android and Objective-C for iOS.
* **Bridge**: the original application/library is written in C/C++ (eg. producing a .so file) with a wrapper in the default language for that platform (eg. Java for Android).
* **Other framework**: the application is written in a framework such as Unity or Xamarin.

We will cover each of these cases below:

## Implementation

### Default language
#### Android
Since Java is the default language of Android applications, you can use our [Java client](https://github.com/cryptolens/cryptolens-java). You need to use the jar file called `cryptolens-android.jar`. Pre-compiled jar files can be found [here](https://github.com/Cryptolens/cryptolens-java/releases).

> Please note that `Helpers.GetMachineCode()` is not supported and that when you call `Helpers.IsOnRightMachine()`, you need to use those that allow you to specify a custom machine code. You can use [InstanceId](https://developers.google.com/instance-id/guides/android-implementation) instead or other approaches described later in the tutorial.

##### Adding the library
You can add the library by adding `api files('libs/cryptolens-android.jar')` in `build.gradle` (Module: app), as shown below. Please note that `cryptolens-android.jar` needs to be in the `libs` folder of your project.

```
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.android.support:appcompat-v7:28.0.0'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    implementation 'com.android.support:design:28.0.0'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'

    api files('libs/cryptolens-android.jar') 
}
```

##### Verifying a license
Since license key verification (eg. Key.Activate) uses networking, we need to call it asynchronously (i.e. not on the main thread). Moreover, we need to ensure that internet permission is set.

To call a method asynchronously, we can wrap it as shown below. For example, we can paste the code snippet from the [key verification tutorial](/examples/key-verification) into the `run` method below:

```java
Thread thread = new Thread(new Runnable() {
    @Override
    public void run() {
        // call eg. Key.Activate.
    }
});

thread.start();
```

Internet permission requires a small change in the manifest file:
```xml
...
<uses-permission android:name="android.permission.INTERNET"/>

<application ...
```

##### Obtaining the machine code
On Android devices, there are many ways of obtaining a unique device identifier, all of them with their own trade offs. If you don't want to require any specific permission,
it's possible to use Android Id, obtained as shown below:

```java
String machineCode = Secure.getString(this.getContentResolver(), Secure.ANDROID_ID);
```
This should be unique for each device, but it will change after a factory reset and there have been instances that certain devices had the same Android Id.

You could also use the following method:
```java
TelephonyManager tm = 
        (TelephonyManager) this.getSystemService(Context.TELEPHONY_SERVICE);
String machineCode = tm.getDeviceId();
```
Although it won't change during a factory restore, it requires a special permission.


#### iOS
There is no client library for Objective-C or Swift at the moment. Instead, we recommend to use the other approaches listed below.

### Bridge
A common way to develop mobile applications or SDKs is to put most of the logic into a C++ library and add a wrapper around it in Java or Objective-C. This approach makes it easier to maintain the application (since you mostly have one code base) and it tends to be harder to reverse engineer (since C++ compiles into native CPU instructions). If you develop an SDK, having a bridge allows you to expose functionality without revealing the source code.

### Other framework
#### Unity
If your application uses Unity, we recommend the [following article](/getting-started/unity) for more information.

#### Xamarin
For Xamarin applications, the same approach that was described in the [Unity article](/getting-started/unity) can be used.

## Considerations

### Machine codes
On mobile platforms, `GetMachineCode` and `IsOnRightMachine` will not work. Instead, you can use the the identifiers provided by respective platform.

#### Android
On Android, we recommend to use Instance Id, as described [here](https://developer.android.com/training/articles/user-data-ids).

#### iOS
On iOS, we recommend to use the [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).