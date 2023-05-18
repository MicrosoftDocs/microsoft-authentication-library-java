---
title: Why use MSAL4J
description: "Decide when to use MSAL4J to authenticate your users."
---

MSAL4J ([Microsoft Authentication Library for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java)) enables developers to **acquire [tokens](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-dev-glossary#security-token) in order to call secured Web APIs**. These Web APIs can be the Microsoft Graph, other Microsoft APIS, 3rd party Web APIs, or your own Web API.

## MSAL4J Supports multiple application architectures

MSAL4J supports all the possible application topologies including:

- [native client](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-dev-glossary#native-client)  (desktop applications) calling the Microsoft Graph in the name of the user,
- daemons/services or [web clients](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-dev-glossary#web-client)  (Web Apps/ Web APIs) calling the Microsoft Graph in the name of a user, or without a user.

With the exception of:

- [User-agent based client](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-dev-glossary#user-agent-based-client) which is only supported in JavaScript

For details about the supported scenarios see [Scenarios](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki/Scenarios).

## Added value by using MSAL4J over OAuth libraries or coding against the protocol

MSAL4J is a token acquisition library. Depending on your scenario it provides you with various way of getting a token, with a consistent API for a number of platforms.
It also adds value by:

- maintaining a **token cache** and **refreshes tokens** for you when they are close to expire.
  > you don't need to handle expiration on your own.
- helping you specify which **audience** you want your application to sign-in (your org, several orgs, work and school and Microsoft personal accounts, Social identities with Azure AD B2C, users in sovereign and national clouds)
- **helping you troubleshoot** your app by exposing actionable exceptions, logging and telemetry.

## MSAL4J is about acquiring tokens, not protecting an API

MSAL4J is used to acquire tokens. It's not used to protect a Web API. If you are interested in protecting a Web API with Azure AD, you might want to check out:

- [Spring Starter for Azure Active Directory](https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-starters/azure-active-directory-spring-boot-starter)
- [Validating tokens manually](https://docs.microsoft.com/en-us/azure/active-directory/develop/access-tokens#validating-tokens)
