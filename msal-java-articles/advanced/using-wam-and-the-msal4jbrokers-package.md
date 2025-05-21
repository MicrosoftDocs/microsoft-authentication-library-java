---
title: Using Web Account Manager with MSAL Java
description: "Web Account Manager (WAM) is a Windows 10+ component that can act as an authentication broker, allowing your users to easily authenticate with external identity providers as well as Microsoft."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 02/27/2024
ms.reviewer:
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
---


# Using Web Account Manager with MSAL Java

[Web Account Manager](/windows/uwp/security/web-account-manager) (WAM) is a Windows 10+ component that can act as an authentication broker, allowing your users to easily authenticate with external identity providers as well as Microsoft. To avoid the hassle of trying to access a native API from a Java application, MSAL Java offers a simple API to easily get your app to start using WAM.

## Before you start

First, you will need to make sure to configure your application to use the Web Account Manager through Azure Portal. To do that, your application needs to have [a WAM-compatible redirect URI](/entra/identity-platform/scenario-desktop-acquire-token-wam#redirect-uri).

Second, in addition to the main `msal4j` package your project will need a new dependency: [msal4j-brokers](https://mvnrepository.com/artifact/com.microsoft.azure/msal4j-brokers). This package contains everything that is needed for MSAL Java to access the native WAM API on your behalf.

## How to enable brokers in your application

Once you have `msal4j` and `msal4j-brokers` as dependencies in your project, you will be able to find two important classes and one important parameter:

- IBroker in msal4j, which is an interface acquireToken APIs similar to the ones already in MSAL Java
- [Broker](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/msal4j-brokers/src/main/java/com/microsoft/aad/msal4jbrokers/Broker.java) in `msal4j-brokers`, which implements IBroker and provides access to the underlying code that's interacting with WAM
- The [broker](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/5ae3186cea6451682664c8ff343033834feb984b/msal4j-sdk/src/main/java/com/microsoft/aad/msal4j/PublicClientApplication.java#L195) parameter in msal4j's `PublicClientApplication` builder, which will allow you to tell MSAL Java which IBroker implementation to use

To start using WAM in your application, you simply need to pass an instance of `Broker` into the `broker` parameter of `PublicClientApplication`:

```java
//Create broker, indicating that you want it to be used when the app is running on a Windows OS
Broker broker = new Broker.Builder().supportWindows(true).build();

PublicClientApplication pca = PublicClientApplication.builder(clientId)
        .authority(authority)
        .broker(broker) //Add the broker when creating your PublicClientApplication
        .build();
```

And that's it. Once a broker is set, whenever you call one of MSAL Java's existing acquireToken APIs it will attempt to use the broker you provided, in this case the one for WAM.

## Limitations

Not all platforms support WAM, and currently MSAL Java only supports WAM on Windows. If your application is running on an OS where WAM is not supported then MSAL Java will log a warning saying so and fall back to traditional authentication flows.

Additionally, only certain scenarios are supported by WAM. Currently only public client scenarios are supported, and only for these auth flows:

Auth flow | Supported | Notes
-----| ------- | ---------|
Silent | Yes | Supports both the existing MSAL Java behavior of retrieving cached tokens, as well new behavior of attempting a silent sign-in using the default OS account
Interactive | Yes | Rather than opening a browser tab, WAM currently pops up a UI window where your users can enter their credentials, and in order to properly show that window to your users you should [provide the window handle](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/7b64feac207fb67aeaa21e1bb19d2a3d37f1c359/msal4j-sdk/src/main/java/com/microsoft/aad/msal4j/InteractiveRequestParameters.java#L103) of your application as part of the request. If your application is console-based we should be able to discover the window handle for you, however if your application contains multiple custom UI elements then you should provide the window handle to avoid issues (such as the popup appearing behind other windows, blocking unrelated UI elements, etc.)
Username/Password | Yes | This flow may become deprecated in MSAL Java in the near future, and is already marked as deprecated in msal4j-brokers

## Logging and exceptions

Although the API in MSAL Java is simple, the underlying functionality is handled by several Java and native packages. 

Exceptions and errors can come from any part of this chain, however MSAL Java tries to add context to logs and exceptions wherever possible.

MSAL Java can also forward logs from the native API it interacts with to access WAM, and this behavior can be toggled:
```java
Broker broker = ...;

//Allow MSAL to forward all logs from WAM
broker.enableBrokerLogging(true);

//Allow PII to appear in WAM logs
broker.enableBrokerPIILogging(true);
```

By default MSAL Java will not forward these logs as they can be very verbose, and is mainly helpful when debugging.

## Extra details

Note: This section is not necessary for using WAM or msal4j-brokers, and is just meant to get extra clarity and background on all the components involved.

Although the API for enabling a broker is very simple, a number of packages are used to provide this feature:

- [msal4j-brokers](https://mvnrepository.com/artifact/com.microsoft.azure/msal4j-brokers) - Essentially a thin layer between msal4j and javamsalruntime, meant to handle the conversion between requests from msal4j and results from javamsalruntime
- [javamsalruntime](https://mvnrepository.com/artifact/com.microsoft.azure/javamsalruntime) - A Java project that uses [JNA](https://mvnrepository.com/artifact/com.microsoft.azure/javamsalruntime) to call into native code, converting Java classes and variables into C#/C++ equivalents and vice versa
- MSALRuntime - A C++/C# project that provides APIs for accessing WAM and a number of other features, and eventually other auth brokers. This project produces the dll packages that javamsalruntime calls via JNA, and is the ultimate decider for what scenarios msal4j-brokers can support