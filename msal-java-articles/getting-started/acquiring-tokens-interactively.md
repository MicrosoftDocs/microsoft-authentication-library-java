---
title: Acquire tokens interactively in MSAL Java
description: "MSAL Java supports acquiring tokens interactively on public clients through use of the system OS browser."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 02/27/2024
ms.reviewer: dayodeji
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
---

# Acquire tokens interactively

## System Browser

MSAL supports acquiring tokens interactively on public clients through use of the system browser. By default, MSAL will start the system browser as a separate process, direct the user to the authorization URL, and intercept the authorization response. You can [customize this behavior](#customizing-the-experience), such as how the browser window is opened and what the user sees after the authorization, by configuring the `SystemBrowserOptions` parameter of an interactive request.

MSAL will listen on `http://localhost:port` and intercept the code that authority sends when the user is done authenticating. MSAL cannot detect if the user navigates away or simply closes the browser. Apps using this technique are encouraged to define a timeout. We recommend a timeout of at least a few minutes to take into account cases where the user is prompted to change password or perform 2FA.

## How to use the default OS browser

During app registration, configure `http://localhost` as a redirect URI. In the case of B2C, you will need to register a specific port `http://locahost:port`.

```java
PublicClientApplication publicClientApplication =
        PublicClientApplication
                .builder(CLIENT_ID)
                .authority(AUTHORITY)
                .build();

InteractiveRequestParameters parameters = InteractiveRequestParameters
        .builder(new URI("http://localhost"))
        .scopes(scope)
        .build();

IAuthenticationResult result = publicClientApplication.acquireToken(parameters).join();
```

## Customizing the experience

MSAL attempts to open the default system browser, if one is available, on the users machine. You can customize this logic by providing your own implementation of `OpenBrowserAction`

- Implement `OpenBrowserAction`
- Pass custom action to `SystemBrowserOptions`

```java
class CustomOpenBrowserAction implements OpenBrowserAction {
    @Override
    public void openBrowser(URL url){
            //Custom logic to open URL 
    }
}

SystemBrowserOptions options =  
        SystemBrowserOptions
                .builder()
                .openBrowserAction(new CustomOpenBrowserAction())
                .build();
```

You can also customize the HTML message that the user sees after authenticating or even provide a URL where you want the user to be redirected to after authenticating.

```java
SystemBrowserOptions options =  
        SystemBrowserOptions
                .builder()
                .htmlMessageSuccess("CUSTOM_HTML_HERE")
                .htmlMessageError("CUSTOM_HTML_HERE")
                .build();
// OR

SystemBrowserOptions options =
        SystemBrowserOptions
                .builder()
                .browserRedirectSuccess(new URI("http://localhost:port"))
                .browserRedirectError(new URI ("http://localhost:port"))
                .build();
```
