---
title: Microsoft Authentication Library for Java
description: "The Microsoft Authentication Library for Java (usually shortened to MSAL Java or MSAL4J) enables applications to integrate with the Microsoft identity platform."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 03/27/2025
ms.service: msal
ms.subservice: msal-java
ms.topic: article
#Customer intent: 
---


# Microsoft Authentication Library for Java

The Microsoft Authentication Library for Java (MSAL Java or MSAL4J) integrates applications with the [Microsoft identity platform](/entra/identity-platform/v2-overview). It allows you to sign in users or apps with Microsoft identities (Microsoft Entra ID, Microsoft accounts, and Azure AD B2C accounts) and get tokens to call Microsoft APIs like [Microsoft Graph](https://graph.microsoft.io/) or your own APIs. MSAL Java uses industry standard OAuth2 and OpenID Connect protocols.

## Overview

1. [Why use MSAL4J?](getting-started/why-use-msal4j.md)
1. **Prerequisite**: Before using MSAL4J you will have to [register your applications with Microsoft Entra ID](/entra/identity-platform/quickstart-register-app).
1. To start using MSAL4J, instantiate and configure the [client application](getting-started/client-applications.md).
1. Learn about the ways to [acquire a token](getting-started/acquiring-tokens.md) using MSAL4J.
1. Follow [best practices for a robust enterprise ready application](advanced/best-practices-enterprise.md).
1. Refer [FAQ](getting-started/faq.md) for common issues and known bugs.

## MSAL Java scenarios

MSAL4J can be used by applications to acquire tokens to access protected APIs. Tokens can be acquired by different **application types**: desktop applications, web applications, web APIs, and applications running on devices that don't have a browser (such as IoT devices). In MSAL4J, applications are categorized as follows:

- **Public client applications (desktop and mobile)**. These types of apps cannot store app secrets securely.
- **Confidential client applications (web apps, web APIs, and daemon applications)**. These type of apps securely store a secret registered with Microsoft Entra ID.

Learn more details about instantiating and configuring the above in the [Client applications](./getting-started/client-applications.md) topic.

MSAL4J supports acquiring tokens either in the name of a user or in the name of the application itself (without a user). In the latter case, a confidential client application must be used.

MSAL4J can be used in applications running on different operating systems (Windows, Linux, macOS).

Key scenarios supported by MSAL4J:

- [Web application that signs in users](/entra/identity-platform/scenario-web-app-sign-user-overview)
- [Web Application signing in a user and calling a Web API in the name of the user](/entra/identity-platform/scenario-web-app-call-api-overview)
- [Desktop application calling a Web API in the name of the signed-in user](/entra/identity-platform/scenario-desktop-overview)
- [Desktop/service daemon application calling Web API without a user](/entra/identity-platform/scenario-daemon-overview)
- [Application without a browser, or IOT application calling an API in the name of the user](/entra/identity-platform/scenario-desktop-acquire-token?tabs=java#command-line-tool-without-web-browser)

Can't find the scenario you are looking for? Check out the [supported scenarios and platforms](/entra/identity-platform/authentication-flows-app-scenarios#scenarios-and-supported-platforms-and-languages) across MSAL libraries.

## Releases

Refer to [MSAL Java releases on GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases).
