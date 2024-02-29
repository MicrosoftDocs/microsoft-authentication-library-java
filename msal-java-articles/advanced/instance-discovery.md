---
title: Providing your own instance discovery metadata
description: "MSAL4J performs instance discovery before any acquireToken() or GetAccount() API calls. The request is used to get the OpenId metadata endpoint and authority aliases used in the token cache, among other things (these details are not important, as MSAL4J abstracts them from developers)."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 01/27/2024
ms.reviewer: dayodeji
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
---

# Providing your own instance discovery metadata

## Context

MSAL4J performs instance discovery before any `acquireToken()` or `GetAccount()` API calls. The request is used to get the OpenId metadata endpoint and authority aliases used in the token cache, among other things (these details are not important, as MSAL4J abstracts them from developers).

MSAL4J caches this metadata in-memory to avoid having to make this network call every time. In this scenario, the first API call will make a network request, the data will be cached, and MSAL will continue to use the cached data for the lifetime of the process.

In scenarios where an application creates a new process for each operation (for instance in some CLI tools), no data instance discovery data is available in the in-memory cache, and the library will always have to make a network call. For certain applications, this behavior can be undesirable for performance reasons, specifically if the application is making a call to `acquireTokenSilently()` or `getAccounts()`, where the tokens or accounts are already on the client, and thus the instance discovery request is the biggest source of latency.

## Workaround

If you have an application where this scenario is applicable, you can now pass in the instance discovery response data at the creation of the MSAL4J client application object. MSAL4J will use this data instead of making a network call.

To do this:

Make an HTTP GET to `https://{host}/common/discovery/instance?api-version=1.1&authorization_endpoint={authorizeEndpoint}`

`{host}` - host of your authority. For example, login.microsoftonline.com

`{authorizeEndpoint}` - Authorization endpoint 

For example - `https://login.microsoftonline.com/common/discovery/instance?api-version=1.1&authorization_endpoint=https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize`

This will return a JSON formatted blob similar to: 

```json
{
  "tenant_discovery_endpoint": "https://login.microsoftonline.com/common/.well-known/openid-configuration",
  "api-version": "1.1",
  "metadata": [
    {
      "preferred_network": "login.microsoftonline.com",
      "preferred_cache": "login.windows.net",
      "aliases": [
        "login.microsoftonline.com",
        "login.windows.net",
        "login.microsoft.com",
        "sts.windows.net"
      ]
    },
    {
      "preferred_network": "login.partner.microsoftonline.cn",
      "preferred_cache": "login.partner.microsoftonline.cn",
      "aliases": [
        "login.partner.microsoftonline.cn",
        "login.chinacloudapi.cn"
      ]
    },
    {
      "preferred_network": "login.microsoftonline.de",
      "preferred_cache": "login.microsoftonline.de",
      "aliases": [
        "login.microsoftonline.de"
      ]
    },
    {
      "preferred_network": "login.microsoftonline.us",
      "preferred_cache": "login.microsoftonline.us",
      "aliases": [
        "login.microsoftonline.us",
        "login.usgovcloudapi.net"
      ]
    },
    {
      "preferred_network": "login-us.microsoftonline.com",
      "preferred_cache": "login-us.microsoftonline.com",
      "aliases": [
        "login-us.microsoftonline.com"
      ]
    }
  ]
}
```

You can then store this information and pass it in when building a `PublicClientApplication` or `ConfidentialClientApplication` object.

```java
String instanceDiscoveryResponse = readResource(
        "/aad_instance_discovery_response.json");

PublicClientApplication app = PublicClientApplication.builder("client_id")
        .instanceDiscoveryMetadata(instanceDiscoveryResponse)
        .build();
```

Keep in mind:

- The data passed in must be valid JSON
- When using this feature, the customer is responsible for storing the data and refreshing it appropriately
- MSAL already caches this data. This only reduces latency in scenarios where the application is creating a new process for each operation.