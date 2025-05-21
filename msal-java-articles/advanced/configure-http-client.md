---
title: Configure custom HTTP client
description: "MSAL lets you provide your own HTTP client for supporting fine grained configurations."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 01/27/2024
ms.reviewer:
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
---

# Configure custom HTTP client

MSAL lets you provide your own HTTP client for supporting fine grained configurations. To use a custom Http client, you'll need to implement `IHttpClient`. This class, which we will class `HttpClientAdapter`, should implement `send`, which takes in the `HttpRequest` composed by MSAL, executes it, and then constructs `IHttpResponse` with the results of the execution.

```java
class OkHttpClientAdapter implements IHttpClient{

    private OkHttpClient client;

    OkHttpClientAdapter(){

        // You can configure OkHttpClient 
        this.client = new OkHttpClient();  
    }

    @Override
    public IHttpResponse send(HttpRequest httpRequest) throws IOException {
        
        // Map URL, headers, and body from MSAL's HttpRequest to OkHttpClient request object  
        Request request = buildOkRequestFromMsalRequest(httpRequest);

        // Execute Http request with OkHttpClient
        Response okHttpResponse= client.newCall(request).execute();
     
        // Map status code, headers, and response body from OkHttpClient's Response object to MSAL's IHttpResponse
        return buildMsalResponseFromOkResponse(okHttpResponse);
    }
}
```

Once `HttpClientAdapter` has been implemented, it can be set on the client application for which you would like to use it.

```java
IHttpClient httpClient = new OkHttpClientAdapter();
PublicClientApplication pca = PublicClientApplication.builder(
        APP_ID).
        authority(AUTHORITY).
        httpClient(httpClient).
        build();
```

All HTTP requests will now be routed through `httpClient`.
