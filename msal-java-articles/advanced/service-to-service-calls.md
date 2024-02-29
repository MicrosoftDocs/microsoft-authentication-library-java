---
title: Service-to-service authentication
description: "Web APIs can acquire tokens in the name of a user, leveraging User assertions."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 02/27/2024
ms.reviewer: dayodeji
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
---


# Service-to-service authentication

Web APIs can acquire tokens in the name of a user, leveraging User assertions. Web API cannot have any user interaction, and therefore when a web API (named "Web API #1") needs to call another Web API (named "Web API #2") in the name of a user, it needs to use the [On Behalf Of OAuth 2.0 flow](/entra/identity-platform/v2-oauth2-on-behalf-of-flow).

This flow is a confidential client flow, and therefore the first web API provides client credentials (client secret or certificate), as well as an `UserAssertion`. The first web API will receive a bearer token and send it to Microsoft Entra ID by embedding it into a `UserAssertion` to request another token to the downstream second Web API.

```java  
// This is the confidential client application representing Web Api #1
ConfidentialClientApplication cca =
        ConfidentialClientApplication.builder(clientId, ClientCredentialFactory.create(CLIENT_SECRET)).
                authority(AUTHORITY).
                build();

// Create an UserAssertion with the access token received from the client application 
UserAssertion userAssertion = new UserAssertion(accessToken);

AuthenticationResult result =
        cca.acquireToken(
                OnBehalfOfParameters.builder(
                        Scope,             
                        userAssertion).
                        build()).
                        get();
```
