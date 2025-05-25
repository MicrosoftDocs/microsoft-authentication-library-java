---
title: Authorization code URL builder
description: "When performing Oauth2 authorization code flow, the first step is to direct the user to the authorization endpoint, where they will authenticate, and the identity provider will respond with an authentication code."
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

# Authorization code URL builder

When performing Oauth2 authorization code flow, the first step is to direct the user to the authorization endpoint where they will authenticate and the identity provider will respond with an authentication code. The authentication code can then be used to redeem a token by calling MSALs [`PublicClientApplication.acquireToken(AuthorizationCodeParameters)`](xref:com.microsoft.aad.msal4j.AbstractClientApplicationBase.acquireToken(com.microsoft.aad.msal4j.AuthorizationCodeParameters)) or [`ConfidentialClientApplication.acquireToken(AuthorizationCodeParameters)`](xref:com.microsoft.aad.msal4j.AbstractClientApplicationBase.acquireToken(com.microsoft.aad.msal4j.AuthorizationCodeParameters)).

## Authorization URL builder

As of MSAL4J 1.4, there is now a helper method, [`getAuthorizationRequestUrl`](xref:com.microsoft.aad.msal4j.AbstractClientApplicationBase.getAuthorizationRequestUrl(com.microsoft.aad.msal4j.AuthorizationRequestUrlParameters)), that can be used to craft the authorization code URL, used in the first step of OAuth2 authorization code flow.

```java
PublicClientApplication publicClientApplication =
        PublicClientApplication
                .builder(CLIENT_ID)
                .authority(AUTHORITY)
                .build();

AuthorizationRequestUrlParameters parameters =
        AuthorizationRequestUrlParameters
                .builder(interactiveRequestParameters.redirectUri().toString(),
                        interactiveRequestParameters.scopes())
                .codeChallenge(verifier)
                .codeChallengeMethod("S256")
                .state(state);
                .build();

URL authorizationCodeUrl = publicClientApplication.getAuthorizationRequestUrl(parameters);
```
