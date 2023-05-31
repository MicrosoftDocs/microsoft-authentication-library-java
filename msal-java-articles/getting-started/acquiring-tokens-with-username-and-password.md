---
title: Acquiring tokens with a username and password
description: "MSAL4J supports in the user name and password flow for public client applications."
---

# Acquiring tokens with a username and password

MSAL4J supports in the user name and password flow for public client applications. In general Microsoft does not advise customers to use it as it's less secure than the other flows, and it is not compatible with conditional access because if the resource requires conditional access, the call to acquire token will just fail, given that this is not an interactive flow (the STS does not have an opportunity to present a dialog to the user to tell them that they need to do multiple factor authentication).

The preferred flow for acquiring a token silently on Windows domain joined machines is [Integrated Windows Authentication](../advanced//integrated-windows-authentication.md). Otherwise you can also use [Device code flow](../getting-started/device-code-flow.md)

> [!NOTE]
> Although this is useful in some cases (DevOps scenarios), if you want to use Username/password in interactive scenarios where you provide your own UI, you should really think about how to move away from it. By using username/password you are giving-up a number of things:
>
> - Core tenets of modern identity: password gets fished, replayed. Because we have this concept of a shared secret that can be intercepted.
> This is incompatible with passwordless.
> - users who need to do MFA won't be able to sign-in (as there is no interaction)
> - Users won't be able to do single sign-on

```java
final String AUTHORITY;
final String APP_ID;
String userName;
String password;
List<String> scopes;

PublicClientApplication pca = new PublicClientApplication.Builder(APP_ID).
        authority(AUTHORITY).
        build();

UserNamePasswordParameters paramaters = 
                UserNamePasswordParameters.builder(
                    scopes,
                    userName,
                    password.toCharArray()).build();

IAuthenticationResult result = pca.acquireToken(parameters).get();
```

For more information about why you want to avoid using this grant, learn why [Microsoft is working to make passwords a thing of the past](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/).
