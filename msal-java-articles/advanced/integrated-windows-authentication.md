---
title: Integrated Windows Authentication
description: "If your desktop or mobile application runs on Windows, and on a machine connected to a Windows domain (AD or AAD joined) it is possible to use the Integrated Windows Authentication (IWA) to acquire a token silently."
---

# Integrated Windows Authentication

If your desktop or mobile application runs on Windows, and on a machine connected to a Windows domain (AD or AAD joined) it is possible to use the Integrated Windows Authentication (IWA) to acquire a token silently. No UI is required when using the application.

```java
final String AUTHORITY;
final String APP_ID;
String userName;
List<String> scopes;

PublicClientApplication app = PublicClientApplication.builder(APP_ID)
        .authority(AUTHORITY)
        .build();

IntegratedWindowsAuthenticationParameters parameters =
        IntegratedWindowsAuthenticationParameters.builder(scope, userName).build();

IAuthenticationResult future = app.acquireToken(parameters).get();
```

### Constraints

- *Federated** users only, i.e. where authentication is being federated to an on-premise authority (ADFS for example), or [hybrid scenarios](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-hybrid-identity) where seamless-sso is enabled.  Pure cloud tenants, where users are directly in Azure Active Directory, without any Active Directory backing, cannot use this flow. 
- IWA does NOT bypass MFA (multi factor authentication). If MFA is configured, IWA might fail if an MFA challenge is required, because MFA requires user interaction. 
 > This one is tricky. IWA is non-interactive, but 2FA requires user interactivity. You do not control when the identity provider requests 2FA to be performed, the tenant admin does. From our observations, 2FA is required when you login from a different country, when not connected via VPN to a corporate network, and sometimes even when connected via VPN. Don’t expect a deterministic set of rules, Azure Active Directory uses AI to continuously learn if 2FA is required. You should fallback to a user prompt if IWA fails
- The authority passed in the `PublicApplication` needs to be:
  - tenanted (of the form `https://login.microsoftonline.com/{tenant}/` where `tenant` is either the guid representing the tenant ID or a domain associated with the tenant.
  - for any work and school accounts (`https://login.microsoftonline.com/organizations/`)

  > Microsoft personal accounts are not supported (you cannot use /common or /consumers tenants)

- Because Integrated Windows Authentication is a silent flow:
  - the user of your application must have previously consented to use the application 
  - or the tenant admin must have previously consented to all users in the tenant to use the application.
  - This means that:
     - either you as a developer have pressed the **Grant** button on the Azure portal for yourself, 
     - or a tenant admin has pressed the **Grant/revoke admin consent for {tenant domain}** button in the **API permissions** tab of the registration for the application 
     - or you have provided a way for users to consent to the application 
     - or you have provided a way for the tenant admin to consent for the application 