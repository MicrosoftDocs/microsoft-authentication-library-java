---
title: Get & remove accounts from the token cache (MSAL4j)
description: Learn how to view and remove accounts from the token cache using the Microsoft Authentication Library for Java.
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 01/27/2024
ms.reviewer:
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
---

# Get and remove accounts from the token cache using MSAL for Java

MSAL for Java provides an in-memory token cache by default. The in-memory token cache persists for the duration of the application execution.

## See which accounts are in the cache

You can check what accounts are in the cache by calling `PublicClientApplication.getAccounts()` as shown in the following example:

```java
PublicClientApplication pca = new PublicClientApplication.Builder(
                labResponse.getAppId()).
                authority(TestConstants.ORGANIZATIONS_AUTHORITY).
                build();

Set<IAccount> accounts = pca.getAccounts().join();
```

## Remove accounts from the cache

To remove an account from the cache, find the account that needs to be removed and then call `PublicClientApplication.removeAccount()` as shown in the following example:

```java
Set<IAccount> accounts = pca.getAccounts().join();

IAccount accountToBeRemoved = accounts.stream().filter(
                x -> x.username().equalsIgnoreCase(
                        UPN_OF_USER_TO_BE_REMOVED)).findFirst().orElse(null);

pca.removeAccount(accountToBeRemoved).join();
```

## Learn more

If you are using MSAL for Java, learn about [Custom token cache serialization in MSAL for Java](msal-java-token-cache-serialization.md).
