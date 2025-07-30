---
title: Known issues
description: "This page covers known issues that exist outside of MSAL4J, such as those found in dependencies or services that MSAL4J relies on, and potential workarounds to use until those issues are resolved."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 01/27/2024
ms.reviewer:
ms.service: msal
ms.subservice: msal-java
ms.topic: troubleshooting-known-issue
#Customer intent: 
---

# Known issues

This page covers known issues that exist outside of MSAL4J, such as those found in dependencies or services that MSAL4J relies on, and potential workarounds to use until those issues are resolved.

## B2C

### Missing access token when using B2C

Cause: There is an assumption in MSAL4J that the authorization server will always return an access token in a response to a valid request, as per the [OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html#TokenResponse) and [OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-4.1.4) specs. However this assumption leads to an exception in MSAL4J in certain B2C flow scenarios, because the B2C service [only returns an access token](/azure/active-directory-b2c/openid-connect#get-a-token) if custom scopes are included the request.

Workaround: Until this issue is resolved on the B2C side, you can avoid an exception in MSAL4J by including a custom scope in the token request. The B2C docs [recommend using the client ID](/azure/active-directory-b2c/openid-connect#get-a-token) of the application as the scope.

See [this issue](https://github.com/AzureAD/microsoft-authentication-library-for-java/issues/140) for more information.
