---
title: Acquire tokens
description: "In general the way to acquire a token is different depending on the application type - public client application (desktop/mobile) or a confidential client application (Web App, Web API, daemon application like a Windows service)."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 02/27/2024
ms.reviewer: dayodeji
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
---


# Acquiring tokens

As explained in [MSAL for Java scenarios](../index.md#msal-java-scenarios), there are many ways of acquiring a token. Some require user interactions through a web browser. Some don't require any user interactions.

In general the way to acquire a token is different depending on the application type - public client application (desktop/mobile) or a confidential client application (Web App, Web API, daemon application like a Windows service).

## Pre-requisites

Before acquiring tokens with MSAL4J, make sure to instantiate a [client application](./client-applications.md)

## Token acquisition methods

Follow the topics below for detailed explanation with MSAL4J code usage for each token acquisition method.

### Public client applications

- Acquire tokens [interactively with system browser](./acquiring-tokens-interactively.md)
- Acquire tokens by [authorization code](./acquiring-tokens-with-authorization-codes.md) after letting the user sign-in through the authorization request URL.
- It's also possible (but not recommended) to get a token with a [username and password](/entra/identity-platform/scenario-desktop-acquire-token?tabs=java#username--password).
- For applications running on Windows machines and joined to a domain or to Microsoft Entra ID, it is possible to acquire a token silently, leveraging [Integrated Windows Authentication (IWA)](../advanced/integrated-windows-authentication.md).
- Finally, for applications running on devices which don't have a web browser, it's possible to acquire a token through the [device code flow](./device-code-flow.md), which provides the user with a URL and a code. The user goes to a web browser on another device, enters the code and signs-in, and then Microsoft Entra ID returns back a token to the browser-less device.

### Confidential client applications

- Acquire token **as the application itself** using [client credentials](./client-credentials.md), and not for a user. For example, in apps which process users in batches and not a particular user such as in syncing tools.
- In the case of Web Apps or Web APIs **calling another downstream Web API in the name of the user**, use the [On Behalf Of flow](../advanced/service-to-service-calls.md) to acquire a token based on some User assertion (SAML for instance, or a JWT token).
- **For Web apps in the name of a user**, acquire tokens by [authorization code](/entra/identity-platform/scenario-web-app-call-api-acquire-token?tabs=java) after letting the user sign-in through the authorization request URL. This is typically the mechanism used by an application which lets the user sign-in using OpenID Connect, but then wants to access Web APIs for this particular user.

## MSAL4J caches tokens

For both Public client and Confidential client applications, MSAL maintains a token cache, and applications should try to get a token from the cache first before any other means (except in the case of [client credentials](./client-credentials.md), which looks at the cache by itself). Take a look at the [recommended pattern](/entra/identity-platform/scenario-desktop-acquire-token?tabs=java) for token acquisition.

To be able to make use of the cache, the application needs to customize the [token cache serialization](../advanced/msal-java-token-cache-serialization.md).
