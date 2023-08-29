---
title: Custom token cache serialization (MSAL4j)
description: Learn how to serialize the token cache for MSAL for Java
---

# Custom token cache serialization in MSAL for Java

To persist the token cache between instances of your application, you will need to customize the serialization. The Java classes and interfaces involved in token cache serialization are the following:

- <xref:com.microsoft.aad.msal4j.ITokenCache>: Interface representing security token cache.
- <xref:com.microsoft.aad.msal4j.ITokenCacheAccessAspect> : Interface representing operation of executing code before and after access. You would `@Override` *beforeCacheAccess* and *afterCacheAccess* with the logic responsible for serializing and deserializing the cache.
- <xref:com.microsoft.aad.msal4j.ITokenCacheAccessContext> : Interface representing context in which the token cache is accessed. 

Below is a naive implementation of custom serialization of token cache serialization/deserialization. Do not copy and paste this into a production environment.

```Java
static class TokenPersistence implements ITokenCacheAccessAspect {
String data;

TokenPersistence(String data) {
        this.data = data;
}

@Override
public void beforeCacheAccess(ITokenCacheAccessContext iTokenCacheAccessContext) {
        iTokenCacheAccessContext.tokenCache().deserialize(data);
}

@Override
public void afterCacheAccess(ITokenCacheAccessContext iTokenCacheAccessContext) {
        data = iTokenCacheAccessContext.tokenCache().serialize();
}
```

```Java
// Loads cache from file
String dataToInitCache = readResource(this.getClass(), "/cache_data/serialized_cache.json");

ITokenCacheAccessAspect persistenceAspect = new TokenPersistence(dataToInitCache);

// By setting *TokenPersistence* on the PublicClientApplication, MSAL will call *beforeCacheAccess()* before accessing the cache and *afterCacheAccess()* after accessing the cache. 
PublicClientApplication app = 
PublicClientApplication.builder("my_client_id").setTokenCacheAccessAspect(persistenceAspect).build();
```

## Learn more

Learn about [Get and remove accounts from the token cache using MSAL for Java](msal-java-get-remove-accounts-token-cache.md).
