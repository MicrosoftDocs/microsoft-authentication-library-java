---
title: Use MSAL4J to sign-in users with social identities
description: "You can use MSAL4J to sign-in users with social identities by using Azure AD B2C."
---

# Use MSAL4J to sign-in users with social identities

You can use MSAL4J to sign-in users with social identities by using [Azure AD B2C](https://aka.ms/aadb2c). AAD B2C is built around the notion of policies. In MSAL4J, specifying a policy translates to providing an authority.

- When you instantiate the client application, you need to specify the policy in authority

## Authority for a B2C tenant and policy

The authority to use is `https://login.microsoftonline.com/tfp/{tenant}/{policyName}` where:

- `tenant` is the name of the Azure AD B2C tenant
- `policyName` the name of the policy to apply (for instance "b2c_1_susi" for sign-in/sign-up).

The current guidance from B2C is to use `b2clogin.com` as the authority. For example, `https://{your-tenant-name}.b2clogin.com/tfp/{your-tenant-ID}/{policyname}`. For more information, see this [documentation](https://docs.microsoft.com/en-us/azure/active-directory-b2c/b2clogin).

## Limitations of the Username/Password flow

- This only works for local accounts (where you register with B2C using an email or username). This flow does not work if federating to any of the IdPs supported by B2C (Facebook, Google, etc...).
- Currently, there is no id_token returned from B2C when implementing the ROPC flow from MSAL. This means an Account object cannot be created, so in the cache, there will be no Account and no user. The AcquireTokenSilent flow will not work in this scenario. However, ROPC does not show a UI, so there will no impact to the user experience.

## Instantiating the application

```java
 ConfidentialClientApplication cca = ConfidentialClientApplication.builder(APP_ID, credential)
                    .b2cAuthority(B2C_AUTHORITY)
                    .build();
```

```java
PublicClientApplication pca = new PublicClientApplication.Builder(APP_ID)
                .b2cAuthority(B2C_AUTHORITY)
                .build();
```
