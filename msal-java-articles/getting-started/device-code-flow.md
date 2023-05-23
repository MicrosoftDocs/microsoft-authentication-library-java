---
title: Device code flow
description: "Interactive authentication with Azure AD requires a web browser. However, in the case of devices and operating systems that do not provide a Web browser, Device code flow lets the user use another device (for instance another computer or a mobile phone) to sign-in interactively."
---

# Device code flow

Interactive authentication with Azure AD requires a web browser. However, in the case of devices and operating systems that do not provide a Web browser, Device code flow lets the user use another device (for instance another computer or a mobile phone) to sign-in interactively. By using the device code flow, the application obtains tokens through a two-step process especially designed for these devices/OS. Examples of such applications are applications running on iOT, or Command-Line tools (CLI). The idea is that:

## Constraints

- Device Code Flow is only available for public client applications
- The authority passed in the `PublicClientApplication` needs to be configured as one of the options below:
  - Tenanted of the form `https://login.microsoftonline.com/{tenant}/` where `tenant` is either the GUID representing the tenant ID or a domain associated with the tenant.
  - Any work and school accounts (`https://login.microsoftonline.com/organizations/`)

> Microsoft personal accounts are not yet supported by the Azure AD v2.0 endpoint (you cannot use `/common` or `/consumers` tenants).

## Code snippet

```java
PublicClientApplication app = PublicClientApplication.builder(PUBLIC_CLIENT_ID)
        .authority(AUTHORITY)
        .build();

Consumer<DeviceCode> deviceCodeConsumer = (DeviceCode deviceCode) -> {
    System.out.println(deviceCode.message());
};

CompletableFuture<IAuthenticationResult> future = app.acquireToken(
        DeviceCodeFlowParameters.builder(scope, deviceCodeConsumer).build());


future.handle((res, ex) -> {
    if(ex != null) {
        System.out.println("message - " + ex.getMessage());
        return "Unknown!";
    }
    System.out.println("Access Token - " + res.accessToken());
    System.out.println("ID Token - " + res.idToken());
    return res;
});

future.join();
```

## Additional information

In case you want to learn more about Device code flow:

- [OAuth standard - device flow](https://tools.ietf.org/html/draft-ietf-oauth-device-flow-07#section-3.4)
