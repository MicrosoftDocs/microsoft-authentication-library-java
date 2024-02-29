---
title: Frequently asked questions
description: "Some of the most common questions asked about MSAL Java."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 02/27/2024
ms.reviewer: dayodeji
ms.service: msal
ms.subservice: msal-java
ms.topic: faq
---


# Frequently asked questions

## MSAL4J Scope

### What is the main functionality of MSAL?

Acquiring token from a Security Token Service (STS) for a client application to access a protected resource.

### What is MSAL4J?

MSAL is available for many programming languages and platforms. MSAL4J is designed to be used in any application that runs on the Java virtual machine.

### What standard protocols does MSAL follow for token acquisition?

MSAL is implementing a custom version of the OAuth2 protocol. Also, for some specific scenarios, it may internally use other protocols (e.g. WS-Trust).

### Is MSAL a general library for token acquisition using OAuth2 protocol?

No. MSAL is a client library for Microsoft Entra ID, Active Directory Federation Services (ADFS), and Azure Active Directory B2C. There are some custom notions such as “resource” required by ADAL which are considered extensions to the general OAuth2 protocol spec and not supported by other STS’s.

## API Ramp Up

### Should I turn off authority validation by passing false to the constructor?

It depends on what type of authority you talk to. If it is ADFS, you have to pass false as ADFS does not currently support authority validation. If it is Microsoft Entra ID, you still have the option to pass false, but it is recommended to be true, especially if you get the address of the authority from a third party (e.g. via 401 challenge). This is to protect applications and users from being redirected to malicious endpoints to enter their credentials.

### What overload of AcquireToken should I call?

It depends on the type of client application you use and the scenario you need a token for. See the guidance documented in [Acquire Tokens](./acquiring-tokens.md).

## Debugging

### What are the common reasons for failure in using MSAL?

Problems in MSAL could have various reasons. These are the common culprits:

1. Your machine has connection issues.
2. Your applications/users are not properly configured on Microsoft Entra ID or ADFS.
3. You are using an incorrect API for your task (MSAL has several similar overloads for the method AcquireToken).
4. There is a bug in MSAL! Yes, that is always possible. If you are certain that none of the items above are the reason for the failure, please report it to us and we will investigate and fix the bug if exists.

### What tools can I use for diagnosing an issue in ADAL?

There are several diagnostics tools you can use:

1. MSAL Samples: The first best tool is the set of samples published along with MSAL ([inside the library repository](https://github.com/AzureAD/microsoft-authentication-library-for-java/tree/dev/msal4j-sdk/src/samples) as well as samples posted available in the [AzureSamples GitHub organization](https://github.com/AzureSamples)). Try to find the closest sample to your application and download and run it on your machine. If the sample works properly, you need to follow the same steps of the sample app in your application.
2. MSAL diagnostic logs: You can enable logging. This will write some logs with information about the internal steps of MSAL. You may analyze the logs to find the issue. Also, in case you contact the MSAL team, you need to send the logs to help with the analysis. You can find the instruction on how to turn on MSAL logs [in the official documentation](/entra/identity-platform/msal-logging-java).
3. Network traces: [Use a tool like Fiddler](../build/fiddler.md) for recording all the http communications MSAL makes with the server. Using Fiddler is especially easy on Windows desktop machines. Please share the network trace file with the MSAL team in case we are involved in diagnosing your issue.

### What kind of errors are returned from MSAL as exception and what kind is reported to the user?

Most errors are returned from MSAL in forms of an exception; however, there are limited cases in which MSAL shows the error on the browser control. These cases happen mostly when the client cannot be validated or authority server cannot be reached.

### Does MSAL have any kind of retry logic inside?

No. If an operations fails, MSAL reports an error via an exception. The exception includes an error code and also a status code in case the error is returned from the authority. In such cases, it is developer’s job to examine the status code (which mostly reflects the http status code of the response) in the exception and decides whether to retry or not. 502 is usually the status code that warrants a retry.

## MSAL Release Model

### How often does MSAL release a new version?

There is no pre-determined schedule. We try to publish servicing releases very regularly to fix bugs and unblock customers. Major releases usually take longer and we release several preview versions before general availability of a major version.

### What is the compatibility model of MSAL versions?

The goal is to maintain backward compatibility within a major version. For that, we try to only fix bugs or add new features in servicing releases (which increase the minor version). However, there is no compatibility guarantee between major versions. We may add or remove support for certain platforms or scenarios, therefore, you are recommended to fully understand the scope of the changes and fully test the new version before switching to it in your production code.
