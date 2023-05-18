---
title: Best practices for enterprises
description: "To build robust, enterprise-ready applications, you will need to follow some of the best patterns and practices in using MSAL Java."
---

# Best practices for enterprises

You've seen that with MSAL4J you can quite simply acquire a token for a protected Web API. You also don't have to handle refreshing tokens yourself.

However, to build robust, enterprise ready applications, you will need to do a bit more. For instance you'll want to:

- Handle exceptions, both when you acquire a token, but also when you call the protected Web API. In particular, if your application runs in an Azure AD tenant where the tenant admins have set Conditional Access policies to enforce Multiple Factor Authentication (MFA), you will need to handle a Claim challenge which is described in [Exceptions](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki/Exceptions).

- You might want to enable [Logging](https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-logging?tabs=java) to troubleshoot your application and help your users, while respecting their privacy and being compliant with GDPR.
