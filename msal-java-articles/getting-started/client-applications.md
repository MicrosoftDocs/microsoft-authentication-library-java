## Instantiating an Application

### Pre-requisites

Before instantiating your app with MSAL4J,
1. Understand the types of Client applications available- [Public Client and Confidential Client applications](https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-client-applications).
1. You'll need to [register](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app) the application with Azure AD. You will therefore know:
    - Its `clientID` (a string representing a GUID)
    - The identity provider  URL (named the instance) and the sign-in audience for your application. These two parameters are collectively known as the authority.
    - Possibly the `TenantID` in the case you are writing a line of business application (just for your organization, also named single-tenant application)
    - In case it's a confidential client app, its application secret (`clientSecret` string) or certificate
    - For web apps, you'll have also set the `redirectUri` where the identity provider will contact back your application with the security tokens.

### Instantiating a Public Client application

```java
String PUBLIC_CLIENT_ID;
String AUTHORITY;

PublicClientApplication app = 
    PublicClientApplication
        .builder(PUBLIC_CLIENT_ID)
        .authority(AUTHORITY)
        .build();
```

### Instantiating a Confidential Client application

You will need either a secret or a certificate, as described in the [Client Credentials page](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki/Client-Credentials). 

If you have a secret:
```java
String PUBLIC_CLIENT_ID;
String AUTHORITY;
String CLIENT_SECRET;

IClientCredential credential = ClientCredentialFactory.createFromSecret(CLIENT_SECRET);
ConfidentialClientApplication app = 
    ConfidentialClientApplication
        .builder(PUBLIC_CLIENT_ID, credential)
        .authority(AUTHORITY)
        .build();
```

If you have a certificate:
```java
String PUBLIC_CLIENT_ID;
String AUTHORITY;
PrivateKey PRIVATE_KEY;  
X509Certificate PUBLIC_KEY;

IClientCredential credential = ClientCredentialFactory.createFromCertificate(PRIVATE_KEY, PUBLIC_KEY);
ConfidentialClientApplication app = 
    ConfidentialClientApplication
        .builder(PUBLIC_CLIENT_ID, credential)
        .authority(AUTHORITY)
        .build();
```
