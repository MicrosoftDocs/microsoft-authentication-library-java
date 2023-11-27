---
title: Acquiring tokens with authorization codes
description: "The Authorization Code flow is suitable when the application requires the user's interaction with the Microsoft Entra STS during authentication."
---

# Acquiring tokens with authorization codes

The Authorization Code flow is suitable when the application requires the user's interaction with the Microsoft Entra STS during authentication. One such case is when users login to Web applications (web sites) using OpenID Connect. The web application receives an authorization code which it can redeem to acquire a token for Web APIs.

Requests for authorization codes are delegated to the developer. To understand how to request an authorization code, see [Authorization code flow](/azure/active-directory/develop/active-directory-protocols-oauth-code). To construct the authorization code URL where the user will input their credentials, you can use the [authorization code URL builder](../advanced/authorization-code-url-builder.md)

## Code snippet

```java
PublicClientApplication pca = new PublicClientApplication.Builder(APP_ID)
        .authority(AUTHORITY)
        .build();

IAuthenticationResult result = pca.acquireToken(AuthorizationCodeParameters
        .builder(authCode, new URI(REPLY_URL))
        .scopes(scope)
        .build())
        .get();
```
