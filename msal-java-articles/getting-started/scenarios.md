---
title: MSAL Java scenarios
description: "MSAL4J can be used by applications to acquire tokens to access a protected API."
---

# MSAL Java scenarios

MSAL4J can be used by applications to acquire tokens to access a protected API. Tokens can be acquired by different **application types**: Desktop applications, Web applications, Web APIs, and applications running on devices that don't have a browser (such as IoT). In MSAL4J, applications are categorized as follows:

- Public client applications (Desktop). These type of apps cannot store app secrets securely.
- Confidential client applications (Web apps, Web APIs, and daemon applications). These type of apps securely store a secret registered with Azure AD.

Learn more details about instantiating and configuring the above in [Client applications](./client-applications.md) topic.
MSAL4J supports acquiring tokens either in the name of a user, or, in the name of the application itself (without a user). In the latter case, a confidential client application must be used.

MSAL4J can be used in applications running on different operating systems (Windows, Linux, Mac). Scenarios might differ depending on the platform.

Here are the key scenarios supported by MSAL4J. You can read the detailed explanations with MSAL4J code usage by following the links.

- [Web application that signs in users](/azure/active-directory/develop/scenario-web-app-sign-user-overview)
- [Web Application signing in a user and calling a Web API in the name of the user](/azure/active-directory/develop/scenario-web-app-call-api-overview)
- [Desktop application calling a Web API in the name of the signed-in user](/azure/active-directory/develop/scenario-desktop-overview)
- [Desktop/service daemon application calling Web API without a user](/azure/active-directory/develop/scenario-daemon-overview)
- [Application without a browser, or IOT application calling an API in the name of the user](/azure/active-directory/develop/scenario-desktop-acquire-token?tabs=java#command-line-tool-without-web-browser)

Can't find the scenario you are looking for? Check out the [supported scenarios and platforms](/azure/active-directory/develop/authentication-flows-app-scenarios#scenarios-and-supported-platforms-and-languages) across MSAL libraries.
