---
title: Why use MSAL4J
description: "Decide when to use MSAL4J to authenticate your users."
---

# Why use MSAL4J

MSAL4J ([Microsoft Authentication Library for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java)) enables developers to **acquire [tokens](/azure/active-directory/develop/active-directory-dev-glossary#security-token) in order to call secured web APIs**. These web APIs include Microsoft Graph, other Microsoft APIS, third party web APIs, or your own web API.

## Multiple application architectures

MSAL for Java, or MSAL4J for short, supports all the possible application topologies including:

- [Native client](/azure/active-directory/develop/active-directory-dev-glossary#native-client)  (desktop applications) calling an API, such as Microsoft Graph, in the name of the user.
- Daemons/services or [web clients](/azure/active-directory/develop/active-directory-dev-glossary#web-client)  (web apps/ web APIs) calling other APIs, such as Microsoft Graph in the name of a user or without a user.

MSAL4J does not support [user-agent based clients](/azure/active-directory/develop/active-directory-dev-glossary#user-agent-based-client), which are only supported in JavaScript.

For details about the supported scenarios see [Scenarios](./scenarios.md).

## Value of MSAL4J over generic lirbaries

MSAL4J is a token acquisition library. Depending on your scenario it provides you with various way of getting a token, with a consistent API for a number of platforms.

It also adds value by:

- Maintaining a **token cache** and **automatically refreshing tokens** for you when they are close to expiration.
- Helping you specify which **audience** you want your application to sign-in (your org, several orgs, work and school and Microsoft personal accounts, social identities with Azure AD B2C, users in sovereign and national clouds).
- **Helping you troubleshoot** your app by exposing actionable exceptions, logging, and telemetry.

## Token acquisition

MSAL4J is used to acquire tokens. It's not used to protect a Web API. If you are interested in protecting a Web API with Azure AD, check out the following resources:

- [Spring Starter for Azure Active Directory](https://github.com/Azure/azure-sdk-for-java/tree/main/sdk/spring/spring-cloud-azure-starter-active-directory)
- [Validating tokens manually](/azure/active-directory/develop/access-tokens#validating-tokens)
