The Microsoft Authentication Library for Java (usually shortened to MSAL Java or MSAL4J) enables applications to integrate with the [Microsoft identity platform](https://aka.ms/aaddevv2). It allows you to sign in users or apps with Microsoft identities (Azure AD, Microsoft accounts and Azure AD B2C accounts) and obtain tokens to call Microsoft APIs such as [Microsoft Graph](https://graph.microsoft.io/) or your own APIs registered with the Microsoft identity platform. It is built using industry standard OAuth2 and OpenID Connect protocols.
<br/><br/>

## Overview

1. [Why use MSAL4J?](Why-use-MSAL4J)

2. **Pre-requisite**: Before using MSAL4J you will have to [register your applications with Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications).

3. Learn about the supported [scenarios](Scenarios).

4. To start using MSAL4J, instantiate and configure the [client application](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki/Client-Applications).

5. Learn about the ways to [acquire a token](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki/Acquiring-Tokens) using MSAL4J. This returns an [IAuthenticationResult](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki/IAuthenticationResult).

6. Follow [best practices for a robust enterprise ready application](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki/Best-Practices-for-a-robust-enterprise-ready-application).

7. Refer [FAQ](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki/FAQs) for common issues and known bugs.
<br/><br/>


## Roadmap
Date | Release | Main features
-----| ------- | ---------|
Releases | [All Releases](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases)|
Mar 1 2023 | [1.13.5](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.13.5) | Added support for current and legacy B2c authority formats.
Jan 30 2023 | [1.13.4](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.13.4) | Regional endpoint updates
Oct 31 2022 | [1.13.3](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.13.3) | Update jackson-databind version to 2.13.4.2
Oct 10 2022 | [1.13.2](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.13.2) | Add IBroker interface
Sep 20 2022 | [1.13.1](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.13.1) | Bug fixes and improvements for region API
Jun 30 2022 | [1.13.0](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.13.0) | Provide token caching functionality for managed identity tokens
May 06 2022 | [1.12.0](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.12.0) | Updates several dependencies to avoid security vulnerabilities 
Mar 25 2022 | [1.11.3](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.11.3) | Allow client assertions as callbacks and as per-request parameters 
Feb 9 2022 | [1.11.2](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.11.2) | Updated oauth2-oidc-sdk version to address security vulnerability
Feb 3 2022 | [1.11.1](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.11.1) | Fixed Retrofit security vulnerabilities
Jul 6 2021| [1.11.0](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.11.0) | Adds ability to override authority in AcquireToken callsSupport for ADFS 2019 
Jun 16 2021| [1.10.1](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.10.1) | Improved behavior when using regional authorities. Scope override issue in OBO flow has been fixed.
Apr 26 2021 | [1.10.0](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.10.0) | Instance aware support for interactive requests. Default cache lookup for on-behalf-of and client credential flows.
_History_ |  | [Memory Lane](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases?page=2)
