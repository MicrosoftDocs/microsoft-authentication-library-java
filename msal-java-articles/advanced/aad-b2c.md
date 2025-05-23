---
title: Use MSAL4J to sign-in users with social identities
description: "You can use MSAL4J to sign-in users with social identities by using Azure AD B2C."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 01/27/2024
ms.reviewer:
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
#Customer intent: 
---

# Use MSAL4J to sign-in users with social identities

You can use MSAL4J to sign users in with social identities by using [Azure Active Directory B2C](https://aka.ms/aadb2c) (Azure AD B2C). Azure AD B2C is built around the notion of [policies](/azure/active-directory-b2c/custom-policy-overview). In MSAL4J, specifying a policy translates to providing an authority - when you instantiate a client application, you need to specify the policy in the authority configuration

## Authority for a B2C tenant and policy

Generally, the authority to use is `https://login.microsoftonline.com/tfp/{tenant}/{policyName}`, where:

- `tenant` is the name of the Azure AD B2C tenant
- `policyName` the name of the policy to apply (for instance `b2c_1_susi` for sign-in/sign-up).

>[!NOTE]
>The current guidance from the Azure AD B2C team is to use `b2clogin.com` as the authority. For example, `https://{your-tenant-name}.b2clogin.com/tfp/{your-tenant-ID}/{policyname}`. For more information, see [Set redirect URLs to b2clogin.com for Azure Active Directory B2C](/azure/active-directory-b2c/b2clogin).

## Limitations of the username and password flow

Microsoft recommends you do not use the Resource Owner Password Credentials (ROPC), also known as username and password flow. In most scenarios, more secure alternatives are available and recommended. This flow requires a very high degree of trust in the application, and carries risks that are not present in other flows. You should only use this flow when other more secure flows aren't viable. For more information about why you want to avoid using this grant, see [why Microsoft is working to make passwords a thing of the past](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/).


If you're using username and password flows with MSAL4J, be aware of the following limitations:

- The flow only works for local accounts, where you register with Azure AD B2C using an email or username. This flow does not work if federating to any of the identity providers supported by B2C (Facebook, Google, etc.).
- Currently, there is no `id_token` returned from B2C when implementing the ROPC flow from MSAL. This means an that an account object cannot be created, so in the cache, there will be no account and no user. The [`acquireTokenSilently`](xref:com.microsoft.aad.msal4j.AbstractClientApplicationBase.acquireTokenSilently(com.microsoft.aad.msal4j.SilentParameters)) flow will not work in this scenario. However, ROPC does not show a UI, so there will no impact to the user experience.

## Instantiating an application

For confidential client applications:

```java
 ConfidentialClientApplication cca = ConfidentialClientApplication.builder(APP_ID, credential)
    .b2cAuthority(B2C_AUTHORITY)
    .build();
```

For public client applications:

```java
PublicClientApplication pca = new PublicClientApplication.Builder(APP_ID)
    .b2cAuthority(B2C_AUTHORITY)
    .build();
```
