Active Directory Federation Services (AD FS) in Windows Server enables you to add OpenID Connect and OAuth 2.0 based authentication and authorization to applications you are developing. Those applications can authenticate users directly against AD FS. For more information, read [AD FS Scenarios for Developers](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/overview/ad-fs-scenarios-for-developers).


There are usually two ways of authenticating against AD FS:

- MSAL connects to Azure Active Directory, which then federates to AD FS.
- MSAL connects directly to an AD FS authority. 

MSAL4J supports both these flows.

## MSAL connects to Azure AD, which then federates to AD FS
MSAL4J supports connecting to Azure AD, which signs in managed-users (users managed in Azure AD) or federated users (users managed by another identity provider such as AD FS). MSAL4J does not know about the fact that users are federated. As far as it’s concerned, it talks to Azure AD.

The authority you use in this case is the usual authority (authority host name + tenant, common, or organizations).

### Acquiring a token interactively for federated users
When you call the AcquireToken with AuthorizationCodeParameters or DeviceCodeParameters, the user experience is typically:

1. The user enters their account ID.
2. Azure AD displays briefly the message "Taking you to your organization's page".
The user is redirected to the sign-in page of the identity provider. The sign-in page is usually customized with the logo of the organization.
3. Supported AD FS versions in this federated scenario are AD FS v2, AD FS v3 (Windows Server 2012 R2), and AD FS v4 (AD FS 2016).

## MSAL connects directly to an AD FS authority

MSAL4J provides support for directly authenticating with AD FS 2019. In this case, you can provide the ADFS specific authority URL to MSAL4J when initializing the client. The authority value should look something like https://adfs.contoso.com/adfs

## Acquiring a token using AcquireToken with IntegratedWindowsAuthenticationParameters or UsernamePasswordParameters
When acquiring a token using AcquireToken with IntegratedWindowsAuthenticationParameters or UsernamePasswordParameters, MSAL4J gets the identity provider which to contact based on the username. MSAL4J receives a SAML token after contacting the identity provider. MSAL4J then provides the SAML token to Azure AD as a user assertion (similar to the on-behalf-of flow) to get back a JWT.