---
title: Client applications
description: "How to start configuring client applications with MSAL Java."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 02/27/2024
ms.reviewer:
ms.service: msal
ms.subservice: msal-java
ms.topic: article
#Customer intent: 
---


# Client applications

## Instantiate an application

### Pre-requisites

Before instantiating your app with MSAL4J:

1. Understand the types of Client applications available- [Public Client and Confidential Client applications](/entra/identity-platform/msal-client-applications).
1. You'll need to [register](/entra/identity-platform/quickstart-register-app) the application with Microsoft Entra ID. You will therefore know:
    - Its `clientID` (a string representing a GUID)
    - The identity provider  URL (named the instance) and the sign-in audience for your application. These two parameters are collectively known as the authority.
    - Possibly the `TenantID` in the case you are writing a line of business application (just for your organization, also named single-tenant application)
    - In case it's a confidential client app, its application secret (`clientSecret` string) or certificate
    - For web apps, you'll have also set the `redirectUri` where the identity provider will contact back your application with the security tokens.

### Instantiate a Public Client application

```java
String PUBLIC_CLIENT_ID;
String AUTHORITY;

PublicClientApplication app = 
    PublicClientApplication
        .builder(PUBLIC_CLIENT_ID)
        .authority(AUTHORITY)
        .build();
```

### Instantiate a Confidential Client application

You will need either a secret or a certificate, as described in [Client Credentials](./client-credentials.md).

If you have a secret:

```java
String PUBLIC_CLIENT_ID;
String AUTHORITY;
String CLIENT_SECRET;

IClientCredential credential = ClientCredentialFactory.createFromSecret(CLIENT_SECRET);
ConfidentialClientApplication app = 
    ConfidentialClientApplication
        .builder(PUBLIC_CLIENT_ID, credential)
        .authority(AUTHORITY)
        .build();
```

If you have a certificate:

```java
String PUBLIC_CLIENT_ID;
String AUTHORITY;
PrivateKey PRIVATE_KEY;  
X509Certificate PUBLIC_KEY;

IClientCredential credential = ClientCredentialFactory.createFromCertificate(PRIVATE_KEY, PUBLIC_KEY);
ConfidentialClientApplication app = 
    ConfidentialClientApplication
        .builder(PUBLIC_CLIENT_ID, credential)
        .authority(AUTHORITY)
        .build();
```
