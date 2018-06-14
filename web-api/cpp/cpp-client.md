---
title: Introduction to C++ Client API
author: Martin Svedin
description: A comprehensive article that describes how the C++ client can be used to access SKM's Web API.
labelID: web_api
---

# SKM Client API for C++

On this page, we have outlined several examples of how to get started with the [SKM Client API](/web-api/skm-client-api) for C++.

> Note, SKM Client API for C++ currently supports **activation** and **deactivation** methods. Support for more methods is coming soon.

You can find the API documentation here: [https://api.serialkeymanager.com/cpp/](https://api.serialkeymanager.com/cpp/).

If you are already familiar with the .NET version of the library, we have summarized key differences in an [announcement](https://cryptolens.io/2017/08/new-client-api-for-c/) on our blog.

**Note**, A new version of the library that combines Windows and Linux versions into one. You can read the release notes [here](https://github.com/Cryptolens/SKM-Client-API-CPP/blob/future/README.md).

## Example usage

The library is built to be flexible in the way the communication with Cryptolens Web API.
The examples below instantiate a handle class of type SKM which deals with communication with the Web API. This handle class is in fact parameterized by several policy classes to which, among other things, communication with the Web API is delegated, see the section
`Handle class` for mor information.  In case this seem too heavy weight it is also possible to
sidestep this entirely and manually make the HTTPS request and simply provide the response as a
string, this is illustrated in the section `Manual HTTPS requests`.

### Basic usage

```cpp
namespace skm = serialkeymanager_com;

SKM skm_handle;
skm::Error e;

skm_handle.signature_verifier.set_modulus_base64(e, "ABCDEFGHI1234");
skm_handle.signature_verifier.set_exponent_base64(e, "ABCD");

optional<RawLicenseKey> raw_license_key = skm_handle.activate(e, "AAAA-BBBB-...", ...);
optional<LicenseKey> license_key= skm::LicenseKey::make(e, raw_license_key);

if (e) { handle_error(e); }

if (license_key->check()->has_expired(1234567))
{
    std::cout << "Your subscription has expired." << std::endl;
    return;
}

if (license_key->check()-has_feature(1)) 
{
    std::cout << "Hello, Mr President!" << std::endl; 
}
else                                    
{ 
    std::cout << "Welcome!" << std::endl; 
}
```

As can be seen above, two types are used to represent license keys, the `RawLicenseKey` and
`LicenseKey`. All queries regarding properties of a license key need to be performed on the
`LicenseKey` object. The purpuse of the `RawLicenseKey` class is to enable offline activation,
where a previous activation can be saved to disk and then later loaded to check the
validity of a license key without needing to make any HTTP requests. This is enabled by the
`RawLicenseKey` containing a cryptographic signature, which is checked during the construction
of the `RawLicenseKey`.

### Offline activation example
```cpp
SKM skm_handle;
skm::Error e;

optional<RawLicenseKey> raw_license_key = skm_handle.activate(e, "AAAA-BBBB-...", ...);

if (e) { handle_error(e); }

file << raw_license_key->get_license() << "-" << raw_license_key->get_signature() << std::endl;
```

and then when offline the license key can be verified via:
```cpp
skm::Error e;

std::string s << file;

std::string license = ...
std::string signature = ...

optional<RawLicenseKey> raw_license_key = skm::RawLicenseKey::make(e, license, signature);

if (e) { handle_error(e); }
```

## Error handling

The library uses an exceptionless design and return values use optionals where they can be missing.
All functions that can fail also accept as the first argument an instance of skm::Error which
is used for reporting more detailed error information.

Furthermore, an additional design decision is used to with regards to error handling. If an
error occurs, the following functions that take the same error object are turned into noops.
Consider the following code:

```cpp
skm::Error e;
f1(e);
f2(e);
```

If an error occurs during the call to `f1()`, `f2()` will not be performed. This allows writing
several function calls after each other without having to check for errors after
every single call.

## Handle class

The SKM class above is really an alias for the `basic_SKM` class, which takes two type parameters
representing a class for dealing with HTTP requests and one for checking cryptographic signatures.
Currently, one version of each is provided, the `RequestHandler_curl` class for making HTTPS requests
built on top of the Curl library and the `SignatureVerifier_OpenSSL` for checking cryptographic
signatures built on top of the OpenSSL/LibreSSL libraries.

```cpp
using SKM = basic_SKM<RequestHandler_curl, SignatureVerifier_OpenSSL>;
```

## Manual HTTPS requests

In some cases it may be an unneccessary amount of work to implement a `RequestHandler`, in
particular it may depend on what part of the code is responsible for initating the call.
For this reason, we also provide a method that parses a string containing the response
from the web API and returns the corresponding license key object:

```cpp
skm::Error e;
optional<LicenseKey> key = skm::handle_activate(e, web_api_resonse);
// rest is the same
```